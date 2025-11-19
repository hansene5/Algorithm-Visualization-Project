# Architecture Documentation

## Executive Summary

AlgoVision is a **client-side static web application** built with vanilla JavaScript and modern CSS (Tailwind). The architecture follows a **monolithic single-file pattern** where all application logic, styles, and content are embedded within individual HTML files. The main platform (algorithm.html) implements a **module-based visualization engine** with generator-driven execution model for step-by-step algorithm animation.

**Key Architectural Decisions**:
- **Zero Build Tooling**: No webpack, Vite, or bundlers - pure HTML/CSS/JS
- **CDN Dependencies**: All external libraries loaded via CDN
- **Generator-Based Execution**: JavaScript generators (`function*`) for pauseable algorithm execution
- **Inline Everything**: Code, styles, and logic embedded in HTML for portability

---

## Technology Stack

| Layer | Technology | Version | Justification |
|-------|-----------|---------|---------------|
| **Markup** | HTML5 | - | Standard web markup, semantic elements |
| **Styling** | Tailwind CSS | Latest (CDN) | Utility-first CSS, rapid UI development, no custom CSS build needed |
| **Icons** | Lucide | Latest (CDN) | Modern, consistent icon set with zero dependencies |
| **Typography** | Google Fonts | - | Inter (UI text), JetBrains Mono (code display) |
| **JavaScript** | Vanilla JS | ES6+ | No framework overhead, direct DOM manipulation, native generators |
| **Module System** | ES6 Modules | - | Not used - everything in global scope (single file) |
| **State Management** | Plain Objects | - | Simple state objects per module, no Redux/MobX needed |
| **Routing** | Custom Router | - | Minimal client-side view switching (dashboard ↔ visualizer) |

### Technology Decisions

**Why No Framework?**
- **Educational Purpose**: Clear, readable code for students
- **Deployment Simplicity**: Single HTML files can be opened locally or hosted anywhere
- **Zero Dependencies**: No npm, no node_modules, no build step
- **Performance**: Minimal overhead, instant page load

**Why CDN Over NPM?**
- **Portability**: Files work without installation
- **Simplicity**: No package.json, no dependency management
- **Caching**: Browser caches CDN resources across sites

---

## Architecture Pattern

### Primary Pattern: **Monolithic SPA**

```
┌─────────────────────────────────────────────────┐
│             algorithm.html                      │
│  ┌──────────────────────────────────────────┐  │
│  │         APPLICATION STATE                │  │
│  │  ┌────────┐  ┌────────┐  ┌────────┐    │  │
│  │  │ Module │  │ Router │  │  App   │    │  │
│  │  │  Data  │  │ State  │  │ State  │    │  │
│  │  └────────┘  └────────┘  └────────┘    │  │
│  └──────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────┐  │
│  │         BUSINESS LOGIC                   │  │
│  │  ┌──────────┐  ┌──────────┐            │  │
│  │  │ Handlers │  │Generator │            │  │
│  │  │   (DS)   │  │  Engine  │            │  │
│  │  └──────────┘  └──────────┘            │  │
│  └──────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────┐  │
│  │         PRESENTATION LAYER               │  │
│  │  ┌─────────┐  ┌──────────┐  ┌────────┐ │  │
│  │  │  DOM    │  │Renderers │  │  UI    │ │  │
│  │  │Manipulation││(bar/graph)││ Events │ │  │
│  │  └─────────┘  └──────────┘  └────────┘ │  │
│  └──────────────────────────────────────────┘  │
└─────────────────────────────────────────────────┘
```

### Secondary Pattern: **Module Registry**

Each algorithm/data structure is a self-contained module object:

```javascript
{
  id: 'bubble-sort',
  type: 'algo',              // 'algo' or 'ds'
  code: '...C++ reference code...',
  factory: (n) => [...],     // Data generator
  generator: function*(arr){ // Execution stepper
    // yields visualization states
  },
  renderType: 'bar',         // Renderer selection
  // ... metadata
}
```

---

## Component Overview

### 1. Module System

**Purpose**: Registry of all algorithms and data structures

**Structure**:
```javascript
const modules = [
  // Data Structures (3)
  { id: 'stack', type: 'ds', handlers: {...}, state: {...} },
  { id: 'queue', type: 'ds', handlers: {...}, state: {...} },
  { id: 'graph', type: 'algo', generator: function*(...) {...} },

  // Algorithms (3+)
  { id: 'bubble-sort', type: 'algo', generator: function*(...) {...} },
  { id: 'quick-sort', type: 'algo', generator: function*(...) {...} },
]
```

**Key Properties**:
- `type`: 'algo' (generator-based) or 'ds' (handler-based)
- `code`: C++ reference implementation (display only)
- `generator` / `handlers`: Execution logic
- `renderType`: Maps to renderer function
- `state`: Runtime state (for data structures)

### 2. Rendering Engine

**Purpose**: Transform algorithm state into visual representation

**Renderers**:
```javascript
const renderers = {
  bar: (el, data, meta) => {
    // Renders array as vertical bars
    // Highlights: compare (yellow), swap (red), sorted (green)
  },
  stack: (el, state, meta) => {
    // Renders LIFO stack with animations
  },
  queue: (el, state, meta) => {
    // Renders FIFO queue with animations
  },
  graph: (el, state, meta) => {
    // Renders nodes + edges with SVG
    // Highlights: active, visited, in-queue
  }
}
```

**Rendering Pipeline**:
1. Generator yields state object
2. App engine calls appropriate renderer
3. Renderer updates DOM with animations
4. Debug panel updates with variable values

### 3. Generator Engine (Algorithms)

**Purpose**: Pauseable, step-by-step algorithm execution

**Pattern**:
```javascript
generator: function*(arr) {
  for (let i = 0; i < n; i++) {
    yield {
      type: 'compare',
      indices: [i, j],
      locals: { i, j, ...vars }
    };
    // ... algorithm logic
    if (condition) {
      yield {
        type: 'swap',
        indices: [i, j],
        values: [...arr]
      };
    }
  }
}
```

**Yield Types**:
- `compare`: Highlight two elements being compared
- `swap`: Animate element swap with new values
- `sorted`: Mark element as sorted (final position)
- `line`: Highlight code line (not currently used)

### 4. Handler System (Data Structures)

**Purpose**: Interactive operations for data structures

**Pattern**:
```javascript
handlers: {
  push: async (ctx) => {
    ctx.log('Pushing...');
    ctx.updateLocals({ val, size });
    ctx.state.items.push(val);
    ctx.render({ highlightIdx: idx, action: 'push' });
    await sleep(1000);
    ctx.render();
  }
}
```

**Context Object** (`ctx`):
- `state`: Module state reference
- `render(meta)`: Trigger re-render with animation metadata
- `log(msg)`: Update status message
- `updateLocals(vars)`: Update debug panel variables

### 5. App Engine

**Purpose**: Core controller managing execution and UI

**Key Functions**:
- `loadModule(id)`: Load and initialize module
- `play()`: Start generator execution loop
- `stop()`: Pause execution
- `step()`: Execute one generator step
- `renderGrid()`: Render module cards in dashboard

**Execution Loop**:
```javascript
play: () => {
  const loop = () => {
    if(!isPlaying) return;
    if(step()) timer = setTimeout(loop, speed);
  };
  loop();
}
```

### 6. Router

**Purpose**: Client-side view switching

**Views**:
- `dashboard`: Module selection grid
- `viz`: Visualization canvas + controls

**Navigation**:
```javascript
router.navigate('dashboard') // Show grid
router.navigate('viz')       // Show visualizer
router.filter('algo')        // Filter by category
```

---

## Data Architecture

### State Management

**Global State** (app object):
```javascript
{
  activeId: null,           // Currently loaded module
  isPlaying: false,         // Execution state
  timer: null,              // setTimeout handle
  speed: 500,               // ms delay between steps
  generator: null,          // Active generator instance
  currentData: null,        // Current algorithm data
  filterCat: null,          // Dashboard filter
  searchTerm: ''            // Search filter
}
```

**Module State** (per data structure):
```javascript
{
  items: [],                // Stack/Queue contents
  limit: 8                  // Max capacity
}
```

**No Persistence**: All state is ephemeral (resets on page refresh).

---

## UI/UX Architecture

### Design System

**Theme**: Dark mode with glassmorphism effects

**Colors** (Tailwind custom):
- `dark.*`: Background grays (#020617 → #475569)
- `brand.cyan`: Primary accent (#06b6d4)
- `brand.purple`: Secondary (#8b5cf6)
- `brand.green`: Success/Sorted (#10b981)
- `brand.yellow`: Warning/Active (#fbbf24)
- `brand.red`: Error/Comparing (#ef4444)

**Components**:
- `.glass-panel`: Frosted glass effect containers
- `.glass-card`: Hoverable module cards
- `.bar`: Visualization bars with tooltips
- `.node`: Graph nodes with states (active/visited/processing)
- `.sq-item`: Stack/Queue items with animations

### Layout Structure

```
┌────────────────────────────────────────────────┐
│  Sidebar (64px)  │  Main Content              │
│  ┌────────────┐  │  ┌──────────────────────┐  │
│  │ Logo       │  │  │ Header + Search      │  │
│  │            │  │  ├──────────────────────┤  │
│  │ Navigation │  │  │ Content Area         │  │
│  │            │  │  │                      │  │
│  │            │  │  │  [Dashboard View]    │  │
│  │            │  │  │  or                  │  │
│  │            │  │  │  [Visualizer View]   │  │
│  │            │  │  │                      │  │
│  └────────────┘  │  └──────────────────────┘  │
└────────────────────────────────────────────────┘
```

### Visualizer Layout

```
┌─────────────────────────────────────────────────┐
│ Debug Panel (72px) │ Canvas          │ Controls│
│  ┌──────────────┐  │  ┌────────────┐ │         │
│  │ Variables    │  │  │ Visual     │ │ Speed   │
│  │              │  │  │ Canvas     │ │ Play    │
│  │ i: 2         │  │  │            │ │ Step    │
│  │ j: 5         │  │  │  [Bars]    │ │         │
│  │ status: Swap │  │  │            │ │         │
│  ├──────────────┤  │  └────────────┘ │         │
│  │ Settings     │  │  ┌────────────┐ │         │
│  │              │  │  │ Code View  │ │         │
│  │ Size slider  │  │  │            │ │         │
│  │ Randomize    │  │  │  main.cpp  │ │         │
│  └──────────────┘  │  └────────────┘ │         │
└─────────────────────────────────────────────────┘
```

---

## Development Workflow

### Prerequisites
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Optional: Local web server for avoiding CORS (Python `http.server`, Live Server extension)

### Local Development
```bash
# Option 1: Open directly in browser
open index.html

# Option 2: Local web server
python3 -m http.server 8000
# Navigate to http://localhost:8000
```

### File Modification Workflow
1. Edit HTML file directly
2. Save file
3. Refresh browser (Cmd+R / Ctrl+R)
4. No build step required

### Adding New Algorithms
1. Add module object to `modules[]` array
2. Define `generator` function with yield statements
3. Specify `renderType` (bar/stack/queue/graph)
4. Add C++ reference code in `code` property
5. Test in browser

---

## Deployment Architecture

### Deployment Options

**1. Static File Hosting**:
- GitHub Pages
- Netlify
- Vercel
- AWS S3 + CloudFront
- Any static hosting service

**2. Local Usage**:
- Download HTML files
- Open in browser
- Works offline (except CDN dependencies)

### Build Process
**None** - Files are production-ready as-is.

### Environment Configuration
**None** - No environment variables or configuration needed.

---

## Testing Strategy

### Current Testing
**None** - No automated tests implemented.

### Recommended Testing Approach

**1. Manual Testing Checklist**:
- [ ] All modules load correctly
- [ ] Play/Pause/Step controls work
- [ ] Visualizations animate smoothly
- [ ] Debug panel updates correctly
- [ ] Speed slider affects animation speed
- [ ] Randomize generates new data
- [ ] Data size slider works
- [ ] Navigation between views works

**2. Future Testing**:
- **Unit Tests**: Test generator logic in isolation
- **Integration Tests**: Test module loading and rendering
- **E2E Tests**: Playwright for full user flows
- **Performance Tests**: Frame rate during animations

---

## Security Considerations

### Current Security Posture
- **No Authentication**: Login page is UI-only (no backend)
- **No Data Storage**: No cookies, localStorage, or backend calls
- **XSS Risk**: Low (no user input stored or rendered)
- **CSRF Risk**: None (no state-changing operations)
- **CDN Dependencies**: Trust third-party CDNs (Tailwind, Lucide, Google Fonts)

### Recommendations
1. Add Subresource Integrity (SRI) hashes to CDN links
2. Implement Content Security Policy (CSP) headers
3. If authentication is added, use proper backend + JWT/sessions

---

## Performance Characteristics

### Load Performance
- **Initial Load**: <2 seconds (HTML + CDN resources)
- **HTML Size**: ~50KB (algorithm.html)
- **Total Page Weight**: ~100-150KB (including Tailwind CDN)

### Runtime Performance
- **Animation FPS**: 30-60 FPS (browser dependent)
- **Generator Overhead**: Minimal (native JS feature)
- **Memory Usage**: Low (~10-20MB per tab)

### Optimization Opportunities
1. **Bundle Tailwind**: Use JIT mode to reduce CSS size
2. **Code Splitting**: Separate modules into files
3. **Service Worker**: Cache resources for offline use
4. **Lazy Loading**: Load algorithm code on demand

---

## Known Limitations

1. **No Backend**: All data ephemeral, no user progress tracking
2. **No Mobile Optimization**: Layout designed for desktop (>1024px)
3. **Limited Algorithms**: Only 6 modules (expandable)
4. **No Code Export**: Can't export generated algorithm steps
5. **Single-User**: No collaboration or sharing features
6. **Accessibility**: Limited keyboard navigation, no screen reader support

---

## Future Architecture Considerations

### Potential Improvements

**1. Module System**:
- Extract modules to separate JSON files
- Dynamic module loading
- Plugin architecture for community contributions

**2. Build Tooling**:
- Introduce Vite for development experience
- Bundle optimization for production
- TypeScript for type safety

**3. State Management**:
- Add persistence (localStorage for progress)
- User accounts (backend integration)
- Shareable visualization links

**4. Testing**:
- Jest for unit tests
- Playwright for E2E tests
- Visual regression testing

---

*Generated by BMad Method document-project workflow*
*Date: 2025-11-19*
