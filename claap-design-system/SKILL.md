---
name: claap-design-system
description: "Claap design system in Tailwind CSS. Use when building Claude artifacts, HTML/React prototypes, or any Claap UI. Covers setup boilerplate, color tokens, typography, and all component recipes. Triggers on: claap design, claap UI, claap artifact, claap prototype, build claap interface."
user-invocable: true
---

# Claap Design System — Tailwind

Dark-first UI. Inter font. Blue as primary. No pink in UI.

---

## ⚠️ Design Rules (read first)

| ❌ Never | ✅ Do instead |
|---------|--------------|
| Left border accent on cards or headings | Clean `border border-gray-1500` only |
| Tabs in a shared container background | Free-floating tabs, active = standalone `rounded-xl` dark pill |
| Underline on active tab | `bg-gray-1500 rounded-xl font-semibold` |
| Pink for any UI element (button, text, border) | Blue `#4C74FF` for everything interactive |
| Blue border on hover | Gray lift: `hover:border-gray-1300 hover:bg-gray-1600` |
| Active nav item with left border | Background fill: `bg-gray-1500 rounded-lg` |
| Pink/salmon text for values or links | `text-blue-400` |

**Color roles:** Blue = all interactive · Pink = logo only · Red = errors · Green = success

---

## Setup Boilerplate

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: { sans: ['Inter', 'system-ui', 'sans-serif'] },
          colors: {
            gray: {
              25:'#F8FAFD', 50:'#F0F2F7', 100:'#E6E8F1', 200:'#D6D9E8',
              300:'#C6CADF', 400:'#B5BBD6', 500:'#A5ACCD', 600:'#959DC4',
              700:'#848EBB', 800:'#747EB1', 900:'#636FA8', 1000:'#57639C',
              1100:'#4E588B', 1200:'#444E7B', 1300:'#3B436A', 1400:'#32395A',
              1500:'#292F4A', 1600:'#202439', 1650:'#1B1F31', 1700:'#141622',
            },
            blue:      { 200:'#99AFFF', 300:'#809CFF', 400:'#4C74FF', 500:'#3864FF', 600:'#1243F2' },
            pink:      { 200:'#FFA8BD', 300:'#FF8FA9', 400:'#FF5A81', 500:'#DF4F71' },
            red:       { 200:'#FDA0A0', 300:'#FD8686', 400:'#F4505D', 500:'#ED4545' },
            green:     { 200:'#96E283', 300:'#84DD6E', 400:'#00CF56', 500:'#00B24A' },
            yellow:    { 200:'#FFD78A', 300:'#FFCE70', 400:'#FFBD3E', 500:'#F3A716' },
            purple:    { 200:'#B88DF6', 300:'#A975F5', 400:'#8E51E7', 500:'#7330D5' },
            turquoise: { 200:'#60DCD5', 300:'#4BD7CF', 400:'#2BC5BB', 500:'#2B918B' },
            bordeaux:  { 200:'#E77EC3', 300:'#E368B9', 400:'#D047A2', 500:'#AB3B85' },
          },
          borderRadius: { sm:'4px', DEFAULT:'6px', md:'6px', lg:'8px', xl:'12px' },
          boxShadow: {
            modal: '0 4px 40px rgba(0,0,0,0.15), 0 3px 20px rgba(0,0,0,0.19)',
            popin: '0 10px 15px rgba(0,0,0,0.4), 0 4px 6px rgba(0,0,0,0.3)',
          },
        },
      },
    }
  </script>
  <style>
    body { font-family: 'Inter', system-ui, sans-serif; }
    * { scrollbar-width: none; }
    *::-webkit-scrollbar { display: none; }
  </style>
</head>
<body class="bg-gray-1700 text-white font-sans">
  <!-- content -->
</body>
</html>
```

---

## Tokens

### Surfaces (dark → light)
| Role | Dark | Light |
|------|------|-------|
| Page bg | `bg-gray-1700` `#141622` | `bg-gray-50` |
| Card / big tile | `bg-gray-1650` `#1B1F31` | `bg-white` |
| Panel / small tile | `bg-gray-1600` `#202439` | `bg-gray-25` |
| Hover | `bg-gray-1500` `#292F4A` | `bg-gray-100` |
| Tooltip / dropdown | `bg-gray-1400` `#32395A` | — |
| Modal overlay | `bg-[rgba(20,22,34,0.7)]` | — |
| Blue action | `bg-blue-400` | same |

### Text
| Role | Class |
|------|-------|
| Headings | `text-white` |
| Body | `text-gray-200` |
| Secondary | `text-gray-400` |
| Muted / labels | `text-gray-600` |
| Links / actions | `text-blue-400` |
| Placeholder | `text-gray-800` |
| Error / Success / Warning | `text-red-400` / `text-green-400` / `text-yellow-400` |

### Borders
| Role | Class |
|------|-------|
| Default card/tile | `border-gray-1500` |
| Dropdown/modal | `border-gray-1400` |
| Hover lift | `hover:border-gray-1300` |
| Divider | `h-px bg-gray-1500` |

### Radius
`rounded-sm`=4px · `rounded-lg`=8px (inputs, buttons, menus) · `rounded-xl`=12px (cards, modals)

### Typography (Inter)
| Size | px | Class |
|------|----|-------|
| 2xs | 11 | `text-[11px]` |
| xs | 12 | `text-xs` |
| sm | 13 | `text-[13px]` |
| md | 15 | `text-[15px]` |
| lg | 18 | `text-lg` |
| xl | 20 | `text-xl` |
| 2xl | 24 | `text-2xl` |

Weights: `font-normal`=400 · `font-medium`=500 · `font-semibold`=600 · `font-bold`=700

---

## Components

### Buttons

```html
<!-- Primary (blue) -->
<button class="inline-flex items-center gap-1.5 px-3 py-1.5 bg-blue-400 hover:bg-blue-500 text-white text-[13px] font-medium rounded-lg transition-colors">
  Save changes
</button>

<!-- Secondary (dark fill + border) -->
<button class="inline-flex items-center gap-1.5 px-3 py-1.5 bg-gray-1600 hover:bg-gray-1500 text-white text-[13px] font-medium rounded-lg border border-gray-1400 hover:border-gray-1300 transition-colors">
  Remove photo
</button>

<!-- Ghost (no bg, gray text) -->
<button class="inline-flex items-center gap-1.5 px-3 py-1.5 hover:bg-gray-1500 text-gray-400 hover:text-white text-[13px] font-medium rounded-lg transition-colors">
  Cancel
</button>

<!-- Destructive -->
<button class="inline-flex items-center gap-1.5 px-3 py-1.5 bg-red-400 hover:bg-red-500 text-white text-[13px] font-medium rounded-lg transition-colors">
  Delete
</button>

<!-- Icon button -->
<button class="w-8 h-8 flex items-center justify-center hover:bg-gray-1500 text-gray-400 hover:text-white rounded-lg transition-colors">
  <svg width="16" height="16" .../>
</button>
```

**Sizes:** `px-2 py-1 text-[11px]` (xs) · `px-3 py-1.5 text-[13px]` (sm, default) · `px-4 py-2 text-[15px]` (md)

---

### Tabs

Free-floating. Active = standalone dark pill. No container, no underline.

```html
<div class="flex items-center gap-1">
  <!-- Active: dark pill -->
  <button class="px-4 py-2 text-[13px] font-semibold text-white bg-gray-1500 rounded-xl transition-colors">
    All meetings
  </button>
  <!-- Inactive: plain gray -->
  <button class="px-4 py-2 text-[13px] font-medium text-gray-400 hover:text-white hover:bg-gray-1600 rounded-xl transition-colors">
    My meetings
  </button>
</div>
```

---

### Cards

```html
<!-- Standard card (hover = gray lift, no blue) -->
<div class="bg-gray-1650 border border-gray-1500 rounded-xl p-4 hover:border-gray-1300 hover:bg-gray-1600 transition-colors">
  content
</div>

<!-- Panel / inner tile -->
<div class="bg-gray-1600 border border-gray-1500 rounded-xl p-4">
  content
</div>

<!-- Hoverable list row -->
<div class="flex items-center px-3 py-2 hover:bg-gray-1600 rounded-lg cursor-pointer transition-colors">
  content
</div>
```

---

### Form & Inputs

```html
<!-- Field: label above + flat dark input -->
<div class="flex flex-col gap-1">
  <label class="text-[13px] text-gray-600">First name</label>
  <input type="text" placeholder="Robin"
    class="w-full px-3 py-2.5 bg-gray-1600 border border-gray-1400 rounded-lg text-[13px] text-white placeholder-gray-800 focus:outline-none focus:border-blue-400 transition-colors" />
</div>

<!-- Password with eye icon -->
<div class="flex flex-col gap-1">
  <label class="text-[13px] text-gray-600">Password</label>
  <div class="relative">
    <input type="password" placeholder="Enter password"
      class="w-full px-3 py-2.5 pr-10 bg-gray-1600 border border-gray-1400 rounded-lg text-[13px] text-white placeholder-gray-800 focus:outline-none focus:border-blue-400 transition-colors" />
    <button class="absolute right-3 top-1/2 -translate-y-1/2 text-gray-600 hover:text-gray-400 transition-colors">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/><circle cx="12" cy="12" r="3"/>
      </svg>
    </button>
  </div>
</div>

<!-- Textarea -->
<div class="flex flex-col gap-1">
  <label class="text-[13px] text-gray-600">Description</label>
  <textarea rows="4" placeholder="Write something..."
    class="w-full px-3 py-2.5 bg-gray-1600 border border-gray-1400 rounded-lg text-[13px] text-white placeholder-gray-800 focus:outline-none focus:border-blue-400 resize-none transition-colors"></textarea>
</div>
```

**Full settings form (2-col grid, sections, divider):**
```html
<div class="flex flex-col gap-6">
  <div class="flex items-center gap-2">
    <button class="px-3 py-1.5 bg-blue-400 hover:bg-blue-500 text-white text-[13px] font-medium rounded-lg transition-colors">Upload photo</button>
    <button class="px-3 py-1.5 bg-gray-1600 hover:bg-gray-1500 text-white text-[13px] font-medium rounded-lg border border-gray-1400 hover:border-gray-1300 transition-colors">Remove photo</button>
  </div>

  <div class="flex flex-col gap-4">
    <h2 class="text-[15px] font-medium text-white">Personal information</h2>
    <div class="grid grid-cols-2 gap-4">
      <div class="flex flex-col gap-1">
        <label class="text-[13px] text-gray-600">First name</label>
        <input type="text" value="Robin" class="w-full px-3 py-2.5 bg-gray-1600 border border-gray-1400 rounded-lg text-[13px] text-white focus:outline-none focus:border-blue-400 transition-colors" />
      </div>
      <div class="flex flex-col gap-1">
        <label class="text-[13px] text-gray-600">Last name</label>
        <input type="text" value="Bonduelle" class="w-full px-3 py-2.5 bg-gray-1600 border border-gray-1400 rounded-lg text-[13px] text-white focus:outline-none focus:border-blue-400 transition-colors" />
      </div>
    </div>
    <div class="flex flex-col gap-1">
      <label class="text-[13px] text-gray-600">Email</label>
      <input type="email" value="robin@claap.io" class="w-full px-3 py-2.5 bg-gray-1600 border border-gray-1400 rounded-lg text-[13px] text-white focus:outline-none focus:border-blue-400 transition-colors" />
    </div>
    <button class="w-fit px-3 py-1.5 bg-blue-400 hover:bg-blue-500 text-white text-[13px] font-medium rounded-lg transition-colors">Save changes</button>
  </div>

  <div class="h-px bg-gray-1500"></div>

  <div class="flex flex-col gap-4">
    <h2 class="text-[15px] font-medium text-white">Password</h2>
    <!-- password fields... -->
    <button class="w-fit px-3 py-1.5 bg-blue-400 hover:bg-blue-500 text-white text-[13px] font-medium rounded-lg transition-colors">Change password</button>
  </div>
</div>
```

---

### Toggle Switch

```html
<!-- ON (blue) -->
<button class="relative w-10 h-6 bg-blue-400 rounded-full transition-colors">
  <span class="absolute left-1 top-1 w-4 h-4 bg-white rounded-full shadow transition-transform translate-x-4"></span>
</button>

<!-- OFF (dark) -->
<button class="relative w-10 h-6 bg-gray-1400 rounded-full transition-colors">
  <span class="absolute left-1 top-1 w-4 h-4 bg-gray-700 rounded-full shadow transition-transform translate-x-0"></span>
</button>
```

**Settings row (label + toggle):**
```html
<div class="bg-gray-1600 rounded-xl overflow-hidden">
  <div class="flex items-center justify-between px-4 py-3 border-b border-gray-1500">
    <span class="text-[13px] text-gray-200">All folders</span>
    <button class="relative w-10 h-6 bg-blue-400 rounded-full"><span class="absolute left-1 top-1 w-4 h-4 bg-white rounded-full shadow translate-x-4"></span></button>
  </div>
  <div class="flex items-center justify-between px-4 py-3">
    <span class="text-[13px] text-gray-200">Meetings insights</span>
    <button class="relative w-10 h-6 bg-gray-1400 rounded-full"><span class="absolute left-1 top-1 w-4 h-4 bg-gray-700 rounded-full shadow translate-x-0"></span></button>
  </div>
</div>
```

---

### Checkbox

```html
<label class="flex items-center gap-2 cursor-pointer">
  <input type="checkbox" class="sr-only peer" />
  <div class="w-4 h-4 rounded-sm border border-gray-1200 peer-checked:bg-blue-400 peer-checked:border-blue-400 flex items-center justify-center transition-colors">
    <svg class="hidden peer-checked:block text-white" width="10" height="10" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3"><path d="M20 6L9 17l-5-5"/></svg>
  </div>
  <span class="text-[13px] text-gray-200">Label</span>
</label>
```

---

### Badges & Dot Labels

```html
<!-- Tag (colored bg + border) -->
<span class="inline-flex items-center px-2 py-0.5 bg-blue-400/10 border border-blue-200/30 text-blue-300 text-xs font-medium rounded-md">Label</span>

<!-- Tag color matrix: swap blue → green/red/yellow/purple/turquoise/gray -->
<!-- gray: bg-gray-1500/50 border-gray-400/30 text-gray-400 -->

<!-- Dot label (in tables / lists — NOT a pill) -->
<span class="inline-flex items-center gap-1.5 text-[13px] text-gray-200">
  <span class="w-2 h-2 rounded-full bg-blue-400 shrink-0"></span>
  Onboarding
</span>

<!-- Count pill -->
<span class="px-2 py-0.5 bg-blue-400 text-white text-xs font-semibold rounded-full">3</span>
```

---

### Avatar

```html
<!-- Image -->
<img class="w-8 h-8 rounded-full object-cover" src="..." alt="" />
<!-- Initials -->
<div class="w-8 h-8 rounded-full bg-blue-400 flex items-center justify-center text-white text-xs font-semibold">RB</div>
<!-- Sizes: w-6(xs) w-8(sm) w-10(md) w-12(lg) -->
```

---

### Table

Full-width, no outer border. Header slightly darker. Dot labels for categories.

```html
<div class="w-full overflow-x-auto">
  <table class="w-full">
    <thead>
      <tr class="border-b border-gray-1600 bg-gray-1700">
        <th class="w-10 px-3 py-2.5"><input type="checkbox" class="w-4 h-4 rounded-sm border border-gray-1200 accent-blue-400" /></th>
        <th class="px-3 py-2.5 text-left text-[11px] font-medium text-gray-600 uppercase tracking-wide">Title</th>
        <th class="px-3 py-2.5 text-left text-[11px] font-medium text-gray-600 uppercase tracking-wide">Owner</th>
        <th class="px-3 py-2.5 text-left text-[11px] font-medium text-gray-600 uppercase tracking-wide">Labels</th>
        <th class="px-3 py-2.5 text-left text-[11px] font-medium text-gray-600 uppercase tracking-wide">Date</th>
      </tr>
    </thead>
    <tbody>
      <tr class="group border-b border-gray-1600 hover:bg-gray-1600 transition-colors cursor-pointer">
        <td class="px-3 py-2.5"><input type="checkbox" class="w-4 h-4 rounded-sm border border-gray-1200 accent-blue-400 opacity-0 group-hover:opacity-100" /></td>
        <td class="px-3 py-2.5">
          <div class="flex items-center gap-2">
            <span class="text-[13px] font-medium text-white">Stripe - Fintech onboarding</span>
            <button class="hidden group-hover:inline-flex items-center gap-1 px-2 py-0.5 border border-gray-1300 text-gray-400 text-xs rounded-md hover:text-white transition-colors">Open</button>
          </div>
        </td>
        <td class="px-3 py-2.5 text-[13px] text-gray-400">Jannik Sinner</td>
        <td class="px-3 py-2.5">
          <span class="inline-flex items-center gap-1.5 text-[13px] text-gray-200">
            <span class="w-2 h-2 rounded-full bg-blue-400"></span>Onboarding
          </span>
        </td>
        <td class="px-3 py-2.5 text-[13px] text-gray-400">2mo ago</td>
      </tr>
    </tbody>
  </table>
  <!-- Pagination -->
  <div class="flex items-center justify-center gap-2 py-4">
    <button class="w-7 h-7 flex items-center justify-center rounded-md text-gray-400 hover:text-white hover:bg-gray-1500 transition-colors">‹</button>
    <span class="w-7 h-7 flex items-center justify-center rounded-md text-[13px] font-medium text-white bg-gray-1500">1</span>
    <button class="w-7 h-7 flex items-center justify-center rounded-md text-gray-400 hover:text-white hover:bg-gray-1500 transition-colors">›</button>
  </div>
</div>
```

---

### Grid Cards

```html
<div class="grid grid-cols-2 gap-4">
  <div class="bg-gray-1650 border border-gray-1500 rounded-xl p-4 hover:border-gray-1300 hover:bg-gray-1600 transition-colors cursor-pointer flex flex-col gap-3">
    <div class="flex items-center gap-2">
      <div class="w-8 h-8 rounded-lg bg-gray-1400 flex items-center justify-center shrink-0">
        <svg width="16" height="16" .../>
      </div>
      <span class="text-[13px] font-semibold text-white flex-1">Card Title</span>
      <span class="w-2 h-2 rounded-full bg-blue-400"></span>
    </div>
    <p class="text-[13px] text-gray-400 leading-relaxed">Card description text.</p>
    <div class="flex items-center gap-3 mt-auto">
      <span class="text-[11px] text-gray-600">Stats</span>
      <div class="ml-auto w-6 h-6 rounded-full bg-blue-400 flex items-center justify-center text-white text-[10px] font-semibold">RB</div>
    </div>
  </div>
</div>
```

---

### Sidebar

Active item = background fill, no left border.

```html
<nav class="w-56 bg-gray-1700 border-r border-gray-1500 flex flex-col h-full">
  <div class="px-4 py-3 border-b border-gray-1500">
    <img src="https://assets-prod.claap.io/pubassets/custom/logo_pink_256.svg" alt="Claap" class="h-6" />
  </div>
  <div class="flex-1 px-2 py-3 flex flex-col gap-0.5">
    <!-- Active -->
    <a class="flex items-center gap-2.5 px-2.5 py-2 bg-gray-1500 text-white text-[13px] font-medium rounded-lg cursor-pointer">
      <svg width="16" height="16" .../> Agents
    </a>
    <!-- Inactive -->
    <a class="flex items-center gap-2.5 px-2.5 py-2 hover:bg-gray-1500 text-gray-400 hover:text-white text-[13px] font-medium rounded-lg transition-colors cursor-pointer">
      <svg width="16" height="16" .../> Meetings
    </a>
  </div>
  <!-- Workspace / user at bottom -->
  <div class="px-3 py-3 border-t border-gray-1500">
    <div class="flex items-center gap-2">
      <div class="w-6 h-6 rounded-full bg-green-400 flex items-center justify-center text-white text-[10px] font-semibold">L</div>
      <span class="text-[13px] text-gray-400">lemlist</span>
    </div>
  </div>
</nav>
```

---

### Collapsible Section

No left border, no decoration.

```html
<button class="flex items-center justify-between w-full py-3">
  <span class="text-[15px] font-medium text-white">Instructions</span>
  <svg class="text-gray-600" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M6 9l6 6 6-6"/></svg>
</button>
```

---

### Modal

```html
<div class="fixed inset-0 bg-[rgba(20,22,34,0.7)] backdrop-blur-sm z-40 flex items-center justify-center">
  <div class="bg-gray-1600 border border-gray-1400 rounded-xl shadow-modal w-full max-w-md mx-4 p-6">
    <div class="flex items-center justify-between mb-4">
      <h2 class="text-xl font-medium text-white">Modal Title</h2>
      <button class="w-8 h-8 flex items-center justify-center hover:bg-gray-1500 rounded-lg text-gray-400 hover:text-white transition-colors">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 6L6 18M6 6l12 12"/></svg>
      </button>
    </div>
    <p class="text-[15px] text-gray-200 mb-6">Content here.</p>
    <div class="flex justify-end gap-2">
      <button class="px-3 py-1.5 hover:bg-gray-1500 text-gray-400 hover:text-white text-[13px] font-medium rounded-lg transition-colors">Cancel</button>
      <button class="px-3 py-1.5 bg-blue-400 hover:bg-blue-500 text-white text-[13px] font-medium rounded-lg transition-colors">Confirm</button>
    </div>
  </div>
</div>
```

---

### Dropdown Menu

```html
<div class="bg-gray-1500 border border-gray-1400 rounded-xl shadow-popin py-1 min-w-40">
  <button class="w-full flex items-center gap-2 px-3 py-2 text-[13px] text-gray-200 hover:bg-gray-1400 hover:text-white transition-colors">Menu item</button>
  <div class="h-px bg-gray-1400 my-1"></div>
  <button class="w-full flex items-center gap-2 px-3 py-2 text-[13px] text-red-400 hover:bg-gray-1400 transition-colors">Delete</button>
</div>
```

---

### Toast

```html
<div class="fixed bottom-6 left-1/2 -translate-x-1/2 z-50">
  <div class="flex items-center gap-3 px-4 py-3 bg-gray-1400 border border-gray-1300 rounded-xl shadow-modal min-w-64">
    <svg class="text-green-400 shrink-0" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M20 6L9 17l-5-5"/></svg>
    <span class="text-[13px] font-medium text-white">Action completed</span>
    <button class="ml-auto text-gray-600 hover:text-white transition-colors">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 6L6 18M6 6l12 12"/></svg>
    </button>
  </div>
</div>
```

---

### Chat Input

```html
<div class="flex items-center gap-2 px-3 py-2 bg-gray-1600 border border-gray-1500 rounded-xl">
  <button class="text-gray-600 hover:text-gray-400 transition-colors shrink-0">
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21.44 11.05l-9.19 9.19a6 6 0 01-8.49-8.49l9.19-9.19a4 4 0 015.66 5.66l-9.2 9.19a2 2 0 01-2.83-2.83l8.49-8.48"/></svg>
  </button>
  <input type="text" placeholder="Message your agent..." class="flex-1 bg-transparent text-[13px] text-white placeholder-gray-800 focus:outline-none" />
  <button class="w-8 h-8 flex items-center justify-center bg-blue-400 hover:bg-blue-500 text-white rounded-full transition-colors shrink-0">
    <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="22" y1="2" x2="11" y2="13"/><polygon points="22 2 15 22 11 13 2 9 22 2"/></svg>
  </button>
</div>
```

---

## Logo

```html
<img src="https://assets-prod.claap.io/pubassets/custom/logo_pink_256.svg" alt="Claap" class="h-6" />
```
Sizes: `h-6` (24px) · `h-8` (32px) · `h-10` (40px)
