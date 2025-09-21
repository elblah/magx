# MagX - Real-time Screen Magnifier

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Shell](https://img.shields.io/badge/shell-bash-green.svg)

MagX is a lightweight, real-time screen magnifier for Linux systems. It creates a zoomed-in view of the area around your mouse cursor, perfect for presentations, accessibility, or detailed work.

## Features

- 🖱️ **Real-time magnification** - Follows your mouse cursor instantly
- 🔍 **Adjustable zoom scale** - Configure the magnification level
- ⚡ **Lightweight** - Minimal resource usage with efficient screen capture
- 🖥️ **Customizable window** - Adjustable position and size
- 🎯 **Smart centering** - Automatically centers on cursor position
- 🚀 **Fast performance** - Optimized for smooth real-time updates

## Requirements

MagX requires the following Linux utilities:

- `bash` (included with most Linux distributions)
- `flock` (util-linux package)
- `feh` - Image viewer
- `xdotool` - X11 automation tool
- `scrot` - Screen capture utility
- `basenc` - Base encoding/decoding utility

### Installation

**Debian/Ubuntu:**
```bash
sudo apt-get install feh xdotool scrot coreutils
```

**Fedora/CentOS:**
```bash
sudo dnf install feh xdotool scrot coreutils
```

**Arch Linux:**
```bash
sudo pacman -S feh xdotool scrot coreutils
```

## Usage

### Basic Usage

Simply run the script:
```bash
./magx
```

This will start MagX with default settings:
- Zoom scale: 2x
- Update interval: 1 second
- Window geometry: 800x160 at position +0+920

### Configuration Options

You can customize MagX's behavior using environment variables:

```bash
# Set zoom scale (higher = more zoom)
SCALE=4 ./magx

# Set update interval in seconds (lower = smoother but more CPU)
INTERVAL=0.5 ./magx

# Set custom window geometry (widthxheight+x+y)
FEH_GEOMETRY=1024x200+100+800 ./magx

# Disable cursor centering (zoom from top-left)
NOCENTER=1 ./magx
```

### Example Configurations

**High zoom for detailed work:**
```bash
SCALE=8 INTERVAL=0.3 ./magx
```

**Large window for presentations:**
```bash
FEH_GEOMETRY=1200x300+0+0 SCALE=3 ./magx
```

**Minimal CPU usage:**
```bash
INTERVAL=2 SCALE=2 ./magx
```

## How It Works

1. **Initialization**: Creates a temporary image file with a default pattern
2. **Window Setup**: Launches `feh` to display the magnified view in a borderless window
3. **Main Loop**: Continuously captures screen regions around the cursor:
   - Gets current mouse position using `xdotool`
   - Calculates the capture area based on zoom scale
   - Captures the screen region using `scrot`
   - Updates the display window with the new image
4. **Cleanup**: Automatically removes temporary files when exiting

## Performance Tips

- **Lower intervals** (e.g., 0.5) provide smoother animation but use more CPU
- **Higher intervals** (e.g., 2) reduce CPU usage but may feel less responsive
- The script automatically runs with low priority (`renice -n 19`) to minimize system impact
- Use appropriate zoom levels - higher zoom requires more processing

## Troubleshooting

### Common Issues

**"Error getting screen resolution"**
- Ensure you're running in an X11 environment
- Check that `xwininfo` is installed and working

**"Feh closed..." message**
- The feh window was closed manually
- The script will exit automatically

**Performance is slow**
- Try increasing the `INTERVAL` value
- Reduce the `SCALE` if zoom level is very high
- Ensure your system has adequate resources

**Window positioning issues**
- Verify your screen resolution with `xrandr`
- Adjust `FEH_GEOMETRY` to fit your screen layout

### Dependencies Check

Run this command to verify all dependencies are installed:
```bash
for cmd in feh xdotool scrot basenc flock xwininfo; do
    if command -v "$cmd" >/dev/null 2>&1; then
        echo "✓ $cmd is installed"
    else
        echo "✗ $cmd is missing"
    fi
done
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Feel free to submit issues and enhancement requests.

## Author

Created by [DanielT](https://github.com/elblah/magx)

---

**Note**: MagX is designed for X11 environments. Wayland support may require additional configuration or alternative tools.