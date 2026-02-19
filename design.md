# Design Document

## Introduction

This document provides the technical design for the education and income-support platform website. The design follows a pure HTML, CSS, and JavaScript approach without frameworks, emphasizing clean architecture, performance, and maintainability.

## System Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         Browser                              │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                    index.html                          │  │
│  │  ┌─────────────────────────────────────────────────┐  │  │
│  │  │  Header (Navigation)                            │  │  │
│  │  └─────────────────────────────────────────────────┘  │  │
│  │  ┌─────────────────────────────────────────────────┐  │  │
│  │  │  Main Content                                   │  │  │
│  │  │  ├─ Hero Section                                │  │  │
│  │  │  ├─ Who This Is For Section                     │  │  │
│  │  │  ├─ What We Offer Section                       │  │  │
│  │  │  ├─ Impact Section                              │  │  │
│  │  │  └─ How It Works Section                        │  │  │
│  │  └─────────────────────────────────────────────────┘  │  │
│  │  ┌─────────────────────────────────────────────────┐  │  │
│  │  │  Footer                                         │  │  │
│  │  └─────────────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────────────┘  │
│         │                    │                    │          │
│         ▼                    ▼                    ▼          │
│   styles.css            script.js          Google Fonts      │
└─────────────────────────────────────────────────────────────┘
```

### File Structure

```
education-support-platform/
├── index.html              # Main HTML structure
├── css/
│   └── styles.css          # All styles (organized in sections)
├── js/
│   └── script.js           # All JavaScript functionality
└── assets/
    └── images/             # Icons and images (if needed)
```

## Component Design

### 1. Navigation Component

**Purpose:** Provides site navigation with responsive behavior and scroll-based styling.

**HTML Structure:**
```html
<nav class="navbar" id="navbar">
  <div class="nav-container">
    <div class="nav-logo">Platform Name</div>
    <button class="nav-toggle" id="navToggle" aria-label="Toggle navigation">
      <span></span>
      <span></span>
      <span></span>
    </button>
    <ul class="nav-menu" id="navMenu">
      <li><a href="#hero" class="nav-link">Home</a></li>
      <li><a href="#who" class="nav-link">Who We Serve</a></li>
      <li><a href="#offers" class="nav-link">What We Offer</a></li>
      <li><a href="#impact" class="nav-link">Impact</a></li>
      <li><a href="#how" class="nav-link">How It Works</a></li>
    </ul>
  </div>
</nav>
```

**CSS Classes:**
- `.navbar` - Base navigation container
- `.navbar--scrolled` - Applied when user scrolls past hero (adds background blur)
- `.nav-menu--active` - Applied to show mobile menu
- `.nav-toggle` - Hamburger menu button (hidden on desktop)

**JavaScript Behavior:**
- Toggle mobile menu on hamburger click
- Add `.navbar--scrolled` class when scroll > 100px
- Smooth scroll to sections on link click
- Close mobile menu after navigation

### 2. Hero Banner Component

**Purpose:** Primary landing section with headline, subheadline, and call-to-action.

**HTML Structure:**
```html
<section class="hero" id="hero">
  <div class="hero-content">
    <h1 class="hero-title animate-fade-in">Empowering Dreams Through Education</h1>
    <p class="hero-subtitle animate-fade-in">Supporting SC/ST and low-income students with resources, mentorship, and opportunities</p>
    <div class="hero-cta animate-fade-in">
      <button class="btn btn-primary">Get Started</button>
      <button class="btn btn-secondary">Learn More</button>
    </div>
  </div>
  <div class="hero-background"></div>
</section>
```

**CSS Design:**
- Gradient background: `linear-gradient(135deg, #1e3a8a 0%, #14b8a6 100%)`
- Min-height: `80vh`
- Flexbox centering for content
- `.hero-background` - Decorative gradient overlay with opacity

**Animation:**
- Fade-in on page load (0.8s duration, staggered by 0.2s)
- Uses CSS animation triggered by `.animate-fade-in` class

### 3. Card Component

**Purpose:** Reusable container for content with glassmorphism and hover effects.

**HTML Structure:**
```html
<div class="card animate-on-scroll">
  <div class="card-icon">
    <!-- Icon or emoji -->
  </div>
  <h3 class="card-title">Title</h3>
  <p class="card-description">Description text</p>
</div>
```

**CSS Design:**
- Background: `rgba(255, 255, 255, 0.1)`
- Backdrop filter: `blur(10px)`
- Border: `1px solid rgba(255, 255, 255, 0.2)`
- Border-radius: `16px`
- Box-shadow: `0 8px 32px rgba(0, 0, 0, 0.1)`
- Padding: `2rem`

**Hover Effect:**
```css
.card:hover {
  transform: translateY(-8px) scale(1.02);
  box-shadow: 0 12px 40px rgba(20, 184, 166, 0.3);
}
```

**Animation:**
- Slide-up on scroll (triggered by Intersection Observer)
- Transition: `all 0.4s cubic-bezier(0.4, 0, 0.2, 1)`

### 4. Counter Component

**Purpose:** Animated numerical display for impact metrics.

**HTML Structure:**
```html
<div class="counter-item animate-on-scroll">
  <div class="counter-number" data-target="5000">0</div>
  <div class="counter-label">Students Helped</div>
</div>
```

**JavaScript Behavior:**
- Triggered when element enters viewport
- Animates from 0 to `data-target` value
- Duration: 2000ms
- Easing: Custom easing function for smooth acceleration/deceleration
- Formats numbers with commas (e.g., 5,000)

**Algorithm:**
```javascript
function animateCounter(element, target, duration) {
  const start = 0;
  const startTime = performance.now();
  
  function update(currentTime) {
    const elapsed = currentTime - startTime;
    const progress = Math.min(elapsed / duration, 1);
    const eased = easeOutQuart(progress);
    const current = Math.floor(eased * target);
    
    element.textContent = formatNumber(current);
    
    if (progress < 1) {
      requestAnimationFrame(update);
    }
  }
  
  requestAnimationFrame(update);
}
```

### 5. Section Components

**Who This Is For Section:**
- Grid layout: 2 columns on desktop, 1 on mobile
- Contains 2 persona cards (Students, Mentors)
- Each card has icon, title, description

**What We Offer Section:**
- Grid layout: 3 columns on desktop, 1 on mobile
- Contains 3+ offering cards
- Staggered animation (100ms delay between cards)

**Impact Section:**
- Flexbox layout for counter items
- Dark background with gradient
- 3-4 counter components
- Centered alignment

**How It Works Section:**
- Vertical timeline on mobile, horizontal on desktop
- 3-5 step cards with numbering
- Visual connectors (lines/arrows) between steps

## CSS Architecture

### CSS Custom Properties (Variables)

```css
:root {
  /* Colors */
  --color-primary: #1e3a8a;
  --color-secondary: #14b8a6;
  --color-accent: #fbbf24;
  --color-text: #1f2937;
  --color-text-light: #6b7280;
  --color-bg: #ffffff;
  --color-bg-dark: #0f172a;
  
  /* Gradients */
  --gradient-primary: linear-gradient(135deg, #1e3a8a 0%, #14b8a6 100%);
  --gradient-secondary: linear-gradient(135deg, #14b8a6 0%, #fbbf24 100%);
  
  /* Spacing */
  --spacing-xs: 0.5rem;
  --spacing-sm: 1rem;
  --spacing-md: 2rem;
  --spacing-lg: 4rem;
  --spacing-xl: 6rem;
  
  /* Typography */
  --font-family: 'Poppins', sans-serif;
  --font-size-base: 1rem;
  --font-size-lg: 1.25rem;
  --font-size-xl: 1.5rem;
  --font-size-2xl: 2rem;
  --font-size-3xl: 3rem;
  
  /* Shadows */
  --shadow-sm: 0 2px 8px rgba(0, 0, 0, 0.1);
  --shadow-md: 0 8px 32px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 12px 40px rgba(0, 0, 0, 0.15);
  --shadow-glow: 0 0 20px rgba(20, 184, 166, 0.4);
  
  /* Border Radius */
  --radius-sm: 8px;
  --radius-md: 16px;
  --radius-lg: 24px;
  
  /* Transitions */
  --transition-fast: 0.2s ease;
  --transition-base: 0.3s ease;
  --transition-slow: 0.5s ease;
  
  /* Breakpoints (for reference in media queries) */
  --breakpoint-mobile: 768px;
  --breakpoint-tablet: 1024px;
}
```

### CSS Organization

```css
/* 1. CSS Reset & Base Styles */
/* 2. CSS Variables */
/* 3. Typography */
/* 4. Layout Utilities */
/* 5. Components */
/*    - Navigation */
/*    - Hero */
/*    - Cards */
/*    - Buttons */
/*    - Counters */
/*    - Sections */
/*    - Footer */
/* 6. Animations */
/* 7. Media Queries */
```

## JavaScript Architecture

### Module Organization

```javascript
// 1. Utility Functions
// 2. Animation Functions
// 3. Navigation Functions
// 4. Scroll Observer
// 5. Counter Animation
// 6. Event Listeners
// 7. Initialization
```

### Key Functions

**1. Intersection Observer Setup**
```javascript
const observerOptions = {
  threshold: 0.2,
  rootMargin: '0px 0px -100px 0px'
};

const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      
      // Trigger counter animation if it's a counter element
      if (entry.target.classList.contains('counter-number')) {
        animateCounter(entry.target);
      }
      
      // Unobserve after animation
      observer.unobserve(entry.target);
    }
  });
}, observerOptions);
```

**2. Smooth Scroll Navigation**
```javascript
function smoothScrollTo(targetId) {
  const target = document.querySelector(targetId);
  const navHeight = document.querySelector('.navbar').offsetHeight;
  const targetPosition = target.offsetTop - navHeight;
  
  window.scrollTo({
    top: targetPosition,
    behavior: 'smooth'
  });
}
```

**3. Mobile Menu Toggle**
```javascript
function toggleMobileMenu() {
  const navMenu = document.getElementById('navMenu');
  const navToggle = document.getElementById('navToggle');
  
  navMenu.classList.toggle('nav-menu--active');
  navToggle.classList.toggle('active');
}
```

**4. Scroll-based Navbar Styling**
```javascript
function handleNavbarScroll() {
  const navbar = document.getElementById('navbar');
  const scrollThreshold = 100;
  
  if (window.scrollY > scrollThreshold) {
    navbar.classList.add('navbar--scrolled');
  } else {
    navbar.classList.remove('navbar--scrolled');
  }
}
```

## Responsive Design Strategy

### Breakpoints

- Mobile: 0 - 768px (base styles)
- Tablet: 769px - 1024px
- Desktop: 1025px+

### Mobile-First Approach

1. Write base styles for mobile devices
2. Use `min-width` media queries to enhance for larger screens
3. Stack elements vertically on mobile, use grid/flexbox on desktop

### Key Responsive Patterns

**Navigation:**
- Mobile: Hamburger menu with slide-down drawer
- Desktop: Horizontal menu bar

**Grid Layouts:**
- Mobile: 1 column
- Tablet: 2 columns
- Desktop: 3-4 columns

**Typography:**
- Mobile: Smaller font sizes (base 16px)
- Desktop: Larger font sizes (base 18px)
- Use `clamp()` for fluid typography

**Spacing:**
- Mobile: Reduced padding/margins
- Desktop: Increased spacing for breathing room

## Animation Design

### Animation Types

**1. Fade-in (Hero elements)**
```css
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
```

**2. Slide-up (Cards on scroll)**
```css
.animate-on-scroll {
  opacity: 0;
  transform: translateY(50px);
  transition: all 0.6s cubic-bezier(0.4, 0, 0.2, 1);
}

.animate-on-scroll.visible {
  opacity: 1;
  transform: translateY(0);
}
```

**3. Scale-glow (Card hover)**
```css
.card {
  transition: all 0.3s ease;
}

.card:hover {
  transform: translateY(-8px) scale(1.02);
  box-shadow: var(--shadow-glow);
}
```

### Animation Timing

- Page load animations: 0.8s - 1s
- Scroll-triggered animations: 0.4s - 0.6s
- Hover effects: 0.2s - 0.3s
- Counter animations: 2s

### Easing Functions

- Standard: `cubic-bezier(0.4, 0, 0.2, 1)` (Material Design standard)
- Bounce: `cubic-bezier(0.68, -0.55, 0.265, 1.55)`
- Counter: Custom easeOutQuart for smooth deceleration

### Reduced Motion Support

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

## Performance Optimization

### CSS Optimization

1. Use CSS transforms instead of position changes
2. Minimize repaints with `will-change` property on animated elements
3. Use CSS containment for isolated components
4. Minimize CSS specificity for faster selector matching

### JavaScript Optimization

1. Cache DOM queries in variables
2. Use event delegation for multiple similar elements
3. Debounce scroll event handlers
4. Use `requestAnimationFrame` for animations
5. Unobserve elements after animation completes

### Loading Strategy

1. Inline critical CSS in `<head>`
2. Defer non-critical JavaScript
3. Use font-display: swap for web fonts
4. Lazy load images below the fold (if any)
5. Minimize HTTP requests

### Performance Targets

- First Contentful Paint: < 1.5s
- Time to Interactive: < 3s
- Total page size: < 500KB
- Lighthouse Performance Score: > 90

## Accessibility Considerations

### Semantic HTML

- Use `<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`
- Proper heading hierarchy (h1 → h2 → h3)
- Use `<button>` for interactive elements

### ARIA Labels

- `aria-label` for hamburger menu
- `aria-expanded` for mobile menu state
- `aria-current` for active navigation link

### Keyboard Navigation

- All interactive elements focusable with Tab
- Visible focus indicators
- Skip to main content link

### Color Contrast

- Ensure 4.5:1 ratio for normal text
- Ensure 3:1 ratio for large text
- Test with contrast checker tools

## Data Flow

### Page Load Sequence

```
1. HTML parsed
2. CSS loaded and applied
3. Google Fonts loaded
4. JavaScript executed
   ├─ Initialize Intersection Observer
   ├─ Attach event listeners
   ├─ Observe all .animate-on-scroll elements
   └─ Trigger hero animations
5. User interactions handled
```

### Scroll Event Flow

```
User scrolls
    ↓
Scroll event fired
    ↓
handleNavbarScroll() called (debounced)
    ↓
Intersection Observer checks elements
    ↓
Elements enter viewport
    ↓
Add 'visible' class → CSS transition
    ↓
If counter element → animateCounter()
```

### Navigation Click Flow

```
User clicks nav link
    ↓
Event listener triggered
    ↓
preventDefault()
    ↓
Extract target section ID
    ↓
smoothScrollTo(targetId)
    ↓
Close mobile menu (if open)
```

## Implementation Notes

### Browser Compatibility

- Target: Modern browsers (Chrome, Firefox, Safari, Edge)
- Minimum: ES6 support required
- Fallbacks: Provide basic styles for older browsers

### Testing Checklist

- [ ] Test on mobile devices (iOS, Android)
- [ ] Test on tablets
- [ ] Test on desktop (various screen sizes)
- [ ] Test keyboard navigation
- [ ] Test with screen reader
- [ ] Test with reduced motion enabled
- [ ] Validate HTML
- [ ] Validate CSS
- [ ] Check JavaScript console for errors
- [ ] Test smooth scrolling
- [ ] Test counter animations
- [ ] Test mobile menu toggle
- [ ] Test all hover effects
- [ ] Check loading performance

### Future Enhancements

- Add form for user registration
- Integrate backend API for dynamic content
- Add testimonials section
- Implement dark mode toggle
- Add language switcher for multilingual support
