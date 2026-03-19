---
name: travel-plan
description: >
  Generate premium, mobile-responsive HTML travel itinerary dashboards from flight,
  hotel, and schedule information. Trigger on "travel-plan", "여행 일정표",
  "travel itinerary", "trip planner", "여행 대시보드", "여행 일정표 만들어줘",
  "항공편이랑 호텔 정보 줄 테니 일정표 만들어줘", "○○ 여행 일정 HTML로",
  "family trip itinerary", "이거 일정표로 정리해줘", or any variation involving
  travel schedule visualization. Use this skill whenever the user provides travel
  details (flights, hotels, daily plans) and wants a visual itinerary. Also trigger
  when the user pastes flight/hotel booking details and asks to organize them.
  Supports Korean and English. Do NOT use for general web content design,
  article visualization, or non-travel dashboards — those belong to web-content-designer.
---

# Travel Plan

Generate a single-file, production-grade HTML travel itinerary dashboard from
user-provided travel information. The output is a self-contained `.html` file
with fixed sidebar navigation, interactive maps, budget charts, checklists,
and a dark/light theme toggle — optimized for both mobile and desktop viewing.

## When This Skill Triggers

This skill converts **travel booking data and plans** into a **visual itinerary
dashboard**. The user provides raw travel information (flights, hotels, schedules);
the skill produces a polished, interactive HTML page.

**This skill** — NOT `web-content-designer` — when the input is travel logistics
(flights, hotels, daily plans) and the output is a trip itinerary.

## Workflow

```
1. Collect travel info → 2. Identify gaps & confirm → 3. Read references →
4. Select theme → 5. Generate HTML → 6. Validate & deliver
```

---

## Step 1: Collect Travel Information

### Required Inputs (minimum viable)

The user must provide at least these to generate a useful dashboard:

| Field | Example | Notes |
|:---|:---|:---|
| Destination | 나트랑, 발리, 오사카 | City/region + country |
| Dates | 3/29 (일) ~ 4/3 (목) | Departure and return |
| Travelers | 4인 가족 (지현, 래인, 기쁨, 래나) | Count + names/roles |
| Accommodation | Alma Resort 3박, Navy Hotel 1박 | Name, dates, cost |

### Optional Inputs (enrich if provided)

| Field | Effect on Output |
|:---|:---|
| Flight details | Generates Flight Info section with departure/arrival cards |
| Daily schedule | Generates per-day timeline cards with time slots |
| Budget breakdown | Generates budget section with doughnut chart |
| Booking references | Adds reservation numbers, cancellation policies |
| Checklist items | Populates interactive pre-departure checklist |
| Special notes | Passport deadlines, visa requirements, dietary needs |

### Handling Incomplete Input

- **No flights**: Omit the Flights section entirely. Show travel dates in Hero only.
- **No daily schedule**: Generate DAY N placeholder cards with date headers and
  a single "자유 일정 / Free day" entry. User can fill in later.
- **No budget data**: Omit budget section and doughnut chart.
- **No specific places**: Omit Leaflet map section.
- **Minimal input** (destination + dates + travelers only): Generate a skeleton
  dashboard with Hero, Overview KPIs, empty day cards, and a default checklist
  for the destination type (domestic vs. international).

If critical information is ambiguous, ask once before generating. Do not ask
more than 2 clarifying questions — generate with what you have and note gaps.

---

## Step 2: Read References

Before generating HTML, read the following reference files in this order:

1. **Always read**: `references/html-architecture.md`
   — Full page structure, section ordering, required HTML skeleton
2. **Always read**: `references/design-system.md`
   — Theme selection, CSS variable sets, dark/light mode implementation
3. **Consult the example**: `examples/camranh-2026.html`
   — Golden reference output. Scan for structural patterns and component
   implementation, but NEVER copy destination-specific content.

---

## Step 3: Select Theme

Based on the destination and trip character, select a theme from
`references/design-system.md`. The theme determines all CSS variables
for both dark and light modes.

Quick selection heuristic:

| Trip Type | Theme |
|:---|:---|
| Beach / resort / tropical island | Coastal Teal (default) |
| City break (Tokyo, Paris, NYC) | Urban Slate |
| Nature / mountain / hiking | Forest Green |
| Winter / ski / northern | Arctic Blue |
| Cultural / historical / temple | Warm Terracotta |
| Luxury / honeymoon / boutique | Elegant Noir |

If the destination doesn't clearly fit, default to **Coastal Teal**.

---

## Step 4: Generate the HTML

### Mandatory Technical Stack

Every output file MUST include:

```html
<!-- Head -->
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/fonts-archive/Paperlogy/subsets/Paperlogy-dynamic-subset.css" type="text/css"/>
```

- **Font**: Paperlogy as primary, system fonts as fallback
- **Single file**: All HTML, CSS, JS in one `.html` file
- **No external assets** except the CDN libraries above
- **Leaflet map**: Include only if place coordinates are available or can be
  looked up via web search. If coordinates cannot be determined, omit the map.

### Section Assembly

Assemble sections in this order. Omit any section whose data is unavailable.

| Order | Section | Required Data | Ref |
|:---|:---|:---|:---|
| 1 | Sidebar Navigation | All sections present | `html-architecture.md` §Sidebar |
| 2 | Mobile Top Bar | Always | `html-architecture.md` §Mobile |
| 3 | Hero Banner | Destination, dates, travelers | `html-architecture.md` §Hero |
| 4 | Overview (KPI cards + timeline summary) | Dates, accommodation | `html-architecture.md` §Overview |
| 5 | Flight Information | Flight details | `html-architecture.md` §Flights |
| 6 | Accommodation Info | Hotel/resort details | `html-architecture.md` §Hotels |
| 7 | Map | Place coordinates | `html-architecture.md` §Map |
| 8 | Day 1 ~ Day N (timeline cards) | Daily schedule | `html-architecture.md` §DayCards |
| 9 | Budget & Checklist | Cost data, checklist items | `html-architecture.md` §Budget |
| 10 | Useful Info (timezone, currency, weather) | Destination country | `html-architecture.md` §UsefulInfo |
| 11 | Emergency Contacts | Destination country | `html-architecture.md` §Emergency |
| 12 | Footer | Always | `html-architecture.md` §Footer |

### Content Rules

1. **Data fidelity**: Every number, name, date, and booking reference must come
   from the user's input. Never fabricate reservation numbers or prices.
2. **Language**: Match the user's input language. Korean input → Korean dashboard.
   English input → English. Do not mix unless the user does.
3. **Useful Info & Emergency Contacts**: For international trips, use web search
   to fill in timezone difference, currency exchange rate, weather, voltage/plug
   type, embassy numbers, and local emergency numbers. For domestic trips, omit
   or simplify this section.
4. **Map coordinates**: Use web search to find latitude/longitude for hotels,
   airports, and key attractions. If a place cannot be geolocated, exclude it
   from the map but keep it in text sections.
5. **Checklist generation**: If the user provides checklist items, use them.
   Otherwise, generate a default checklist appropriate for the trip type:
   - International: passport validity, visa, SIM/eSIM, travel insurance,
     foreign payment cards, transport app (Grab/Uber), medication, sunscreen
   - Domestic: accommodation confirmation, transport booking, weather check

### Size & Performance Guidelines

- Target: **1,200–2,000 lines** of HTML
- Day timeline items: **4–6 per day** maximum
- Sidebar nav items: **8–12** maximum
- Chart.js instances: **1–2** maximum (budget doughnut, optionally a bar chart)
- Leaflet markers: match the number of key locations (typically 5–10)

### Code Stability Checklist

Before delivering, verify:

- [ ] All JS inside `DOMContentLoaded` listener
- [ ] Every `querySelector` / `getElementById` wrapped in `if (el)` guard
- [ ] Leaflet map initialized only if `#leaflet-map` element exists
- [ ] Chart.js initialized only if canvas element exists
- [ ] localStorage calls wrapped in `try/catch`
- [ ] Mobile menu toggle works (sidebar transform + overlay)
- [ ] ScrollSpy updates active nav link on scroll
- [ ] D-Day counter calculates from departure date
- [ ] Checklist state persists via localStorage
- [ ] Theme toggle persists via localStorage
- [ ] No Tailwind classes conflict with custom CSS variables
- [ ] All `<a>` links to external sites have `target="_blank" rel="noopener"`

---

## Step 5: Validate & Deliver

1. Save to `/mnt/user-data/outputs/` with filename pattern:
   `[Destination]_여행일정_[YYYY].html` (Korean) or
   `[Destination]_Itinerary_[YYYY].html` (English)
2. Use `present_files` to share
3. Provide a brief summary: destination, dates, sections included, theme used

---

## File Naming Examples

```
깜란_여행일정_2026.html
Bali_Itinerary_2026.html
오사카_여행일정_2025.html
Paris_Itinerary_2025.html
```

## Error Handling

- If destination is ambiguous: ask user to clarify (e.g., "나트랑 시내 vs 깜란 공항 근처?")
- If web search for coordinates fails: omit map, note in delivery message
- If budget data is partial: show only confirmed items, label estimates clearly
- If dates span a year boundary: handle correctly in D-Day calculation
