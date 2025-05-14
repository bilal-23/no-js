# Tooltip Component

A lightweight, accessible tooltip component built entirely with CSS and HTML, requiring no JavaScript.

## Features

- **Pure CSS implementation** - No JavaScript required
- **Accessible** - ARIA attributes and semantic HTML
- **Multiple positions** - Top, right, bottom, and left positioning
- **Keyboard support** - Tooltips appear on focus for keyboard users
- **Form field integration** - Works well with form elements
- **High contrast** - Meets WCAG contrast requirements
- **Customizable** - Easy to style with CSS variables

## GitHub Repository

This component is part of the NoJS UI Components library:
[https://github.com/bilal-23/no-js/tree/main/components/tooltip](https://github.com/bilal-23/no-js/tree/main/components/tooltip)

## Implementation

### HTML Structure

```html
<!-- Basic Tooltip -->
<div class="tooltip">
  <button class="tooltip-trigger" aria-describedby="tooltip-1" tabindex="0">
    Hover me
  </button>
  <span class="tooltiptext" id="tooltip-1" role="tooltip">
    This is a tooltip that appears on top
  </span>
</div>

<!-- Right Positioned Tooltip -->
<div class="tooltip position-right">
  <button class="tooltip-trigger" aria-describedby="tooltip-2" tabindex="0">
    Right tooltip
  </button>
  <span class="tooltiptext" id="tooltip-2" role="tooltip">
    This tooltip appears on the right side
  </span>
</div>

<!-- Left Positioned Tooltip -->
<div class="tooltip position-left">
  <button class="tooltip-trigger" aria-describedby="tooltip-3" tabindex="0">
    Left tooltip
  </button>
  <span class="tooltiptext" id="tooltip-3" role="tooltip">
    This tooltip appears on the left side
  </span>
</div>

<!-- Bottom Positioned Tooltip -->
<div class="tooltip position-bottom">
  <button class="tooltip-trigger" aria-describedby="tooltip-4" tabindex="0">
    Bottom tooltip
  </button>
  <span class="tooltiptext" id="tooltip-4" role="tooltip">
    This tooltip appears at the bottom
  </span>
</div>
```

### For Form Fields

To integrate tooltips with form fields:

```html
<div class="form-field">
  <label for="username">Username</label>
  <div class="input-with-tooltip">
    <input
      type="text"
      id="username"
      name="username"
      aria-describedby="username-tooltip"
    />
    <div class="tooltip tooltip-help">
      <button
        class="tooltip-trigger tooltip-icon"
        aria-describedby="username-tooltip"
        tabindex="0"
        aria-label="Help for username field"
      >
        ?
      </button>
      <span class="tooltiptext" id="username-tooltip" role="tooltip">
        Username must be 3-20 characters and contain only letters, numbers, and
        underscores.
      </span>
    </div>
  </div>
</div>
```

### CSS (Key Components)

```css
/* Tooltip Container */
.tooltip {
  position: relative;
  display: inline-block;
  margin: 2rem;
}

/* Tooltip Trigger - using button for better semantics */
.tooltip-trigger {
  background-color: var(--primary-color, #0077cc);
  color: white;
  padding: 0.75rem 1.5rem;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 500;
  display: inline-block;
  transition: background-color 0.3s ease;
  border: none;
  font-family: inherit;
  font-size: 1rem;
}

/* Tooltip Text */
.tooltiptext {
  visibility: hidden;
  width: 200px;
  background-color: #333;
  color: #fff;
  text-align: center;
  border-radius: 6px;
  padding: 10px;
  position: absolute;
  z-index: 1;
  bottom: 125%;
  left: 50%;
  transform: translateX(-50%);
  opacity: 0;
  transition: opacity 0.3s, visibility 0.3s;
  pointer-events: none; /* Prevents tooltip from blocking interactions */
}

/* Show the tooltip on hover AND focus for accessibility */
.tooltip:hover .tooltiptext,
.tooltip-trigger:focus + .tooltiptext {
  visibility: visible;
  opacity: 1;
}

/* Accessible focus indicator - high contrast */
.tooltip-trigger:focus-visible {
  outline: 2px solid #fff;
  outline-offset: 2px;
  box-shadow: 0 0 0 4px var(--primary-color, #0077cc);
}

/* Different tooltip positions */
.tooltip.position-right .tooltiptext {
  top: 50%;
  left: 110%;
  bottom: auto;
  transform: translateY(-50%);
}

.tooltip.position-left .tooltiptext {
  top: 50%;
  right: 110%;
  left: auto;
  bottom: auto;
  transform: translateY(-50%);
}

.tooltip.position-bottom .tooltiptext {
  top: 125%;
  bottom: auto;
  left: 50%;
  transform: translateX(-50%);
}
```

## Accessibility Features

### ARIA Attributes

| Attribute                       | Purpose                                                   |
| ------------------------------- | --------------------------------------------------------- |
| `aria-describedby="tooltip-id"` | Associates the trigger element with its tooltip content   |
| `role="tooltip"`                | Identifies the tooltip element for assistive technologies |
| `aria-label`                    | Provides accessible names for icon buttons (e.g., ?)      |
| `tabindex="0"`                  | Makes tooltip triggers keyboard focusable                 |

### Focus Management

The implementation includes:

- Tooltips that appear on `:focus` as well as `:hover`
- High-contrast focus indicators that meet WCAG guidelines
- Keyboard focusable elements with proper tabindex values
- Appropriate semantic elements (using `<button>` elements for interactive triggers)

### Color and Contrast

- Text in tooltips uses high contrast (light text on dark background)
- Focus states have distinct visual indicators
- Colors can be customized through CSS variables to maintain proper contrast

## How It Works

This tooltip component is built using only HTML and CSS, with no JavaScript required. It demonstrates how to create accessible, interactive UI components using CSS pseudo-classes and positioning.

The key aspects of the implementation are:

1. **CSS Visibility Toggle**: The tooltip content is hidden by default and shown using the `:hover` and `:focus` pseudo-classes
2. **Positioning**: Different tooltip positions are achieved through CSS absolute positioning and transforms
3. **Transitions**: Smooth fade-in and fade-out effects use CSS transitions
4. **Focus Management**: Keyboard users can access tooltips through proper focus management
5. **Semantic HTML**: Using proper HTML elements for accessibility (buttons for triggers, unique IDs for labels)

### CSS-Only Implementation Benefits

Using pure CSS for tooltips provides several advantages:

- **Performance**: No JavaScript overhead or execution time
- **Accessibility**: Native browser behavior for keyboard users
- **Reliability**: Works without JavaScript enabled
- **Simplicity**: Fewer moving parts, less to maintain
- **Progressive Enhancement**: Works on almost all browsers

## What Not To Add

❌ **Don't add these to your tooltips component:**

- **JavaScript for core functionality**: The tooltips work perfectly well without it
- **title attributes**: These are less accessible and have inconsistent browser support
- **Additional wrappers**: Keep the HTML structure minimal for performance
- **Focus traps**: Tooltips should not trap keyboard focus, as they're supplemental information
- **Click-to-open behavior**: Tooltips should appear on hover/focus and disappear when hover/focus is removed

## Additional Resources

- [WAI-ARIA: Tooltip Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tooltip/)
- [MDN Web Docs: ARIA Tooltip Role](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/tooltip_role)
- [WCAG 2.1 Accessibility Guidelines](https://www.w3.org/TR/WCAG21/)
