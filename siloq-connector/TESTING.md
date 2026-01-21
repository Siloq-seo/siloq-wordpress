# Siloq WordPress Plugin - Testing Checklist

## ✅ Pre-Deployment Testing

### Environment Setup
- [ ] Fresh WordPress 5.0+ installation
- [ ] PHP 7.4+ confirmed
- [ ] Siloq backend API running and accessible
- [ ] Valid API credentials available

---

## 🔧 Installation Testing

### Plugin Installation
- [ ] Upload via WordPress admin (Plugins → Upload)
- [ ] Activate without errors
- [ ] Menu item "Siloq" appears in admin sidebar
- [ ] All submenu items visible (Settings, Sync Status, Content Import)

### Plugin Deactivation/Reactivation
- [ ] Deactivate plugin - no errors
- [ ] Settings preserved after deactivation
- [ ] Reactivate plugin - works correctly
- [ ] Uninstall (optional test) - cleans up properly

---

## ⚙️ Configuration Testing

### Settings Page
- [ ] Navigate to Siloq → Settings
- [ ] API URL field accepts valid URLs
- [ ] API URL validation rejects invalid URLs
- [ ] API Key field accepts input (password type)
- [ ] Auto-Sync checkbox toggles correctly
- [ ] "Save Settings" button works
- [ ] Success message displays after save
- [ ] Settings persist after page reload

### Connection Testing
- [ ] "Test Connection" button appears
- [ ] Click test with invalid credentials → Error message
- [ ] Click test with valid credentials → Success message
- [ ] Connection status displayed clearly

---

## 🔄 Sync Functionality Testing

### Single Page Sync
- [ ] Create a test page with content
- [ ] Publish the page
- [ ] Go to Siloq → Sync Status
- [ ] Click "Sync Now" for the test page
- [ ] Sync completes without errors
- [ ] Last synced timestamp updates
- [ ] Status badge changes to "Synced"
- [ ] Check Siloq backend - page data received

### Bulk Sync
- [ ] Create 5+ test pages
- [ ] Go to Siloq → Settings
- [ ] Click "Sync All Pages"
- [ ] Progress bar displays
- [ ] All pages sync successfully
- [ ] Results summary shows correct counts
- [ ] Sync Status page reflects all pages synced

### Auto-Sync
- [ ] Enable Auto-Sync in settings
- [ ] Create new page and publish
- [ ] Check Sync Status - page auto-synced
- [ ] Edit existing page and update
- [ ] Check Sync Status - page re-synced with new timestamp
- [ ] Disable Auto-Sync
- [ ] Publish new page - should NOT auto-sync

### Re-Sync Detection
- [ ] Sync a page
- [ ] Edit the page content
- [ ] Check Sync Status - should show "Needs Re-sync"
- [ ] Click "Sync Now" - should re-sync successfully

---

## 📥 Content Import Testing

### Content Generation
- [ ] Go to Siloq → Content Import
- [ ] Select a page from dropdown
- [ ] Click "Generate Content"
- [ ] Job creation succeeds
- [ ] Job ID displayed
- [ ] Status polling starts
- [ ] Status updates appear (if backend supports)

### Import as Draft
- [ ] Wait for content generation to complete
- [ ] Click "Import as Draft" button
- [ ] New draft page created
- [ ] Redirected to edit page
- [ ] Content visible in editor
- [ ] Original page unchanged

### Replace Content
- [ ] Generate content for a page
- [ ] Click "Replace Content" button
- [ ] Confirmation dialog appears
- [ ] Confirm replacement
- [ ] Page content replaced
- [ ] Page status changed to draft (for review)
- [ ] Backup created

### Restore Backup
- [ ] After replacing content, check for "Restore Backup" button
- [ ] Click "Restore Backup"
- [ ] Confirmation dialog appears
- [ ] Original content restored
- [ ] Backup cleared

### FAQ Injection
- [ ] Import content with FAQ items
- [ ] Check page content - FAQ section present
- [ ] FAQ styled correctly (check frontend CSS)
- [ ] Questions and answers properly formatted

---

## 🔗 Schema Markup Testing

### Schema Injection
- [ ] Sync a page
- [ ] Wait for schema generation (backend)
- [ ] View page source (frontend)
- [ ] Search for `<script type="application/ld+json">`
- [ ] Schema markup present and valid JSON
- [ ] Test with Google's Rich Results Test tool
- [ ] Schema validates without errors

### Schema Update
- [ ] Update page content
- [ ] Re-sync page
- [ ] Check if schema updates
- [ ] Verify new schema in page source

---

## 🪝 Webhook Testing

### Webhook Configuration
- [ ] Go to Siloq → Content Import
- [ ] Copy webhook URL displayed
- [ ] Configure webhook in Siloq backend
- [ ] Webhook URL format correct

### Webhook Events
- [ ] Trigger content.generated event from backend
- [ ] Check WordPress - notification received
- [ ] Content import page shows new content available
- [ ] Trigger schema.updated event
- [ ] Check page meta - schema updated
- [ ] Test invalid webhook signature - rejected

---

## 🚨 Error Handling Testing

### Invalid API Credentials
- [ ] Enter wrong API URL
- [ ] Test connection - clear error message
- [ ] Enter wrong API key
- [ ] Test connection - clear error message
- [ ] Try syncing with wrong credentials - fails gracefully

### Network Errors
- [ ] Stop Siloq backend
- [ ] Try syncing page - error message displayed
- [ ] Try testing connection - timeout handled
- [ ] Restart backend - functionality restored

### Invalid Data
- [ ] Try syncing non-page post type - skipped
- [ ] Try syncing draft page - handled appropriately
- [ ] Try syncing trashed page - handled appropriately

### Permission Errors
- [ ] Log in as Editor (not Admin)
- [ ] Try accessing Siloq → Settings - denied
- [ ] Log in as Author
- [ ] Try syncing page they can edit - works
- [ ] Try syncing page they can't edit - denied

---

## 🎨 UI/UX Testing

### Admin Interface
- [ ] All pages load without console errors
- [ ] Buttons styled correctly
- [ ] Loading indicators work
- [ ] Success/error messages clear
- [ ] Tables responsive on mobile
- [ ] Forms validate inputs
- [ ] No layout breaking

### Frontend
- [ ] FAQ sections styled properly
- [ ] Internal links work correctly
- [ ] Schema markup invisible to visitors
- [ ] No frontend JavaScript errors
- [ ] Mobile responsive

---

## ⚡ Performance Testing

### Bulk Operations
- [ ] Sync 50+ pages - completes without timeout
- [ ] Memory usage acceptable
- [ ] No database errors
- [ ] Page load times reasonable

### API Rate Limiting
- [ ] Rapid sync requests handled
- [ ] API errors logged properly
- [ ] Plugin doesn't crash on API errors

---

## 🔒 Security Testing

### Authentication
- [ ] API key never visible in browser (password field)
- [ ] API key not in page source
- [ ] API requests use Authorization header
- [ ] No API key in JavaScript

### AJAX Security
- [ ] All AJAX requests use nonces
- [ ] Nonces validated server-side
- [ ] Unauthorized users rejected
- [ ] SQL injection prevention (prepared statements)

### Data Sanitization
- [ ] User inputs sanitized
- [ ] Content from API escaped when displayed
- [ ] No XSS vulnerabilities
- [ ] File uploads rejected (if any)

---

## 🌍 Compatibility Testing

### WordPress Versions
- [ ] WordPress 5.0 - 5.9
- [ ] WordPress 6.0+
- [ ] Latest WordPress version

### PHP Versions
- [ ] PHP 7.4
- [ ] PHP 8.0
- [ ] PHP 8.1
- [ ] PHP 8.2

### Themes
- [ ] Twenty Twenty-One
- [ ] Twenty Twenty-Two
- [ ] Twenty Twenty-Three
- [ ] Popular theme (e.g., Astra, GeneratePress)

### Plugin Conflicts
- [ ] Test with Yoast SEO (schema conflicts?)
- [ ] Test with caching plugins (WP Super Cache, W3 Total Cache)
- [ ] Test with security plugins (Wordfence)
- [ ] Test with page builders (Elementor, if applicable)

---

## 📝 Documentation Testing

### README Accuracy
- [ ] Installation instructions work
- [ ] Configuration steps correct
- [ ] Troubleshooting section helpful
- [ ] All screenshots up to date (if any)

### Code Comments
- [ ] Functions documented
- [ ] Complex logic explained
- [ ] TODOs addressed or documented

---

## 🐛 Known Issues to Test

### Edge Cases
- [ ] Page with very long content (10,000+ words)
- [ ] Page with special characters in title
- [ ] Page with shortcodes
- [ ] Page with Gutenberg blocks
- [ ] Page with classic editor content
- [ ] Multilingual page (if WPML installed)

### Stress Testing
- [ ] Sync 100+ pages consecutively
- [ ] Multiple admins syncing simultaneously
- [ ] Rapid publish/edit/sync cycles

---

## ✅ Final Checks Before Deployment

- [ ] All critical tests passed
- [ ] No PHP errors or warnings
- [ ] No JavaScript console errors
- [ ] Plugin meets WordPress coding standards
- [ ] Version number correct in main file
- [ ] Changelog updated
- [ ] README.md complete and accurate
- [ ] INSTALL.md tested by following steps
- [ ] ZIP file created without unnecessary files
- [ ] ZIP file under 5MB

---

## 📊 Test Results Template

```
Date: _____________
Tester: _____________
Environment: _____________
WordPress Version: _____________
PHP Version: _____________

CRITICAL TESTS:
[ ] Installation
[ ] Configuration
[ ] Single Sync
[ ] Bulk Sync
[ ] Content Import
[ ] Schema Injection
[ ] Error Handling

BUGS FOUND:
1. _____________________________________________
2. _____________________________________________
3. _____________________________________________

SEVERITY:
[ ] Critical (blocks functionality)
[ ] Major (degrades experience)
[ ] Minor (cosmetic)

STATUS:
[ ] PASS - Ready for deployment
[ ] FAIL - Needs fixes before deployment
```

---

## 🚀 Deployment Checklist

After all tests pass:

- [ ] Create final ZIP file
- [ ] Test ZIP on clean WordPress install
- [ ] Update version to 1.0.0 (no -beta, -rc)
- [ ] Tag release in Git: `git tag v1.0.0`
- [ ] Push to GitHub
- [ ] Create GitHub release with ZIP
- [ ] Update documentation site (if any)
- [ ] Notify client of release

---

**Testing Status:** ⏳ In Progress / ✅ Complete / ❌ Failed

**Notes:**
_________________________________________
_________________________________________
_________________________________________
