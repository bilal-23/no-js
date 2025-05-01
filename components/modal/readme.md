# Modal Dialog Component

A lightweight, accessible modal dialog component built with the HTML `<dialog>` element and minimal JavaScript.

## Features

- **Semantic HTML** - Uses the native `<dialog>` element
- **Accessible** - Full keyboard navigation, screen reader support, and ARIA attributes
- **Minimal JavaScript** - Only uses inline event handlers for essential functionality
- **Multiple closing methods** - Close button, outside click, or Escape key
- **Focus management** - Automatically traps focus within the modal
- **Clean styling** - Modern appearance with smooth transitions

## Implementation

### HTML Structure

```html
<!-- Modal trigger button -->
<button
  class="modal-trigger"
  onclick="document.getElementById('modal').showModal()"
  aria-haspopup="dialog"
  aria-controls="modal"
>
  Open Modal
</button>

<!-- Modal dialog -->
<dialog
  id="modal"
  class="modal-dialog"
  aria-labelledby="modal-title"
  aria-describedby="modal-description"
  aria-modal="true"
  onclick="if(event.target === this) this.close()"
>
  <div class="modal-content">
    <h2 id="modal-title">Modal Title</h2>
    <p id="modal-description">
      This is a modal dialog using the semantic HTML dialog element.
    </p>
    <button
      class="modal-close"
      aria-label="Close modal"
      onclick="this.closest('dialog').close()"
    >
      &times;
    </button>
    <div class="modal-actions">
      <button
        class="modal-button"
        aria-label="Close modal"
        onclick="this.closest('dialog').close()"
      >
        Close
      </button>
    </div>
  </div>
</dialog>
```

### CSS (Key Components)

```css
/* Dialog styling */
.modal-dialog {
  padding: 0;
  border: none;
  border-radius: 8px;
  max-width: 90%;
  width: 500px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1000;
}

/* Dialog backdrop */
.modal-dialog::backdrop {
  background-color: rgba(0, 0, 0, 0.5);
}

/* Close button styling */
.modal-close {
  position: absolute;
  top: 10px;
  right: 10px;
  /* Additional styling */
}
```

## Accessibility Features

### ARIA Attributes

| Attribute                              | Purpose                                                                                            |
| -------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `aria-haspopup="dialog"`               | Indicates the button opens a dialog popup                                                          |
| `aria-controls="modal"`                | Establishes a relationship between the button and the dialog it controls                           |
| `aria-labelledby="modal-title"`        | Associates the dialog with its title, announcing the title when the dialog opens                   |
| `aria-describedby="modal-description"` | Provides additional context about the dialog's purpose                                             |
| `aria-modal="true"`                    | Tells assistive technologies that this is a modal window that blocks interaction with page content |
| `aria-label="Close modal"`             | Provides an accessible name for buttons that only have icon content                                |

### Keyboard Navigation

- **Tab key**: Moves focus between interactive elements within the dialog
- **Escape key**: Closes the dialog (built-in behavior of the dialog element)
- **Enter/Space**: Activates buttons when they have keyboard focus

### Focus Management

The `<dialog>` element with `showModal()` automatically:

- Traps focus within the dialog when open
- Returns focus to the triggering element when closed
- Makes content outside the dialog inert and unfocusable

## JavaScript Functionality

The component uses minimal inline JavaScript:

1. **Opening the modal**:

   ```js
   document.getElementById("modal").showModal();
   ```

2. **Closing with buttons**:

   ```js
   this.closest("dialog").close();
   ```

3. **Clicking outside to close**:
   ```js
   if (event.target === this) this.close();
   ```

## Browser Support

The `<dialog>` element is supported in all modern browsers:

- Chrome 37+
- Firefox 98+
- Safari 15.4+
- Edge 79+

## Additional Resources

- [MDN Web Docs: The Dialog element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog)
- [WCAG 2.1 Accessibility Guidelines](https://www.w3.org/TR/WCAG21/)
- [WAI-ARIA Authoring Practices: Dialog Modal](https://www.w3.org/WAI/ARIA/apg/patterns/dialogmodal/)
