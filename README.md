# NavigationMenu

A sleek, animated bottom/top navigation bar built with pure HTML and CSS — no JavaScript required. Features a smooth sliding indicator that highlights the active tab using only CSS radio inputs and the `~` sibling selector.

---

## Table of Contents

- [Demo](#demo)
- [Features](#features)
- [File Structure](#file-structure)
- [How It Works](#how-it-works)
- [Customization](#customization)
- [Responsive Behavior](#responsive-behavior)
- [Browser Support](#browser-support)
- [Dependencies](#dependencies)
- [Known Limitations](#known-limitations)
- [License](#license)

---

## Demo

Open `NavigationMenu.html` directly in any modern browser — no build step or server required.

---

## Features

- ✅ **Zero JavaScript** — state management is handled entirely with CSS and HTML radio inputs
- ✅ **Animated sliding indicator** — smooth cubic-bezier transition between tabs
- ✅ **5 navigation items** — Home, Category, Cart, Wishlist, Account
- ✅ **Font Awesome icons** — each tab includes a recognizable icon
- ✅ **Responsive design** — adapts to mobile screens via media queries
- ✅ **Accessible structure** — uses `<label for>` associations with hidden radio inputs
- ✅ **Lightweight** — single HTML file + single CSS file, no frameworks

---

## File Structure

```
NavigationMenu/
├── NavigationMenu.html   # Markup: radio inputs, nav labels, Font Awesome icons
└── NavigationMenu.css    # All styles, animations, and responsive rules
```

---

## How It Works

### State Management via Radio Inputs

The active tab is tracked using hidden `<input type="radio">` elements placed **before** the `<nav>` in the DOM. Each radio input has a unique `id` matching the `for` attribute of its corresponding `<label>`:

```html
<input type="radio" name="slider" id="home" checked>
<input type="radio" name="slider" id="categories">
<!-- ... -->
<nav>
  <label for="home" class="home">...</label>
  <label for="categories" class="categories">...</label>
</nav>
```

Because all inputs share `name="slider"`, only one can be checked at a time — exactly like a tab group.

### CSS General Sibling Selector (`~`)

The CSS uses `~` (general sibling combinator) to reach into the `<nav>` from a checked radio input and apply active styles:

```css
/* Turn the active label text white */
#home:checked ~ nav label.home {
    color: #fff;
}

/* Move the slider to the correct position */
#categories:checked ~ nav .slider {
    left: 20%;
}
```

This works because the radio inputs are **siblings** of `<nav>` in the DOM — they share the same `.container` parent.

### Sliding Indicator

The `.slider` div is absolutely positioned inside `<nav>` and takes up exactly `20%` of the nav width (one fifth for five tabs). Its `left` property shifts by increments of `20%` for each tab:

| Tab        | `left` value |
|------------|-------------|
| Home       | `0%`         |
| Category   | `20%`        |
| Cart       | `40%`        |
| Wishlist   | `60%`        |
| Account    | `80%`        |

The transition uses a cubic-bezier curve for a satisfying elastic bounce:

```css
transition: all 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
```

---

## Customization

### Change the Accent Color

The purple `#8e44ad` appears in three places in `NavigationMenu.css`. Replace all three to retheme the component:

```css
/* Slider background */
.slider {
    background: #YOUR_COLOR;
}

/* Label hover tint */
nav label:hover {
    background: rgba(YOUR_R, YOUR_G, YOUR_B, 0.3);
}

/* Default label color */
nav label {
    color: #YOUR_COLOR;
}
```

### Add or Remove Tabs

1. Add a new radio input in the HTML:
   ```html
   <input type="radio" name="slider" id="notifications">
   ```
2. Add the corresponding label inside `<nav>`:
   ```html
   <label for="notifications" class="notifications">
     <i class="fa-solid fa-bell"></i>Notifications
   </label>
   ```
3. Update the slider width and positions in CSS. For 6 tabs, each tab is `16.666%`:
   ```css
   .slider { width: 16.666%; }
   #notifications:checked ~ nav .slider { left: 83.333%; }
   ```
4. Add the active color rule:
   ```css
   #notifications:checked ~ nav label.notifications { color: #fff; }
   ```

### Change Tab Icons

Icons come from [Font Awesome 7](https://fontawesome.com/icons). Replace the `class` on any `<i>` tag:

```html
<!-- Before -->
<i class="fas fa-home"></i>

<!-- After (example) -->
<i class="fa-solid fa-house-chimney"></i>
```

### Adjust the Nav Size

```css
nav {
    width: 600px;   /* Total width */
    height: 60px;   /* Bar height */
}
```

`line-height` on `nav label` should match `height` to keep text vertically centered.

---

## Responsive Behavior

Two breakpoints are defined:

| Breakpoint      | Behavior                                                      |
|-----------------|---------------------------------------------------------------|
| `≤ 700px`       | Nav stretches to `95vw`; font size reduced to `16px`         |
| `≤ 500px`       | Nav fills full viewport width (`100vw`); height drops to `50px`; border-radius removed for an edge-to-edge feel |

---

## Browser Support

| Browser         | Support |
|-----------------|---------|
| Chrome / Edge   | ✅ Full  |
| Firefox         | ✅ Full  |
| Safari (iOS 14+)| ✅ Full  |
| Opera           | ✅ Full  |
| IE 11           | ❌ No (CSS variables & cubic-bezier not fully supported) |

---

## Dependencies

All dependencies are loaded from CDN — no installation needed.

| Dependency      | Version | Purpose           | CDN URL |
|-----------------|---------|-------------------|---------|
| Font Awesome    | 7.0.0   | Tab icons         | `https://cdnjs.cloudflare.com/ajax/libs/font-awesome/7.0.0/css/all.min.css` |
| Poppins (Google Fonts) | —  | Typography (referenced in CSS but not explicitly imported — add a `<link>` to Google Fonts if needed) | `https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600&display=swap` |

> **Note:** The CSS references `font-family: "Poppins"` but the HTML does not include a Google Fonts `<link>`. Add the following inside `<head>` to ensure Poppins loads correctly:
> ```html
> <link rel="preconnect" href="https://fonts.googleapis.com">
> <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600&display=swap" rel="stylesheet">
> ```

---

## Known Limitations

- **No JavaScript hooks** — there is no event you can listen to for tab changes. To trigger JS on tab switch, you would need to add `change` event listeners on the radio inputs.
- **Not a router** — this component only manages visual state. Integrating it with page navigation or a SPA router requires additional JavaScript.
- **Radio input visibility** — the radio inputs are visually hidden via their position in the DOM and CSS, but they are not explicitly set to `display: none` or `visibility: hidden`. Screen readers may announce them. For better accessibility, add `aria-hidden="true"` to the inputs and appropriate `aria-label` / `role="tab"` attributes to the labels.
- **Fixed number of tabs** — the `20%` width math assumes exactly 5 tabs. Adding or removing tabs requires manual CSS updates (see [Customization](#customization)).

---

## License

This project is provided as-is for educational and personal use.
