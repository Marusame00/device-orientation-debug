# Device Orientation Debug

A lightweight, mobile-friendly web tool for testing and debugging device orientation sensors (alpha, beta, gamma) in real-time. Perfect for diagnosing sensor issues, testing browser compatibility, or learning how the DeviceOrientation API works.

![Status](https://img.shields.io/badge/status-active-success?style=for-the-badge) ![Dependencies](https://img.shields.io/badge/dependencies-none-blue?style=for-the-badge) ![Mobile](https://img.shields.io/badge/platform-mobile%20%26%20desktop-orange?style=for-the-badge)

## 🎯 Purpose

This tool helps you:

- ✅ **Verify** if your device's motion sensors are working
- 🔍 **Debug** browser sensor access issues (HTTPS, permissions, etc.)
- 📊 **Monitor** real-time alpha/beta/gamma values
- 🧪 **Test** DeviceOrientation API support across browsers
- 📱 **Diagnose** why compass/motion apps aren't working

## ✨ Features

- 📈 **Live sensor readouts** - Alpha (Z), Beta (X), Gamma (Y), and Absolute flag
- 📝 **Scrollable debug log** - Timestamped event history
- 🔢 **Event counter** - See how many orientation events fired
- ⏱️ **Last update timestamp** - Know when the last data arrived
- 🎨 **Clean, modern UI** - Dark theme optimized for mobile
- 🚦 **Status indicators** - Visual feedback (idle/active/error states)
- 🍎 **iOS permission handling** - Automatic permission request on iOS 13+
- 🤖 **Android compatibility** - Works with Chrome, Brave, Firefox, Edge
- ⚡ **Zero dependencies** - Pure HTML/CSS/JS in one file

## 🚀 Live Demo

Visit: `https://yourusername.github.io/device-orientation-debug/`

*(Replace `yourusername` with your actual GitHub username)*

## 📱 How to Use

### Basic Usage

1. **Open the page** on your mobile device (or desktop with motion sensors)
2. **Tap "Start"** to begin listening for orientation events
3. **Grant permission** if prompted (iOS only)
4. **Move/rotate your device** to see values update in real-time
5. **Check the debug log** for detailed event history
6. **Tap "Stop"** when done

### What the Values Mean

| Sensor | Axis | Range | Description |
|--------|------|-------|-------------|
| **Alpha (α)** | Z | 0° to 360° | Rotation around vertical axis (compass heading) |
| **Beta (β)** | X | -180° to 180° | Front-to-back tilt (pitch) |
| **Gamma (γ)** | Y | -90° to 90° | Left-to-right tilt (roll) |
| **Absolute** | — | true/false | Whether values are relative to Earth's coordinate frame |

### Visual Guide

```
        ↑ (device top)
        │
        │  Beta (β)
        │  Tilt forward/back
        │
        •────────→ Gamma (γ)
       ╱           Tilt left/right
      ╱
     ↻ Alpha (α)
    Rotate flat (compass)
```

## 🛠️ Troubleshooting

### "No events received" error

**Possible causes:**

1. **Not using HTTPS** ❌
   - Modern browsers block motion sensors on non-secure contexts
   - ✅ **Solution:** Use GitHub Pages (HTTPS) or `http://localhost`
   - ❌ **Won't work:** `http://192.168.x.x` or `file://` URLs

2. **Motion sensors blocked in browser** 🚫
   - **Brave:** Settings → Site settings → Motion sensors → **Allowed**
   - **Chrome:** Tap lock icon in address bar → Permissions → Motion sensors → **Allow**
   - **Firefox:** Usually allows by default on HTTPS
   - **Edge:** Similar to Chrome (site permissions)

3. **Device has no sensors** 📵
   - Some tablets, laptops, and desktops lack gyroscope/magnetometer
   - ✅ **Test:** Install a native sensor app (e.g., "Sensor Kinetics" on Android)

4. **iOS permission denied** 🍎
   - iOS 13+ requires explicit permission
   - ✅ **Solution:** Tap "Start" again and grant permission when prompted
   - If denied: Settings → Safari → Motion & Orientation Access → **On**

### Values show "—" (dashes)

- The browser is firing events but `alpha`/`beta`/`gamma` are `null`
- Usually means sensors are blocked (see #2 above)
- Check the **event counter** - if it's increasing but values stay "—", it's a permission issue

### Values are all zeros

- Some browsers return `0` instead of `null` when blocked
- Same fix as above (enable motion sensors in browser settings)

### Compass (alpha) not working but tilt (beta/gamma) works

- Device may lack a **magnetometer** (compass sensor)
- Alpha will be `null` or unreliable without a magnetometer
- Beta/Gamma only need an **accelerometer/gyroscope**

## 🌐 Browser Compatibility

| Browser | Android | iOS | Desktop | Notes |
|---------|---------|-----|---------|-------|
| **Chrome** | ✅ | ✅ | ⚠️* | Requires HTTPS; desktop needs motion-capable device |
| **Safari** | — | ✅ | ⚠️* | iOS 13+ requires permission; desktop limited |
| **Firefox** | ✅ | ✅ | ⚠️* | Generally good support |
| **Brave** | ✅ | ✅ | ⚠️* | Requires manual sensor permission |
| **Edge** | ✅ | — | ⚠️* | Chromium-based, similar to Chrome |

*Desktop support depends on hardware (laptops with accelerometers, tablets, etc.)

### Requirements

- ✅ **HTTPS** (or `localhost` for development)
- ✅ Device with gyroscope/accelerometer (magnetometer for alpha)
- ✅ Motion sensor permissions granted
- ✅ Modern browser (ES6+ support)

## 💻 Development

### Local Testing

**Option 1: Python HTTP server**
```bash
python -m http.server 8080
# Open http://localhost:8080 in your browser
```

**Option 2: Node.js http-server**
```bash
npx http-server -p 8080
# Open http://localhost:8080
```

**Option 3: GitHub Pages (recommended)**
- Push to GitHub
- Enable Pages in repo settings
- Access via HTTPS URL

### File Structure

```
device-orientation-debug/
├── index.html          # Complete app (HTML + CSS + JS)
└── README.md          # This file
```

That's it! Single-file app with no build process.

## 🎨 Customization

### Change Color Scheme

Edit the CSS variables in `<style>`:

```css
body {
  background: #050510;  /* Dark background */
  color: #f5f5f5;       /* Light text */
}

.card {
  background: #111322;  /* Card background */
  border: 1px solid #2d3650;  /* Card border */
}
```

### Adjust Log Size

Find `.log` in CSS:

```css
.log {
  max-height: 140px;  /* Change to 200px, 300px, etc. */
  overflow-y: auto;
}
```

### Change Update Frequency

In JavaScript, adjust the throttle:

```javascript
function appendLog(line) {
  const now = Date.now();
  // Change 200 to higher (slower) or lower (faster)
  if (now - lastLogTime < 200 && eventsCount > 5) return;
  // ...
}
```

## 🔬 Technical Details

### DeviceOrientationEvent API

This tool uses the standard [DeviceOrientationEvent](https://developer.mozilla.org/en-US/docs/Web/API/DeviceOrientationEvent) Web API.

**Event properties:**
- `event.alpha` - Rotation around Z-axis (0-360°)
- `event.beta` - Rotation around X-axis (-180 to 180°)
- `event.gamma` - Rotation around Y-axis (-90 to 90°)
- `event.absolute` - Boolean indicating if values are Earth-relative

**iOS 13+ Permission:**
```javascript
DeviceOrientationEvent.requestPermission()
  .then(state => {
    if (state === 'granted') {
      // Start listening
    }
  });
```

**Standard listener (Android/other):**
```javascript
window.addEventListener('deviceorientation', (event) => {
  console.log(event.alpha, event.beta, event.gamma);
});
```

### Security Context Requirements

Modern browsers require:
- **HTTPS** (or `localhost`) to access motion sensors
- **User gesture** to request permission (button click, tap, etc.)
- **Explicit permission** on iOS 13+ and some Android browsers

This is a privacy/security feature to prevent:
- Fingerprinting via sensor data
- Unauthorized motion tracking
- Keylogging via motion patterns

## 📋 Use Cases

- 🧭 **Compass app development** - Test heading values before building
- 🎮 **Game development** - Verify tilt controls work on target devices
- 📱 **Device testing** - Check if sensors are functional
- 🐛 **Bug diagnosis** - Figure out why your motion app isn't working
- 🎓 **Learning** - Understand how DeviceOrientation API works
- 🔧 **Browser testing** - Compare sensor behavior across browsers

## 🔒 Privacy

- **No data collection** - Everything runs locally in your browser
- **No analytics** - No tracking, cookies, or third-party scripts
- **No network requests** - Works completely offline after initial load
- **Sensor data stays local** - Never transmitted or stored anywhere

## 📄 License

MIT License - free to use, modify, and distribute.

## 🙏 Acknowledgments

Built as a diagnostic companion to the [Synthwave Compass](https://github.com/yourusername/synthwave-compass) project.

Inspired by:
- [Audero Device Orientation Demo](https://www.audero.it/demo/device-orientation-api-demo.html)
- MDN DeviceOrientationEvent documentation
- Hours of debugging sensor issues on mobile browsers 😅

## 🤝 Contributing

Found a bug or have a feature idea?

1. Fork the repo
2. Make your changes to `index.html`
3. Test on multiple devices/browsers
4. Submit a pull request

**Ideas for contributions:**
- [ ] Add DeviceMotion API support (acceleration data)
- [ ] Export log to CSV/JSON
- [ ] Add visual 3D device orientation preview
- [ ] Implement sensor calibration instructions
- [ ] Add comparison mode (side-by-side device testing)
- [ ] Create "sensor health check" with pass/fail tests

## 📞 Support

Having issues?

1. Check the **Troubleshooting** section above
2. Verify you're using **HTTPS** (not HTTP or file://)
3. Test on a known-working demo like [Audero's](https://www.audero.it/demo/device-orientation-api-demo.html)
4. Open an issue on GitHub with:
   - Device model
   - Browser + version
   - Screenshot of the debug page
   - What the event counter shows

## 🔗 Related Projects

- [Synthwave Compass](https://github.com/yourusername/synthwave-compass) - Retro 80s compass using these same sensors
- [MDN DeviceOrientation Docs](https://developer.mozilla.org/en-US/docs/Web/API/DeviceOrientationEvent)
- [Can I Use - DeviceOrientation](https://caniuse.com/deviceorientation)

---

**Made with 🔬 for debugging sensor mysteries**

*Works best on mobile devices with motion sensors*

Vibe Coded with abacus.ai
