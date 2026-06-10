# Build Instructions for Ship Loadicator Windows Installer

## Prerequisites

- Node.js 18+ installed
- npm or yarn
- Windows 10/11 (for building)
- Git

## Building the Executable

### Method 1: Using pkg (Recommended - Simple)

This creates a standalone .exe file without installation:

```bash
# Install dependencies
npm install

# Build the executable
npm run build:exe
```

Output: `dist/ship-loadicator.exe`

### Method 2: Creating an Installer with NSIS

For a proper installer with Start Menu shortcuts:

#### Step 1: Install NSIS
- Download: https://nsis.sourceforge.io/Download
- Install to default location: `C:\Program Files (x86)\NSIS`

#### Step 2: Build
```bash
npm install
npm run build:installer
```

Output: `ShipLoadicator-Setup.exe`

## Distribution

### Standalone Executable (.exe)
- **File:** `ship-loadicator.exe`
- **Size:** ~50-80 MB
- **Installation:** Just run it
- **Best for:** Quick testing, portable use

### Installer (.exe)
- **File:** `ShipLoadicator-Setup.exe`
- **Size:** ~30-40 MB
- **Installation:** Full installer with uninstall support
- **Best for:** Production, corporate deployment

## Post-Build

### Testing Locally
```bash
cd dist
./ship-loadicator.exe
# Application will start at http://localhost:3001
```

### Distribution Checklist
- [ ] Run executable locally to test
- [ ] Check that http://localhost:3001 loads
- [ ] Test adding cargo and calculations
- [ ] Verify stability alerts work
- [ ] Test uninstaller (if using NSIS)
- [ ] Share .exe file

## Customization

### Change Application Icon
1. Create 256x256 PNG icon
2. Save as `installer/icon.ico`
3. Rebuild installer

### Change Installation Directory
Edit `installer/ship-loadicator.nsi`:
```nsi
InstallDir "$PROGRAMFILES\ShipLoadicator"
```

### Change Application Name
Edit `package.json`:
```json
"name": "ship-loadicator",
"productName": "Ship Loadicator"
```

## Troubleshooting

### Build fails: "pkg not found"
```bash
npm install -g pkg
npm run build:exe
```

### NSIS installer won't build
- Ensure NSIS is installed in `C:\Program Files (x86)\NSIS`
- Or download portable NSIS version

### Executable won't run
- Check that Node.js is properly installed
- Ensure Windows Defender isn't blocking it
- Try running as Administrator

## Advanced: Custom Branding

### Create Branded Installer

Edit `installer/ship-loadicator.nsi`:

```nsi
; Change company name
BrandingText "Your Company Name"

; Change welcome message
!define MUI_WELCOMEFINISHPAGE_BITMAP "path/to/banner.bmp"
```

## CI/CD Integration

### GitHub Actions Example

```yaml
name: Build Windows Installer

on: [push, pull_request]

jobs:
  build:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '18'
      - run: npm install
      - run: npm run build:exe
      - uses: actions/upload-artifact@v2
        with:
          name: ship-loadicator.exe
          path: dist/ship-loadicator.exe
```

## Support

For issues with building, see: https://github.com/alison9747/ship-loadicator/issues
