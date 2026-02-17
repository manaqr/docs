# GitHub Pages Setup Instructions

This documentation site is ready to be deployed to GitHub Pages. Follow these steps to enable it:

## Automatic Deployment (Recommended)

1. **Go to Repository Settings**
   - Navigate to https://github.com/manaqr/docs/settings

2. **Enable GitHub Pages**
   - Go to **Settings** → **Pages** (in the left sidebar)
   - Under "Build and deployment":
     - **Source**: Select "GitHub Actions"
   - Click **Save**

3. **Merge this Pull Request**
   - Once the PR is merged to `main`, the GitHub Actions workflow will automatically build and deploy the site

4. **Access Your Documentation**
   - The site will be available at: https://manaqr.github.io/docs/
   - It may take a few minutes for the first deployment to complete

## Manual Deployment (Alternative)

If you prefer to deploy from a branch:

1. **Go to Repository Settings** → **Pages**
2. Under "Build and deployment":
   - **Source**: Select "Deploy from a branch"
   - **Branch**: Select `main` and `/root` folder
   - Click **Save**

3. GitHub Pages will automatically detect the Jekyll configuration and build the site

## Verify Deployment

After enabling GitHub Pages:

1. Go to **Settings** → **Pages**
2. You should see: "Your site is live at https://manaqr.github.io/docs/"
3. Click the URL to view your documentation

## Custom Domain (Optional)

To use a custom domain like docs.manaqr.io:

1. Add a CNAME record in your DNS settings:
   ```
   docs.manaqr.io → manaqr.github.io
   ```

2. In **Settings** → **Pages**, under "Custom domain":
   - Enter: `docs.manaqr.io`
   - Click **Save**
   - Wait for DNS check to complete
   - Enable "Enforce HTTPS" (recommended)

## Troubleshooting

### Build Errors

If the GitHub Actions workflow fails:

1. Check the workflow logs: **Actions** tab → Select the failed workflow
2. Common issues:
   - Missing dependencies (should auto-install from Gemfile)
   - YAML syntax errors in front matter
   - Invalid Jekyll configuration

### 404 Errors

If pages show 404:

1. Verify the `baseurl` in `_config.yml` matches your repository name
2. Current setting: `baseurl: "/docs"` (correct for https://manaqr.github.io/docs/)
3. For custom domain, set: `baseurl: ""`

### Styling Issues

If CSS doesn't load:

1. Check browser console for errors
2. Verify `assets/css/style.scss` exists
3. Clear browser cache and refresh

## Documentation Structure

```
docs/
├── index.md              # Home page
├── getting-started.md    # Quick start guide
├── user-guide.md         # Comprehensive user guide
├── api-reference.md      # Complete API documentation
├── faq.md               # Frequently asked questions
├── contributing.md      # Contributing guidelines
├── _config.yml          # Jekyll configuration
├── Gemfile              # Ruby dependencies
└── assets/
    └── css/
        └── style.scss   # Custom styles
```

## Local Development

To test the documentation locally:

1. Install Ruby and Bundler
2. Run:
   ```bash
   bundle install
   bundle exec jekyll serve
   ```
3. Visit http://localhost:4000/docs/

## Need Help?

- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
