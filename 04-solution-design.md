# 04 — Solution Design

## SavannaLens

*Community-powered wildlife intelligence. Science and income, from the same shot.*

---

## Provider

**Type:** B-Corp social enterprise (registered in Ghana, HQ in Accra, field operations in Damongo)

**Structure:**
- Private company with a legally embedded social mission: minimum 20% of net revenue reinvested into community cooperative funds
- Governed by a board that includes two elected community representatives from the Damongo District
- Seed-funded by a mix of impact investors (Novastar Ventures, TLcom Capital) and conservation grants (National Geographic Society, WWF-UK)

---

## The Offering — Three Interlocking Layers

### Layer 1 — The Mobile App (iOS + Android)
An offline-first wildlife documentation app designed for low-bandwidth, low-literacy, entry-level Android devices.

**Core functions:**
- **Capture:** GPS-tagged photo and video capture with structured observation metadata (species, behaviour, time, habitat type)
- **AI Identify:** On-device AI species identification for ~420 species present in Ghana's savanna biome — works without internet
- **Sync:** Batch upload when connected to any wifi or 3G signal
- **Earn:** Real-time earnings dashboard showing photo licensing revenue, cooperative fund balance, and "Field Ranger" status points

**Design principle:** Functional with one hand, usable in bright sunlight, no reading required for core functions (icon-first UI, voice-guided onboarding in Dagbanli, Twi, and English).

### Layer 2 — The Cooperative Marketplace
A B2B photo licensing platform where verified SavannaLens images are sold to:
- Conservation NGOs (habitat reports, funding applications)
- Academic researchers (species distribution papers)
- Tourism operators (lodge brochures, social media content)
- Media publishers (editorial, educational, documentary)
- Government bodies (IUCN Red List national reporting)

**Revenue split per licensed photo:**
| Recipient | Share |
|-----------|-------|
| Photographer | 70% |
| Village cooperative fund | 20% |
| Platform maintenance | 10% |

### Layer 3 — The Conservation Data API
Anonymised, aggregated, and expert-verified sighting data streamed to institutional subscribers via REST API.

**Subscribers include:**
- Wildlife Division, Ghana Forestry Commission (government data contract)
- African Wildlife Foundation (annual research partnership)
- iNaturalist / GBIF data federation (open science contribution)
- Commercial tourism operators (real-time wildlife location intelligence)

**Data products:**
- Species occurrence heatmaps
- Seasonal movement corridors
- Human-wildlife conflict incident mapping
- Population trend indices (quarterly)

---

## The Field Ranger Program

Top community contributors (top 10% by verified sightings per quarter) receive:
- Upgraded equipment: optical zoom lens attachment, solar power bank, waterproof phone case
- Priority access to paid ecotourism guiding contracts via partner lodges
- "Certified Community Ranger" credential, recognised by Ghana Wildlife Division
- Mentorship from professional wildlife photographers (quarterly workshop, Accra)

---

## Key Partnerships

| Partner | Role |
|---------|------|
| Ghana Wildlife Division | Data offtake agreement, ranger accreditation |
| Mole Motel & Zaina Lodge | Ecotourism referrals, Field Ranger placement |
| iNaturalist | Open science data federation |
| Vodafone Ghana | Zero-rated mobile data for app sync |
| MEST Africa | Startup infrastructure, talent pipeline |
| National Geographic Society | Founding grant, editorial distribution channel |

---

## Why This Works

1. **Supply side:** Community members have the access, presence, and knowledge — they lack only the tools and the market connection
2. **Demand side:** Conservation data buyers are actively seeking higher-resolution, community-sourced datasets — they lack only the supply infrastructure
3. **Economic alignment:** The micro-income model creates durable participation incentives that no volunteer program can match
4. **Network effects:** Every new community contributor adds spatial coverage that increases the platform's data value to institutional buyers
