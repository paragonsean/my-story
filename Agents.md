# AGENTS.md

## Overview

Welcome, Agents!  
This document explains how to modify, extend, and maintain the **paragonsean/my-story** site. It covers the repository layout, development setup, best practices, and contribution workflow specific to this project.

---

## 1. Project Structure

- **`index.html` / HTML files** — Main site content and structure.
- **`_posts/`** — (For Jekyll blog) Blog post files in Markdown format.
- **`_layouts/`, `_includes/`, `_config.yml`** — Jekyll templates and configuration.
- **`public/`** — Static assets (images, icons, etc.), if present.
- **`.github/`** — GitHub-specific files (Actions, issue templates, etc.).
- **`AGENTS.md`** — This guide for contributors.
- **`README.md`** — General project information.
- **Other files** — Supporting files (e.g., `CNAME`, `robots.txt`).

> **Note:** This project is currently composed primarily of HTML, but can be extended with Jekyll blog features.  
> If you plan to introduce CSS, JS, or other assets, please follow best practices for file organization.

---

## 2. Setting Up for Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/paragonsean/my-story.git
   cd my-story
   ```

2. **(Optional) Use a local web server**  
   For advanced HTML or JS features, you may want a local server:
   ```bash
   # Python 3.x
   python -m http.server 8000
   # or use another static server tool
   ```

3. **For Jekyll blog development (optional):**
   - Install [Ruby](https://www.ruby-lang.org/en/documentation/installation/) and [Bundler](https://bundler.io/).
   - Install Jekyll and dependencies:
     ```bash
     gem install bundler jekyll
     bundle install
     ```
   - Run the local server:
     ```bash
     bundle exec jekyll serve
     ```
   - Open [http://localhost:4000](http://localhost:4000).

4. **Open in your browser**
   - Visit your local server (see above) or open the HTML file directly if not using Jekyll.

---

## 3. Making Modifications

### a. **Editing HTML Content**

- Update `index.html` or other HTML files for content changes.
- Follow existing structure and indentation for readability.
- Add new sections or pages as needed, linking them appropriately.

### b. **Adding Styles or Scripts**

- If adding CSS, create a `styles.css` (or similar) and link it in your HTML or Jekyll layout.
- For JavaScript, add a `script.js` (or similar) and link it.
- Place all new assets in a dedicated folder (e.g., `css/`, `js/`, `assets/`).

### c. **Adding Images or Static Files**

- Store images and media in a folder like `images/` or `public/`.
- Reference them with relative paths in your HTML or Markdown.

### d. **Configuration**

- For meta tags or SEO, update the `<head>` section of `index.html` or Jekyll layouts.
- For Jekyll, configure site-wide settings in `_config.yml`.

---

## 4. GitHub Actions: Build & Deploy with Jekyll

This repository includes a GitHub Actions workflow for building and deploying a Jekyll site to GitHub Pages.

**Workflow: `.github/workflows/gh-pages.yml`**

```yaml
name: Deploy Jekyll with GitHub Pages dependencies preinstalled

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Build with Jekyll
        uses: actions/jekyll-build-pages@v1
        with:
          source: ./
          destination: ./_site
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### **What it does:**

- **Triggers:** On push to `main` branch and manually from the Actions tab.
- **Builds:** Uses GitHub's Jekyll builder to generate the site.
- **Deploys:** Publishes the built site to GitHub Pages.
- **Permissions:** Sets secure permissions for deployment.
- **Concurrency:** Ensures only one deployment is processed at a time.

To customize the workflow, edit or add YAML files in `.github/workflows/`.

---

## 5. Extending the Site into a Full Jekyll Blog

To turn this static site into a full-featured Jekyll blog:

### a. **Enable Jekyll Blog Structure**

1. **Create a `_posts/` directory:**  
   Blog posts go here as Markdown files (`.md`), named like `YYYY-MM-DD-title.md`.

2. **Add a sample post:**  
   ```markdown
   ---
   layout: post
   title: "My First Blog Post"
   date: 2025-05-22
   categories: [blog]
   ---
   Content of your post goes here!
   ```

3. **Create a blog index page:**  
   Add a `blog.html` or use your home page to list posts:
   ```liquid
   ---
   layout: default
   title: Blog
   ---
   <h1>Blog Posts</h1>
   <ul>
     {% for post in site.posts %}
       <li>
         <a href="{{ post.url }}">{{ post.title }}</a> ({{ post.date | date: "%Y-%m-%d" }})
       </li>
     {% endfor %}
   </ul>
   ```

4. **Update `_config.yml`:**  
   Set site metadata and blog settings. Example:
   ```yaml
   title: My Story Blog
   description: Personal blog built with Jekyll
   baseurl: ""
   url: "https://paragonsean.github.io/my-story"
   ```

### b. **Customize Layouts and Includes**

- Use the `_layouts/` folder for templates (e.g., `default.html`, `post.html`).
- Use `_includes/` for reusable HTML snippets (e.g., navigation, footer).

### c. **Add Navigation**

- Add links to `blog.html`, categories, or tags in your navigation bar/layout.

### d. **Add Plugins (Optional)**

- In GitHub Pages, you can use [supported plugins](https://pages.github.com/versions/).
- Popular plugins: `jekyll-feed` (RSS), `jekyll-seo-tag`, etc.
- Add them to your `_config.yml`:
  ```yaml
  plugins:
    - jekyll-feed
    - jekyll-seo-tag
  ```

### e. **Theming**

- Use a Jekyll theme (`minima` is the default) or create your own in `_layouts/` and `assets/`.
- Change theme in `_config.yml`:
  ```yaml
  theme: minima
  ```

### f. **Categories, Tags, and More**

- Organize posts with categories and tags.
- Add category/tag navigation pages if desired.

---

## 6. Best Practices

- **Consistent Formatting:** Use clean indentation in HTML and Markdown.
- **Descriptive Commits:** Summarize your changes clearly.
- **Test Locally:** Open your changes in a browser before pushing.
- **Accessibility:** Use semantic HTML where possible.
- **Documentation:** Update `README.md` and this file when workflows or structure change.

---

## 7. Submitting Changes

1. **Create a Feature Branch**
   ```bash
   git checkout -b feature/your-description
   ```

2. **Commit Your Changes**
   ```bash
   git add .
   git commit -m "Describe your change"
   ```

3. **Push and Open a Pull Request**
   ```bash
   git push origin feature/your-description
   ```
   - Go to GitHub and open a PR targeting `main`.

---

## 8. Getting Help

- Check the `README.md` for more info.
- Open an issue in GitHub for bugs or questions.
- Use Discussions if enabled.

---

## 9. Additional Notes

- Respect the straightforward, static nature of the project unless there’s consensus to add frameworks or tooling.
- Document any significant structural or workflow changes here.
- Keep this file updated for all agents!

---

Happy blogging! 🚀
