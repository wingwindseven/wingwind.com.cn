# WingWind Static Site – Comprehensive Documentation

## Table of Contents
1. Introduction
2. Getting Started
3. Site Architecture
4. Public Components
5. JavaScript Utilities & Functions
6. Customisation Guide
7. Contributing
8. License

---

## 1. Introduction
This documentation describes **WingWind**’s single-page marketing site contained in `wingwind.com.cn.html`. It covers the purpose and structure of every **public component** (HTML/CSS) and **runtime behaviour** implemented in the embedded JavaScript.  Concrete usage examples are provided so that designers, developers, and content editors can quickly adapt or extend the project.

## 2. Getting Started

1. **Clone or download** the repository.
2. Open `wingwind.com.cn.html` directly in any modern browser – no additional build-step is required.
3. Optionally host it from any HTTP server (e.g. `python3 -m http.server`).

```bash
# Example – launch local preview on port 8000
cd /path/to/repo
python3 -m http.server 8000
# Visit http://localhost:8000/wingwind.com.cn.html
```

> 💡  Because all assets (styles, fonts, icons) are loaded from public CDNs the page will work out-of-the-box as long as an Internet connection is available.

## 3. Site Architecture

```text
wingwind.com.cn.html
├─ <head>  – metadata, fonts, icon & global <style>
└─ <body>
   ├─ .navbar            – Sticky navigation bar
   ├─ #zh-version        – Chinese content wrapper
   │   ├─ #home          – Hero section (zh-CN)
   │   ├─ #products      – Products grid (zh-CN)
   │   ├─ #applications  – Use-case tabs (zh-CN)
   │   └─ #about / #contact – Company info & footer (zh-CN)
   ├─ #en-version        – English content wrapper (mirrors zh)
   └─ <script>           – Behaviour layer (see §5)
```

Two **language containers** (`#zh-version`, `#en-version`) coexist in the DOM; the script toggles their visibility so SEO crawlers can still index both variants.

## 4. Public Components

Below is a quick reference of reusable HTML fragments. Feel free to copy-paste them elsewhere or transform them into framework components (React/Vue/Svelte) as required.

### 4.1 Navbar (`.navbar`)
Sticky bar containing logo, anchor links and the **language switcher**.

Example markup:
```html
<header class="navbar">
  <div class="nav-container">
    <a class="logo" href="#home"><i class="fas fa-paper-plane"></i><span>WingWind</span></a>
    <ul class="nav-links">
      <li><a href="#products">Products</a></li>
      <li><a href="#applications">Applications</a></li>
      <li><a href="#about">About</a></li>
      <li><a href="#contact">Contact</a></li>
    </ul>
    <div class="lang-switcher">…</div>
  </div>
</header>
```

Behaviour: shrinks and adds drop-shadow on scroll (§5.2).

### 4.2 Hero (`.hero`)
Fullscreen splash with headline, subtitle and CTA button.

```html
<section class="hero" id="home">
  <div class="hero-content container">
    <h1><span>WingWind</span> – Intelligent Drone Solutions</h1>
    <p>Revolutionising aerial technology …</p>
    <a class="btn" href="#products">Explore Products</a>
  </div>
</section>
```

The `background` is an angled gradient with subtle radial overlays declared in root CSS.

### 4.3 Product Card (`.product-card`)

```html
<div class="product-card">
  <i class="fas fa-helicopter drone-icon"></i>
  <h3>WW-X1</h3>
  <p class="product-desc">Payload 15 kg • 50 km range</p>
  <a href="docs/WW-X1.pdf" class="btn">Download Brochure</a>
</div>
```
Hovering the card triggers a subtle icon animation (§5.5).

### 4.4 Applications Tabs (`.applications`)
Shows four use cases.  Clicking a **tab** swaps the descriptive text in-place (§5.3).

```html
<section class="applications" id="applications">
  <div class="tabs">
     <span class="app-tab active">Logistics</span>
     …
  </div>
  <div class="app-text">…</div>
</section>
```

### 4.5 Footer (`.footer`)
Contains address, quick links and social icons.

---

## 5. JavaScript Utilities & Functions
All logic is embedded at the bottom of `wingwind.com.cn.html` so no separate bundle is necessary.  The code is *vanilla* ES2015+ and free of external runtime dependencies.

| # | Purpose | Implementation | Usage Example |
|---|---------|----------------|---------------|
| 5.1 | **Language Switcher** – toggles between Chinese and English versions | `langButtons.forEach(button => { … })` | User clicks the `lang-btn`; internally, the handler reads `data-lang` ("zh" / "en"), removes/sets `.active` on **buttons** and **content wrappers**. |
| 5.2 | **Navbar Scroll Effect** – compress navbar after 50 px | `window.addEventListener('scroll', …)` adds or removes `.scrolled` class. | Simply scroll the page; CSS animates padding & shadow. |
| 5.3 | **Applications Tabs** – dynamic text per tab & language | `tabs.forEach(tab => { tab.addEventListener('click', …) })` | Click any `.app-tab` (e.g. "Surveying"); the script injects the corresponding HTML snippet into `.app-text`. |
| 5.4 | **Smooth Anchor Scrolling** | Intercepts anchor clicks: `document.querySelectorAll('a[href^="#"]').forEach(anchor => { … })` | Click a navbar link; `scrollIntoView({behavior:'smooth'})` animates the jump. |
| 5.5 | **Product Card Hover Animation** | mouseenter / mouseleave listeners on `.product-card` | Hover over a product to scale & rotate `.drone-icon`. |

### Code Snippet – Language Switcher (5.1)
```js
const langButtons = document.querySelectorAll('.lang-btn');
langButtons.forEach(button => {
  button.addEventListener('click', () => {
    langButtons.forEach(btn => btn.classList.remove('active'));
    button.classList.add('active');

    const targetLang = button.dataset.lang; // "zh" or "en"
    document.querySelectorAll('.language-version').forEach(version => {
      version.classList.remove('active');
    });
    document.getElementById(`${targetLang}-version`).classList.add('active');
  });
});
```

> **Extending**: If you add a third language, simply append another `.lang-btn` with `data-lang="xx"` and duplicate the corresponding content wrapper `#xx-version`.

---

## 6. Customisation Guide

1. **Colours & Typography** – global variables reside in `:root` inside `<style>`.
2. **Icons** – powered by Font Awesome 6.4 CDN. Change the `<i class="fas fa-*">` classes accordingly.
3. **Products / Applications** – duplicate a `.product-card` or extend the switch-case blocks in §5.3.
4. **Performance** – Consider extracting the CSS & JS into separate files and adding `defer` to the `<script>` tag for faster first paint.

---

## 7. Contributing

Pull requests are welcome!  Please ensure any submitted HTML validates via the W3C validator and that JavaScript is linted with ESLint (Airbnb preset).

---

## 8. License

Unless stated otherwise, this project is released under the **MIT License**.