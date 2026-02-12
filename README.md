# OTAiOS

## Deploy to GitHub Pages for OTA Installation

This repository is configured for Over-the-Air (OTA) iOS app installation via GitHub Pages.

### Quick Install
1. Open this page on your iOS device in Safari
2. Tap "Install App" below
3. Go to Settings > General > VPN & Device Management > Trust Developer
4. App will install

### Manual OTA Setup (for your own deployment)

To implement Over-the-Air (OTA) installation of an IPA file on your iOS device without a computer, you'll create a manifest.plist file, host it and the IPA on an HTTPS server, and access a special install link via Safari on the device.

#### Prerequisites
Your IPA must be signed with a valid Developer or Enterprise provisioning profile (via Xcode or tools like Sideloadly). You'll need an HTTPS web server (e.g., GitHub Pages, Netlify drop, or your own like AWS S3/Vercel). Free Apple Developer account suffices for personal use.

#### Step 1: Create manifest.plist
Save this as manifest.plist, replacing placeholders with your details. Use a text editor; it must be exact XML format.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>items</key>
    <array>
        <dict>
            <key>assets</key>
            <array>
                <dict>
                    <key>kind</key>
                    <string>software-package</string>
                    <key>url</key>
                    <string>https://yourserver.com/path/to/yourapp.ipa</string>
                </dict>
            </array>
            <key>metadata</key>
            <dict>
                <key>bundle-identifier</key>
                <string>com.yourcompany.yourapp</string>
                <key>bundle-version</key>
                <string>1.0</string>
                <key>kind</key>
                <string>software</string>
                <key>title</key>
                <string>Your App Name</string>
            </dict>
        </dict>
    </array>
</dict>
</plist>
```
Get bundle-identifier and bundle-version from your app's Info.plist.

#### Step 2: Host Files
Upload manifest.plist and yourapp.ipa to your HTTPS server. Note the direct public URLs (e.g., https://example.com/manifest.plist and https://example.com/yourapp.ipa). Server must support byte-range requests for large IPAs.

#### Step 3: Create Install Page
Make a simple index.html on the same server:

```html
<!DOCTYPE html>
<html>
<body>
    <h1>Install App</h1>
    <a href="itms-services://?action=download-manifest&amp;url=https://yourserver.com/path/to/manifest.plist">Install Your App</a>
</body>
</html>
```
Open this page in Safari on your iOS device and tap the link.

#### Step 4: Install and Trust
Safari prompts "Untrusted Enterprise Developer?"—ignore and install. Then go to Settings > General > VPN & Device Management > Trust your developer profile. App installs and runs.

#### Notes
- Links must use HTTPS and itms-services:// scheme
- Certificates may revoke after 7 days (free account); re-sign as needed
- Test on iOS 13+; not for App Store apps

---

## App Details
- **Name:** Avto Krka QA
- **Bundle ID:** com.avtokrka.ver.qa
- **Version:** 1.0.30+30