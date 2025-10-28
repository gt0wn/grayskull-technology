# Grayskull Technology Website

Professional enterprise website for Grayskull Technology LLC - Infrastructure & Security Platform

## 📦 What's Included

- **index.html** - Complete single-page application website
- Fully responsive design (mobile, tablet, desktop)
- Dark mode auto-enabled based on system preferences
- Employee login modal with Duo SSO placeholder
- All CSS and JavaScript included inline (no external dependencies)

## 🚀 Quick Deploy to Cloudflare Pages (5 Minutes)

### Option 1: Direct Upload (Easiest)

1. **Go to Cloudflare Dashboard**
   - Visit: https://dash.cloudflare.com
   - Navigate to: **Workers & Pages**

2. **Create New Project**
   - Click: **Create Application** → **Pages** → **Upload assets**
   - Drag and drop `index.html`
   - Project name: `grayskull-technology`
   - Click: **Deploy site**

3. **Connect Custom Domain**
   - In your project, click: **Custom domains**
   - Click: **Set up a custom domain**
   - Enter: `grayskulltech.com` or `www.grayskulltech.com`
   - Cloudflare auto-configures DNS
   - SSL auto-provisions in 1-2 minutes

**Done!** Your site is live at `https://grayskulltech.com`

**Cost: $0** (Cloudflare Pages is free)

### Option 2: GitHub + Cloudflare Pages (For Version Control)

1. **Create GitHub Repository**
   ```bash
   # In your project folder
   git init
   git add index.html README.md
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/grayskull-technology.git
   git push -u origin main
   ```

2. **Connect to Cloudflare Pages**
   - Go to Cloudflare Pages → **Create application**
   - Click: **Connect to Git**
   - Authorize GitHub and select your repository
   - Build settings: Leave empty (static HTML)
   - Click: **Save and Deploy**

**Benefit:** Every push to GitHub auto-deploys

## 📝 How to Update Content

### Update Contact Information

Find and replace in `index.html`:

```html
<!-- Email -->
info@grayskulltech.com → your-email@grayskulltech.com

<!-- Phone -->
(XXX) XXX-XXXX → (757) 555-1234

<!-- License Numbers -->
#2705XXXXXXA → your actual license number
XXXXX → your CAGE code
XXXXXXXX → your UEI number
```

### Update Credentials Status

Change status badges from "Pending" to "Active":

```html
<!-- Find this: -->
<span class="status status-pending">Pending</span>

<!-- Replace with: -->
<span class="status status-active">Active</span>
```

### Redeploy After Changes

**Cloudflare Pages (Direct Upload):**
1. Edit `index.html` locally
2. Go to Cloudflare Pages → Your project → **Deployments**
3. Click **Create deployment**
4. Upload updated `index.html`

**Cloudflare Pages (GitHub):**
1. Edit `index.html` locally
2. Commit and push:
   ```bash
   git add index.html
   git commit -m "Updated contact info"
   git push
   ```
3. Cloudflare auto-deploys in ~30 seconds

## 🔒 Configure Duo SSO (Production)

The employee login button currently shows a demo. To enable real Duo SSO:

1. **Register Application in Duo Admin Panel**
   - Login to Duo Admin: https://admin.duosecurity.com
   - Applications → Protect an Application
   - Search for "Web SDK" and protect it
   - Note your Integration Key, Secret Key, and API Hostname

2. **Update JavaScript in index.html**

Find the `handleDuoLogin()` function and replace with:

```javascript
function handleDuoLogin() {
    const duoAuthUrl = 'https://api-XXXXXXXX.duosecurity.com/oauth/v1/authorize';
    const params = new URLSearchParams({
        client_id: 'YOUR_INTEGRATION_KEY',
        redirect_uri: 'https://grayskulltech.com/auth/callback',
        response_type: 'code',
        scope: 'openid profile email',
        state: generateSecureRandomState()
    });
    window.location.href = duoAuthUrl + '?' + params.toString();
}

function generateSecureRandomState() {
    return Array.from(crypto.getRandomValues(new Uint8Array(16)))
        .map(b => b.toString(16).padStart(2, '0')).join('');
}
```

3. **Create Callback Page**

Create `auth-callback.html` to handle OAuth response:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Authentication...</title>
</head>
<body>
    <script>
        // Handle OAuth callback
        const urlParams = new URLSearchParams(window.location.search);
        const code = urlParams.get('code');
        const state = urlParams.get('state');
        
        if (code) {
            // Send code to your backend for token exchange
            window.location.href = '/employee-dashboard';
        } else {
            alert('Authentication failed');
            window.location.href = '/';
        }
    </script>
</body>
</html>
```

## 📊 Add Analytics

### Cloudflare Web Analytics (Privacy-Friendly, Free)

1. Go to Cloudflare Dashboard → **Analytics & Logs** → **Web Analytics**
2. Click **Add a site**
3. Copy the JavaScript snippet
4. Paste before `</head>` in index.html:

```html
<script defer src='https://static.cloudflareinsights.com/beacon.min.js' 
        data-cf-beacon='{"token": "YOUR_TOKEN"}'></script>
```

### Google Analytics 4

Add before `</head>`:

```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

## 🔍 SEO Optimization

The site includes basic SEO meta tags. Additional optimizations:

### Add Structured Data (Schema.org)

Add before `</head>`:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Grayskull Technology LLC",
  "url": "https://grayskulltech.com",
  "logo": "https://grayskulltech.com/logo.png",
  "description": "Enterprise IAM, infrastructure, and smart building solutions. SDVOSB serving government, healthcare, and commercial clients.",
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Chesapeake",
    "addressRegion": "VA",
    "addressCountry": "US"
  }
}
</script>
```

### Create robots.txt

Create `robots.txt` file:

```
User-agent: *
Allow: /

Sitemap: https://grayskulltech.com/sitemap.xml
```

### Create sitemap.xml

Create `sitemap.xml` file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://grayskulltech.com</loc>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
</urlset>
```

## 🎨 Customization Guide

### Change Brand Colors

Find the `:root` CSS variables:

```css
:root {
    --accent: #0052cc;        /* Primary blue */
    --accent-hover: #0065ff;   /* Hover state */
}
```

Replace with your brand colors.

### Add Logo Image

Replace the text logo with an image:

```html
<!-- Find this: -->
<div class="logo-icon">GS</div>

<!-- Replace with: -->
<img src="logo.png" alt="Grayskull Technology" style="height: 32px;">
```

### Change Fonts

Add Google Fonts before `</head>`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
```

Update CSS:

```css
body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}
```

## 🐛 Troubleshooting

### Site Not Loading

1. Check DNS propagation: `dig grayskulltech.com`
2. Wait 5-10 minutes for Cloudflare DNS updates
3. Clear browser cache (Cmd+Shift+R or Ctrl+Shift+R)

### SSL Certificate Not Working

1. Verify domain is on Cloudflare
2. Check Cloudflare SSL/TLS settings (should be "Full" or "Full (strict)")
3. Wait up to 24 hours for initial certificate issuance

### Dark Mode Not Working

1. Check system settings (macOS: System Preferences → General → Appearance)
2. Test with browser DevTools: Toggle "Emulate CSS media feature prefers-color-scheme"

### Mobile Menu Not Opening

1. Check JavaScript console for errors
2. Ensure `onclick` handlers are intact
3. Test on actual device (not just browser resize)

## 📱 Testing Checklist

Before going live:

- [ ] Update contact email
- [ ] Update phone number
- [ ] Update license numbers
- [ ] Test all navigation links
- [ ] Test employee login modal opens/closes
- [ ] Test on mobile device
- [ ] Test dark mode (system preference)
- [ ] Verify SSL certificate active
- [ ] Check DNS propagation
- [ ] Test form submissions (if added)
- [ ] Run Lighthouse audit (Chrome DevTools)

## 💰 Costs

| Service | Cost |
|---------|------|
| Cloudflare Pages Hosting | **FREE** |
| SSL Certificate | **FREE** |
| CDN (Global) | **FREE** |
| Bandwidth (Unlimited) | **FREE** |
| **Total Monthly Cost** | **$0** |

## 🆘 Support

For issues with:
- **Cloudflare:** https://support.cloudflare.com
- **DNS/Domain:** Contact your domain registrar
- **Website Code:** Edit `index.html` directly

## 📄 License

© 2025 Grayskull Technology LLC - All Rights Reserved

---

**Ready to launch?** Upload `index.html` to Cloudflare Pages and you're live in 5 minutes!
