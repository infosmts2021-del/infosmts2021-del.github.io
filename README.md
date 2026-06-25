# International Travel Website

A Jekyll-based travel guide and blog website hosted on GitHub Pages.

## Features

- 📍 Destination guides for 5 continents
- ✍️ Travel blog for tips and stories
- 📱 Responsive design
- 🌍 SEO optimized
- 💬 Contact form

## Getting Started Locally

### Prerequisites

- Ruby 2.7+
- Jekyll
- Bundler

### Installation

1. Clone the repository
2. Install dependencies:
   ```bash
   bundle install
   ```

3. Run the development server:
   ```bash
   bundle exec jekyll serve
   ```

4. Visit `http://localhost:4000`

## Customization

- Edit `_config.yml` to change site title and description
- Add new pages in the root directory (`.md` files)
- Create blog posts in `_posts/` directory
- Customize styling in `_sass/` or using Jekyll themes

## File Structure

```
.
├── _config.yml          # Site configuration
├── _posts/              # Blog posts
├── about.md            # About page
├── blog.md             # Blog listing
├── contact.md          # Contact page
├── destinations.md     # Destinations guide
├── index.md            # Home page
└── README.md           # This file
```

## Deployment

The site is automatically deployed to GitHub Pages when you push to the main branch.

Visit: `https://infosmts2021-del.github.io`

## Contributing

Have travel tips or destination guides to add? Create a new post in `_posts/` with the filename format:

```
YYYY-MM-DD-your-post-title.md
```

## License

MIT License - feel free to use this template for your own travel blog!
