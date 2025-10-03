# Deceptive Pattern Paper

A Jekyll-based research website for showcasing academic research on deceptive patterns in web interfaces.

## Features

- **Modern Design**: Clean, professional layout with responsive design
- **Research Showcase**: Dedicated sections for research projects and publications
- **SEO Optimized**: Built-in meta tags and structured data
- **Fast Loading**: Optimized CSS and JavaScript with lazy loading
- **Accessible**: Follows web accessibility best practices
- **Mobile Responsive**: Works perfectly on all device sizes
- **Easy Customization**: Simple markdown-based content management

## Quick Start

### Prerequisites

- Ruby 2.7 or higher
- Bundler gem
- Git

### Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/wi-pi/deceptive-pattern-paper.git
   cd deceptive-pattern-paper
   ```

2. **Install dependencies**:
   ```bash
   bundle install
   ```

3. **Serve the site locally**:
   ```bash
   bundle exec jekyll serve
   ```

4. **Open your browser** and navigate to `http://localhost:4000`

## Customization

### 1. Site Configuration

Edit `_config.yml` to customize your site:

```yaml
# Site settings
title: "Your Research Lab"
email: "your-email@university.edu"
description: "Your research description"
url: "https://your-username.github.io"

# Author information
author:
  name: "Your Name"
  email: "your-email@university.edu"
  bio: "Your bio"
  avatar: "/assets/images/avatar.jpg"

# Navigation
navigation:
  - title: "Home"
    url: "/"
  - title: "Research"
    url: "/research/"
  - title: "Publications"
    url: "/publications/"
  - title: "About"
    url: "/about/"

# Social links
social:
  - name: "GitHub"
    url: "https://github.com/your-username"
  - name: "Twitter"
    url: "https://twitter.com/your-twitter"
  - name: "LinkedIn"
    url: "https://linkedin.com/in/your-profile"
```

### 2. Adding Research Projects

Create new research projects in the `_research/` directory:

```markdown
---
layout: research
title: "Your Research Project"
subtitle: "Brief description"
authors: "Author 1, Author 2"
venue: "Conference Name"
year: "2024"
image: "/assets/images/research/project.jpg"
paper_url: "https://arxiv.org/abs/example"
code_url: "https://github.com/your-username/project"
demo_url: "https://demo.your-site.com"
abstract: "Your abstract here..."
---

Your detailed project description here...
```

### 3. Adding Publications

Create new publications in the `_publications/` directory:

```markdown
---
layout: publication
title: "Your Paper Title"
authors: "Author 1, Author 2"
venue: "Conference Name"
year: "2024"
pages: "123-134"
paper_url: "https://arxiv.org/abs/example"
code_url: "https://github.com/your-username/project"
bibtex: |
  @inproceedings{author2024title,
    title={Your Paper Title},
    author={Author 1 and Author 2},
    booktitle={Conference Name},
    year={2024}
  }
---

Your paper description here...
```

### 4. Adding Blog Posts

Create new blog posts in the `_posts/` directory:

```markdown
---
layout: post
title: "Your Blog Post Title"
date: 2024-01-01 12:00:00 -0000
categories: research update
author: "Your Name"
---

Your blog post content here...
```

### 5. Customizing Pages

Edit the main pages:
- `index.md` - Homepage
- `about.md` - About page
- `research.md` - Research overview
- `publications.md` - Publications overview

### 6. Adding Images

Place your images in the `assets/images/` directory:

```
assets/images/
├── hero-image.jpg          # Homepage hero image
├── about-image.jpg         # About section image
├── avatar.jpg              # Author avatar
├── favicon.ico             # Site favicon
├── og-image.jpg            # Social sharing image
└── research/
    ├── project1.jpg        # Research project images
    └── project2.jpg
```

## Content Types

### Research Projects (`_research/`)
- Detailed project pages with abstracts, links, and media
- Support for paper URLs, code repositories, demos, and videos
- Automatic listing on the research page

### Publications (`_publications/`)
- Academic papers with BibTeX support
- Links to papers, code, demos, and videos
- Automatic listing on the publications page

### Blog Posts (`_posts/`)
- Regular updates and announcements
- Support for categories and tags
- Automatic RSS feed generation

### Static Pages
- About, contact, and other informational pages
- Custom layouts and content

## Deployment

### GitHub Pages (Automated with GitHub Actions)

This repository includes a GitHub Actions workflow (`.github/workflows/jekyll.yml`) that automatically builds and deploys the site to GitHub Pages on every push to the `main` branch.

**Setup Steps:**

1. **Push to GitHub**:
   ```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

2. **Enable GitHub Pages**:
   - Go to your repository settings
   - Navigate to "Pages" section
   - Under "Source", select "GitHub Actions"

3. **Automatic Deployment**:
   - The workflow will automatically trigger on push to `main`
   - View progress in the "Actions" tab of your repository
   - Once complete, your site will be live

4. **Your site will be available at**:
   `https://wi-pi.github.io/deceptive-pattern-paper`

**Manual Deployment:**
You can also trigger the deployment manually from the "Actions" tab using the workflow dispatch option.

### Other Hosting Platforms

The site can be deployed to any static hosting platform:
- Netlify
- Vercel
- AWS S3 + CloudFront
- Firebase Hosting

## Customization Options

### Styling

- **Colors**: Edit CSS variables in `assets/css/main.css`
- **Fonts**: Change font families in the CSS file
- **Layout**: Modify the HTML templates in `_layouts/`

### Functionality

- **Navigation**: Update navigation items in `_config.yml`
- **Social Links**: Add/remove social media links
- **Analytics**: Add Google Analytics or other tracking codes
- **Comments**: Integrate Disqus or other comment systems

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)
- Mobile browsers (iOS Safari, Chrome Mobile)

## Performance

The site is optimized for performance:
- Minified CSS and JavaScript
- Optimized images with lazy loading
- Efficient Jekyll build process
- CDN-ready static assets

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test locally
5. Submit a pull request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Inspired by the [Nerfies website](https://nerfies.github.io/)
- Built with [Jekyll](https://jekyllrb.com/)
- Uses [Inter font](https://rsms.me/inter/) for typography
- Icons from [Feather Icons](https://feathericons.com/)

## Support

If you encounter any issues or have questions:

1. Check the [Jekyll documentation](https://jekyllrb.com/docs/)
2. Search existing [GitHub issues](https://github.com/wi-pi/deceptive-pattern-paper/issues)
3. Create a new issue with detailed information

## Changelog

### Version 1.0.0
- Initial release
- Modern responsive design
- Research and publication support
- SEO optimization
- Mobile-friendly navigation
- Performance optimizations
- GitHub Actions workflow for automated deployment
