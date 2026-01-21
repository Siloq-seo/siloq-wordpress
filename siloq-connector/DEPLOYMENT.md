# Siloq WordPress Plugin - Deployment Guide

## 🚀 Production Deployment Instructions

### Pre-Deployment Checklist

✅ **Code Quality**
- [ ] All tests passed (see TESTING.md)
- [ ] No PHP errors or warnings
- [ ] No JavaScript console errors
- [ ] Code follows WordPress coding standards
- [ ] All TODOs resolved or documented

✅ **Documentation**
- [ ] README.md complete and accurate
- [ ] INSTALL.md tested and working
- [ ] CHANGELOG.md updated
- [ ] Inline code comments added
- [ ] API endpoints documented

✅ **Version Control**
- [ ] All changes committed to Git
- [ ] Version number updated in plugin header
- [ ] Git tag created for release

---

## 📦 Creating Release Package

### Step 1: Clean Working Directory

```bash
cd /path/to/siloq-wordpress-plugin

# Remove development files
rm -rf node_modules
rm -rf .git
rm -f .gitignore
rm -f package-lock.json
rm -f composer.lock

# Remove test files
rm -rf tests
rm -f phpunit.xml
```

### Step 2: Create ZIP Archive

```bash
# From parent directory
cd ..

# Create ZIP (exclude unnecessary files)
zip -r siloq-connector-v1.0.0.zip siloq-connector/ \
  -x "*.git*" \
  -x "*node_modules*" \
  -x "*.DS_Store" \
  -x "*tests*" \
  -x "*.log"
```

### Step 3: Verify ZIP Contents

```bash
# List contents
unzip -l siloq-connector-v1.0.0.zip

# Should contain:
# siloq-connector/
# ├── siloq-connector.php
# ├── includes/
# │   ├── class-siloq-api-client.php
# │   ├── class-siloq-sync-engine.php
# │   ├── class-siloq-admin.php
# │   ├── class-siloq-content-import.php
# │   └── class-siloq-webhook-handler.php
# ├── assets/
# │   ├── css/
# │   │   ├── admin.css
# │   │   └── frontend.css
# │   └── js/
# │       └── admin.js
# ├── README.md
# └── INSTALL.md
```

---

## 🌐 WordPress.org Submission (Optional)

If submitting to WordPress.org plugin directory:

### Requirements
- [ ] Unique plugin name and slug
- [ ] GPL-compatible license
- [ ] No external dependencies (or properly documented)
- [ ] Meets WordPress Plugin Guidelines
- [ ] SVN repository access

### Submission Process

1. **Create Plugin Page:**
   - Go to https://wordpress.org/plugins/developers/add/
   - Submit plugin for review
   - Wait for approval (2-7 days)

2. **Setup SVN:**
```bash
# Checkout SVN repository
svn co https://plugins.svn.wordpress.org/siloq-connector/

cd siloq-connector

# Copy files to trunk
cp -r /path/to/siloq-connector/* trunk/

# Add files
svn add trunk/*

# Commit
svn ci -m "Initial commit v1.0.0"

# Tag release
svn cp trunk tags/1.0.0
svn ci -m "Tagging version 1.0.0"
```

3. **Assets:**
```bash
# Add screenshots and banner to assets folder
# /assets/
# ├── banner-772x250.png
# ├── icon-128x128.png
# └── screenshot-1.png
```

---

## 🏢 Client Deployment

### Deployment to Client's WordPress Site

#### Option A: WordPress Admin Upload

1. **Prepare:**
   - Ensure client site is backed up
   - Verify PHP version compatibility
   - Check for plugin conflicts

2. **Upload:**
   ```
   1. Log in to client's WordPress admin
   2. Go to Plugins → Add New → Upload Plugin
   3. Choose siloq-connector-v1.0.0.zip
   4. Click "Install Now"
   5. Click "Activate Plugin"
   ```

3. **Configure:**
   ```
   1. Go to Siloq → Settings
   2. Enter API URL: http://[siloq-backend-ip]:3000/api/v1
   3. Enter API Key: [provided by client]
   4. Enable Auto-Sync (optional)
   5. Click "Save Settings"
   6. Click "Test Connection" to verify
   ```

4. **Initial Sync:**
   ```
   1. Go to Siloq → Settings
   2. Click "Sync All Pages"
   3. Wait for completion
   4. Verify in Siloq → Sync Status
   ```

#### Option B: FTP/SSH Deployment

```bash
# Connect via SSH
ssh user@client-site.com

# Navigate to plugins directory
cd /var/www/html/wp-content/plugins

# Upload ZIP (via SCP from local)
# From your machine:
scp siloq-connector-v1.0.0.zip user@client-site.com:/tmp/

# Back on server, extract
unzip /tmp/siloq-connector-v1.0.0.zip

# Set permissions
chmod -R 755 siloq-connector
chown -R www-data:www-data siloq-connector

# Activate via WP-CLI (if available)
wp plugin activate siloq-connector
```

---

## 🔧 Backend Configuration

### Siloq Backend Setup

1. **API Endpoint Verification:**
   ```bash
   # Test endpoints are accessible
   curl -X POST http://your-backend:3000/api/v1/auth/verify \
     -H "Authorization: Bearer YOUR_API_KEY"
   
   # Should return: {"authenticated": true}
   ```

2. **Webhook Configuration:**
   ```
   In Siloq backend config, add:
   
   WORDPRESS_WEBHOOK_URL=https://client-site.com/wp-json/siloq/v1/webhook
   WEBHOOK_SECRET=[same as WordPress API Key]
   ```

3. **Firewall Rules:**
   ```bash
   # If using DigitalOcean private networking
   # Allow WordPress droplet to access backend
   
   # On backend server:
   sudo ufw allow from [wordpress-droplet-ip] to any port 3000
   ```

---

## 📊 Post-Deployment Verification

### Immediate Checks (First 30 Minutes)

- [ ] Plugin activates without errors
- [ ] Admin menu appears
- [ ] Settings page loads
- [ ] Connection test succeeds
- [ ] At least one page syncs successfully
- [ ] Schema appears in page source
- [ ] No PHP errors in logs

### 24-Hour Monitoring

- [ ] Auto-sync working for new/updated pages
- [ ] Webhook notifications received
- [ ] No performance degradation
- [ ] No errors in WordPress debug log
- [ ] No errors in Siloq backend logs

### Weekly Checks

- [ ] All synced pages up-to-date
- [ ] Schema markup valid (Google Rich Results)
- [ ] Content import working
- [ ] No conflicts with other plugins
- [ ] Client feedback collected

---

## 🐛 Rollback Procedure

If issues arise post-deployment:

### Quick Rollback

```bash
# 1. SSH to site
ssh user@client-site.com

# 2. Deactivate plugin
wp plugin deactivate siloq-connector

# 3. Remove plugin
rm -rf /var/www/html/wp-content/plugins/siloq-connector

# 4. Restore backup if needed
# (depends on backup solution)
```

### Via WordPress Admin

```
1. Go to Plugins
2. Deactivate "Siloq Connector"
3. Delete plugin
4. Restore from backup if data was corrupted
```

---

## 📈 Monitoring & Maintenance

### Log Files to Monitor

**WordPress:**
- `/wp-content/debug.log` (if WP_DEBUG_LOG enabled)
- Server error logs: `/var/log/apache2/error.log` or `/var/log/nginx/error.log`

**Siloq Backend:**
- Application logs
- API request logs

### Key Metrics to Track

- Sync success rate
- API response times
- Content generation completion times
- Error frequency
- Plugin usage (pages synced, content imported)

### Regular Maintenance

**Weekly:**
- Check error logs
- Verify syncs completing
- Test content import

**Monthly:**
- Update plugin if new version released
- Review webhook activity
- Check for WordPress core updates
- Test plugin after WordPress updates

---

## 🆘 Support Procedures

### Client Support Contacts

**Level 1 - Client Self-Help:**
- README.md troubleshooting section
- INSTALL.md quick start
- In-plugin help text

**Level 2 - Email Support:**
- Email: support@siloq.com
- Response time: 24 hours
- Provide: WordPress version, PHP version, error messages

**Level 3 - Emergency:**
- For site-down issues only
- Include access credentials (if pre-arranged)
- Remote debugging via screen share

### Common Issues & Solutions

**"Connection Failed":**
1. Check API URL format
2. Verify API key
3. Test backend accessibility: `curl http://backend:3000/health`
4. Check firewall rules

**"Sync Not Working":**
1. Check WordPress debug log
2. Verify auto-sync setting
3. Test manual sync
4. Check API key validity

**"Schema Not Appearing":**
1. Verify page was synced
2. Check page source for `application/ld+json`
3. Clear caching plugins
4. Re-sync page

---

## 📝 Deployment Checklist

### Pre-Deployment
- [ ] All tests passed
- [ ] Documentation complete
- [ ] Version number updated
- [ ] ZIP file created
- [ ] Client notified

### Deployment
- [ ] Backup taken
- [ ] Plugin uploaded
- [ ] Plugin activated
- [ ] Settings configured
- [ ] Connection tested
- [ ] Initial sync completed

### Post-Deployment
- [ ] Verification checks passed
- [ ] Client trained
- [ ] Support contact info provided
- [ ] Monitoring set up
- [ ] Success criteria met

---

## 🎉 Success Criteria

Deployment is successful when:

✅ Plugin installed and activated without errors  
✅ API connection established  
✅ At least 10 pages synced successfully  
✅ Schema markup visible in page source  
✅ Auto-sync working  
✅ Content import functional  
✅ Client can navigate admin interface  
✅ No critical bugs reported  
✅ Performance acceptable  
✅ Client satisfied  

---

## 📞 Emergency Contacts

**Development Team:**
- Lead: [Your Name]
- Email: [Your Email]
- Phone: [Your Phone] (emergencies only)

**Infrastructure:**
- DigitalOcean Support: support@digitalocean.com
- Backend Server: [IP/Domain]
- WordPress Host: [hosting provider]

**Client:**
- Contact: [Client Name]
- Email: [Client Email]
- Availability: [hours/timezone]

---

**Deployment Date:** _____________  
**Deployed By:** _____________  
**Status:** ⏳ Pending / 🚀 In Progress / ✅ Complete  

**Notes:**
_________________________________________
_________________________________________
_________________________________________
