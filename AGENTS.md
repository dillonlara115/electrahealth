# AGENTS.md - AI Agent Guide for ElectraHealth Theme

## Project Overview

**ElectraHealth** is a custom Shopify theme built on top of **Shopify Dawn**, Shopify's reference theme. This is a forked/customized version where we maintain our own customizations while pulling updates from the upstream Dawn repository.

### Key Principles
- **HTML-first, JavaScript-only-as-needed**: Server-rendered HTML with Liquid, minimal client-side JS
- **Performance-focused**: Lean, fast, and reliable code
- **Progressive enhancement**: Works across browsers without polyfills
- **Functional over pixel-perfect**: Semantic markup ensures functionality

## Repository Structure

### Git Configuration
- **Upstream**: `https://github.com/Shopify/dawn.git` (for pulling Dawn updates)
- **Origin**: `git@github.com:dillonlara115/electrahealth.git` (your custom theme repository)

⚠️ **IMPORTANT**: Always pull from `upstream`, push to `origin`. Never push to `upstream`.

### Directory Structure

```
electrahealth/
├── .cursor/              # Cursor IDE rules and guidelines
│   └── rules/           # Development standards (Liquid, sections, blocks, etc.)
├── assets/              # Static files (CSS, JS, images, fonts)
│   ├── *.css           # Stylesheets
│   ├── *.js            # JavaScript files
│   └── *.svg           # SVG icons and images
├── config/             # Theme configuration
│   ├── settings_schema.json    # Theme settings definitions
│   └── settings_data.json      # Current theme settings values
├── layout/             # Theme layouts
│   ├── theme.liquid   # Main theme wrapper
│   └── password.liquid # Password page layout
├── locales/            # Translation files
│   ├── en.default.json        # English translations
│   ├── en.default.schema.json # English schema translations
│   └── [other locales]/       # Multi-language support
├── sections/           # Theme sections (Online Store 2.0)
│   ├── header.liquid
│   ├── footer.liquid
│   ├── featured-product.liquid
│   └── [other sections]/
├── snippets/          # Reusable Liquid snippets
├── templates/         # Page templates
│   ├── index.json            # Homepage
│   ├── product.json           # Product pages
│   ├── collection.json        # Collection pages
│   └── customers/             # Customer account templates
└── translation.yml    # Translation configuration
```

## Development Workflow

### Pulling Updates from Dawn

When Dawn releases updates you want to incorporate:

```bash
# Fetch latest changes from Dawn
git fetch upstream

# Preview what's new
git log HEAD..upstream/main --oneline

# Merge Dawn's updates into your main branch
git merge upstream/main

# Resolve conflicts if any (your changes take priority)
# Then push your merged changes
git push origin main
```

### Making Custom Changes

```bash
# Create a feature branch
git checkout -b feature/your-feature-name

# Make your changes
# ... edit files ...

# Commit and push
git add .
git commit -m "Description of your changes"
git push origin feature/your-feature-name

# Merge to main when ready
git checkout main
git merge feature/your-feature-name
git push origin main
```

## Key Technologies

### Liquid Template Language
- **Server-side rendering**: All HTML rendered by Shopify servers
- **Objects**: `product`, `collection`, `cart`, `customer`, etc.
- **Filters**: String manipulation, formatting, arrays
- **Tags**: Control flow, includes, loops

📖 **Reference**: See `.cursor/rules/liquid.mdc` for complete Liquid syntax guide

### Shopify Online Store 2.0
- **Sections**: Modular, configurable page sections
- **Blocks**: Nested components within sections
- **Schemas**: JSON configuration for theme editor settings
- **JSON Templates**: Section-based page composition

📖 **Reference**: See `.cursor/rules/sections.mdc` and `.cursor/rules/blocks.mdc`

### CSS Architecture
- **CSS Custom Properties**: Used for theming and customization
- **BEM-like naming**: Semantic class names
- **Responsive design**: Mobile-first approach
- **Performance**: Minimal CSS, optimized selectors

### JavaScript
- **Vanilla JS**: No frameworks, modern browser APIs
- **Progressive enhancement**: Works without JS, enhanced with it
- **Modular**: Small, focused utility functions
- **Accessibility**: Keyboard navigation, ARIA attributes

## File Organization Patterns

### Sections (`sections/`)
- One file per section
- Includes Liquid markup, CSS, JavaScript, and schema
- Sections can be added/removed/reordered in theme editor
- Example: `sections/featured-product.liquid`

### Snippets (`snippets/`)
- Reusable Liquid components
- Included via `{% render 'snippet-name' %}`
- Used for DRY (Don't Repeat Yourself) code
- Example: `snippets/product-card.liquid`

### Assets (`assets/`)
- Organized by type: CSS, JS, images
- Namespaced to avoid conflicts
- Minified in production
- Example: `assets/component-product-form.js`

### Locales (`locales/`)
- Translation files for all supported languages
- Two files per locale: `.json` (content) and `.schema.json` (settings)
- English default: `en.default.json`

## Common Tasks

### Adding a New Section

1. Create file: `sections/my-section.liquid`
2. Include Liquid markup with schema
3. Add CSS inline or in assets
4. Add JavaScript if needed
4. Reference `.cursor/rules/sections.mdc` for structure

### Modifying an Existing Section

1. Locate section in `sections/`
2. Make changes preserving schema structure
3. Test in theme editor
4. Consider impact on Dawn updates (may cause merge conflicts)

### Adding Translations

1. Add translation key to `locales/en.default.json`
2. Add same key to other locale files
3. Use in Liquid: `{{ 'general.my_key' | t }}`
4. Schema translations go in `*.schema.json` files

### Creating a New Block

1. Define block in section's schema
2. Add block markup in section template
3. Style block with CSS
4. Reference `.cursor/rules/blocks.mdc`

### Customizing Theme Settings

1. Edit `config/settings_schema.json`
2. Add setting groups, settings, and presets
3. Use settings in Liquid: `{{ settings.my_setting }}`
4. Reference `.cursor/rules/settings-schema.mdc`

## Code Standards

### Liquid Code
- Use semantic HTML
- Prefer `{% render %}` over `{% include %}`
- Use Liquid filters for formatting
- Keep business logic server-side
- Reference `.cursor/rules/liquid.mdc`

### Sections
- Include `{% schema %}` tag at end of file
- Use proper schema structure (settings, presets, blocks)
- Follow naming conventions
- Reference `.cursor/rules/sections.mdc`

### Blocks
- Nested blocks require proper schema structure
- Blocks can have sub-blocks
- Use schema presets for common configurations
- Reference `.cursor/rules/blocks.mdc`

### CSS
- Use CSS custom properties for theming
- Mobile-first responsive design
- Semantic class names
- Avoid inline styles in templates

### JavaScript
- Vanilla JavaScript only
- Modular functions
- Handle errors gracefully
- Progressive enhancement

## Development Tools

### Shopify CLI
```bash
# Start local development server
shopify theme dev

# Pull theme from store
shopify theme pull

# Push theme to store
shopify theme push

# Run theme check
shopify theme check
```

### Theme Check
- Validates Liquid syntax
- Checks for best practices
- Identifies potential issues
- VS Code extension available

### Local Development
- Use `shopify theme dev` for live preview
- Connect to development store
- Hot reload for changes
- Preview in browser

## Important Considerations

### When Modifying Dawn Files

⚠️ **Warning**: Changes to Dawn files may conflict with upstream updates.

**Best Practices**:
1. Minimize changes to core Dawn files
2. Document why changes are necessary
3. Create custom sections instead of modifying Dawn sections when possible
4. Use snippets for reusable customizations
5. When merging Dawn updates, prioritize your customizations

### Merge Conflicts

When merging Dawn updates:
1. Review conflicts carefully
2. Dawn updates are usually improvements/bug fixes
3. Preserve your customizations
4. Test thoroughly after merge
5. Document conflict resolutions

### Performance

- Keep CSS minimal
- Optimize images (use Shopify CDN)
- Minimize JavaScript
- Use lazy loading for images
- Test with Lighthouse

### Accessibility

- Semantic HTML
- ARIA attributes where needed
- Keyboard navigation
- Screen reader support
- Color contrast compliance

## File Naming Conventions

- **Sections**: `kebab-case.liquid` (e.g., `featured-product.liquid`)
- **Snippets**: `kebab-case.liquid` (e.g., `product-card.liquid`)
- **Assets**: `kebab-case.ext` (e.g., `component-product-form.js`)
- **Templates**: `kebab-case.json` (e.g., `product.json`)
- **Locales**: `locale-code.json` (e.g., `en.default.json`)

## Testing Checklist

Before pushing changes:
- [ ] Test in theme editor
- [ ] Test on mobile devices
- [ ] Test all affected pages
- [ ] Run `shopify theme check`
- [ ] Verify translations (if modified)
- [ ] Check browser console for errors
- [ ] Test accessibility features
- [ ] Verify performance (Lighthouse)

## Resources

- **Shopify Docs**: https://shopify.dev/themes
- **Dawn Repository**: https://github.com/Shopify/dawn
- **Liquid Documentation**: https://shopify.dev/docs/api/liquid
- **Theme Check**: https://github.com/shopify/theme-check
- **Shopify CLI**: https://shopify.dev/docs/themes/tools/cli

## Quick Reference

### Common Liquid Objects
- `product` - Current product data
- `collection` - Current collection data
- `cart` - Shopping cart
- `customer` - Logged-in customer
- `settings` - Theme settings
- `section` - Current section settings

### Common Liquid Filters
- `t` - Translation
- `money` - Format money
- `image_url` - Image URL
- `asset_url` - Asset URL
- `link_to` - Create link
- `default` - Default value

### Git Commands
```bash
# Pull Dawn updates
git fetch upstream && git merge upstream/main

# Create feature branch
git checkout -b feature/name

# Push to your repo
git push origin main
```

---

**Remember**: This is a custom theme based on Dawn. Always consider how changes interact with Dawn updates, and prioritize maintainability and performance.

