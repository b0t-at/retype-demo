---
label: Deployment
icon: rocket
order: 100
---

# Deployment Guide

Learn how to deploy your Retype site to various hosting platforms.

## Build Your Site

Before deployment, build your site:

```bash
retype build
```

This generates static files in the `.retype` folder (or your configured output directory).

## GitHub Pages

GitHub Pages is a free hosting service for static sites.

### Method 1: GitHub Actions (Recommended)

Create `.github/workflows/retype.yml`:

```yaml
name: Publish Retype

on:
  workflow_dispatch:
  push:
    branches:
      - main

jobs:
  publish:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - uses: actions/setup-node@v3
        with:
          node-version: 18
      
      - name: Install Retype
        run: npm install retypeapp --global
      
      - name: Build
        run: retype build
      
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./.retype
```

Enable GitHub Pages in your repository settings:
1. Go to Settings > Pages
2. Source: Deploy from a branch
3. Branch: `gh-pages` / `root`
4. Save

### Method 2: Manual Deployment

```bash
# Build the site
retype build

# Push to gh-pages branch
cd .retype
git init
git add -A
git commit -m "Deploy to GitHub Pages"
git branch -M gh-pages
git remote add origin https://github.com/username/repo.git
git push -f origin gh-pages
```

## Netlify

Netlify offers easy deployment with continuous integration.

### Deploy via Netlify UI

1. Push your code to GitHub
2. Log in to [Netlify](https://netlify.com)
3. Click "New site from Git"
4. Select your repository
5. Configure build settings:
   - **Build command**: `npm install retypeapp --global && retype build`
   - **Publish directory**: `.retype`
6. Click "Deploy site"

### Deploy via Netlify CLI

```bash
# Install Netlify CLI
npm install -g netlify-cli

# Build your site
retype build

# Deploy
netlify deploy --prod --dir=.retype
```

### netlify.toml Configuration

Create `netlify.toml` in your project root:

```toml
[build]
  command = "npm install retypeapp --global && retype build"
  publish = ".retype"

[[redirects]]
  from = "/*"
  to = "/404.html"
  status = 404
```

## Vercel

Deploy to Vercel with zero configuration.

### Deploy via Vercel UI

1. Push your code to GitHub
2. Log in to [Vercel](https://vercel.com)
3. Click "Import Project"
4. Select your repository
5. Configure:
   - **Build Command**: `npm install retypeapp --global && retype build`
   - **Output Directory**: `.retype`
6. Click "Deploy"

### Deploy via Vercel CLI

```bash
# Install Vercel CLI
npm install -g vercel

# Build your site
retype build

# Deploy
vercel --prod
```

### vercel.json Configuration

Create `vercel.json`:

```json
{
  "buildCommand": "npm install retypeapp --global && retype build",
  "outputDirectory": ".retype",
  "cleanUrls": true,
  "trailingSlash": false
}
```

## Cloudflare Pages

Deploy to Cloudflare Pages for global CDN distribution.

### Deployment Steps

1. Push your code to GitHub
2. Log in to [Cloudflare Pages](https://pages.cloudflare.com)
3. Click "Create a project"
4. Connect your Git repository
5. Configure build settings:
   - **Build command**: `npm install retypeapp --global && retype build`
   - **Build output directory**: `.retype`
6. Click "Save and Deploy"

### Custom Domain

1. Go to your project settings
2. Click "Custom domains"
3. Add your domain
4. Update DNS records as instructed

## Azure Static Web Apps

Deploy to Microsoft Azure Static Web Apps.

### GitHub Actions Workflow

```yaml
name: Azure Static Web Apps CI/CD

on:
  push:
    branches:
      - main

jobs:
  build_and_deploy_job:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Build And Deploy
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          action: "upload"
          app_location: "/"
          api_location: ""
          output_location: ".retype"
          app_build_command: "npm install retypeapp --global && retype build"
```

## AWS S3 + CloudFront

Deploy to AWS for enterprise-grade hosting.

### Deployment Script

```bash
#!/bin/bash

# Build the site
retype build

# Sync to S3
aws s3 sync .retype/ s3://your-bucket-name --delete

# Invalidate CloudFront cache
aws cloudfront create-invalidation \
  --distribution-id YOUR_DISTRIBUTION_ID \
  --paths "/*"
```

### S3 Bucket Configuration

1. Create an S3 bucket
2. Enable static website hosting
3. Set bucket policy for public read access:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```

## Self-Hosted

Deploy to your own server using any web server.

### Apache

1. Build your site: `retype build`
2. Copy `.retype` contents to your web root
3. Configure `.htaccess`:

```apache
# .htaccess
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ /$1.html [L]
```

### Nginx

1. Build your site: `retype build`
2. Copy `.retype` contents to your web root
3. Configure nginx:

```nginx
server {
    listen 80;
    server_name docs.example.com;
    root /var/www/docs;
    
    location / {
        try_files $uri $uri.html $uri/ =404;
    }
    
    error_page 404 /404.html;
}
```

## Docker

Deploy using Docker containers.

### Dockerfile

```dockerfile
FROM nginx:alpine

COPY .retype /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### Build and Run

```bash
# Build your site
retype build

# Build Docker image
docker build -t my-docs .

# Run container
docker run -d -p 80:80 my-docs
```

### docker-compose.yml

```yaml
version: '3'
services:
  docs:
    build: .
    ports:
      - "80:80"
    restart: unless-stopped
```

## Custom Domain

### Configure DNS

Add DNS records for your custom domain:

**For apex domain (example.com):**
- Type: A
- Host: @
- Value: [Your hosting provider's IP]

**For subdomain (docs.example.com):**
- Type: CNAME
- Host: docs
- Value: [Your hosting provider's domain]

### Update retype.yml

```yaml
url: docs.example.com
```

## CI/CD Best Practices

!!! Success Deployment Tips
1. **Automate** - Use CI/CD for automatic deployments
2. **Test first** - Build and test locally before deploying
3. **Use environments** - Separate staging and production
4. **Monitor** - Set up monitoring and alerts
5. **Backup** - Keep backups of your content
6. **Cache** - Use CDN and proper cache headers
7. **SSL** - Always use HTTPS in production
!!!

## Deployment Checklist

- [ ] Build site locally and test
- [ ] Configure `retype.yml` with production URL
- [ ] Set up hosting platform
- [ ] Configure build command and output directory
- [ ] Deploy and verify
- [ ] Configure custom domain (if applicable)
- [ ] Set up SSL certificate
- [ ] Test all pages and links
- [ ] Configure redirects (if needed)
- [ ] Set up monitoring
- [ ] Document deployment process

## Troubleshooting

### Build Fails

**Issue**: Build command fails in CI/CD

**Solution**: Ensure Retype is installed:
```bash
npm install retypeapp --global
```

### 404 Errors

**Issue**: Pages return 404 errors

**Solution**: 
- Check output directory configuration
- Verify file paths are correct
- Configure server for clean URLs

### Slow Load Times

**Issue**: Site loads slowly

**Solution**:
- Enable CDN
- Optimize images
- Enable compression
- Use caching headers

## Resources

- [GitHub Pages Documentation](https://docs.github.com/pages)
- [Netlify Documentation](https://docs.netlify.com)
- [Vercel Documentation](https://vercel.com/docs)
- [Cloudflare Pages](https://developers.cloudflare.com/pages)

---

Congratulations! Your Retype site is now deployed and accessible to the world! 🎉
