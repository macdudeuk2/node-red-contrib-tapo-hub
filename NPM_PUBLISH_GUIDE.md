# Publishing to npm

Guide for publishing `node-red-contrib-tapo-hub` to the npm registry.

## Pre-Publication Checklist

- [ ] All code tested and working
- [ ] Logging cleaned up (errors only)
- [ ] README updated
- [ ] LICENSE file included
- [ ] package.json metadata complete
- [ ] Example flows work
- [ ] Version number correct
- [ ] Git repository clean
- [ ] All changes committed

## Repository Setup

### 1. Rename GitHub Repository

Since we renamed from `node-red-contrib-tapo-s220` to `node-red-contrib-tapo-hub`:

**On GitHub:**
1. Go to repository settings
2. Rename repository to: `node-red-contrib-tapo-hub`
3. Update local remote:

```bash
cd /Users/andy/cursor/node
git remote set-url origin https://github.com/macdudeuk2/node-red-contrib-tapo-hub.git
git remote -v  # Verify
```

### 2. Create npm Account

If you don't have an npm account:

```bash
# Create account at: https://www.npmjs.com/signup

# Login to npm
npm login
```

## Publication Steps

### 1. Final Version Check

```bash
cd /Users/andy/cursor/node

# Verify version in package.json
cat package.json | grep version

# Should be: "version": "2.0.0"
```

### 2. Test Package Locally

```bash
# Install locally to test
cd ~/.node-red
npm uninstall node-red-contrib-tapo-s220  # Remove old
npm install /Users/andy/cursor/node       # Install new

# Test in Node-RED
# - Check all nodes appear
# - Test discovery
# - Test switch control
# - Test sensor reading

# If all good, continue
```

### 3. Create .npmignore

```bash
cd /Users/andy/cursor/node

cat > .npmignore << 'EOF'
# Development files
.git
.gitignore
*.md.backup
*.swp
*~

# Test files
test-*.js

# Documentation (keep README.md)
IMPLEMENTATION_NOTES.md
CONCURRENCY_FIX.md
T310_SUPPORT_ANALYSIS.md
IMPLEMENTATION_OPTIONS.md
BACKWARD_COMPATIBILITY_PLAN.md

# CI/CD
.github

# IDE
.vscode
.idea
*.code-workspace

# OS
.DS_Store
Thumbs.db
EOF
```

### 4. Verify Package Contents

```bash
# See what will be published
npm pack --dry-run

# Check the output - should include:
# - tapo-hub.js
# - tapo-switch.js
# - tapo-sensor.js
# - tapo-hub-connect.js
# - *.html files
# - package.json
# - README.md
# - LICENSE
# - example-flows.json
```

### 5. Create Git Tag

```bash
git add -A
git commit -m "chore: Prepare v2.0.0 for npm publication

- Rename package to node-red-contrib-tapo-hub
- Remove legacy tapo-s220 node
- Clean up logging (errors only)
- Add LICENSE file
- Update README for npm
- Clean example flows"

git tag -a v2.0.0 -m "Release v2.0.0 - Hub config architecture with T310 sensor support"
git push origin main
git push origin v2.0.0
```

### 6. Publish to npm

```bash
cd /Users/andy/cursor/node

# Dry run first (see what happens without publishing)
npm publish --dry-run

# If everything looks good, publish!
npm publish

# You'll see:
# + node-red-contrib-tapo-hub@2.0.0
```

### 7. Verify Publication

```bash
# Check it's published
npm view node-red-contrib-tapo-hub

# Try installing
cd /tmp
mkdir test-install
cd test-install
npm install node-red-contrib-tapo-hub
ls -la node_modules/node-red-contrib-tapo-hub/
```

## Post-Publication

### 1. Update Repository README

Add npm badge to GitHub README (already done):
```markdown
[![npm version](https://img.shields.io/npm/v/node-red-contrib-tapo-hub.svg)]
```

### 2. Create GitHub Release

On GitHub:
1. Go to Releases
2. Create new release from tag v2.0.0
3. Add release notes

**Release Notes Template:**
```markdown
## v2.0.0 - Hub Config Architecture

### Features
- Hub config node for shared credentials
- T310/T315 temperature sensor support
- Request queuing for reliability
- Battery and signal monitoring

### Supported Devices
- S220/S210 switches
- T310/T315 temperature sensors

### Breaking Changes
- Package renamed to node-red-contrib-tapo-hub
- Legacy tapo-s220 node removed
- Users must reconfigure with new hub config pattern

### Installation
npm install node-red-contrib-tapo-hub
```

### 3. Test Installation

Test that users can install it:

```bash
cd ~/.node-red
npm install node-red-contrib-tapo-hub
node-red-restart
```

### 4. Monitor

Watch for issues:
- Check npm download stats
- Monitor GitHub issues
- Respond to user questions

## Updating After Publication

### Patch Version (2.0.0 → 2.0.1)

For bug fixes:
```bash
npm version patch
git push origin main --tags
npm publish
```

### Minor Version (2.0.0 → 2.1.0)

For new features:
```bash
npm version minor
git push origin main --tags
npm publish
```

### Major Version (2.0.0 → 3.0.0)

For breaking changes:
```bash
npm version major
git push origin main --tags
npm publish
```

## Unpublish (Emergency Only)

If you need to unpublish (within 72 hours):

```bash
npm unpublish node-red-contrib-tapo-hub@2.0.0
```

**Warning:** Can only unpublish versions published <72 hours ago.

## Package Naming Guidelines

✅ **Good package names:**
- `node-red-contrib-tapo-hub`
- Follows Node-RED naming convention
- Descriptive and clear

❌ **Avoid:**
- Generic names (`tapo`, `hub`)
- Names without `node-red-contrib-` prefix
- Trademark conflicts

## npm Account Security

**Important:**
- Enable 2FA on npm account
- Use automation tokens for CI/CD (if needed)
- Never commit `.npmrc` with auth tokens
- Keep credentials secure

## Summary

**To publish:**
```bash
# 1. Test locally
npm pack --dry-run

# 2. Commit and tag
git commit -m "Release v2.0.0"
git tag v2.0.0
git push --tags

# 3. Publish
npm publish

# 4. Verify
npm view node-red-contrib-tapo-hub
```

That's it! Your package will be available on npm. 🚀

