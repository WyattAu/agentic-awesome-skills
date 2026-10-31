## 6. Security Policies: Cross-Platform
<!-- macOS configuration profile; password policy -->
# macOS: require password after sleep/screensaver (via script or profile)
# Windows: lock screen after 5 minutes of inactivity
### 7.1 macOS: Homebrew Bundle
# Brewfile: deploy via MDM script or Git checkout
### 7.2 Windows: winget / Chocolatey
# macOS: enable automatic updates via MDM or command
# Windows: configure Windows Update via registry
# Wipe a device (factory reset): DESTRUCTIVE
## 10. Onboarding Automation: Zero-Touch Enrollment
# onboard-mac.sh: runs as a post-enrollment script via MDM
# deploy.ps1: assigned as an Intune PowerShell script
# onboarding-checklist.yml: track in your ticketing system or Fleet
    - User signs into SSO; all apps authenticate via SAML/OIDC