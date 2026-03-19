# Design System Reference

Theme selection, CSS variable architecture, and dark/light mode implementation.

---

## Theme Selection

Select ONE theme based on the destination and trip character.
Each theme provides a complete set of CSS variables for both dark and light modes.

### Theme Matrix

| # | Trip Type / Destination | Theme Name | Mood Keywords |
|:--|:--|:--|:--|
| 1 | Beach, resort, tropical island, Southeast Asia | **Coastal Teal** | ocean, sand, palm, sunset |
| 2 | Urban city break (Tokyo, Seoul, NYC, London) | **Urban Slate** | concrete, neon, steel, night |
| 3 | Nature, mountain, hiking, national park | **Forest Green** | canopy, moss, earth, mist |
| 4 | Winter, ski, northern Europe, Iceland | **Arctic Blue** | ice, snow, aurora, fjord |
| 5 | Cultural, historical, temple, Mediterranean | **Warm Terracotta** | clay, spice, mosaic, amber |
| 6 | Luxury, honeymoon, boutique, Maldives | **Elegant Noir** | gold, silk, marble, velvet |

### Selection Rules

1. Match the **primary destination character**, not the activities.
   - "Bali surf trip" → Coastal Teal (beach destination)
   - "Tokyo food tour" → Urban Slate (city destination)
   - "Swiss Alps hiking" → Forest Green (nature destination)
2. If multiple themes could apply, choose the one matching the **accommodation type**.
   - Beach resort → Coastal Teal, even if the city nearby is urban.
3. If the user specifies a color preference, override the automatic selection and
   adapt the closest theme's variable structure with the requested colors.
4. Default: **Coastal Teal** when no clear match.

---

## CSS Variable Architecture

Every theme defines variables in two scopes: `:root` (dark mode default) and
`html[data-theme="light"]`. The theme toggle switches the `data-theme` attribute
on `<html>`.

### Variable Categories

```css
:root {
    /* === Identity Colors === */
    --color-primary: ...;        /* Main accent — buttons, links, active states */
    --color-primary-dark: ...;   /* Darker variant — hover states */
    --color-primary-light: ...;  /* Lighter variant — subtle highlights */
    --color-accent: ...;         /* Secondary accent — highlights, alerts */
    --color-accent-light: ...;   /* Accent with low opacity */

    /* === Destination Colors === */
    /* 2-3 colors that evoke the destination. Named semantically. */
    --color-sand: ...;           /* Example: Coastal Teal uses sand */
    --color-sea-light: ...;
    --color-palm: ...;
    --color-sunset: ...;

    /* === Surfaces === */
    --bg: ...;                   /* Page background */
    --bg-secondary: ...;        /* Slightly different bg for gradient */
    --surface: ...;              /* Card background (with transparency) */
    --surface-strong: ...;       /* Opaque card background */
    --surface-soft: ...;         /* Softer card background */
    --surface-contrast: ...;     /* Very subtle contrast layer */

    /* === Text === */
    --text-strong: ...;          /* Headings, bold text */
    --text-body: ...;            /* Body copy */
    --text-muted: ...;           /* Secondary, labels */
    --text-inverse: ...;         /* Text on dark backgrounds */

    /* === Borders === */
    --border: ...;               /* Default border */
    --border-strong: ...;        /* Hover/focus border */

    /* === Hero Section === */
    --hero-start: ...;           /* Hero gradient start */
    --hero-mid: ...;             /* Hero gradient midpoint */
    --hero-end: ...;             /* Hero gradient end */

    /* === Sidebar === */
    --sidebar-start: ...;        /* Sidebar gradient start */
    --sidebar-end: ...;          /* Sidebar gradient end */

    /* === Glow Effects === */
    --glow-primary: ...;         /* Subtle background glow (primary) */
    --glow-accent: ...;          /* Subtle background glow (accent) */

    /* === Shadows === */
    --shadow-soft: ...;          /* Large soft shadow */
    --shadow-card: ...;          /* Card shadow */

    /* === Tags === */
    --tag-primary-bg: ...;       --tag-primary-text: ...;
    --tag-success-bg: ...;       --tag-success-text: ...;
    --tag-accent-bg: ...;        --tag-accent-text: ...;
    --tag-danger-bg: ...;        --tag-danger-text: ...;
    --tag-warm-bg: ...;          --tag-warm-text: ...;

    /* === Charts === */
    --chart-primary: ...;
    --chart-success: ...;
    --chart-accent: ...;
    --chart-muted: ...;
    --chart-legend: ...;
    --tooltip-bg: ...;
}
```

---

## Theme 1: Coastal Teal (Default)

For beach, resort, tropical destinations.

### Dark Mode (default)

```css
:root {
    --color-primary: #7dbbc5;
    --color-primary-dark: #4f93a0;
    --color-primary-light: #b8d9df;
    --color-accent: #e4a46f;
    --color-accent-light: rgba(228, 164, 111, 0.16);
    --color-sand: #f2d4b1;
    --color-sea-light: rgba(125, 187, 197, 0.14);
    --color-palm: #78b398;
    --color-sunset: #da8877;
    --bg: #091017;
    --bg-secondary: #0d1720;
    --surface: rgba(16, 24, 34, 0.82);
    --surface-strong: #111b26;
    --surface-soft: rgba(19, 31, 43, 0.76);
    --surface-contrast: rgba(255, 255, 255, 0.05);
    --text-strong: #ecf2f8;
    --text-body: #c5d1de;
    --text-muted: #8194a8;
    --text-inverse: #f8fafc;
    --border: rgba(148, 163, 184, 0.16);
    --border-strong: rgba(148, 163, 184, 0.24);
    --hero-start: #071018;
    --hero-mid: #132634;
    --hero-end: #23434f;
    --sidebar-start: #081018;
    --sidebar-end: #162736;
    --glow-primary: rgba(125, 187, 197, 0.22);
    --glow-accent: rgba(228, 164, 111, 0.18);
    --shadow-soft: 0 18px 60px -32px rgba(0,0,0,0.8);
    --shadow-card: 0 16px 36px -24px rgba(0,0,0,0.9);
    --tag-primary-bg: rgba(125,187,197,0.16);   --tag-primary-text: #a7d6dd;
    --tag-success-bg: rgba(120,179,152,0.16);    --tag-success-text: #98d0b3;
    --tag-accent-bg: rgba(228,164,111,0.16);     --tag-accent-text: #f1b98a;
    --tag-danger-bg: rgba(218,136,119,0.14);     --tag-danger-text: #f0a08e;
    --tag-warm-bg: rgba(242,212,177,0.14);       --tag-warm-text: #f3c998;
    --chart-primary: #7dbbc5;
    --chart-success: #78b398;
    --chart-accent: #e4a46f;
    --chart-muted: #7b8798;
    --chart-legend: #b8c6d6;
    --tooltip-bg: rgba(10, 18, 27, 0.94);
}
```

### Light Mode

```css
html[data-theme="light"] {
    --color-primary: #4f8f98;
    --color-primary-dark: #244a52;
    --color-primary-light: #a9d0d6;
    --color-accent: #c78656;
    --color-accent-light: rgba(199, 134, 86, 0.16);
    --color-sand: #e7c9a6;
    --color-sea-light: rgba(79, 143, 152, 0.12);
    --color-palm: #668c76;
    --color-sunset: #bf7766;
    --bg: #e9e3d9;
    --bg-secondary: #f5f1ea;
    --surface: rgba(255, 252, 247, 0.74);
    --surface-strong: #fcfaf6;
    --surface-soft: rgba(250, 246, 240, 0.8);
    --surface-contrast: rgba(15, 23, 42, 0.04);
    --text-strong: #132131;
    --text-body: #384a5b;
    --text-muted: #6d7e90;
    --text-inverse: #f8fafc;
    --border: rgba(29, 48, 63, 0.12);
    --border-strong: rgba(29, 48, 63, 0.2);
    --hero-start: #0f1822;
    --hero-mid: #253845;
    --hero-end: #426575;
    --sidebar-start: #101821;
    --sidebar-end: #223340;
    --glow-primary: rgba(79, 143, 152, 0.16);
    --glow-accent: rgba(199, 134, 86, 0.14);
    --shadow-soft: 0 18px 60px -34px rgba(15,23,42,0.42);
    --shadow-card: 0 18px 38px -28px rgba(15,23,42,0.28);
    --tag-primary-bg: rgba(79,143,152,0.12);     --tag-primary-text: #37656d;
    --tag-success-bg: rgba(102,140,118,0.14);     --tag-success-text: #53725f;
    --tag-accent-bg: rgba(199,134,86,0.14);       --tag-accent-text: #9b653c;
    --tag-danger-bg: rgba(191,119,102,0.14);      --tag-danger-text: #975949;
    --tag-warm-bg: rgba(231,201,166,0.18);        --tag-warm-text: #927252;
    --chart-primary: #4f8f98;
    --chart-success: #668c76;
    --chart-accent: #c78656;
    --chart-muted: #8b97a6;
    --chart-legend: #536579;
    --tooltip-bg: rgba(252, 250, 246, 0.96);
}
```

---

## Theme 2: Urban Slate

For city breaks — Tokyo, Seoul, NYC, London, Paris.

### Dark Mode

```css
:root {
    --color-primary: #a8b4c8;
    --color-primary-dark: #7a8ba6;
    --color-primary-light: #c8d2e0;
    --color-accent: #e8976f;
    --color-accent-light: rgba(232, 151, 111, 0.16);
    --color-neon: #c4b5fd;
    --color-concrete: rgba(168, 180, 200, 0.14);
    --color-steel: #94a3b8;
    --color-night: #64748b;
    --bg: #0c0e14;
    --bg-secondary: #12151e;
    --surface: rgba(18, 21, 30, 0.85);
    --surface-strong: #161a25;
    --surface-soft: rgba(22, 26, 37, 0.78);
    --surface-contrast: rgba(255, 255, 255, 0.04);
    --text-strong: #e8ecf2;
    --text-body: #b8c4d4;
    --text-muted: #7889a0;
    --text-inverse: #f8fafc;
    --border: rgba(148, 163, 184, 0.14);
    --border-strong: rgba(148, 163, 184, 0.22);
    --hero-start: #0a0c12;
    --hero-mid: #1a1e2e;
    --hero-end: #2a3348;
    --sidebar-start: #0c0e16;
    --sidebar-end: #1a2030;
    --glow-primary: rgba(168, 180, 200, 0.18);
    --glow-accent: rgba(232, 151, 111, 0.14);
    --shadow-soft: 0 18px 60px -32px rgba(0,0,0,0.85);
    --shadow-card: 0 16px 36px -24px rgba(0,0,0,0.9);
    --tag-primary-bg: rgba(168,180,200,0.14);    --tag-primary-text: #b8c8dc;
    --tag-success-bg: rgba(134,172,142,0.14);    --tag-success-text: #9cc8a6;
    --tag-accent-bg: rgba(232,151,111,0.14);     --tag-accent-text: #f0b48e;
    --tag-danger-bg: rgba(220,130,120,0.14);     --tag-danger-text: #f0a098;
    --tag-warm-bg: rgba(232,200,160,0.14);       --tag-warm-text: #e8c898;
    --chart-primary: #a8b4c8;
    --chart-success: #86ac8e;
    --chart-accent: #e8976f;
    --chart-muted: #6b7a8e;
    --chart-legend: #a0b0c4;
    --tooltip-bg: rgba(12, 14, 20, 0.95);
}
```

### Light Mode

```css
html[data-theme="light"] {
    --color-primary: #5a6a82;
    --color-primary-dark: #3a4a62;
    --color-primary-light: #94a4b8;
    --color-accent: #c47a52;
    --color-accent-light: rgba(196, 122, 82, 0.14);
    --bg: #e8e6e2;
    --bg-secondary: #f2f0ec;
    --surface: rgba(255, 253, 250, 0.78);
    --surface-strong: #faf8f5;
    --surface-soft: rgba(248, 246, 242, 0.82);
    --surface-contrast: rgba(15, 23, 42, 0.04);
    --text-strong: #1a2030;
    --text-body: #3a4a5e;
    --text-muted: #6a7a8e;
    --text-inverse: #f8fafc;
    --border: rgba(30, 42, 58, 0.12);
    --border-strong: rgba(30, 42, 58, 0.2);
    --hero-start: #141822;
    --hero-mid: #2a3245;
    --hero-end: #485068;
    --sidebar-start: #161a24;
    --sidebar-end: #2a3244;
    --glow-primary: rgba(90, 106, 130, 0.14);
    --glow-accent: rgba(196, 122, 82, 0.12);
    --shadow-soft: 0 18px 60px -34px rgba(15,23,42,0.38);
    --shadow-card: 0 18px 38px -28px rgba(15,23,42,0.24);
    --tag-primary-bg: rgba(90,106,130,0.12);     --tag-primary-text: #4a5a72;
    --tag-success-bg: rgba(90,130,100,0.14);     --tag-success-text: #4a6e54;
    --tag-accent-bg: rgba(196,122,82,0.14);      --tag-accent-text: #96603a;
    --tag-danger-bg: rgba(190,110,100,0.14);     --tag-danger-text: #904a42;
    --tag-warm-bg: rgba(210,180,140,0.16);       --tag-warm-text: #8a6e4a;
    --chart-primary: #5a6a82;
    --chart-success: #5a8264;
    --chart-accent: #c47a52;
    --chart-muted: #8494a6;
    --chart-legend: #4a5a72;
    --tooltip-bg: rgba(250, 248, 245, 0.96);
}
```

---

## Theme 3: Forest Green

For nature, mountain, hiking, national park trips.

### Dark Mode

```css
:root {
    --color-primary: #7aab82;
    --color-primary-dark: #5a8a62;
    --color-primary-light: #a8cead;
    --color-accent: #d4a05a;
    --color-accent-light: rgba(212, 160, 90, 0.16);
    --color-moss: #6b9a6e;
    --color-earth: rgba(140, 120, 90, 0.14);
    --color-canopy: #5a8866;
    --color-mist: #8aaa98;
    --bg: #0a100c;
    --bg-secondary: #0e1610;
    --surface: rgba(14, 22, 16, 0.84);
    --surface-strong: #141e16;
    --surface-soft: rgba(18, 28, 20, 0.78);
    --surface-contrast: rgba(255, 255, 255, 0.04);
    --text-strong: #e4ece6;
    --text-body: #b4c8ba;
    --text-muted: #7a9484;
    --text-inverse: #f8faf8;
    --border: rgba(120, 160, 130, 0.16);
    --border-strong: rgba(120, 160, 130, 0.24);
    --hero-start: #081008;
    --hero-mid: #162818;
    --hero-end: #2a4a2e;
    --sidebar-start: #0a120a;
    --sidebar-end: #1a2e1c;
    --glow-primary: rgba(122, 171, 130, 0.20);
    --glow-accent: rgba(212, 160, 90, 0.16);
    --shadow-soft: 0 18px 60px -32px rgba(0,0,0,0.82);
    --shadow-card: 0 16px 36px -24px rgba(0,0,0,0.88);
    --tag-primary-bg: rgba(122,171,130,0.16);    --tag-primary-text: #a0d0a8;
    --tag-success-bg: rgba(108,160,110,0.16);    --tag-success-text: #90c498;
    --tag-accent-bg: rgba(212,160,90,0.14);      --tag-accent-text: #e4c08a;
    --tag-danger-bg: rgba(200,120,100,0.14);     --tag-danger-text: #e0a090;
    --tag-warm-bg: rgba(200,180,140,0.14);       --tag-warm-text: #d8c498;
    --chart-primary: #7aab82;
    --chart-success: #6b9a6e;
    --chart-accent: #d4a05a;
    --chart-muted: #6a7e70;
    --chart-legend: #a4b8aa;
    --tooltip-bg: rgba(10, 16, 12, 0.94);
}
```

### Light Mode

```css
html[data-theme="light"] {
    --color-primary: #4a7a52;
    --color-primary-dark: #2e5a36;
    --color-primary-light: #8ab892;
    --color-accent: #b8863e;
    --color-accent-light: rgba(184, 134, 62, 0.14);
    --bg: #e6e8e0;
    --bg-secondary: #f0f2ea;
    --surface: rgba(252, 254, 250, 0.78);
    --surface-strong: #f8faf6;
    --surface-soft: rgba(246, 250, 244, 0.82);
    --surface-contrast: rgba(10, 30, 15, 0.04);
    --text-strong: #1a2a1c;
    --text-body: #3a4e3e;
    --text-muted: #6a7e6e;
    --text-inverse: #f8faf8;
    --border: rgba(30, 50, 35, 0.12);
    --border-strong: rgba(30, 50, 35, 0.2);
    --hero-start: #10200e;
    --hero-mid: #264428;
    --hero-end: #3e6842;
    --sidebar-start: #121e12;
    --sidebar-end: #243a26;
    --glow-primary: rgba(74, 122, 82, 0.14);
    --glow-accent: rgba(184, 134, 62, 0.12);
    --shadow-soft: 0 18px 60px -34px rgba(10,30,15,0.38);
    --shadow-card: 0 18px 38px -28px rgba(10,30,15,0.24);
    --tag-primary-bg: rgba(74,122,82,0.12);      --tag-primary-text: #3a6a42;
    --tag-success-bg: rgba(80,130,84,0.14);      --tag-success-text: #3e6244;
    --tag-accent-bg: rgba(184,134,62,0.14);      --tag-accent-text: #8a6430;
    --tag-danger-bg: rgba(180,100,80,0.14);      --tag-danger-text: #884838;
    --tag-warm-bg: rgba(200,170,120,0.16);       --tag-warm-text: #7a6240;
    --chart-primary: #4a7a52;
    --chart-success: #3e6244;
    --chart-accent: #b8863e;
    --chart-muted: #7a8e80;
    --chart-legend: #3a5040;
    --tooltip-bg: rgba(248, 250, 246, 0.96);
}
```

---

## Theme 4: Arctic Blue

For winter, ski, northern Europe, Iceland.

### Dark Mode

```css
:root {
    --color-primary: #88b8d8;
    --color-primary-dark: #5a90b4;
    --color-primary-light: #b4d4e8;
    --color-accent: #c49070;
    --color-accent-light: rgba(196, 144, 112, 0.16);
    --color-ice: #a8cce0;
    --color-snow: rgba(180, 200, 220, 0.14);
    --color-aurora: #7aa8c8;
    --color-fjord: #5a8aaa;
    --bg: #080c12;
    --bg-secondary: #0c1018;
    --surface: rgba(12, 16, 24, 0.84);
    --surface-strong: #101820;
    --surface-soft: rgba(16, 22, 32, 0.78);
    --surface-contrast: rgba(255, 255, 255, 0.04);
    --text-strong: #e4eef6;
    --text-body: #b0c4d6;
    --text-muted: #7490a8;
    --text-inverse: #f8fafc;
    --border: rgba(130, 170, 200, 0.14);
    --border-strong: rgba(130, 170, 200, 0.22);
    --hero-start: #060a10;
    --hero-mid: #142030;
    --hero-end: #283e52;
    --sidebar-start: #080c14;
    --sidebar-end: #162838;
    --glow-primary: rgba(136, 184, 216, 0.20);
    --glow-accent: rgba(196, 144, 112, 0.14);
    --shadow-soft: 0 18px 60px -32px rgba(0,0,0,0.84);
    --shadow-card: 0 16px 36px -24px rgba(0,0,0,0.9);
    --tag-primary-bg: rgba(136,184,216,0.16);    --tag-primary-text: #a4d0e8;
    --tag-success-bg: rgba(120,170,140,0.14);    --tag-success-text: #94c8a8;
    --tag-accent-bg: rgba(196,144,112,0.14);     --tag-accent-text: #e0b898;
    --tag-danger-bg: rgba(200,120,110,0.14);     --tag-danger-text: #e8a498;
    --tag-warm-bg: rgba(200,180,150,0.14);       --tag-warm-text: #d8c4a0;
    --chart-primary: #88b8d8;
    --chart-success: #78a890;
    --chart-accent: #c49070;
    --chart-muted: #6880a0;
    --chart-legend: #a0b8d0;
    --tooltip-bg: rgba(8, 12, 18, 0.95);
}
```

### Light Mode

```css
html[data-theme="light"] {
    --color-primary: #4a82a0;
    --color-primary-dark: #2e6280;
    --color-primary-light: #88b4c8;
    --color-accent: #a87450;
    --color-accent-light: rgba(168, 116, 80, 0.14);
    --bg: #e4e8ec;
    --bg-secondary: #eef2f6;
    --surface: rgba(252, 254, 255, 0.78);
    --surface-strong: #f8fafc;
    --surface-soft: rgba(246, 250, 252, 0.82);
    --surface-contrast: rgba(10, 20, 40, 0.04);
    --text-strong: #142030;
    --text-body: #344a60;
    --text-muted: #687e94;
    --text-inverse: #f8fafc;
    --border: rgba(20, 40, 60, 0.12);
    --border-strong: rgba(20, 40, 60, 0.2);
    --hero-start: #0e1820;
    --hero-mid: #223848;
    --hero-end: #3a5870;
    --sidebar-start: #101a22;
    --sidebar-end: #1e3242;
    --glow-primary: rgba(74, 130, 160, 0.14);
    --glow-accent: rgba(168, 116, 80, 0.12);
    --shadow-soft: 0 18px 60px -34px rgba(10,20,40,0.36);
    --shadow-card: 0 18px 38px -28px rgba(10,20,40,0.22);
    --tag-primary-bg: rgba(74,130,160,0.12);     --tag-primary-text: #34627a;
    --tag-success-bg: rgba(80,130,100,0.14);     --tag-success-text: #3a6250;
    --tag-accent-bg: rgba(168,116,80,0.14);      --tag-accent-text: #7e5634;
    --tag-danger-bg: rgba(180,100,90,0.14);      --tag-danger-text: #884440;
    --tag-warm-bg: rgba(190,170,130,0.16);       --tag-warm-text: #7a6240;
    --chart-primary: #4a82a0;
    --chart-success: #4a7860;
    --chart-accent: #a87450;
    --chart-muted: #788ea0;
    --chart-legend: #3a5068;
    --tooltip-bg: rgba(248, 250, 252, 0.96);
}
```

---

## Theme 5: Warm Terracotta

For cultural, historical, temple, Mediterranean trips.

### Dark Mode

```css
:root {
    --color-primary: #c49478;
    --color-primary-dark: #a07058;
    --color-primary-light: #d8b8a0;
    --color-accent: #8aaa82;
    --color-accent-light: rgba(138, 170, 130, 0.16);
    --color-clay: #b8886a;
    --color-spice: rgba(196, 148, 120, 0.14);
    --color-mosaic: #c4a468;
    --color-amber: #d8a860;
    --bg: #100c0a;
    --bg-secondary: #16100c;
    --surface: rgba(22, 16, 12, 0.84);
    --surface-strong: #1c1410;
    --surface-soft: rgba(28, 20, 14, 0.78);
    --surface-contrast: rgba(255, 255, 255, 0.04);
    --text-strong: #f0e8e2;
    --text-body: #cec0b4;
    --text-muted: #9a8878;
    --text-inverse: #faf8f6;
    --border: rgba(180, 150, 120, 0.16);
    --border-strong: rgba(180, 150, 120, 0.24);
    --hero-start: #0e0a08;
    --hero-mid: #2a1e16;
    --hero-end: #443426;
    --sidebar-start: #100c08;
    --sidebar-end: #2c201a;
    --glow-primary: rgba(196, 148, 120, 0.20);
    --glow-accent: rgba(138, 170, 130, 0.14);
    --shadow-soft: 0 18px 60px -32px rgba(0,0,0,0.82);
    --shadow-card: 0 16px 36px -24px rgba(0,0,0,0.88);
    --tag-primary-bg: rgba(196,148,120,0.16);    --tag-primary-text: #d8b898;
    --tag-success-bg: rgba(138,170,130,0.14);    --tag-success-text: #a4c89c;
    --tag-accent-bg: rgba(196,164,104,0.14);     --tag-accent-text: #d4c088;
    --tag-danger-bg: rgba(200,110,90,0.14);      --tag-danger-text: #e0a088;
    --tag-warm-bg: rgba(210,180,140,0.14);       --tag-warm-text: #dcc498;
    --chart-primary: #c49478;
    --chart-success: #8aaa82;
    --chart-accent: #c4a468;
    --chart-muted: #887868;
    --chart-legend: #c0b0a0;
    --tooltip-bg: rgba(16, 12, 10, 0.94);
}
```

### Light Mode

```css
html[data-theme="light"] {
    --color-primary: #9a7060;
    --color-primary-dark: #7a5444;
    --color-primary-light: #c0a090;
    --color-accent: #6a8a64;
    --color-accent-light: rgba(106, 138, 100, 0.14);
    --bg: #eae4dc;
    --bg-secondary: #f4eee6;
    --surface: rgba(255, 252, 248, 0.78);
    --surface-strong: #faf6f2;
    --surface-soft: rgba(250, 246, 240, 0.82);
    --surface-contrast: rgba(30, 20, 10, 0.04);
    --text-strong: #2a1e16;
    --text-body: #4e3e34;
    --text-muted: #7a6a5e;
    --text-inverse: #faf8f6;
    --border: rgba(50, 35, 25, 0.12);
    --border-strong: rgba(50, 35, 25, 0.2);
    --hero-start: #1a1208;
    --hero-mid: #3a2a1c;
    --hero-end: #5a4430;
    --sidebar-start: #1c1410;
    --sidebar-end: #382a1e;
    --glow-primary: rgba(154, 112, 96, 0.14);
    --glow-accent: rgba(106, 138, 100, 0.12);
    --shadow-soft: 0 18px 60px -34px rgba(30,20,10,0.36);
    --shadow-card: 0 18px 38px -28px rgba(30,20,10,0.22);
    --tag-primary-bg: rgba(154,112,96,0.12);     --tag-primary-text: #7a5444;
    --tag-success-bg: rgba(90,120,84,0.14);      --tag-success-text: #4a6244;
    --tag-accent-bg: rgba(180,150,90,0.14);      --tag-accent-text: #80682e;
    --tag-danger-bg: rgba(180,90,70,0.14);       --tag-danger-text: #884038;
    --tag-warm-bg: rgba(200,170,120,0.16);       --tag-warm-text: #7a5e38;
    --chart-primary: #9a7060;
    --chart-success: #6a8a64;
    --chart-accent: #a88a48;
    --chart-muted: #8a7e72;
    --chart-legend: #4e3e34;
    --tooltip-bg: rgba(250, 246, 242, 0.96);
}
```

---

## Theme 6: Elegant Noir

For luxury, honeymoon, boutique travel, Maldives.

### Dark Mode

```css
:root {
    --color-primary: #c8a870;
    --color-primary-dark: #a08050;
    --color-primary-light: #dcc898;
    --color-accent: #9898a8;
    --color-accent-light: rgba(152, 152, 168, 0.14);
    --color-gold: #c8a870;
    --color-silk: rgba(200, 168, 112, 0.12);
    --color-marble: #b0b0b8;
    --color-velvet: #7a6a5a;
    --bg: #0a0a0c;
    --bg-secondary: #101012;
    --surface: rgba(16, 16, 18, 0.86);
    --surface-strong: #161618;
    --surface-soft: rgba(20, 20, 22, 0.80);
    --surface-contrast: rgba(255, 255, 255, 0.04);
    --text-strong: #f0ece6;
    --text-body: #c8c2b8;
    --text-muted: #8a847a;
    --text-inverse: #faf8f4;
    --border: rgba(200, 168, 112, 0.14);
    --border-strong: rgba(200, 168, 112, 0.22);
    --hero-start: #080808;
    --hero-mid: #1a1610;
    --hero-end: #302820;
    --sidebar-start: #0a0a08;
    --sidebar-end: #1e1a14;
    --glow-primary: rgba(200, 168, 112, 0.16);
    --glow-accent: rgba(152, 152, 168, 0.10);
    --shadow-soft: 0 18px 60px -32px rgba(0,0,0,0.88);
    --shadow-card: 0 16px 36px -24px rgba(0,0,0,0.92);
    --tag-primary-bg: rgba(200,168,112,0.14);    --tag-primary-text: #dcc898;
    --tag-success-bg: rgba(140,168,130,0.14);    --tag-success-text: #a8c8a0;
    --tag-accent-bg: rgba(152,152,168,0.12);     --tag-accent-text: #b8b8c8;
    --tag-danger-bg: rgba(200,120,110,0.14);     --tag-danger-text: #e0a498;
    --tag-warm-bg: rgba(200,180,140,0.14);       --tag-warm-text: #d8c498;
    --chart-primary: #c8a870;
    --chart-success: #8aaa84;
    --chart-accent: #9898a8;
    --chart-muted: #686868;
    --chart-legend: #b0a898;
    --tooltip-bg: rgba(10, 10, 12, 0.95);
}
```

### Light Mode

```css
html[data-theme="light"] {
    --color-primary: #96783c;
    --color-primary-dark: #705828;
    --color-primary-light: #c4a870;
    --color-accent: #74747e;
    --color-accent-light: rgba(116, 116, 126, 0.12);
    --bg: #e8e6e2;
    --bg-secondary: #f4f2ee;
    --surface: rgba(255, 254, 252, 0.80);
    --surface-strong: #faf8f6;
    --surface-soft: rgba(250, 248, 244, 0.84);
    --surface-contrast: rgba(20, 16, 10, 0.04);
    --text-strong: #1e1a14;
    --text-body: #4a4438;
    --text-muted: #7a7468;
    --text-inverse: #faf8f4;
    --border: rgba(40, 30, 20, 0.12);
    --border-strong: rgba(40, 30, 20, 0.20);
    --hero-start: #141008;
    --hero-mid: #2a2218;
    --hero-end: #443828;
    --sidebar-start: #161210;
    --sidebar-end: #2e261e;
    --glow-primary: rgba(150, 120, 60, 0.12);
    --glow-accent: rgba(116, 116, 126, 0.10);
    --shadow-soft: 0 18px 60px -34px rgba(20,16,10,0.34);
    --shadow-card: 0 18px 38px -28px rgba(20,16,10,0.20);
    --tag-primary-bg: rgba(150,120,60,0.12);     --tag-primary-text: #705828;
    --tag-success-bg: rgba(90,120,84,0.14);      --tag-success-text: #4a6244;
    --tag-accent-bg: rgba(116,116,126,0.12);     --tag-accent-text: #585860;
    --tag-danger-bg: rgba(180,100,90,0.14);      --tag-danger-text: #884038;
    --tag-warm-bg: rgba(190,170,130,0.16);       --tag-warm-text: #7a6238;
    --chart-primary: #96783c;
    --chart-success: #6a8264;
    --chart-accent: #74747e;
    --chart-muted: #8a8478;
    --chart-legend: #4a4438;
    --tooltip-bg: rgba(250, 248, 246, 0.96);
}
```

---

## Theme Toggle Implementation

### HTML (on every page)

Two toggle buttons: one in the sidebar (desktop), one in the mobile top bar.

```html
<button type="button" data-theme-toggle class="theme-toggle" aria-label="라이트 모드로 전환">
    <span class="theme-icon" data-theme-icon>☀</span>
    <span class="theme-label" data-theme-label>라이트</span>
</button>
```

### JavaScript

```javascript
var root = document.documentElement;
var THEME_KEY = '[destination]-trip-theme';
var themeButtons = document.querySelectorAll('[data-theme-toggle]');

function applyTheme(theme) {
    root.setAttribute('data-theme', theme);
    var isDark = theme === 'dark';
    themeButtons.forEach(function(btn) {
        var icon = btn.querySelector('[data-theme-icon]');
        var label = btn.querySelector('[data-theme-label]');
        if (icon) icon.textContent = isDark ? '☀' : '☾';
        if (label) label.textContent = isDark ? '라이트' : '다크';
    });
    // Update Chart.js instances if any
    updateChartTheme();
}

var saved = null;
try { saved = localStorage.getItem(THEME_KEY); } catch(e) {}
applyTheme(saved || 'dark');

themeButtons.forEach(function(btn) {
    btn.addEventListener('click', function() {
        var next = root.getAttribute('data-theme') === 'dark' ? 'light' : 'dark';
        applyTheme(next);
        try { localStorage.setItem(THEME_KEY, next); } catch(e) {}
    });
});
```

### Chart theme update function

```javascript
function getThemeValue(name) {
    return getComputedStyle(root).getPropertyValue(name).trim();
}

function updateChartTheme() {
    if (!budgetChart) return;
    budgetChart.data.datasets[0].backgroundColor = [
        getThemeValue('--chart-primary'),
        getThemeValue('--chart-success'),
        getThemeValue('--chart-accent'),
        getThemeValue('--chart-muted')
    ];
    budgetChart.options.plugins.legend.labels.color = getThemeValue('--chart-legend');
    budgetChart.options.plugins.tooltip.backgroundColor = getThemeValue('--tooltip-bg');
    budgetChart.update();
}
```

---

## Body Background Pattern

Every theme uses the same background structure with theme-specific glow colors:

```css
body {
    background:
        radial-gradient(circle at top left, var(--glow-primary), transparent 32%),
        radial-gradient(circle at top right, var(--glow-accent), transparent 28%),
        linear-gradient(180deg, var(--bg-secondary) 0%, var(--bg) 55%, var(--bg-secondary) 100%);
    background-color: var(--bg);
}

/* Subtle grid overlay */
body::before {
    content: "";
    position: fixed; inset: 0;
    background-image:
        linear-gradient(rgba(255,255,255,0.035) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);
    background-size: 120px 120px;
    opacity: 0.3;
    mask-image: linear-gradient(180deg, rgba(0,0,0,0.9), transparent 100%);
    pointer-events: none;
}
```

---

## Tailwind Overrides

Since Tailwind utility classes use hardcoded colors, override them with
CSS variable mappings to ensure theme consistency:

```css
.bg-white { background: var(--surface-strong) !important; }
.bg-gray-50 { background: rgba(127,148,168,0.08) !important; }
.bg-gray-100 { background: rgba(127,148,168,0.12) !important; }
.text-gray-900 { color: var(--text-strong) !important; }
.text-gray-700 { color: var(--text-body) !important; }
.text-gray-500 { color: var(--text-muted) !important; }
.border-gray-200 { border-color: var(--border) !important; }
.shadow-sm { box-shadow: var(--shadow-card) !important; }
/* ... add more as needed for tag colors */
```

This ensures Tailwind utility classes respect the active theme.
