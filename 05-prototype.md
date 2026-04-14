# 05 — Prototype + Design Specs

## MVP Scope

The MVP targets the Damongo/Larabanga corridor around Mole National Park. Pilot: 200 community contributors, 6-month deployment, Q1 2026.

---

## Feature Specification

### F1 — Capture Module
| Feature | Spec |
|---------|------|
| Camera trigger | Native device camera, full resolution |
| GPS tagging | Automatic lat/lng + ±10m accuracy indicator |
| Observation form | 6 fields: species (AI-assisted), behaviour, habitat, group size, confidence level, notes (voice-to-text) |
| Offline storage | Up to 500 photos cached locally before sync required |
| Batch sync | Auto-sync on wifi or manual sync on any connection |

### F2 — AI Species Identification
| Feature | Spec |
|---------|------|
| Model | MobileNetV3 fine-tuned on 420 savanna species (Ghana-specific dataset compiled from iNaturalist + Ghana Wildlife Division records) |
| Inference | On-device, TensorFlow Lite, <2s per image |
| Accuracy target | Top-3 accuracy >85% for common species; flagged for expert review otherwise |
| Training data | 120,000 labelled images; quarterly model updates pushed via delta download |
| Fallback | Unknown species flag → routes to expert reviewer queue |

### F3 — Earnings Dashboard
| Feature | Spec |
|---------|------|
| Wallet display | Total earned (lifetime), pending (under review), cooperative fund contribution |
| Photo status tracker | Submitted → Under review → Listed → Licensed → Paid |
| Payout | Mobile Money (MTN Mobile Money, Vodafone Cash) — bi-weekly settlement |
| Minimum payout | GHS 10 (~$0.65 USD) |
| Transaction history | 12-month scrollable ledger |

### F4 — Marketplace Backend
| Feature | Spec |
|---------|------|
| Photo ingestion | Automated metadata extraction + quality scoring (sharpness, exposure, species confidence) |
| Expert review queue | Flagged submissions reviewed within 48h by partnered ecologists |
| Licensing tiers | Editorial (single use), Research (unlimited internal use), Commercial (full rights) |
| Watermarking | Automated watermark on preview; removed on license purchase |
| API integration | RESTful API with webhook notifications for institutional subscribers |

### F5 — Conservation Data API
| Feature | Spec |
|---------|------|
| Data model | GeoJSON sighting records with species, date, confidence, photo reference |
| Access | API key, tiered rate limits |
| Update frequency | Near-real-time (15-minute lag) for premium subscribers; daily batch for standard |
| Privacy | Exact coordinates fuzzed to 1km grid for non-government subscribers |

---

## Architecture

```
┌─────────────────────────────────────────────────────┐
│                  MOBILE APP (React Native)           │
│  ┌──────────┐  ┌──────────┐  ┌──────────────────┐   │
│  │ Capture  │  │ AI Model │  │ Earnings/Wallet  │   │
│  │ Module   │  │ (TFLite) │  │ Dashboard        │   │
│  └────┬─────┘  └────┬─────┘  └────────┬─────────┘   │
│       └─────────────┴─────────────────┘              │
│                      │                               │
│              Offline SQLite cache                    │
└──────────────────────┼──────────────────────────────┘
                       │ HTTPS sync (when connected)
┌──────────────────────▼──────────────────────────────┐
│                 BACKEND (Node.js / AWS)              │
│  ┌───────────────┐  ┌──────────────┐                 │
│  │  Photo Store  │  │  Expert      │                 │
│  │  (S3 + CDN)   │  │  Review Queue│                 │
│  └───────┬───────┘  └──────┬───────┘                 │
│          └────────┬─────────┘                        │
│         ┌─────────▼──────────┐                       │
│         │  Marketplace DB    │                       │
│         │  (PostgreSQL/RDS)  │                       │
│         └─────────┬──────────┘                       │
│                   │                                  │
│    ┌──────────────▼────────────────┐                 │
│    │  Conservation Data API        │                 │
│    │  (REST + WebSocket feeds)     │                 │
│    └───────────────────────────────┘                 │
└─────────────────────────────────────────────────────┘
                       │
          ┌────────────▼──────────────┐
          │  External Integrations    │
          │  iNaturalist · GBIF       │
          │  Mobile Money APIs        │
          │  Ghana Wildlife Division  │
          └───────────────────────────┘
```

---

## Brand Guidelines

### Voice
Warm, grounded, proud. Never patronising. Addresses the community photographer as a professional expert, not a data volunteer.

### Colour Palette
| Token | Hex | Usage |
|-------|-----|-------|
| `--gold` | `#C88415` | Primary CTA, highlights, active states |
| `--gold-light` | `#E8A830` | Hover states, secondary accents |
| `--bg-deep` | `#080503` | App background, night mode |
| `--bg-surface` | `#1A1208` | Cards, modals |
| `--cream` | `#EDE0C4` | Primary text |
| `--cream-muted` | `#9A8870` | Secondary text, captions |
| `--moonlight` | `#A8C4D8` | Status indicators, map overlays |
| `--savanna-green` | `#2E4D1A` | Success states, cooperative fund |

### Typography
| Role | Font | Weight | Size |
|------|------|--------|------|
| Display | Cormorant Garamond | 700 | 48–96px |
| Headline | Syne | 700 | 24–40px |
| Body | Libre Baskerville | 400 | 15–18px |
| UI / Data | Syne | 500 | 12–16px |
| Monospace | JetBrains Mono | 400 | 13–15px |

### Logo
- Icon: Camera aperture blade, geometric, with a lion head silhouette visible in the negative space
- Wordmark: "SavannaLens" in Syne 700, letter-spacing: 0.05em
- Clearspace: minimum 1× the cap-height of the wordmark on all sides

---

## User Flows

### Flow 1 — First Sighting (New User)
```
Open app → Voice-guided onboarding (3 screens) → 
GPS permission → Camera permission → 
Spot wildlife → Tap capture → 
AI shows "Lion — 94% confident" → 
Add behaviour tag (one tap) → Submit → 
"Submitted for review — earn GHS 0–45 if licensed"
```

### Flow 2 — Payout
```
Notification: "Your photo of African elephant was licensed by WWF-UK" →
Tap notification → Photo detail screen →
License value: GHS 38 → "Pay to wallet" button →
MTN Mobile Money confirmation →
Funds in wallet within 2 hours
```

---

## MVP Development Timeline

| Phase | Duration | Deliverables |
|-------|----------|-------------|
| **0 — Foundation** | Weeks 1–4 | Backend API skeleton, auth system, S3 photo storage, PostgreSQL schema |
| **1 — Capture MVP** | Weeks 5–8 | Android app: camera, GPS tagging, offline queue, basic species form |
| **2 — AI Integration** | Weeks 9–12 | TFLite model integration, inference pipeline, expert review queue |
| **3 — Marketplace Alpha** | Weeks 13–16 | Photo listing, watermarking, licensing workflow, mobile money payout |
| **4 — Pilot Deploy** | Weeks 17–20 | 50-user pilot in Larabanga, community onboarding, feedback sprint |
| **5 — Data API Beta** | Weeks 21–24 | Conservation Data API, first institutional subscriber onboarding |
| **6 — Scale Prep** | Weeks 25–26 | Performance hardening, iOS app, Dagbanli localisation complete |

**MVP tech stack:** React Native (mobile) · Node.js/Express (API) · PostgreSQL on AWS RDS · S3 + CloudFront · TensorFlow Lite (on-device AI) · MTN Mobile Money API · Mapbox (mapping)

**Team for MVP:** 1 Lead Mobile Engineer · 1 Backend Engineer · 1 ML Engineer (part-time) · 1 UX Designer · 1 Community Liaison (Damongo-based, bilingual)
