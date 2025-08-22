# AGENTS.md - Development Guidelines for jpmrblood.github.io

## Build/Lint/Test Commands

### Jekyll Build Commands
- **Local development server**: `bundle exec jekyll serve` - Starts development server with live reload
- **Build for production**: `bundle exec jekyll build` - Builds static site to `_site` directory
- **Clean build**: `bundle exec jekyll clean` - Removes generated files

### Ruby/Bundler Commands
- **Install dependencies**: `bundle install` - Installs Ruby gems from Gemfile
- **Update dependencies**: `bundle update` - Updates gems to latest compatible versions
- **Check for outdated gems**: `bundle outdated` - Lists outdated dependencies

### Asset Compilation
- **SCSS compilation**: Handled automatically by Jekyll during build process
- **JavaScript minification**: Already minified files present (main.min.js, search-script.min.js)
- **No dedicated test framework** - This is a static Jekyll site without automated tests

## Code Style Guidelines

### Jekyll/Liquid Template Conventions
- **File naming**: Use lowercase with hyphens (e.g., `about.md`, `tag-archive.md`)
- **Front matter**: Always include YAML front matter with proper indentation
- **Layout usage**: Use appropriate layouts (`single`, `home`, etc.) consistently
- **Include organization**: Use `_includes/` for reusable components
- **Data files**: Store site data in `_data/` directory as YAML files

### HTML/Markdown Standards
- **Semantic HTML**: Use proper semantic elements (`<article>`, `<section>`, `<nav>`)
- **Accessibility**: Include alt text for images, proper heading hierarchy
- **Responsive design**: Ensure content works on mobile and desktop
- **Clean URLs**: Use descriptive, hyphen-separated URLs

### SCSS/CSS Guidelines
- **File organization**: Keep styles organized in `assets/css/` directory
- **Naming convention**: Use BEM methodology for CSS classes
- **Variables**: Use SCSS variables for colors, fonts, and spacing
- **Responsive breakpoints**: Follow mobile-first approach
- **No CSS frameworks**: Custom styles only, following Jekyll theme conventions

### JavaScript Standards
- **File organization**: Place scripts in `assets/js/` directory
- **Minification**: Minified files should have corresponding source maps
- **Library usage**: Use established libraries (jQuery, plugins) as needed
- **No framework**: Vanilla JavaScript with jQuery for DOM manipulation
- **Performance**: Optimize for page load speed and user experience

### Ruby/Jekyll Best Practices
- **Gem management**: Keep Gemfile and Gemfile.lock in sync
- **Plugin usage**: Only use necessary Jekyll plugins
- **Performance**: Optimize image sizes and loading
- **SEO**: Implement proper meta tags and structured data

### Git Workflow
- **Commit messages**: Use descriptive, imperative mood (e.g., "Add new blog post about Docker")
- **Branch naming**: Use descriptive names (e.g., `feature/add-dark-mode`, `fix/broken-link`)
- **Pull requests**: Provide clear descriptions of changes
- **No direct pushes**: Always use feature branches and pull requests

### Content Guidelines
- **Blog posts**: Store in `_posts/` with format `YYYY-MM-DD-title.md`
- **Categories/Tags**: Use consistent categorization system
- **Image optimization**: Compress images and use appropriate formats
- **Alt text**: Always include descriptive alt text for accessibility

### Security Considerations
- **Dependencies**: Keep gems and libraries updated for security patches
- **Input validation**: Sanitize user inputs in forms and comments
- **HTTPS**: Ensure all external links use HTTPS
- **Privacy**: Respect user privacy and data protection regulations

### Performance Optimization
- **Image optimization**: Use appropriate formats (WebP, JPEG, PNG) and sizes
- **Lazy loading**: Implement lazy loading for images and content
- **Caching**: Leverage browser caching and Jekyll's asset handling
- **Minification**: Ensure CSS and JavaScript are properly minified

### Accessibility Standards
- **WCAG compliance**: Follow Web Content Accessibility Guidelines
- **Keyboard navigation**: Ensure all interactive elements are keyboard accessible
- **Screen readers**: Test with screen readers and ensure proper semantic markup
- **Color contrast**: Maintain sufficient color contrast ratios
- **Focus management**: Implement proper focus indicators and management

### Documentation
- **README updates**: Keep README.md current with setup and deployment instructions
- **Code comments**: Add comments for complex logic and configurations
- **Change logs**: Document significant changes and updates
- **Contributing guidelines**: Provide clear contribution guidelines for collaborators