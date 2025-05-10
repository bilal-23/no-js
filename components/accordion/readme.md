# Accordion Component

A lightweight, accessible accordion component built with CSS and minimal JavaScript for enhanced keyboard accessibility.

## Features

- **CSS-powered core functionality** - Show/hide content works without JavaScript
- **Multiple modes** - Single-active (radio buttons) and multiple-active (checkboxes) versions
- **Accessible** - ARIA attributes, keyboard navigation, and focus management
- **Minimal JavaScript** - Only used for keyboard support and ARIA state updates
- **Smooth animations** - Fade-in and slide effects when toggling content
- **Keyboard support** - Tab, Enter, and Space navigation
- **Responsive design** - Mobile-friendly with appropriate touch targets
- **High contrast support** - Works well in high-contrast modes

## GitHub Repository

This component is part of the NoJS UI Components library:
[https://github.com/bilal-23/no-js/tree/main/components/accordion](https://github.com/bilal-23/no-js/tree/main/components/accordion)

## Implementation

### HTML Structure

#### Single-Active Accordion (Radio Buttons)

```html
<!-- Single active accordion (radio buttons) -->
<div class="accordion-container" role="region" aria-label="Accordion group">
  <div class="accordion-item">
    <label
      class="accordion-header"
      for="accordion-1"
      tabindex="0"
      role="button"
      aria-expanded="false"
      aria-controls="content-accordion-1"
    >
      <p>Accordion Item 1</p>
      <span class="accordion-icon" aria-hidden="true"></span>
    </label>
    <input type="radio" name="accordion" id="accordion-1" />
    <div class="accordion-content" id="content-accordion-1">
      <p>Content for accordion item 1</p>
    </div>
  </div>

  <!-- Additional accordion items -->
  <div class="accordion-item">
    <label
      class="accordion-header"
      for="accordion-2"
      tabindex="0"
      role="button"
      aria-expanded="false"
      aria-controls="content-accordion-2"
    >
      <p>Accordion Item 2</p>
      <span class="accordion-icon" aria-hidden="true"></span>
    </label>
    <input type="radio" name="accordion" id="accordion-2" />
    <div class="accordion-content" id="content-accordion-2">
      <p>Content for accordion item 2</p>
    </div>
  </div>
</div>
```

#### Multiple-Active Accordion (Checkboxes)

```html
<!-- Multiple active accordions (checkboxes) -->
<div class="accordion-container" role="region" aria-label="Multiple accordions">
  <div class="accordion-item">
    <label
      class="accordion-header"
      for="m-accordion-1"
      tabindex="0"
      role="button"
      aria-expanded="false"
      aria-controls="content-m-accordion-1"
    >
      <p>Accordion Item 1</p>
      <span class="accordion-icon" aria-hidden="true"></span>
    </label>
    <input type="checkbox" name="m-accordion" id="m-accordion-1" />
    <div class="accordion-content" id="content-m-accordion-1">
      <p>Content for accordion item 1</p>
    </div>
  </div>

  <!-- Additional accordion items -->
  <div class="accordion-item">
    <label
      class="accordion-header"
      for="m-accordion-2"
      tabindex="0"
      role="button"
      aria-expanded="false"
      aria-controls="content-m-accordion-2"
    >
      <p>Accordion Item 2</p>
      <span class="accordion-icon" aria-hidden="true"></span>
    </label>
    <input type="checkbox" name="m-accordion" id="m-accordion-2" />
    <div class="accordion-content" id="content-m-accordion-2">
      <p>Content for accordion item 2</p>
    </div>
  </div>
</div>
```

### CSS (Key Components)

```css
/* Hide radio buttons and checkboxes but keep them accessible */
input[type="radio"],
input[type="checkbox"] {
  display: none;
}

/* Accordion header styling */
.accordion-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: var(--white);
  padding: 1rem 1.5rem;
  width: 100%;
  cursor: pointer;
  font-weight: 500;
  color: var(--text);
  transition: var(--transition);
  border-bottom: 1px solid var(--light-gray);
  position: relative;
  z-index: 1;
}

/* Accordion content styling */
.accordion-content {
  display: none;
  padding: 0;
  max-height: 0;
  overflow: hidden;
  background-color: var(--white);
  transition: max-height 0.3s ease, padding 0.3s ease;
}

/* Show content when accordion is active */
input[type="radio"]:checked ~ .accordion-content,
input[type="checkbox"]:checked ~ .accordion-content {
  display: block;
  padding: 1.5rem;
  max-height: 300px;
  border-bottom: 1px solid var(--light-gray);
}

/* Style for active accordion header */
input[type="radio"]:checked ~ .accordion-header,
input[type="checkbox"]:checked ~ .accordion-header {
  background-color: rgba(179, 157, 219, 0.1);
  color: var(--primary);
  font-weight: 600;
  border-bottom: none;
}

/* Accordion icon animation */
.accordion-icon::before,
.accordion-icon::after {
  content: "";
  position: absolute;
  background-color: var(--primary);
  transition: transform 0.3s ease;
}

/* Rotate plus to minus when active */
input[type="radio"]:checked ~ .accordion-header .accordion-icon::after,
input[type="checkbox"]:checked ~ .accordion-header .accordion-icon::after {
  transform: rotate(90deg);
  opacity: 0;
}

/* Animation for accordion content */
@keyframes fadeDown {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

input[type="radio"]:checked ~ .accordion-content,
input[type="checkbox"]:checked ~ .accordion-content {
  animation: fadeDown 0.3s ease forwards;
}
```

### JavaScript (Required for Keyboard Accessibility)

```javascript
// This script enables keyboard interaction (Enter/Space) for accordion headers
document.addEventListener("DOMContentLoaded", function () {
  // Get all accordion headers
  const accordionHeaders = document.querySelectorAll(".accordion-header");

  // Add keyboard event listeners to all accordion headers
  accordionHeaders.forEach((header) => {
    // Handle keyboard events (Enter and Space)
    header.addEventListener("keydown", function (e) {
      // Trigger the accordion when Enter or Space is pressed
      if (e.key === "Enter" || e.key === " ") {
        e.preventDefault(); // Prevent scrolling on Space

        // Simulate a click on the header
        this.click();

        // Get the associated input
        const inputId = this.getAttribute("for");
        const input = document.getElementById(inputId);

        // Update aria-expanded state
        const isExpanded = input.checked;
        this.setAttribute("aria-expanded", isExpanded ? "true" : "false");
      }
    });

    // Also update aria-expanded when clicked with mouse
    header.addEventListener("click", function () {
      // Get the associated input
      const inputId = this.getAttribute("for");
      const input = document.getElementById(inputId);

      // We need to wait a moment (10ms) for the checked state to update
      // This tiny delay ensures the browser has updated the input's checked state before we read it
      setTimeout(() => {
        const isExpanded = input.checked;
        this.setAttribute("aria-expanded", isExpanded ? "true" : "false");
      }, 10);
    });
  });
});
```

## Accessibility Features

### ARIA Attributes

| Attribute            | Purpose                                               |
| -------------------- | ----------------------------------------------------- |
| `role="region"`      | Identifies the container as a landmark region         |
| `aria-label`         | Provides a label for the accordion group              |
| `role="button"`      | Indicates that the header acts as a button            |
| `aria-expanded`      | Indicates whether the accordion panel is expanded     |
| `aria-controls`      | Associates the header with the panel it controls      |
| `aria-hidden="true"` | Applied to decorative elements that should be ignored |
| `tabindex="0"`       | Makes the accordion headers keyboard focusable        |

### Keyboard Support

- **Tab key**: Moves focus between accordion headers
- **Enter/Space**: Toggles the accordion panel when focus is on a header

### Why JavaScript is Necessary

While we've minimized JavaScript usage, there are key limitations with a CSS-only approach:

1. **Keyboard interaction**: CSS cannot detect keyboard events like Enter or Space. Only JavaScript can listen for these events and trigger the accordion toggle.
2. **ARIA state updates**: CSS cannot dynamically update ARIA attributes like `aria-expanded` when the accordion state changes.
3. **Non-native elements**: Our accordion uses `<label>` elements, which don't have built-in keyboard interaction like true `<button>` elements would.

The JavaScript is minimal (only ~30 lines) and focuses solely on:

- Handling keyboard events (Enter/Space) to toggle accordions
- Updating the `aria-expanded` attribute to match the accordion's state

Without this JavaScript, the accordion would still work for mouse users but would fail accessibility requirements for keyboard-only users.

## How It Works

The accordion component is built primarily using HTML and CSS, with minimal JavaScript added only for keyboard accessibility:

1. **Hidden Input Controls**: The implementation relies on hidden radio inputs (for single-active accordions) and checkboxes (for multiple-active accordions) to control the state.

2. **CSS-Powered Toggle**: The CSS `:checked` selector and general sibling combinator (`~`) handle the showing/hiding of content based on the state of the radio buttons or checkboxes.

3. **Visual Feedback**: When an accordion is toggled, CSS transitions and animations provide smooth visual feedback.

4. **Accessibility Layer**: JavaScript adds keyboard support (Enter/Space) and ensures ARIA states are properly updated for screen readers.

The use of radio buttons for single-active mode ensures that only one panel can be open at a time (as radio buttons in the same group are mutually exclusive). For the multiple-active version, checkboxes allow any number of panels to be open simultaneously.

## Accessibility Considerations

- The accordion headers have appropriate focus states for keyboard users
- ARIA attributes provide semantic information to screen readers
- The JavaScript ensures keyboard users can toggle accordions with Enter or Space
- The `aria-expanded` state is dynamically updated to reflect the current state
- Decorative elements have `aria-hidden="true"` to avoid unnecessary announcements

## Browser Support

The accordion component works in all modern browsers that support:

- CSS3 selectors (especially the general sibling combinator `~`)
- CSS transitions and animations
- Basic JavaScript (ES5+)

For older browsers, consider:

1. Adding appropriate fallbacks for CSS transitions
2. Using a simple polyfill for ES6 features if needed

## Responsive Design

The accordion is mobile-friendly with:

- Appropriate touch target sizes (at least 44×44px for interactive elements)
- Responsive layout that adjusts to screen width
- Optimized typography for small screens
- High-contrast support for accessibility

## Additional Resources

- [WAI-ARIA: Accordion Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/accordion/)
- [MDN Web Docs: ARIA Button Role](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/button_role)
- [WCAG 2.1 Accessibility Guidelines](https://www.w3.org/TR/WCAG21/)
