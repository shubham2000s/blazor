# CSS Optimization Test Checklist

## Pre-Deployment Testing

Before deploying `app.clean.css` or `app.min.css`, run through this checklist to verify no visual regressions.

---

## Visual Regression Checklist

### 1. Homepage (`/`)

- [ ] Hero section displays correctly (heading, subtitle, description)
- [ ] Role selection cards render with proper shadows and hover effects
- [ ] "I am a" dropdown and button alignment correct
- [ ] How It Works fintech cards (green, dark, purple) display properly
- [ ] Key Features section - icons, titles, descriptions aligned
- [ ] Why Choose Us - 2x2 grid + highlight card layout correct
- [ ] FAQ accordion opens/closes smoothly
- [ ] FAQ first item opens by default
- [ ] All buttons have correct colors and hover states

### 2. Navbar (All Pages)

- [ ] Logo displays correctly
- [ ] Tagline visible
- [ ] Navigation links have proper spacing
- [ ] Active link highlighted in blue (#2563EB)
- [ ] Login/Get Started buttons styled correctly
- [ ] Mobile hamburger menu works
- [ ] Responsive collapse behavior

### 3. Login Page (`/login`)

- [ ] Close button (X) visible and functional
- [ ] Form inputs styled correctly
- [ ] Validation messages appear in red
- [ ] "Proceed" button has correct styling
- [ ] Social login button displays

### 4. Footer (All Pages)

- [ ] App download card renders
- [ ] App store badges display
- [ ] Links functional
- [ ] Responsive stacking on mobile

### 5. About Pages (`/about/*`)

- [ ] Page headers display
- [ ] Content sections styled
- [ ] Contact forms work

---

## Responsive Testing

Test at these breakpoints:

| Breakpoint | Test Device/Width |
|------------|-------------------|
| Desktop | 1920px, 1440px, 1280px |
| Tablet | 992px, 768px |
| Mobile | 576px, 414px, 375px, 320px |

### Responsive Checks:

- [ ] **992px**: Fintech cards → 2 columns, Why Choose grid stacks
- [ ] **768px**: All sections stack vertically, padding reduces
- [ ] **767px**: Navbar collapses, font sizes reduce
- [ ] **576px**: App store badges stack vertically

---

## Interactive Elements

- [ ] All buttons have hover effects
- [ ] Card hover animations work (translateY, shadow)
- [ ] FAQ accordion smooth animation
- [ ] Focus states visible (accessibility)
- [ ] Focus-visible outlines on keyboard navigation

---

## Automated Testing

### 1. Lighthouse Audit

```bash
# Run in Chrome DevTools > Lighthouse
# Or use CLI:
npx lighthouse http://localhost:5071 --output html --output-path ./lighthouse-report.html
```

**Target Scores:**
- Performance: > 90
- Accessibility: > 95
- Best Practices: > 90

### 2. CSS Validation

```bash
# W3C CSS Validator
npx css-validator app.clean.css

# Or online: https://jigsaw.w3.org/css-validator/
```

### 3. Visual Diff Testing (Optional)

```bash
# Using BackstopJS
npm install -g backstopjs
backstop init
backstop test
```

**BackstopJS Config (backstop.json):**
```json
{
  "viewports": [
    { "label": "phone", "width": 375, "height": 812 },
    { "label": "tablet", "width": 768, "height": 1024 },
    { "label": "desktop", "width": 1440, "height": 900 }
  ],
  "scenarios": [
    { "label": "Homepage", "url": "http://localhost:5071/" },
    { "label": "Login", "url": "http://localhost:5071/login" }
  ]
}
```

### 4. CSS Coverage Check

```bash
# In Chrome DevTools:
# 1. Open DevTools (F12)
# 2. Press Ctrl+Shift+P
# 3. Type "Coverage" and select "Show Coverage"
# 4. Click reload button in Coverage panel
# 5. Navigate through the site
# 6. Check unused CSS percentage
```

**Target:** < 30% unused CSS

---

## Browser Testing

Test in these browsers:

- [ ] Chrome (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest)
- [ ] Edge (latest)
- [ ] Mobile Safari (iOS)
- [ ] Chrome Mobile (Android)

---

## Quick Smoke Test

Run this minimal test before any deployment:

1. Load homepage → Check hero section visible
2. Click FAQ item → Verify toggle works
3. Hover over fintech card → Verify animation
4. Resize to mobile → Verify responsive layout
5. Navigate to login → Verify form displays
6. Click close (X) → Verify navigation back

---

## Sign-Off

| Test | Tester | Date | Pass/Fail |
|------|--------|------|-----------|
| Visual Regression | | | |
| Responsive | | | |
| Interactive | | | |
| Lighthouse | | | |
| Browser Compat | | | |

**Approved for deployment:** [ ] Yes [ ] No

**Notes:**


