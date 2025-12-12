# CSS Optimization Tools & Commands

## Recommended Tools

| Tool | Purpose | Install |
|------|---------|---------|
| PostCSS | CSS processing pipeline | `npm i -D postcss postcss-cli` |
| cssnano | Minification | `npm i -D cssnano` |
| Autoprefixer | Vendor prefixes | `npm i -D autoprefixer` |
| PurgeCSS | Remove unused CSS | `npm i -D purgecss` |
| Stylelint | CSS linting | `npm i -D stylelint` |

---

## 1. PostCSS Pipeline Setup

### Install Dependencies

```bash
npm init -y
npm install -D postcss postcss-cli cssnano autoprefixer postcss-combine-media-query
```

### postcss.config.js

```javascript
module.exports = {
  plugins: [
    require('autoprefixer'),
    require('postcss-combine-media-query'),
    require('cssnano')({
      preset: ['default', {
        discardComments: { removeAll: true },
        normalizeWhitespace: true,
        minifyFontValues: true,
        minifyGradients: true,
      }]
    })
  ]
}
```

### Build Commands

```bash
# Development (readable output)
npx postcss app.css -o app.build.css --no-map

# Production (minified)
npx postcss app.css -o app.min.css

# Watch mode
npx postcss app.css -o app.min.css --watch
```

---

## 2. PurgeCSS Configuration

### Install

```bash
npm install -D purgecss
```

### purgecss.config.js

```javascript
module.exports = {
  content: [
    './Components/**/*.razor',
    './Components/**/*.cs',
    './wwwroot/**/*.html',
    './wwwroot/**/*.js'
  ],
  css: ['./wwwroot/app.css'],
  output: './wwwroot/app.purged.css',
  safelist: [
    // Blazor dynamic classes
    /^blazor/,
    /^valid/,
    /^invalid/,
    /^validation/,
    // Bootstrap utilities
    /^d-/,
    /^col-/,
    /^row/,
    /^container/,
    /^btn/,
    /^form/,
    // Dynamic state classes
    /active$/,
    /open$/,
    /-active$/,
    /-open$/,
  ],
  // Preserve keyframes
  keyframes: true,
  // Preserve font-face
  fontFace: true,
}
```

### Run PurgeCSS

```bash
npx purgecss --config purgecss.config.js
```

---

## 3. Stylelint Configuration

### Install

```bash
npm install -D stylelint stylelint-config-standard stylelint-order
```

### .stylelintrc.json

```json
{
  "extends": "stylelint-config-standard",
  "plugins": ["stylelint-order"],
  "rules": {
    "no-duplicate-selectors": true,
    "declaration-block-no-duplicate-properties": true,
    "no-descending-specificity": "warning",
    "color-hex-length": "long",
    "selector-class-pattern": null,
    "order/properties-alphabetical-order": true
  }
}
```

### Run Stylelint

```bash
# Check for issues
npx stylelint "wwwroot/**/*.css"

# Auto-fix issues
npx stylelint "wwwroot/**/*.css" --fix
```

---

## 4. Complete Build Pipeline

### package.json Scripts

```json
{
  "scripts": {
    "css:lint": "stylelint 'wwwroot/**/*.css'",
    "css:lint:fix": "stylelint 'wwwroot/**/*.css' --fix",
    "css:purge": "purgecss --config purgecss.config.js",
    "css:build": "postcss wwwroot/app.css -o wwwroot/app.build.css",
    "css:minify": "postcss wwwroot/app.css -o wwwroot/app.min.css",
    "css:all": "npm run css:lint && npm run css:purge && npm run css:minify"
  }
}
```

### Run Full Pipeline

```bash
npm run css:all
```

---

## 5. CI/CD Integration

### GitHub Actions (css-optimize.yml)

```yaml
name: CSS Optimization

on:
  push:
    paths:
      - '**/*.css'

jobs:
  optimize:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Lint CSS
        run: npm run css:lint
      
      - name: Build CSS
        run: npm run css:minify
      
      - name: Upload optimized CSS
        uses: actions/upload-artifact@v3
        with:
          name: optimized-css
          path: wwwroot/app.min.css
```

---

## 6. Manual Optimization Commands

### Using Clean-CSS CLI

```bash
npm install -g clean-css-cli

# Minify with source maps
cleancss -o app.min.css app.css --source-map

# Level 2 optimization (aggressive)
cleancss -O2 -o app.min.css app.css
```

### Using CSSO

```bash
npm install -g csso-cli

# Optimize
csso app.css -o app.min.css

# With restructuring
csso app.css -o app.min.css --restructure-off
```

---

## 7. Measure CSS Size

```bash
# File size
ls -lh app.css app.min.css

# Gzipped size (what browsers actually download)
gzip -c app.min.css | wc -c

# Compare before/after
echo "Original: $(wc -c < app.css) bytes"
echo "Minified: $(wc -c < app.min.css) bytes"
echo "Gzipped:  $(gzip -c app.min.css | wc -c) bytes"
```

---

## 8. VSCode Extensions

Install these for better CSS workflow:

- **Stylelint** - Real-time linting
- **CSS Peek** - Peek at CSS definitions
- **IntelliSense for CSS** - Autocomplete
- **Prettier** - Code formatting

### settings.json

```json
{
  "css.validate": true,
  "stylelint.enable": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.stylelint": true
  }
}
```

---

## Quick Reference

| Task | Command |
|------|---------|
| Lint CSS | `npx stylelint "**/*.css"` |
| Fix lint issues | `npx stylelint "**/*.css" --fix` |
| Minify CSS | `npx postcss app.css -o app.min.css` |
| Purge unused | `npx purgecss --config purgecss.config.js` |
| Check size | `ls -lh *.css` |
| Validate CSS | `npx css-validator app.css` |


