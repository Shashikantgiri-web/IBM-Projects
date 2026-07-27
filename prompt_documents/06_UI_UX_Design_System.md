# 06 — UI/UX Design System
## Employee Performance Intelligence System (EPIS)

---

## 1. Design Philosophy

EPIS uses an IBM-inspired enterprise design language. The interface is:
- **Clean** — no unnecessary decoration
- **Data-first** — information is always the center of attention
- **Trustworthy** — consistent colors, spacing, and typography build confidence
- **Accessible** — meets WCAG AA contrast standards
- **Responsive** — works on all screens from 375px wide upward

---

## 2. Color Palette

### Primary Colors
| Name | Light Mode | Dark Mode | Usage |
|---|---|---|---|
| Primary Blue | `#0F62FE` | `#4589FF` | Buttons, links, active states |
| Primary Dark | `#001141` | `#E0E0E0` | Headings, body text |
| Background | `#FFFFFF` | `#161616` | Page background |
| Surface | `#F4F4F4` | `#262626` | Cards, sidebar |
| Border | `#E0E0E0` | `#393939` | Dividers, card borders |

### Status Colors
| Name | Hex | Usage |
|---|---|---|
| Success Green | `#24A148` | Positive metrics, retained status |
| Warning Yellow | `#F1C21B` | Medium risk indicators |
| Danger Red | `#DA1E28` | Attrition, high risk |
| Info Blue | `#0043CE` | Informational badges |

### Chart Colors (Ordered)
```
#0F62FE  → Primary Blue
#42BE65  → Green
#FF832B  → Orange
#BE95FF  → Purple
#08BDBA  → Teal
#EE538B  → Pink
#F1C21B  → Yellow
#A2191F  → Dark Red
```

---

## 3. Typography

| Element | Font | Size | Weight |
|---|---|---|---|
| Page Title (H1) | IBM Plex Sans | 32px | 600 |
| Section Title (H2) | IBM Plex Sans | 24px | 600 |
| Card Title (H3) | IBM Plex Sans | 18px | 500 |
| Body Text | IBM Plex Sans | 14px | 400 |
| Label / Caption | IBM Plex Sans | 12px | 400 |
| KPI Number | IBM Plex Mono | 36px | 700 |
| Table Data | IBM Plex Sans | 13px | 400 |
| Button Text | IBM Plex Sans | 14px | 500 |
| Navbar Link | IBM Plex Sans | 14px | 400 |

**Google Fonts import:**
```css
@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@400;500;600&family=IBM+Plex+Mono:wght@400;700&display=swap');
```

---

## 4. Spacing System

Uses an 8px base grid. All spacing values are multiples of 8.

| Token | Value | Usage |
|---|---|---|
| spacing-1 | 4px | Tight internal padding |
| spacing-2 | 8px | Icon gaps, inline spacing |
| spacing-3 | 12px | Small component padding |
| spacing-4 | 16px | Standard component padding |
| spacing-6 | 24px | Card padding, section gaps |
| spacing-8 | 32px | Page section spacing |
| spacing-12 | 48px | Major section dividers |
| spacing-16 | 64px | Page-level padding |

---

## 5. Component Specifications

### 5.1 KPI Card
```
┌─────────────────────────────┐
│  Icon                       │
│  Card Title (Label)         │
│  ─────────────────          │
│  1,470                      │  ← IBM Plex Mono, 36px
│  +2.4% from last period     │  ← Caption, green/red
└─────────────────────────────┘
```
- Background: Surface color
- Border: 1px border
- Padding: 24px
- Border radius: 4px (IBM style — minimal radius)
- Min-height: 140px

---

### 5.2 Chart Card
```
┌─────────────────────────────────────┐
│  Chart Title              [Export]  │
│  Subtitle (optional)                │
│  ─────────────────────────────      │
│                                     │
│         [Chart renders here]        │
│                                     │
│  ─────────────────────────────      │
│  Legend (if applicable)             │
└─────────────────────────────────────┘
```
- Background: Surface color
- Padding: 24px
- Chart height: 280px (default), 400px (large)
- Border radius: 4px

---

### 5.3 Buttons

| Type | Background | Text | Border | Usage |
|---|---|---|---|---|
| Primary | `#0F62FE` | White | None | Main actions (Login, Export) |
| Secondary | Transparent | `#0F62FE` | `#0F62FE` | Secondary actions |
| Ghost | Transparent | Primary Dark | None | Tertiary actions |
| Danger | `#DA1E28` | White | None | Destructive actions |
| Disabled | `#C6C6C6` | `#8D8D8D` | None | Disabled state |

Button specs:
- Height: 40px (default), 32px (small), 48px (large)
- Padding: 0 16px
- Border radius: 2px (IBM flat style)
- Font: 14px / 500 weight

---

### 5.4 Input Fields
```
Label (12px, weight 500)
┌─────────────────────────────┐
│  Placeholder text           │
└─────────────────────────────┘
Helper text (12px, gray)
```
- Height: 40px
- Border: 1px `#E0E0E0`
- Border bottom on focus: 2px `#0F62FE`
- Background: White (light) / `#262626` (dark)
- Padding: 0 16px
- No border radius (IBM flat style)

---

### 5.5 Data Table
```
┌────────┬──────────────┬───────────┬──────────────┬──────────┐
│ ID     │ Name         │ Dept      │ Performance  │ Status   │
├────────┼──────────────┼───────────┼──────────────┼──────────┤
│ EMP001 │ John Smith   │ Sales     │ ●●●●○ 4/5   │ Active   │
│ EMP002 │ Jane Doe     │ R&D       │ ●●●○○ 3/5   │ Attrited │
└────────┴──────────────┴───────────┴──────────────┴──────────┘
         Page 1 of 12                    [< Previous] [Next >]
```
- Row height: 48px
- Header background: `#F4F4F4`
- Row hover: `#E8F0FE`
- Zebra striping: Off
- Sortable headers show arrow icon
- Status badges use color coding

---

### 5.6 Sidebar
- Width: 256px (expanded), 64px (collapsed)
- Background: Surface color
- Active link: Left border 4px `#0F62FE`, background `#E8F0FE`
- Icon size: 20px
- Item height: 48px

---

### 5.7 Navbar
- Height: 64px
- Background: White (light) / `#161616` (dark)
- Shadow: 0 1px 0 border color
- Logo: Left aligned
- Links: Center
- User menu: Right

---

### 5.8 Badges / Tags
| Status | Background | Text Color |
|---|---|---|
| Active | `#D4EFDF` | `#1D6A34` |
| Attrited | `#FCDCDB` | `#6E1A19` |
| High Performance | `#D4EFDF` | `#1D6A34` |
| Low Performance | `#FCDCDB` | `#6E1A19` |

---

## 6. Icons

Use **Lucide React** icon library throughout the application.

| Usage | Icon Name |
|---|---|
| Dashboard | `LayoutDashboard` |
| Employees | `Users` |
| Department | `Building2` |
| Performance | `TrendingUp` |
| Salary | `DollarSign` |
| Training | `GraduationCap` |
| Settings | `Settings` |
| Dark Mode | `Moon` / `Sun` |
| Export | `Download` |
| Search | `Search` |
| Filter | `Filter` |
| Logout | `LogOut` |
| Profile | `UserCircle` |
| Alert | `AlertTriangle` |

---

## 7. Responsive Breakpoints

| Breakpoint | Width | Layout |
|---|---|---|
| Mobile | 375px–767px | Single column, sidebar hidden (hamburger menu) |
| Tablet | 768px–1023px | Two column, collapsible sidebar |
| Desktop | 1024px+ | Full layout, sidebar always visible |
| Wide | 1280px+ | Wider chart areas |

**Grid:** 12-column grid system via Tailwind's `grid-cols-*`

**KPI Cards:**
- Desktop: 4 per row
- Tablet: 2 per row
- Mobile: 1 per row

**Charts:**
- Desktop: 2 per row
- Tablet: 1 per row
- Mobile: 1 per row

---

## 8. Dark Mode

Dark mode uses Tailwind's `dark:` class prefix. The `ThemeProvider` context manages the toggle.

| Element | Light | Dark |
|---|---|---|
| Page background | `#FFFFFF` | `#161616` |
| Card background | `#F4F4F4` | `#262626` |
| Text primary | `#161616` | `#F4F4F4` |
| Text secondary | `#525252` | `#A8A8A8` |
| Border | `#E0E0E0` | `#393939` |
| Input background | `#FFFFFF` | `#262626` |
| Table row hover | `#E8F0FE` | `#333333` |

---

## 9. Animation & Transitions

- Page transitions: Fade in (200ms)
- Sidebar collapse: Slide (250ms ease)
- Modal open: Scale + fade (200ms)
- Toast appear: Slide from right (250ms)
- Skeleton loading: Pulse animation

All transitions use `transition-all duration-200 ease-in-out` as Tailwind class.

---

## 10. Accessibility Standards

- All interactive elements are keyboard navigable
- Focus rings visible on all focusable elements (`focus:ring-2 focus:ring-blue-500`)
- Color contrast minimum 4.5:1 for body text (WCAG AA)
- All images have `alt` text
- Form inputs have associated `<label>` elements
- Charts have text fallbacks for screen readers
- ARIA roles applied to sidebar navigation and modals
