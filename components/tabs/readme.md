# Tabs Component

A lightweight, accessible tabs component built primarily with CSS and minimal JavaScript for enhanced accessibility.

## Features

- **CSS-powered functionality** - Core tab switching works without JavaScript
- **Accessible** - ARIA attributes, keyboard navigation and focus management
- **Minimal JavaScript** - Only used for accessibility enhancements
- **Smooth transitions** - Fade-in effect when switching tabs
- **Keyboard support** - Arrow keys, Tab, Enter and Space navigation
- **Proper focus management** - Focus is maintained within the component
- **Orientation support** - Works in both horizontal and vertical layouts

## GitHub Repository

This component is part of the NoJS UI Components library:
[https://github.com/bilal-23/no-js/tree/main/components/tabs](https://github.com/bilal-23/no-js/tree/main/components/tabs)

## Implementation

### HTML Structure

```html
<!-- Tab container with radio inputs at the top level -->
<div class="tab-container">
  <input
    type="radio"
    id="tab-1"
    name="tab"
    data-tab="tab-1"
    checked
    aria-controls="panel-1"
  />
  <input
    type="radio"
    id="tab-2"
    name="tab"
    data-tab="tab-2"
    aria-controls="panel-2"
  />

  <!-- Tab header with tab buttons as labels -->
  <div class="tab-header" role="tablist" aria-orientation="horizontal">
    <label
      for="tab-1"
      class="tab-button"
      role="tab"
      tabindex="0"
      id="label-tab-1"
      aria-selected="true"
      >Tab 1</label
    >
    <label
      for="tab-2"
      class="tab-button"
      role="tab"
      tabindex="0"
      id="label-tab-2"
      aria-selected="false"
      >Tab 2</label
    >
  </div>

  <!-- Tab content panels -->
  <div class="tabs-content">
    <div
      class="tab-content"
      id="panel-1"
      role="tabpanel"
      aria-labelledby="label-tab-1"
      tabindex="0"
      data-tab="tab-1"
    >
      <h2>Tab 1</h2>
      <p>This is the content for Tab 1.</p>
    </div>
    <div
      class="tab-content"
      id="panel-2"
      role="tabpanel"
      aria-labelledby="label-tab-2"
      tabindex="0"
      data-tab="tab-2"
    >
      <h2>Tab 2</h2>
      <p>This is the content for Tab 2.</p>
    </div>
  </div>
</div>
```

### For Vertical Orientation

To create a vertical tab layout, simply add the `tabs-vertical` class to the container and change the `aria-orientation` attribute:

```html
<div class="tab-container tabs-vertical">
  <!-- Radio inputs remain the same -->

  <div class="tab-header" role="tablist" aria-orientation="vertical">
    <!-- Tab labels remain the same -->
  </div>

  <div class="tabs-content">
    <!-- Tab content panels remain the same -->
  </div>
</div>
```

### CSS (Key Components)

```css
/* Hide radio buttons but keep them accessible */
input[type="radio"] {
  display: none;
}

/* Style for tab buttons */
.tab-button {
  background-color: var(--primary);
  color: #fff;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  width: 100%;
  text-align: center;
}

/* Tab header layout - horizontal (default) */
.tab-header {
  display: flex;
  gap: 1rem;
  padding: 1rem;
  width: 100%;
}

/* Vertical orientation styles */
.tabs-vertical {
  display: flex;
  gap: 1rem;
}

.tabs-vertical .tab-header {
  flex-direction: column;
  width: 200px; /* Adjust as needed */
}

.tabs-vertical .tabs-content {
  flex: 1;
}

/* Hide all tab content by default */
.tab-content {
  display: none;
  animation: fadeIn 0.3s ease-in-out;
}

/* Key CSS: Show content for active tab using sibling selector */
#tab-1:checked ~ .tabs-content .tab-content[data-tab="tab-1"],
#tab-2:checked ~ .tabs-content .tab-content[data-tab="tab-2"] {
  display: block;
}

/* Key CSS: Highlight active tab label */
#tab-1:checked ~ .tab-header label[for="tab-1"],
#tab-2:checked ~ .tab-header label[for="tab-2"] {
  background-color: var(--secondary, #0056b3);
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
  font-weight: bold;
}

/* Fade-in animation for tab content */
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
```

### JavaScript (For Accessibility Only)

```javascript
document.addEventListener("DOMContentLoaded", function () {
  const tabButtons = document.querySelectorAll(".tab-button");
  const tablists = document.querySelectorAll("[role='tablist']");

  // Add keyboard navigation with arrow keys
  tablists.forEach((tablist) => {
    const isVertical = tablist.getAttribute("aria-orientation") === "vertical";
    const tabs = Array.from(tablist.querySelectorAll("[role='tab']"));

    tabs.forEach((tab) => {
      tab.addEventListener("keydown", function (e) {
        // Determine which keys to use based on orientation
        const nextKey = isVertical ? "ArrowDown" : "ArrowRight";
        const prevKey = isVertical ? "ArrowUp" : "ArrowLeft";

        // Handle arrow keys
        if (e.key === nextKey || e.key === prevKey) {
          e.preventDefault();

          const currentIndex = tabs.indexOf(this);
          let newIndex;

          if (e.key === nextKey) {
            newIndex = currentIndex < tabs.length - 1 ? currentIndex + 1 : 0;
          } else {
            newIndex = currentIndex > 0 ? currentIndex - 1 : tabs.length - 1;
          }

          // Activate new tab and move focus
          tabs[newIndex].click();
          tabs[newIndex].focus();
        }

        // Handle Enter or Space
        if (e.key === "Enter" || e.key === " ") {
          e.preventDefault();
          this.click();
        }
      });
    });
  });

  // Update ARIA attributes when tabs change
  const radioButtons = document.querySelectorAll(
    'input[type="radio"][name="tab"]'
  );
  radioButtons.forEach((radio) => {
    radio.addEventListener("change", function () {
      tabButtons.forEach((tab) => {
        const isSelected = tab.getAttribute("for") === this.id;
        tab.setAttribute("aria-selected", isSelected ? "true" : "false");
      });
    });
  });
});
```

## Accessibility Features

### ARIA Attributes

| Attribute                                | Purpose                                           |
| ---------------------------------------- | ------------------------------------------------- |
| `role="tablist"`                         | Identifies the container as a collection of tabs  |
| `role="tab"`                             | Identifies each tab button/trigger                |
| `role="tabpanel"`                        | Identifies each tab content panel                 |
| `aria-orientation="horizontal/vertical"` | Indicates the layout direction of tabs            |
| `aria-selected="true/false"`             | Indicates which tab is currently selected         |
| `aria-controls="panel-id"`               | Associates each tab with the panel it controls    |
| `aria-labelledby="tab-id"`               | Associates each panel with the tab that labels it |
| `tabindex="0"`                           | Makes tabs keyboard focusable                     |

### What Not To Add

❌ **Don't add these to your tabs component:**

- **`role="button"`** on tab elements: Not needed and conflicts with `role="tab"`.

- **`aria-hidden="true"`** on inactive tab panels: The CSS hiding is sufficient, and using aria-hidden would remove them from the accessibility tree.

- **`tabindex="-1"`** on inactive tab elements: All tabs should remain in the tab order for proper keyboard navigation.

- **Complex JavaScript** for core functionality: CSS can handle the basic tab switching.

### Keyboard Navigation

- **Tab key**: Moves focus to the tabs and between tabs and their panels
- **Right/Down arrows**: Move to next tab (depends on orientation)
- **Left/Up arrows**: Move to previous tab (depends on orientation)
- **Enter/Space**: Activates the tab that currently has keyboard focus

### Focus Management

The JavaScript enhances keyboard accessibility by:

- Supporting arrow key navigation between tabs (adapting to orientation)
- Automatically focusing the newly activated tab
- Ensuring proper ARIA states are updated when tabs change

## How It Works

This tab interface is built primarily using CSS with hidden radio inputs to control tab states. The core functionality (showing/hiding tab content and highlighting the active tab) works without any JavaScript through CSS selectors.

The implementation uses the CSS sibling selector `~` to show/hide content and highlight the active tab, making it lightweight and performant.

For orientation support, the component uses:

- `aria-orientation` attribute to semantically indicate the tab direction
- CSS classes to visually arrange the tabs horizontally or vertically
- JavaScript that adapts keyboard navigation based on the orientation

The minimal JavaScript is used solely for accessibility improvements:

1. **ARIA attribute management**: Updates `aria-selected` states dynamically when tabs change
2. **Keyboard navigation**: Enables appropriate arrow key navigation based on orientation
3. **Focus management**: Moves focus to the newly activated tab when switching with keyboard

Without this JavaScript, the tabs would still function for mouse users, but would lack proper keyboard accessibility and screen reader announcements.

## Additional Resources

- [WAI-ARIA: Tab Panel Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tabpanel/)
- [MDN Web Docs: ARIA Tab Role](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/tab_role)
- [WCAG 2.1 Accessibility Guidelines](https://www.w3.org/TR/WCAG21/)
