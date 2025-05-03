# Dropdown Menu Component

A lightweight, accessible dropdown menu built primarily with CSS and minimal JavaScript for enhanced accessibility.

## Features

- **CSS-powered functionality** - Core dropdown behavior works without JavaScript
- **Accessible** - ARIA attributes, keyboard navigation and focus management
- **Minimal JavaScript** - Only used for accessibility enhancements
- **Smooth transitions** - Fade-in/out effects when opening/closing
- **Keyboard support** - Arrow keys, Tab, Enter and Space navigation
- **Proper focus management** - Focus is maintained within the component
- **Mobile friendly** - Responsive design for touch devices
- **Nested support** - Create multi-level dropdown menus

## GitHub Repository

This component is part of the NoJS UI Components library:
[https://github.com/bilal-23/no-js/tree/main/components/dropdown-menu](https://github.com/bilal-23/no-js/tree/main/components/dropdown-menu)

## Implementation

### HTML Structure

```html
<!-- Simple Dropdown Menu -->
<div class="dropdown">
  <button
    class="dropdown-toggle"
    id="dropdown-toggle-1"
    aria-haspopup="true"
    aria-expanded="false"
  >
    Settings
    <span class="caret">▼</span>
  </button>
  <ul class="dropdown-menu" role="menu" aria-labelledby="dropdown-toggle-1">
    <li>
      <a href="#" class="dropdown-item" role="menuitem">Profile</a>
    </li>
    <li>
      <a href="#" class="dropdown-item" role="menuitem">Preferences</a>
    </li>
    <li>
      <a href="#" class="dropdown-item" role="menuitem">Notifications</a>
    </li>
    <li class="divider" role="separator"></li>
    <li>
      <a href="#" class="dropdown-item" role="menuitem">Logout</a>
    </li>
  </ul>
</div>
```

### For Nested Dropdowns

To create a nested dropdown menu with multiple levels:

```html
<nav class="nav-dropdown nested-dropdown">
  <ul class="nav-menu" role="menubar">
    <li class="nav-item" role="none">
      <a href="#" class="nav-link" role="menuitem">Home</a>
    </li>
    <li class="nav-item has-dropdown" role="none">
      <a
        class="nav-link"
        id="products-menu"
        role="menuitem"
        aria-haspopup="true"
        aria-expanded="false"
      >
        Products <span class="caret">▼</span>
      </a>
      <ul class="nav-dropdown-menu" role="menu" aria-labelledby="products-menu">
        <li role="none">
          <a href="#" class="dropdown-item" role="menuitem">Electronics</a>
        </li>
        <li class="has-dropdown" role="none">
          <a
            class="dropdown-item"
            id="home-garden-menu"
            role="menuitem"
            aria-haspopup="true"
            aria-expanded="false"
          >
            Home & Garden <span class="caret-right">▶</span>
          </a>
          <ul
            class="nav-dropdown-submenu"
            role="menu"
            aria-labelledby="home-garden-menu"
          >
            <li role="none">
              <a href="#" class="dropdown-item" role="menuitem">Furniture</a>
            </li>
          </ul>
        </li>
      </ul>
    </li>
  </ul>
</nav>
```

### CSS (Key Components)

```css
/* Dropdown toggle button */
.dropdown {
  position: relative;
  display: inline-block;
}
.dropdown-toggle {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background: var(--white);
  color: var(--primary);
  border: 2px solid var(--primary);
  border-radius: var(--radius);
  padding: 0.75rem 1.25rem;
  cursor: pointer;
}
.caret {
  font-size: 0.75rem;
  transition: transform 0.2s ease;
}

/* Dropdown menu */
.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  min-width: 180px;
  background-color: var(--white);
  border-radius: var(--radius);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
  padding: 0.5rem 0;
  margin-top: 0.5rem;
  list-style: none;
  z-index: 10;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-10px);
  transition: opacity 0.2s ease, transform 0.2s ease, visibility 0.2s;
  border: 1px solid var(--primary);
}

/* Key CSS: Show dropdown menu on hover and focus */
.dropdown:hover .dropdown-menu,
.dropdown:focus-within .dropdown-menu {
  display: block;
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}
.dropdown:hover .caret,
.dropdown:focus-within .caret {
  transform: rotate(180deg);
}

/* Mobile-responsive nested dropdowns */
@media (max-width: 768px) {
  .nav-dropdown-menu {
    position: static;
    width: 100%;
    opacity: 0;
    visibility: hidden;
    max-height: 0;
    overflow: hidden;
    transition: opacity 0.3s ease, visibility 0.3s ease, max-height 0.3s ease;
  }

  .has-dropdown:hover > .nav-dropdown-menu,
  .has-dropdown:focus-within > .nav-dropdown-menu {
    opacity: 1;
    visibility: visible;
    max-height: 500px;
    padding: 0.5rem 0;
  }
}
```

### JavaScript (For Accessibility Only)

```javascript
document.addEventListener("DOMContentLoaded", function () {
  const dropdownButtons = document.querySelectorAll('[aria-haspopup="true"]');

  // Add keyboard navigation and ARIA state management
  dropdownButtons.forEach((button) => {
    button.addEventListener("click", function () {
      const expanded = this.getAttribute("aria-expanded") === "true";

      // Toggle expanded state
      this.setAttribute("aria-expanded", !expanded);

      // Find the dropdown menu
      const menuId =
        this.getAttribute("aria-controls") || this.getAttribute("id");
      const menu = document.querySelector('[aria-labelledby="' + menuId + '"]');

      if (menu) {
        // Add keyboard navigation within menu
        const menuItems = menu.querySelectorAll('[role="menuitem"]');

        if (expanded) {
          // Close the menu
          this.focus();
        } else {
          // Open the menu and focus first item
          if (menuItems.length > 0) {
            menuItems[0].focus();
          }
        }
      }
    });

    // Handle arrow key navigation
    button.addEventListener("keydown", function (e) {
      const menuId =
        this.getAttribute("aria-controls") || this.getAttribute("id");
      const menu = document.querySelector('[aria-labelledby="' + menuId + '"]');

      if (!menu) return;

      // Down arrow - open menu
      if (e.key === "ArrowDown" || e.key === "Down") {
        e.preventDefault();
        this.click();
      }

      // Escape key - close menu
      if (e.key === "Escape" || e.key === "Esc") {
        e.preventDefault();
        this.setAttribute("aria-expanded", "false");
        this.focus();
      }
    });
  });

  // Add keyboard navigation within menus
  document.querySelectorAll('[role="menu"]').forEach((menu) => {
    menu.addEventListener("keydown", function (e) {
      const items = Array.from(
        this.querySelectorAll('[role="menuitem"]:not([aria-disabled="true"])')
      );
      const currentIndex = items.indexOf(document.activeElement);

      // Handle up/down navigation
      if (e.key === "ArrowDown" || e.key === "Down") {
        e.preventDefault();
        if (currentIndex < items.length - 1) {
          items[currentIndex + 1].focus();
        } else {
          items[0].focus(); // Wrap to first item
        }
      }

      if (e.key === "ArrowUp" || e.key === "Up") {
        e.preventDefault();
        if (currentIndex > 0) {
          items[currentIndex - 1].focus();
        } else {
          items[items.length - 1].focus(); // Wrap to last item
        }
      }

      // Escape - close current menu
      if (e.key === "Escape" || e.key === "Esc") {
        e.preventDefault();
        const button = document.querySelector(
          '[aria-controls="' +
            this.id +
            '"], #' +
            this.getAttribute("aria-labelledby")
        );
        if (button) {
          button.setAttribute("aria-expanded", "false");
          button.focus();
        }
      }
    });
  });
});
```

## Accessibility Features

### ARIA Attributes

| Attribute                    | Purpose                                            |
| ---------------------------- | -------------------------------------------------- |
| `aria-haspopup="true"`       | Indicates that the button controls a popup menu    |
| `aria-expanded="false/true"` | Indicates whether the dropdown menu is expanded    |
| `role="menu"`                | Identifies the dropdown as a menu                  |
| `role="menuitem"`            | Identifies each item in the dropdown menu          |
| `role="menubar"`             | Used for horizontal navigation with dropdowns      |
| `role="separator"`           | Used for divider elements in the menu              |
| `aria-labelledby`            | Associates the menu with the button controlling it |
| `role="none"`                | Removes implicit role from structural list items   |

### What Not To Add

❌ **Don't add these to your dropdown component:**

- **`tabindex="-1"`** on menu items: This prevents keyboard users from accessing menu items.

- **`autofocus`** attributes: These can be disorienting and should be avoided.

- **Using `<div>` elements** for interactive menu items without proper roles and keyboard support.

- **Non-semantic toggle mechanisms**: Using arbitrary elements as toggles without proper ARIA attributes.

### Keyboard Navigation

- **Tab key**: Moves focus between dropdown buttons and other focusable elements
- **Enter/Space**: Opens the dropdown when a dropdown button has focus
- **Escape**: Closes an open dropdown menu
- **Down arrow**: Opens a dropdown and moves focus to the first item
- **Up/Down arrows**: Navigate between items within an open dropdown
- **Right arrow**: Opens a submenu when focus is on a menu item with a submenu

### Focus Management

The JavaScript enhances keyboard accessibility by:

- Supporting arrow key navigation between dropdown items
- Automatically focusing the first menu item when opened via keyboard
- Returning focus to the trigger button when a dropdown is closed
- Ensuring proper ARIA states are updated when dropdowns open/close

## How It Works

This dropdown menu is built primarily using HTML and CSS with the `:hover` and `:focus-within` pseudo-classes to show/hide dropdown content without JavaScript on desktop devices.

For mobile devices, the component uses a CSS-only approach with responsive styles to handle touch interactions, making it accessible across all devices.

The minimal JavaScript is used solely for accessibility improvements:

1. **ARIA attribute management**: Updates `aria-expanded` states dynamically when dropdowns open/close
2. **Keyboard navigation**: Enables arrow key navigation within dropdown menus
3. **Focus management**: Ensures proper focus handling and screen reader announcements

Without this JavaScript, the dropdowns would still function for mouse and touch users, but would lack proper keyboard accessibility.

## Additional Resources

- [WAI-ARIA: Menu Button Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/menubutton/)
- [MDN Web Docs: ARIA Menu Role](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/menu_role)
- [WCAG 2.1 Accessibility Guidelines](https://www.w3.org/TR/WCAG21/)
