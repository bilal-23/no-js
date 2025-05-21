# Toggle Switch Component

A lightweight, accessible toggle switch component built with HTML, CSS, and minimal JavaScript for enhanced keyboard accessibility and screen reader support.

## Features

- **Semantic HTML** - Uses native checkbox inputs for proper form submission
- **Dual State Management** - Combines native `checked` attribute with `aria-checked` for maximum compatibility
- **Accessible** - ARIA attributes, keyboard navigation, and screen reader support
- **Minimal JavaScript** - Only used for keyboard support and ARIA state updates
- **Smooth animations** - Smooth transitions for toggle state changes
- **Keyboard support** - Tab, Enter, and Space navigation
- **Responsive design** - Mobile-friendly with appropriate touch targets
- **High contrast support** - Works well in high-contrast modes

## GitHub Repository

This component is part of the NoJS UI Components library:
[https://github.com/bilal-23/no-js/tree/main/components/toggle-switch](https://github.com/bilal-23/no-js/tree/main/components/toggle-switch)

## Implementation

### HTML Structure

```html
<!-- Basic Toggle Switch -->
<div class="toggle-switch">
  <input
    type="checkbox"
    id="switch1"
    class="toggle-input"
    aria-describedby="switch1-desc"
  />
  <label
    for="switch1"
    class="toggle-label"
    role="switch"
    aria-checked="false"
    tabindex="0"
  >
    <span class="toggle-button"></span>
    <span class="sr-only">Toggle switch</span>
  </label>
  <span id="switch1-desc" class="sr-only">Toggle switch description</span>
</div>

<!-- Group of Toggle Switches -->
<div class="toggle-container" role="group" aria-labelledby="toggles-label">
  <span id="toggles-label" class="sr-only">Group of toggle switches</span>
  <!-- Toggle switches here -->
</div>
```

### CSS (Key Components)

```css
/* Toggle Switch Container */
.toggle-switch {
  position: relative;
  display: inline-block;
}

/* Hide the default checkbox */
.toggle-input {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}

/* The switch track (background) */
.toggle-label {
  display: block;
  width: 3.5rem;
  height: 1.75rem;
  border-radius: 1.75rem;
  background-color: #ccc;
  cursor: pointer;
  transition: background-color 0.3s;
  position: relative;
}

/* The switch button (the circle) */
.toggle-button {
  position: absolute;
  top: 0.25rem;
  left: 0.25rem;
  width: 1.25rem;
  height: 1.25rem;
  border-radius: 50%;
  background-color: white;
  transition: transform 0.3s;
}

/* When checked - change track color */
.toggle-input:checked + .toggle-label,
.toggle-label[aria-checked="true"] {
  background-color: var(--primary-color, #0077cc);
}

/* When checked - move the button */
.toggle-input:checked + .toggle-label .toggle-button,
.toggle-label[aria-checked="true"] .toggle-button {
  transform: translateX(1.75rem);
}

/* Focus styles for accessibility */
.toggle-input:focus-visible + .toggle-label,
.toggle-label:focus-visible {
  outline: 2px solid var(--primary-color, #0077cc);
  outline-offset: 2px;
}
```

### JavaScript (Required for Keyboard Accessibility)

```javascript
document.addEventListener("DOMContentLoaded", function () {
  const toggleLabels = document.querySelectorAll('label[role="switch"]');

  toggleLabels.forEach((label) => {
    // Handle click events
    label.addEventListener("click", function (e) {
      e.preventDefault();
      const input = document.getElementById(this.getAttribute("for"));
      if (input) {
        input.checked = !input.checked;
        // Update aria-checked to match the checked state
        this.setAttribute("aria-checked", input.checked);
      }
    });

    // Handle keyboard events (Space and Enter)
    label.addEventListener("keydown", function (e) {
      if (e.key === " " || e.key === "Enter") {
        e.preventDefault();
        const input = document.getElementById(this.getAttribute("for"));
        if (input) {
          input.checked = !input.checked;
          // Update aria-checked to match the checked state
          this.setAttribute("aria-checked", input.checked);
        }
      }
    });
  });
});
```

## Accessibility Features

### ARIA Attributes

The toggle switch component implements several ARIA attributes to ensure proper accessibility:

1. **`role="switch"`**

   - Applied to the label element
   - Indicates that the component functions as a switch control
   - Helps screen readers understand the component's purpose and behavior

2. **`aria-checked`**

   - Used in conjunction with `role="switch"`
   - Reflects the current state of the toggle switch
   - Updated dynamically via JavaScript when the state changes

3. **`aria-describedby`**

   - Associates descriptive text with each toggle switch
   - Provides additional context for screen reader users
   - Links to a hidden span containing the description

4. **`tabindex="0"`**

   - Added to the label element
   - Ensures the toggle is focusable
   - Enables keyboard navigation

5. **`role="group"`**

   - Applied to the container of multiple toggle switches
   - Indicates that the toggles form a logical group
   - Helps screen readers understand the relationship between multiple toggles

6. **`aria-labelledby`**
   - Used on the group container
   - Associates the group with a descriptive heading
   - Provides context for screen reader users

### Keyboard Navigation

- **Tab**: Move focus between toggle switches
- **Space/Enter**: Toggle the switch on/off when focused
- **Focus Indicators**: Clear visual focus styles for keyboard users

### Screen Reader Support

- Proper announcement of toggle state changes
- Clear descriptions of toggle purpose
- Logical grouping of related toggles
- Dual state management for maximum compatibility

## Browser Support

The toggle switch component works in all modern browsers that support:

- CSS transitions
- CSS transforms
- ARIA attributes
- JavaScript (for keyboard support)

## Dependencies

- No external dependencies required
- Uses CSS custom properties for theming
- Minimal JavaScript for accessibility features

## Usage Notes

1. **State Management**

   - The component uses both native `checked` attribute and `aria-checked`
   - JavaScript ensures both states stay synchronized
   - This dual approach ensures maximum compatibility

2. **Styling**

   - Customizable through CSS variables
   - Maintains accessibility in high-contrast modes
   - Responsive design with appropriate touch targets

3. **Accessibility**
   - Follows WAI-ARIA switch pattern
   - Supports keyboard navigation
   - Provides clear focus indicators
   - Includes screen reader support

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This component is part of the NoJS UI Components library and is available under the MIT License.
