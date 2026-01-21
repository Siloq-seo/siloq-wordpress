# Siloq WordPress Plugin - Quick Installation Guide

## ⚡ Quick Start (5 Minutes)

### 1. Upload Plugin to WordPress

**Option A - Via WordPress Admin (Easiest):**
```
1. Create a ZIP file of the 'siloq-connector' folder
2. Log in to WordPress Admin
3. Go to Plugins → Add New → Upload Plugin
4. Choose the ZIP file
5. Click "Install Now"
6. Click "Activate Plugin"
```

**Option B - Via FTP/SSH:**
```bash
# Upload the 'siloq-connector' folder to:
/wp-content/plugins/siloq-connector

# Then activate it in WordPress Admin → Plugins
```

### 2. Configure API Settings

```
1. In WordPress, go to: Siloq → Settings
2. Enter API URL: http://your-siloq-backend:3000/api/v1
3. Enter API Key: (get this from your Siloq backend)
4. Click "Save Settings"
5. Click "Test Connection" to verify
```

### 3. Sync Your Pages

```
1. Go to: Siloq → Settings
2. Click "Sync All Pages"
3. Wait for sync to complete (shows progress bar)
4. Check Siloq → Sync Status to verify
```

---

## 🔧 What You Need

✅ **WordPress site** (running version 5.0+)  
✅ **Siloq backend API** (running and accessible)  
✅ **API credentials** (URL + Key from Siloq backend)

---

## 📊 Plugin Features

✅ Sync WordPress pages to Siloq  
✅ Auto-sync on publish/update  
✅ Inject AI-generated schema markup  
✅ Bulk sync all pages  
✅ Monitor sync status  
✅ Easy admin dashboard

---

## 🚨 Troubleshooting

### Connection Test Fails
- ✓ Check API URL format: `http://IP:3000/api/v1` (no trailing slash)
- ✓ Verify API key is correct
- ✓ Ensure Siloq backend is running
- ✓ Check firewall allows WordPress → Siloq backend

### Pages Not Syncing
- ✓ Test API connection first
- ✓ Make sure pages are published (not draft)
- ✓ Check WordPress error logs

### Schema Not Showing
- ✓ Verify page was synced successfully
- ✓ Clear cache (if using caching plugin)
- ✓ View page source and search for: `application/ld+json`

---

## 📁 Plugin File Structure

```
siloq-connector/
├── siloq-connector.php           ← Main plugin file
├── includes/                     ← Core classes
│   ├── class-siloq-api-client.php
│   ├── class-siloq-sync-engine.php
│   └── class-siloq-admin.php
└── assets/                       ← CSS/JS
    ├── css/admin.css
    └── js/admin.js
```

---

## 🔐 Security Notes

- API keys stored securely in WordPress options
- All AJAX requests use WordPress nonces
- Bearer token authentication for API calls
- Admin-only access to plugin pages

---

## 📞 Support

**Need help?**
- Email: support@siloq.com
- GitHub: https://github.com/Siloq-seo/siloq-wordpress-plugin
- Docs: https://siloq.com/docs

---

**Ready to install?** Follow the 3 steps above! ⬆️
