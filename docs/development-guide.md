# Development Guide

## Prerequisites

- **Web Browser**: Modern browser (Chrome 90+, Firefox 88+, Safari 14+, Edge 90+)
- **Text Editor**: VS Code, Sublime Text, or any code editor
- **Optional**: Local web server for development

## Environment Setup

### Quick Start (No Installation)

```bash
# 1. Clone or download the project
git clone <repository-url>
cd Algorithm-Visualization-Project

# 2. Open in browser directly
open index.html
# or double-click index.html
```

### Recommended Setup (Local Server)

```bash
# Option 1: Python HTTP Server
python3 -m http.server 8000

# Option 2: Node.js http-server
npx http-server -p 8000

# Option 3: VS Code Live Server Extension
# Install "Live Server" extension and click "Go Live"

# Navigate to: http://localhost:8000
```

## Project Structure

```
Algorithm-Visualization-Project/
├── index.html       # Landing page
├── login.html       # Login interface (UI only)
├── algorithm.html   # Main visualization platform
└── readme.md        # Project info
```

## Local Development

### Development Workflow

1. **Edit Files**: Modify HTML directly in your editor
2. **Save**: Ctrl+S / Cmd+S
3. **Refresh Browser**: Ctrl+R / Cmd+R or auto-refresh with Live Server
4. **Test**: Interact with the visualization

### Hot Tips

- **Browser DevTools**: F12 or Cmd+Option+I for debugging
- **Console Logging**: Add `console.log()` in `<script>` sections
- **CSS Changes**: Modify `<style>` section or Tailwind classes
- **JavaScript Changes**: Edit inline `<script>` code

## Building New Features

### Adding a New Algorithm

1. **Add Module Object** to `modules[]` array in algorithm.html:

```javascript
{
  id: 'merge-sort',
  type: 'algo',
  title: 'Merge Sort',
  cat: 'algo',
  tags: ['Sorting', 'D&C'],
  complexity: 'O(n log n)',
  difficulty: 'Medium',
  desc: 'Divide and conquer merge strategy.',
  icon: 'git-merge',
  code: `...C++ code...`,
  factory: (n) => Array.from({length: n}, () => Math.floor(Math.random() * 50) + 5),
  generator: function*(arr) {
    // Your algorithm logic with yield statements
  },
  renderType: 'bar'
}
```

2. **Implement Generator Function**:
```javascript
generator: function*(arr) {
  // Yield visualization states
  yield { type: 'compare', indices: [i, j], locals: { i, j } };
  yield { type: 'swap', indices: [i, j], values: [...arr] };
  yield { type: 'sorted', index: i };
}
```

3. **Add C++ Reference Code**: Include in `code` property

4. **Test**: Load module and verify visualization

### Adding a New Data Structure

1. **Add Module with Handlers**:
```javascript
{
  id: 'binary-tree',
  type: 'ds',
  title: 'Binary Tree',
  state: { root: null },
  handlers: {
    insert: async (ctx) => {
      // Implementation
      ctx.state.root = ...;
      ctx.render();
    }
  },
  renderType: 'tree', // You'll need to create this renderer
  ops: [
    { id: 'insert', label: 'Insert' },
    { id: 'search', label: 'Search' }
  ]
}
```

2. **Create Renderer** if needed (in `renderers` object)

### Creating a Custom Renderer

```javascript
renderers.tree = (el, state, meta) => {
  el.innerHTML = '';
  // Your rendering logic
  // Example: Create SVG tree visualization
};
```

## Testing

### Manual Testing Checklist

#### Algorithm Testing
- [ ] Module loads without errors
- [ ] Play button starts animation
- [ ] Pause button stops animation
- [ ] Step button advances one step
- [ ] Speed slider changes animation speed
- [ ] Data size slider generates new data
- [ ] Randomize button resets with new data
- [ ] Debug panel shows correct variables
- [ ] Code view highlights correct lines (if implemented)
- [ ] Visualization animates smoothly

#### Data Structure Testing
- [ ] Operations execute without errors
- [ ] Visual feedback for operations
- [ ] State updates correctly
- [ ] Overflow/underflow handled gracefully

### Browser Testing

Test in multiple browsers:
- Chrome/Edge (Chromium)
- Firefox
- Safari (if on Mac)

### Performance Testing

```javascript
// Add to measure performance
console.time('render');
renderers[mod.renderType](el, data, meta);
console.timeEnd('render');
```

## Common Development Tasks

### Modify Styling

**Change Colors**:
```javascript
// In <script> Tailwind config
colors: {
  brand: {
    cyan: '#yourcolor',
    purple: '#yourcolor'
  }
}
```

**Update CSS**:
```css
/* In <style> section */
.glass-panel {
  background: rgba(15, 23, 42, 0.6);
  /* Your changes */
}
```

### Add New Icon

1. Find icon name at [Lucide Icons](https://lucide.dev/)
2. Use in HTML: `<i data-lucide="icon-name"></i>`
3. Call `lucide.createIcons()` after adding icons

### Debugging Tips

**Console Logging**:
```javascript
// Add to generator
yield { locals: { debug: 'checkpoint', arr } };

// Check app state
console.log(app.activeId, app.currentData);
```

**Breakpoints**:
- Open DevTools (F12)
- Sources tab → algorithm.html
- Click line numbers to set breakpoints

**React DevTools**: Not applicable (no React)

## Code Style Guidelines

### JavaScript

- Use ES6+ features (arrow functions, destructuring, spread)
- Prefer `const` over `let`, avoid `var`
- Use template literals for strings with variables
- Async/await for asynchronous operations

```javascript
// Good
const data = [...arr];
await sleep(1000);

// Avoid
var data = arr.slice();
setTimeout(() => {}, 1000);
```

### HTML

- Use semantic elements (`<aside>`, `<main>`, `<section>`)
- Tailwind utility classes instead of custom CSS where possible
- Keep inline scripts organized with comments

### CSS

- Follow existing naming conventions
- Use Tailwind utilities first
- Custom CSS only when necessary
- Namespace custom classes to avoid conflicts

## Deployment

### Production Checklist

- [ ] Remove console.log statements
- [ ] Test all modules
- [ ] Verify CDN links are accessible
- [ ] Check browser compatibility
- [ ] Test on mobile (if supporting)
- [ ] Optimize images (if any added)
- [ ] Add meta tags for SEO

### Deploy to GitHub Pages

```bash
# 1. Push to GitHub
git add .
git commit -m "Ready for deployment"
git push origin main

# 2. Enable GitHub Pages
# Settings → Pages → Source: main branch

# 3. Access at: https://username.github.io/repo-name/
```

### Deploy to Netlify

```bash
# 1. Drag and drop project folder to Netlify
# or
# 2. Connect GitHub repo → Deploy automatically
```

## Troubleshooting

### Module Not Loading
- Check browser console for errors
- Verify module ID is unique
- Ensure all required properties are present

### Visualization Not Animating
- Check if generator is yielding states
- Verify renderer exists for `renderType`
- Check if `isPlaying` is set to true

### Styles Not Applied
- Call `lucide.createIcons()` after DOM updates
- Check Tailwind CDN loaded successfully
- Verify class names are correct

### Performance Issues
- Reduce animation speed
- Limit data size
- Check for memory leaks (DevTools → Memory)

## Contributing

### Adding Documentation

- Update README.md with new features
- Add inline code comments for complex logic
- Document new modules in code

### Code Review Checklist

- [ ] Code follows style guidelines
- [ ] No console errors
- [ ] Tested in multiple browsers
- [ ] Performance is acceptable
- [ ] Documentation updated

---

*Generated by BMad Method document-project workflow*
*Date: 2025-11-19*
