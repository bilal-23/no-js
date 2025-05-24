# Gallery Carousel Component

A fully accessible, WAI-ARIA compliant image gallery carousel component that combines CSS for visual performance with minimal JavaScript for comprehensive accessibility compliance. Features radio button state management, dynamic ARIA updates, enhanced keyboard navigation, screen reader announcements, and responsive design.

## Features

- **WAI-ARIA Compliant** - Follows the official WAI-ARIA Carousel pattern
- **Hybrid Architecture** - CSS for visual state management + JavaScript for accessibility
- **Full Keyboard Navigation** - Arrow keys, Home/End, Tab navigation
- **Screen Reader Support** - Real-time announcements and proper semantic structure
- **Dynamic ARIA Updates** - Live ARIA attribute updates and contextual button labels
- **Responsive Design** - Mobile-friendly with touch optimizations
- **Smooth Animations** - CSS transitions with reduced motion support
- **Graceful Degradation** - Works without JavaScript for basic functionality
- **High Contrast Support** - Accessible in forced-colors mode
- **Focus Management** - Proper focus indicators and keyboard interaction

## GitHub Repository

This component is part of the NoJS UI Components library:
[https://github.com/bilal-23/no-js/tree/main/components/gallery](https://github.com/bilal-23/no-js/tree/main/components/gallery)

## Core Architecture

The gallery uses a **hybrid approach** that combines the best of both CSS and JavaScript:

### CSS Handles:

- Visual transitions and animations
- Active state styling via `:checked` selectors
- Responsive design and layout
- Performance-optimized state management

### JavaScript Handles:

- ARIA attribute updates (CSS cannot do this)
- Screen reader announcements via live regions
- Enhanced keyboard navigation (arrow keys, Home/End)
- Contextual button labels with current slide information
- Focus management and state synchronization

### Why This Hybrid Approach?

1. **Performance** - CSS handles visual state changes efficiently
2. **Accessibility** - JavaScript provides comprehensive accessibility features
3. **Reliability** - Radio buttons ensure state persistence
4. **Fallback** - Basic functionality works without JavaScript

## Implementation

### HTML Structure

```html
<!-- Carousel with proper ARIA structure -->
<section
  role="region"
  aria-label="Image gallery carousel"
  aria-roledescription="carousel"
>
  <!-- Hidden radio buttons for CSS state management -->
  <input type="radio" name="gallery" id="img1" class="gallery-radio" checked />
  <input type="radio" name="gallery" id="img2" class="gallery-radio" />
  <!-- ... more radio buttons ... -->

  <!-- Carousel Controls -->
  <div class="carousel-controls">
    <button
      class="carousel-btn carousel-btn-prev"
      aria-label="Previous image"
      type="button"
    >
      <span aria-hidden="true">‹</span>
      <span class="sr-only">Go to previous image</span>
    </button>
    <button
      class="carousel-btn carousel-btn-next"
      aria-label="Next image"
      type="button"
    >
      <span aria-hidden="true">›</span>
      <span class="sr-only">Go to next image</span>
    </button>
  </div>

  <!-- Main Slides Container -->
  <div class="gallery-slides" aria-live="polite" aria-atomic="false">
    <div
      class="slide"
      role="group"
      aria-roledescription="slide"
      aria-label="1 of 6"
      data-slide="1"
      aria-current="true"
    >
      <img src="image.jpg" alt="Descriptive alt text" />
      <div class="slide-caption">
        <h4>Image Title</h4>
        <p>Image description</p>
      </div>
    </div>
    <!-- ... more slides ... -->
  </div>

  <!-- Slide Picker Controls (Dots) -->
  <div class="slide-picker" role="group" aria-label="Choose slide to display">
    <button
      class="slide-picker-btn"
      aria-label="Show slide 1"
      data-slide="1"
      aria-current="true"
      type="button"
    >
      <span class="sr-only">Slide 1 of 6</span>
    </button>
    <!-- ... more slide picker buttons ... -->
  </div>
</section>
```

### CSS State Management

```css
/* Hide all slides by default */
.slide {
  opacity: 0;
  transform: scale(0.95);
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
  pointer-events: none;
}

/* Show active slide when radio is checked */
#img1:checked ~ .gallery-slides .slide[data-slide="1"],
#img2:checked ~ .gallery-slides .slide[data-slide="2"],
#img3:checked ~ .gallery-slides .slide[data-slide="3"] {
  opacity: 1;
  transform: scale(1);
  pointer-events: auto;
}

/* Active slide picker styling */
.slide-picker-btn[aria-current="true"] {
  background: var(--primary);
  transform: scale(1.3);
}

/* Carousel controls */
.carousel-controls {
  pointer-events: none; /* Don't block slide interactions */
}

.carousel-btn {
  pointer-events: auto; /* But buttons should be clickable */
}
```

### JavaScript Accessibility Enhancement

```javascript
// Update ARIA states dynamically
function updateCarousel(index) {
  // Update slides
  slides.forEach((slide, i) => {
    slide.setAttribute("aria-current", i === index ? "true" : "false");
  });

  // Update slide picker buttons
  slidePickerBtns.forEach((btn, i) => {
    btn.setAttribute("aria-current", i === index ? "true" : "false");
  });

  // Update button context
  const currentSlideInfo = slides[index].querySelector("h4").textContent;
  prevBtn.setAttribute(
    "aria-label",
    `Previous image (currently viewing: ${currentSlideInfo})`
  );
  nextBtn.setAttribute(
    "aria-label",
    `Next image (currently viewing: ${currentSlideInfo})`
  );
}

// Announce changes to screen readers
function announceSlideChange(index) {
  const slide = slides[index];
  const slideTitle = slide.querySelector("h4").textContent;
  const slideDescription = slide.querySelector("p").textContent;
  const announcement = `Slide ${
    index + 1
  } of ${totalSlides}: ${slideTitle}. ${slideDescription}`;

  // Create temporary live region for announcement
  const announcementElement = document.createElement("div");
  announcementElement.setAttribute("aria-live", "polite");
  announcementElement.setAttribute("aria-atomic", "true");
  announcementElement.className = "sr-only";
  announcementElement.textContent = announcement;

  // Add briefly then remove
  slidesContainer.appendChild(announcementElement);
  setTimeout(() => {
    if (announcementElement.parentNode) {
      announcementElement.parentNode.removeChild(announcementElement);
    }
  }, 1000);
}

// Enhanced keyboard navigation
function handleKeyDown(event) {
  if (!carousel.contains(document.activeElement)) return;

  switch (event.key) {
    case "ArrowLeft":
      event.preventDefault();
      goToPrevSlide();
      break;
    case "ArrowRight":
      event.preventDefault();
      goToNextSlide();
      break;
    case "Home":
      event.preventDefault();
      goToSlide(0);
      break;
    case "End":
      event.preventDefault();
      goToSlide(totalSlides - 1);
      break;
  }
}
```

## Comprehensive Accessibility Features

### ARIA Roles and Semantic Structure

#### What it does:

- `role="region"` on carousel container with `aria-roledescription="carousel"`
- `role="group"` on each slide with `aria-roledescription="slide"`
- `role="group"` on slide picker controls for logical grouping

#### Why it matters:

Screen readers need to understand the structure and purpose of interface elements. Without proper roles, a screen reader might announce this as "list of 6 buttons and 6 images" instead of "carousel with 6 slides." The semantic structure helps users understand they're in a navigation component with expected interaction patterns.

#### User experience:

- **NVDA**: "Image gallery carousel, region" - immediately informing users of the component type
- **JAWS**: "Carousel region, Image gallery carousel"
- **VoiceOver**: "Image gallery carousel, group"

### Dynamic ARIA States

#### Implementation:

```javascript
// JavaScript updates current state
slide.setAttribute("aria-current", i === index ? "true" : "false");
slidePickerBtn.setAttribute("aria-current", i === index ? "true" : "false");
```

#### Why it matters:

`aria-current="true"` is the standard way to indicate the current item in a set. This is crucial for spatial orientation - users need to know where they are in the carousel. Without this, screen reader users would have no way to determine which slide is currently displayed.

#### User experience:

- **JAWS**: "Slide 1 of 6, current, group"
- **VoiceOver**: "Slide 1 of 6, current, group, Mountain Peaks"
- **NVDA**: "Show slide 1, Mountain landscape, button, current"

### Live Region Announcements

#### Implementation:

```javascript
// Creates temporary announcement
const announcement = `Slide ${
  index + 1
} of ${totalSlides}: ${slideTitle}. ${slideDescription}`;
announcementElement.setAttribute("aria-live", "polite");
announcementElement.setAttribute("aria-atomic", "true");
```

#### Why it matters:

When slides change, screen reader users need to be informed of the new content. `aria-live="polite"` ensures announcements don't interrupt the user's current reading, while `aria-atomic="true"` ensures the entire announcement is read as one unit.

#### Without this feature:

Users would click next/previous and hear nothing, leaving them confused about whether anything changed. They'd need to manually navigate to discover what's now displayed.

#### User experience:

After clicking "Next": "Slide 2 of 6: Ocean Waves. Powerful waves meeting the rocky coastline" - users immediately understand what changed and where they are.

### Contextual Button Labels

#### Implementation:

```javascript
// Updates button context dynamically
prevBtn.setAttribute(
  "aria-label",
  `Previous image (currently viewing: ${currentSlideInfo})`
);
nextBtn.setAttribute(
  "aria-label",
  `Next image (currently viewing: ${currentSlideInfo})`
);
```

#### Why it matters:

Generic "Previous" and "Next" buttons provide minimal context. Enhanced labels give users spatial awareness and help them understand the current state before deciding to navigate.

#### Cognitive accessibility benefit:

Users with cognitive disabilities benefit from the extra context, reducing mental load and providing confirmation of current location within the carousel.

#### User experience:

Tab to Previous button: "Previous image (currently viewing: Mountain Peaks), button" - users know exactly where they are and what will happen if they activate this control.

### Position and Context Information

#### Features:

- Each slide labeled as "X of Y" (e.g., "1 of 6")
- Slide picker buttons include position and content preview
- Carousel container updates with current slide information

#### Why it matters:

Spatial orientation is crucial for accessibility. Sighted users can see thumbnails and progress indicators, but screen reader users need explicit position information. This helps users understand the scope of content and their current location.

#### WCAG 2.1 Compliance:

- **2.4.6 Headings and Labels**: Descriptive labels help users understand purpose
- **2.4.8 Location**: Users can determine their location within the carousel
- **3.2.4 Consistent Identification**: Navigation controls have consistent, predictable behavior

### Enhanced Keyboard Navigation

#### Implementation:

```javascript
// Arrow key navigation
case 'ArrowLeft': goToPrevSlide(); break;
case 'ArrowRight': goToNextSlide(); break;
case 'Home': goToSlide(0); break;
case 'End': goToSlide(totalSlides - 1); break;
```

#### Why it matters:

The WAI-ARIA Carousel pattern specifies that arrow keys should navigate between slides when focus is within the carousel. This provides efficient navigation for keyboard users who might otherwise need to tab through individual slide picker buttons.

#### Motor disability benefits:

Users with limited mobility can navigate the entire carousel with just arrow keys, reducing the number of key presses required. Home/End keys provide quick access to carousel boundaries.

#### Pattern consistency:

Follows established conventions that users expect from carousel components, reducing cognitive load and leveraging existing user knowledge.

### Focus Management and Visual Indicators

#### Features:

- Clear focus rings on all interactive elements
- Focus remains on activated control after use
- Focus doesn't get trapped or lost during navigation

#### Why it matters:

Users with low vision or motor disabilities rely on visible focus indicators to understand which element is active. Proper focus management ensures users don't lose their place when interacting with the carousel.

#### WCAG 2.1 Compliance:

- **2.4.7 Focus Visible**: Clear visual focus indicators
- **3.2.1 On Focus**: No unexpected context changes when receiving focus
- **3.2.2 On Input**: Predictable behavior when controls are activated

### Multi-Modal Accessibility Support

#### Screen Readers:

- NVDA, JAWS, VoiceOver all receive proper announcements
- Role information helps users understand component structure
- Live regions provide change notifications

#### Voice Control:

- Descriptive button labels enable voice commands like "click previous image"
- Semantic roles help voice software identify interactive elements

#### Switch Navigation:

- All controls accessible via sequential Tab navigation
- No mouse-only interactions required
- Predictable tab order through carousel controls

#### Cognitive Accessibility:

- Consistent interaction patterns
- Clear labeling with context information
- Predictable behavior following established conventions

## Keyboard Navigation

### Navigation Keys:

- **Tab**: Navigate between carousel controls (prev, next, slide picker dots)
- **Space/Enter**: Activate focused buttons
- **Arrow Left/Right**: Navigate between slides when focus is on carousel
- **Home/End**: Jump to first/last slide

### Focus Order:

1. Previous button
2. Next button
3. Slide picker buttons (dots)
4. Any focusable content within slides

## JavaScript Requirements

While the visual functionality works without JavaScript (thanks to CSS), the accessibility features require minimal JavaScript for:

1. **ARIA Attribute Updates**: CSS cannot dynamically update ARIA attributes
2. **Screen Reader Announcements**: Proper live region management
3. **Enhanced Keyboard Events**: Arrow key navigation and Home/End support
4. **Context-Aware Labels**: Dynamic button labels with current slide information

### What JavaScript Does:

- Handles keyboard events (Enter/Space/Arrow keys) to navigate slides
- Updates `aria-current`, `aria-label`, and other ARIA attributes
- Creates screen reader announcements for slide changes
- Keeps native `checked` attribute and ARIA states synchronized

## Graceful Degradation

If JavaScript is disabled, the carousel still functions with:

- **Click navigation** via slide picker dots
- **Basic keyboard navigation** (Tab + Space)
- **Visual state management** via CSS `:checked` selectors
- **All images and content** remain accessible
- **Form submission** works (radio buttons retain state)

## Responsive Design

### Breakpoints:

- **Desktop** (1000px+): Full-size carousel with all features
- **Tablet** (768px-999px): Reduced spacing, smaller controls
- **Mobile** (480px-767px): Optimized for touch, always-visible captions
- **Small Mobile** (<480px): Compact layout, smaller buttons

### Touch Optimizations:

- Larger touch targets on mobile devices
- Always-visible captions on touch devices
- Optimized spacing for finger navigation

## Performance

### Optimizations:

- CSS handles all visual state transitions
- JavaScript only runs for accessibility features
- Efficient event delegation
- Minimal DOM manipulation
- Debounced resize events

## Customization

### CSS Custom Properties:

```css
:root {
  --primary: #7c4dff;
  --white: #ffffff;
  --dark: #1a1a1a;
  --light-gray: #f5f5f5;
  --shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  --radius: 8px;
  --transition: 0.3s ease;
  --focus-ring: #0066cc;
}
```

### Slide Configuration:

- Easily add/remove slides by updating HTML and CSS selectors
- Support for any number of slides
- Configurable slide dimensions and aspect ratios

## Usage Notes

1. **State Management**

   - Uses radio buttons for reliable state persistence
   - JavaScript enhances with ARIA attributes
   - State survives page refreshes

2. **Images**

   - Always provide descriptive alt text
   - Use consistent aspect ratios for best results
   - Optimize images for web (WebP recommended)

3. **Captions**

   - Keep titles concise but descriptive
   - Provide meaningful descriptions for screen readers
   - Consider multilingual support

4. **Accessibility Testing**
   - Test with actual screen readers (NVDA, JAWS, VoiceOver)
   - Verify keyboard navigation works completely
   - Check high contrast mode appearance

## Dependencies

- **No external libraries required**
- **CSS Custom Properties** for theming
- **Modern JavaScript** for accessibility features
- **ARIA attributes** support in browsers

## Contributing

Contributions are welcome! Please ensure any changes maintain or improve accessibility. Test with:

1. Keyboard navigation only
2. Screen reader software
3. High contrast mode
4. Reduced motion preferences

## License

This component is part of the NoJS UI Components library and is available under the MIT License.

## Related Resources

- [WAI-ARIA Carousel Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/carousel/)
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/Understanding/)
- [MDN Web Docs: :checked pseudo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/:checked)
- [CSS Grid Layout Guide](https://css-tricks.com/snippets/css/complete-guide-grid/)
