# No-JS UI Components

A modern, lightweight component library built with minimal JavaScript. Create responsive, accessible interfaces using primarily HTML and CSS with just enough JavaScript for accessibility and enhanced user experience.

## 🌟 Features

- **Minimal JavaScript**: Components work primarily with HTML and CSS
- **Accessibility First**: WAI-ARIA compliant and keyboard navigation support
- **Responsive Design**: Mobile-first approach with responsive layouts
- **Modern Styling**: Clean, contemporary design with smooth animations
- **Zero Dependencies**: No external libraries required
- **Easy Integration**: Drop-in components for any project

## 🚀 Live Demo

Visit the [live demo](https://nojs.bilalmansuri.com/) to see all components in action.

## 📦 Available Components

- **Dropdown Menu** - Accessible dropdown with keyboard navigation
- **Modal/Popup** - Lightweight modal dialog with focus management
- **Tabs** - Tab interface with minimal JavaScript for accessibility
- **Accordion** - Expandable content sections
- **Tooltips** - Pure CSS tooltips with accessibility features
- **Toggle Switch** - Accessible toggle component
- **Gallery Carousel** - Image carousel with full accessibility support

## 🛠️ Usage

Each component is self-contained in its own directory with:

- `index.html` - Component demo and usage example
- `style.css` - Component-specific styles
- `script.js` - Minimal JavaScript (when needed)

### Basic Integration

1. Copy the component's HTML structure
2. Include the component's CSS
3. Add the JavaScript file (if the component requires it)
4. Customize styles as needed

Example for dropdown menu:

```html
<!-- Include the CSS -->
<link rel="stylesheet" href="components/dropdown-menu/style.css" />

<!-- Use the HTML structure -->
<div class="dropdown">
  <button class="dropdown-toggle" aria-expanded="false">Menu</button>
  <ul class="dropdown-menu">
    <li><a href="#">Option 1</a></li>
    <li><a href="#">Option 2</a></li>
  </ul>
</div>

<!-- Include the JS (if needed) -->
<script src="components/dropdown-menu/script.js"></script>
```

## 🎯 Philosophy

This project demonstrates that modern, interactive UI components don't always need heavy JavaScript frameworks. By leveraging:

- **CSS pseudo-classes** (`:hover`, `:focus`, `:checked`)
- **CSS transforms and transitions** for animations
- **HTML5 semantic elements** for structure
- **Minimal JavaScript** only for accessibility and complex interactions

We can create beautiful, functional components that are:

- Faster to load
- Easier to maintain
- More accessible by default
- Framework agnostic

## 🤝 Contributing

Contributions are welcome! We're looking for more creative no-JS solutions and improvements to existing components.

### Components We'd Love to See

- **Progress Bars** - Animated progress indicators
- **Side Navigation** - Collapsible sidebar navigation
- **Multi-step Forms** - Wizard-style form progression
- **Rating Systems** - Interactive star ratings
- **Responsive Navbars** - Mobile-friendly navigation
- **Toast Notifications** - Temporary notification messages
- **Skeleton Loading** - Loading state animations
- **Image Lightbox** - Full-screen image viewer
- **Data Tables** - Sortable, filterable tables
- **Calendar/Date Picker** - Date selection components

### How to Contribute

1. **Fork the repository**
2. **Create a new branch** for your component: `git checkout -b feature/new-component`
3. **Create a component directory** under `/components/your-component-name/`
4. **Include these files**:
   - `index.html` - Demo page with usage examples
   - `style.css` - Component styles
   - `script.js` - JavaScript (only if absolutely necessary)
   - `README.md` - Component documentation
5. **Follow our guidelines**:
   - Prioritize CSS solutions over JavaScript
   - Ensure accessibility (ARIA labels, keyboard navigation)
   - Use semantic HTML
   - Include responsive design
   - Add clear documentation and examples
6. **Submit a pull request** with a clear description

### Component Guidelines

- **Accessibility**: Must be keyboard navigable and screen reader friendly
- **Responsive**: Should work on all screen sizes
- **Browser Support**: Modern browsers (ES6+ is fine)
- **Documentation**: Include clear usage instructions and examples
- **Minimal JS**: Use JavaScript only when CSS cannot achieve the functionality
- **Semantic HTML**: Use appropriate HTML elements and ARIA attributes

### Code Style

- Use meaningful class names (BEM methodology preferred)
- Comment complex CSS and JavaScript
- Keep JavaScript functions small and focused
- Use CSS custom properties for theming when appropriate

## 📁 Project Structure

```
no-js/
├── index.html              # Main landing page
├── style.css              # Global styles
├── README.md              # This file
└── components/
    ├── dropdown-menu/
    │   ├── index.html
    │   ├── style.css
    │   └── script.js
    ├── modal/
    │   ├── index.html
    │   ├── style.css
    │   └── script.js
    └── ...
```

---

**Ready to contribute?** Check out our [issues](https://github.com/bilal-23/no-js/issues) or create a new component! Every contribution, no matter how small, is appreciated. 🚀
