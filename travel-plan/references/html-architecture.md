# HTML Architecture Reference

Complete structural blueprint for the travel itinerary dashboard.
Each section includes the HTML pattern, required CSS, and behavioral JS.

---

## Page Shell

The page uses a sidebar + main content layout identical to `web-content-designer`
Layout 1 (Dashboard), but with travel-specific sections and a dual-theme system.

```
┌─────────────────────────────────────────────────────────┐
│ ┌─ Mobile Top Bar (md:hidden) ──────────────────────┐   │
│ └───────────────────────────────────────────────────┘   │
│ ┌──────────┐ ┌────────────────────────────────────┐     │
│ │ SIDEBAR  │ │  Hero Banner (gradient)            │     │
│ │ (fixed)  │ ├────────────────────────────────────┤     │
│ │          │ │  Overview (KPIs + timeline)         │     │
│ │ ✈ 개요   │ ├────────────────────────────────────┤     │
│ │ 🛫 항공  │ │  Flights                           │     │
│ │ 🏨 숙소  │ ├────────────────────────────────────┤     │
│ │ 🗺 지도  │ │  Hotels                            │     │
│ │ 1 DAY 1  │ ├────────────────────────────────────┤     │
│ │ 2 DAY 2  │ │  Map (Leaflet)                     │     │
│ │ ...      │ ├────────────────────────────────────┤     │
│ │ 💰 예산  │ │  Day 1 .. Day N (timelines)        │     │
│ │          │ ├────────────────────────────────────┤     │
│ │          │ │  Budget & Checklist                 │     │
│ │          │ ├────────────────────────────────────┤     │
│ │          │ │  Useful Info + Emergency            │     │
│ │          │ ├────────────────────────────────────┤     │
│ │          │ │  Footer                             │     │
│ └──────────┘ └────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────┘
```

### Root HTML

```html
<body class="antialiased">
  <div class="relative min-h-screen flex">
    <!-- Mobile Top Bar -->
    <!-- Sidebar -->
    <!-- Sidebar Overlay (mobile) -->
    <!-- Main Content -->
  </div>
</body>
```

---

## §Sidebar

Fixed left sidebar with gradient background from theme's `--sidebar-start` to
`--sidebar-end`. Contains trip title, date range, nav links, and theme toggle.

### Structure

```html
<aside id="sidebar" class="sidebar-shell w-64 fixed inset-y-0 left-0
    transform -translate-x-full md:translate-x-0 transition-transform
    duration-300 ease-in-out z-40 overflow-y-auto pt-20 md:pt-0 pb-8
    flex flex-col h-screen">

  <!-- Header -->
  <div class="p-6 pb-4">
    <div class="sidebar-overline"><!-- trip type tag, e.g. "Coastal Escape" --></div>
    <span class="text-xs font-bold tracking-wider uppercase">Family Trip 2026</span>
    <h2 class="text-xl font-bold text-white leading-tight mt-1">
      [Destination]<br>가족 여행 일정
    </h2>
    <p class="text-xs mt-2">[date range]</p>
    <!-- Theme toggle button (desktop) -->
  </div>

  <!-- Navigation -->
  <nav id="main-nav" class="flex flex-col space-y-1 px-3">
    <a href="#overview" class="nav-link ..."><span class="section-number">✈</span>여행 개요</a>
    <a href="#flights" class="nav-link ..."><span class="section-number">🛫</span>항공편 정보</a>
    <a href="#hotels" class="nav-link ..."><span class="section-number">🏨</span>숙소 정보</a>
    <a href="#map" class="nav-link ..."><span class="section-number">🗺</span>지도</a>
    <!-- Day links: section-number shows "1", "2", etc. -->
    <a href="#day1" class="nav-link ..."><span class="section-number">1</span>DAY 1 · [date]</a>
    <!-- ... -->
    <a href="#budget" class="nav-link ..."><span class="section-number">💰</span>예산 · 체크리스트</a>
  </nav>

  <!-- Footer meta -->
  <div class="mt-auto pt-6 px-6 text-xs">
    <p>🌊 [Destination], [Country]</p>
    <p class="mt-1">[N]박 [N+1]일 [trip type]</p>
  </div>
</aside>
```

### Nav link styling

```css
.nav-link {
    color: rgba(236, 242, 248, 0.72);
    border: 1px solid transparent;
}
.nav-link:hover {
    color: #ffffff;
    background: rgba(255,255,255,0.08);
    transform: translateX(4px);
}
.nav-link.active {
    color: #ffffff;
    background: rgba(255,255,255,0.14);
    border-color: rgba(255,255,255,0.12);
}
```

### ScrollSpy JS

```javascript
var sections = document.querySelectorAll('.content-section');
var navLinks = document.querySelectorAll('#main-nav a');
function updateNav() {
    var current = '';
    sections.forEach(function(s) {
        if (s.getBoundingClientRect().top <= 120) current = s.id;
    });
    navLinks.forEach(function(l) {
        l.classList.toggle('active', l.getAttribute('href') === '#' + current);
    });
}
window.addEventListener('scroll', updateNav);
updateNav();
```

---

## §Mobile

Mobile top bar visible only below `md` breakpoint. Contains trip title,
theme toggle, and hamburger menu button.

```html
<div class="mobile-topbar md:hidden fixed top-0 left-0 right-0
    flex justify-between items-center p-4 z-50">
  <div>
    <div class="text-[11px] tracking-[0.22em] uppercase font-bold"
         style="color:var(--text-muted)">Family Trip 2026</div>
    <h1 class="text-lg font-bold" style="color:var(--text-strong)">
      [Destination] 일정</h1>
  </div>
  <div class="flex items-center gap-2">
    <button data-theme-toggle class="theme-toggle">...</button>
    <button id="mobile-menu-button" class="menu-button">
      <!-- hamburger SVG -->
    </button>
  </div>
</div>

<!-- Overlay -->
<div id="sidebar-overlay" class="fixed inset-0 bg-black/50 z-30 hidden md:hidden"></div>
```

### Mobile menu JS

```javascript
var sidebar = document.getElementById('sidebar');
var menuBtn = document.getElementById('mobile-menu-button');
var overlay = document.getElementById('sidebar-overlay');
menuBtn.addEventListener('click', function() {
    sidebar.classList.toggle('-translate-x-full');
    overlay.classList.toggle('hidden');
});
overlay.addEventListener('click', function() {
    sidebar.classList.add('-translate-x-full');
    overlay.classList.add('hidden');
});
// Close on nav link click (mobile)
document.querySelectorAll('#main-nav a').forEach(function(link) {
    link.addEventListener('click', function() {
        if (window.innerWidth < 768) {
            sidebar.classList.add('-translate-x-full');
            overlay.classList.add('hidden');
        }
    });
});
```

---

## §Hero

Full-width gradient banner at the top of main content. Contains trip title,
key stats, D-Day counter, and a "Travel Brief" side panel.

### Layout

```
┌──────────────────────────────────────────┐
│  hero-layout (CSS Grid)                  │
│  ┌────────────────┐ ┌─────────────────┐  │
│  │ Left Column    │ │ Right Panel     │  │
│  │ • Kicker       │ │ (Travel Brief)  │  │
│  │ • Pills        │ │ • Stay          │  │
│  │ • Title (h2)   │ │ • Move          │  │
│  │ • Description  │ │ • Focus         │  │
│  │ • Stat badges  │ │                 │  │
│  └────────────────┘ └─────────────────┘  │
└──────────────────────────────────────────┘
```

### Key components

**D-Day Counter**: Calculate days until departure date. Show `D-N` before trip,
`D-DAY!` on departure, `D+N` after.

```javascript
var tripDate = new Date('[DEPARTURE_DATE]T00:00:00+09:00');
var today = new Date(); today.setHours(0,0,0,0);
var diff = Math.ceil((tripDate - today) / 86400000);
var el = document.getElementById('dday-count');
if (diff > 0) el.textContent = 'D-' + diff;
else if (diff === 0) el.textContent = 'D-DAY!';
else el.textContent = 'D+' + Math.abs(diff);
```

**Stat badges**: 4 badges showing trip duration, number of accommodations,
total budget, and D-Day. Use `hero-stat` class with glassmorphism styling.

**Travel Brief panel**: 3 rows (Stay / Move / Focus) summarizing the trip's
accommodation strategy, transportation approach, and core theme.

### Responsive

On screens below `lg`, the hero grid collapses to single column.
On mobile, the Travel Brief panel stacks below the title area.

---

## §Overview

Section with 4 KPI cards (grid) + a timeline summary card showing all days
at a glance.

### KPI Cards

4-column grid (`sm:grid-cols-2 lg:grid-cols-4`). Each card:

```html
<div class="kpi-card border-t-4" style="border-color: var(--color-primary)">
  <div class="text-2xl mb-1">[emoji]</div>
  <div class="text-sm font-bold" style="color:var(--text-muted)">[label]</div>
  <div class="text-xl font-black" style="color:var(--text-strong)">[value]</div>
  <div class="text-xs" style="color:var(--text-muted)">[detail]</div>
</div>
```

Typical KPIs: airline, main accommodation, traveler count, delivery/utility app.

### Timeline Summary

A vertical list of day summaries, each with date, day number, and one-line
description. Color-coded by day type (travel day, resort day, activity day,
departure day).

---

## §Flights

Two side-by-side cards: outbound and return flight. Each shows:
- Departure/arrival time (large font)
- Airport codes
- Flight duration
- Animated flight path line (CSS)
- Date and notes

```html
<div class="flight-card">
  <div class="flex items-center gap-2 mb-4">
    <span class="info-badge bg-cyan-100 text-cyan-800">🛫 가는편</span>
    <span class="info-badge bg-gray-100 text-gray-700">[airline]</span>
  </div>
  <div class="flex items-center justify-between mb-4">
    <div class="text-center">
      <div class="text-2xl font-black">[dep_time]</div>
      <div class="text-sm font-bold" style="color:var(--text-muted)">[dep_airport]</div>
    </div>
    <div class="flex-1 px-4">
      <div class="flex items-center">
        <div class="h-0.5 flex-1" style="background:var(--color-primary)"></div>
        <div class="mx-2 text-lg">✈️</div>
        <div class="h-0.5 flex-1" style="background:var(--color-primary)"></div>
      </div>
      <div class="text-xs text-center" style="color:var(--text-muted)">약 [duration]</div>
    </div>
    <div class="text-center">
      <div class="text-2xl font-black">[arr_time]</div>
      <div class="text-sm font-bold" style="color:var(--text-muted)">[arr_airport]</div>
    </div>
  </div>
</div>
```

Below the flight cards: booking reference panel and cancellation policy table.

---

## §Hotels

Vertical stack of hotel cards. The **main accommodation** gets a highlighted
card with colored border and "⭐ 메인 숙소" badge. Other hotels use standard
`hotel-card` styling.

### Main hotel card

```html
<div class="relative overflow-hidden rounded-2xl border-2 shadow-lg"
     style="border-color: var(--color-primary)">
  <div class="absolute top-0 right-0 text-white text-xs font-black px-4 py-1 rounded-bl-xl"
       style="background: var(--color-primary)">⭐ 메인 숙소</div>
  <div class="p-6">
    <!-- badges, title, description, amenity icons, pricing, booking link -->
  </div>
</div>
```

### Secondary hotel card

Standard `hotel-card` class with border, shadow, and hover lift effect.

### Amenity icon grid

For the main hotel, show 3 feature icons in a `sm:grid-cols-3` grid:

```html
<div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
  <div class="rounded-xl p-4 text-center border" style="border-color:var(--border)">
    <div class="text-2xl mb-1">[emoji]</div>
    <div class="text-xs font-bold">[feature name]</div>
  </div>
</div>
```

---

## §Map

Leaflet.js interactive map with emoji markers for key locations.

### Container

```html
<div class="map-container mb-6">
  <div id="leaflet-map" style="height:450px; width:100%;"></div>
</div>
```

### Marker initialization

```javascript
var places = {
    airport: { lat: X, lng: Y, emoji: '🛫', name: '...', desc: '...' },
    hotel1:  { lat: X, lng: Y, emoji: '🏝️', name: '...', desc: '...' },
    // ...
};

var map = L.map('leaflet-map', { scrollWheelZoom: false }).setView([centerLat, centerLng], 11);
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap contributors'
}).addTo(map);

Object.keys(places).forEach(function(key) {
    var p = places[key];
    var icon = L.divIcon({
        html: '<div style="font-size:1.5rem;text-align:center">' + p.emoji + '</div>',
        className: '', iconSize: [30,30], iconAnchor: [15,15]
    });
    L.marker([p.lat, p.lng], { icon: icon }).addTo(map)
      .bindPopup('<h4>' + p.emoji + ' ' + p.name + '</h4><p>' + p.desc + '</p>');
});

// Fit all markers
map.fitBounds(Object.values(places).map(function(p) { return [p.lat, p.lng]; }));
```

### Place cards below map

Clickable cards that pan the map to the selected location:

```javascript
card.addEventListener('click', function() {
    var p = places[key];
    map.setView([p.lat, p.lng], 15, { animate: true });
    markers[key].openPopup();
    document.getElementById('leaflet-map').scrollIntoView({ behavior: 'smooth', block: 'center' });
});
```

### Map height responsive

```css
#leaflet-map { height: 450px; }
@media (max-width: 767px) { #leaflet-map { height: 350px; } }
```

---

## §DayCards

Each day is a `day-card` with a colored header and a timeline body.

### Day card structure

```html
<section id="dayN" class="content-section mb-12">
  <div class="day-card">
    <!-- Header -->
    <div class="day-header">
      <div class="flex items-center gap-3">
        <div class="bg-white/20 rounded-xl px-3 py-1 backdrop-blur-sm">
          <div class="text-2xl font-black">DAY N</div>
        </div>
        <div>
          <div class="text-xl font-black">[date with day of week]</div>
          <div class="text-sm" style="color:var(--color-primary-light)">[day summary]</div>
        </div>
      </div>
    </div>

    <!-- Timeline body -->
    <div class="p-6">
      <div class="timeline-container">
        <!-- timeline items -->
      </div>
    </div>
  </div>
</section>
```

### Timeline item

```html
<div class="timeline-item">
  <div class="timeline-dot"></div> <!-- or .timeline-dot.accent for highlights -->
  <div class="rounded-lg p-4 border" style="background:var(--tag-primary-bg)">
    <div class="text-xs font-bold mb-1" style="color:var(--tag-primary-text)">
      [time] — [location]
    </div>
    <h4 class="font-bold" style="color:var(--text-strong)">[activity title]</h4>
    <p class="text-sm" style="color:var(--text-body)">[description]</p>
    <!-- Optional activity tags -->
    <div class="flex flex-wrap gap-2 mt-2">
      <span class="activity-tag" style="background:var(--tag-primary-bg);color:var(--tag-primary-text)">
        [emoji] [tag]
      </span>
    </div>
  </div>
</div>
```

### Timeline CSS

```css
.timeline-container {
    position: relative;
    padding-left: 2.5rem;
}
.timeline-container::before {
    content: "";
    position: absolute;
    left: 0.75rem; top: 0; bottom: 0;
    width: 3px;
    background: linear-gradient(to bottom,
        rgba(125,187,197,0.16), var(--color-primary), rgba(228,164,111,0.2));
    border-radius: 2px;
}
.timeline-dot {
    position: absolute;
    left: -2.35rem; top: 0.5rem;
    width: 1.1rem; height: 1.1rem;
    border-radius: 50%;
    background: var(--color-primary);
    border: 3px solid var(--surface-strong);
    box-shadow: 0 0 0 2px rgba(125,187,197,0.22);
}
.timeline-dot.accent {
    background: var(--color-accent);
}
```

### Timeline item color coding

Use different background tints for different activity types:

| Activity Type | Background Variable | Dot Class |
|:---|:---|:---|
| Transport / arrival | `--tag-primary-bg` | `.timeline-dot` |
| Highlight / key event | `--tag-accent-bg` | `.timeline-dot.accent` |
| Meal | `--tag-warm-bg` | `.timeline-dot` |
| Accommodation | `--tag-success-bg` | `.timeline-dot` |
| Departure / alert | `--tag-danger-bg` | `.timeline-dot.accent` |
| Free time / rest | neutral gray | `.timeline-dot` |

### Day 4 pattern: Option cards

When a day has alternative plans (e.g., Theme Park vs City Tour), show
option cards in a 2-column grid before the timeline:

```html
<div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
  <div class="rounded-xl p-5 border-2" style="border-color:var(--color-primary)">
    <span class="text-white text-xs font-black px-2 py-0.5 rounded"
          style="background:var(--color-primary)">옵션 A</span>
    <span class="font-bold">[Option A name]</span>
    <!-- bullet list of activities -->
  </div>
  <div class="rounded-xl p-5 border-2" style="border-color:var(--color-accent)">
    <span class="text-white text-xs font-black px-2 py-0.5 rounded"
          style="background:var(--color-accent)">옵션 B</span>
    <!-- ... -->
  </div>
</div>
```

---

## §Budget

Two-column layout: left = budget bars + doughnut chart, right = checklist.

### Budget bars

For each cost item, show a label, amount, and proportional bar:

```html
<div>
  <div class="flex justify-between text-sm mb-1">
    <span class="font-bold">[emoji] [item]</span>
    <span class="font-bold" style="color:var(--tag-primary-text)">₩[amount]</span>
  </div>
  <div class="h-2.5 rounded-full overflow-hidden" style="background:var(--surface-contrast)">
    <div class="expense-bar rounded-full" style="width:[pct]%;background:var(--color-primary)"></div>
  </div>
  <div class="text-xs mt-1" style="color:var(--text-muted)">[note]</div>
</div>
```

### Doughnut chart

```javascript
var chart = new Chart(canvas, {
    type: 'doughnut',
    data: {
        labels: [...],
        datasets: [{ data: [...], backgroundColor: [
            getThemeValue('--chart-primary'),
            getThemeValue('--chart-success'),
            getThemeValue('--chart-accent'),
            getThemeValue('--chart-muted')
        ], borderWidth: 0 }]
    },
    options: {
        responsive: true, maintainAspectRatio: false, cutout: '60%',
        plugins: {
            legend: { position: 'bottom', labels: { color: getThemeValue('--chart-legend') } },
            tooltip: { backgroundColor: getThemeValue('--tooltip-bg') }
        }
    }
});
```

**Important**: Chart colors must update when theme toggles. Store chart instance
globally and call `chart.update()` inside the theme toggle handler.

### Checklist

Interactive checklist with localStorage persistence:

```html
<label class="checklist-item">
  <input type="checkbox" class="checklist-cb" data-id="[unique-id]">
  <span class="checklist-label text-sm">[item text]</span>
</label>
```

- Progress bar at top showing `[done]/[total]`
- `checked` class adds strikethrough to label
- State saved to localStorage under key `[destination]-trip-checklist-[year]`

---

## §UsefulInfo

4-column grid of info cards: timezone, currency, weather, voltage/plug type.

```html
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
  <div class="rounded-xl p-4 text-center">
    <div class="text-2xl mb-2">[emoji]</div>
    <div class="text-sm font-bold">[label]</div>
    <div class="text-xs mt-1">[value]</div>
  </div>
</div>
```

Use web search to fill in accurate values for the destination.

---

## §Emergency

Two-row, two-column grid of emergency contact cards:
1. Local emergency numbers (police, ambulance, fire)
2. Korean embassy/consulate (for Korean travelers)
3. Korean emergency hotline abroad (영사콜센터)
4. Local hospital recommendations

Phone numbers should be clickable `<a href="tel:...">`.

---

## §Footer

Simple centered footer with destination name and traveler names.

```html
<div class="text-center py-8 text-sm" style="color:var(--text-muted);border-top:1px solid var(--border)">
  <p>[Destination] [Trip Type] [Year]</p>
  <p class="mt-1">[traveler names]</p>
</div>
```
