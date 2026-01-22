# Blue Square

An interactive audio-reactive visual art application featuring 230 blue abstract images in a dynamic grid display. Originally created for Raspberry Pi, now available as a web experience.

🔗 **[Live Demo](https://jorch-wav.github.io/blue-square/)**

![Blue Square Preview](https://img.shields.io/badge/Status-Final%20Release-success)
![Images](https://img.shields.io/badge/Images-230-blue)
![License](https://img.shields.io/badge/License-MIT-green)

## Features

### Two Display Modes

**Mode 1: Uniform** 🟦  
All cells display the same image simultaneously, changing every 8 beats at the current BPM. Creates a synchronized, hypnotic visual experience.

**Mode 2: Random** 🎲  
Each cell operates independently with 7 different presets:
- **Calm** - Slow, peaceful transitions
- **Breathing** - Rhythmic pulsing patterns
- **Waves** - Horizontal wave effects
- **Sparse** - Minimal, zen-like changes
- **Dense** - Rich, frequent updates
- **Chaos** - Unpredictable rapid changes
- **Flicker** - Fast, stroboscopic effects

### Dynamic Features

- **Fullscreen Canvas** - Fills your entire browser window
- **Adaptive Grid** - Automatically adjusts to screen size
- **Smooth Transitions** - Modes blend seamlessly without resetting
- **High-Quality Rendering** - Anti-aliased images with device pixel ratio support
- **BPM-Scaled Animation** - Speed controls affect all visual elements
- **Responsive Zoom** - Adjust grid size from 10% to 100% of screen height

## Controls

### Basic Controls
- **M** - Toggle between Uniform and Random modes
- **D** - Toggle background (White/Black)
- **P** - Pause/Resume animation
- **I** - Hide/Show info and controls
- **R** - Reset to 1 cell / Restore previous configuration

### Grid Manipulation
- **← →** - Add/Remove cells one at a time
- **Shift + ← →** - Jump to perfect square grids (1×1, 2×2, 3×3, 4×4...)
- **Scroll Wheel** - Zoom grid in/out (10% to 100%)

### Speed Control
- **↑ ↓** - Increase/Decrease BPM (30-300 BPM, default: 120)

### Mode 2 Presets
- **1** - Calm
- **2** - Breathing
- **3** - Waves
- **4** - Sparse
- **5** - Dense
- **6** - Chaos
- **7** - Flicker

## Running Locally

### Option 1: Python HTTP Server
```bash
cd blue-square-web
python3 -m http.server 8001
```
Then open `http://localhost:8001` in your browser.

### Option 2: Direct File
Open `index.html` directly in your browser (may have CORS issues with images).

### Option 3: Live Server (VS Code)
Install the "Live Server" extension and click "Go Live" on `index.html`.

## Technical Details

- **Images**: 230 blue abstract JPG files (~115MB total)
- **Canvas**: Dynamic sizing with device pixel ratio support
- **Animation**: RequestAnimationFrame loop at 60fps
- **Image Loading**: Asynchronous with promises
- **State Management**: Preserves cell continuity across changes
- **BPM System**: Scales all animations proportionally (120 BPM baseline)

## Browser Compatibility

Works best in modern browsers with HTML5 Canvas support:
- Chrome/Edge (Recommended)
- Firefox
- Safari
- Opera

## Project Structure

```
blue-square-web/
├── index.html          # Main application (single file)
├── images/             # 230 blue abstract images
│   ├── blue-1.jpg
│   ├── blue-2.jpg
│   └── ... (blue-231.jpg, excluding blue-57.jpg)
├── README.md           # This file
└── HANDOVER.md         # Technical documentation
```

## Development History

This web version was created from an original Raspberry Pi Python project that displayed blue abstract images in a beat-synchronized grid. The web version maintains the original's aesthetic while adding:
- Interactive controls
- Multiple display modes
- Real-time BPM adjustment
- Smooth transitions
- Fullscreen responsive design

## Performance

- Starts at 50% zoom for optimal initial view
- High-DPI display support (Retina, 4K)
- Smooth image rendering with anti-aliasing
- Efficient canvas updates
- No lag with 100+ cells on modern hardware

## Credits

Created by Jorge Arreola  
Original Blue Square concept for Raspberry Pi 5

## License

MIT License - Feel free to use and modify!

---

**Enjoy the visual journey! 🟦✨**

*Press M to switch modes, use arrow keys to add cells, and experiment with different BPM speeds and presets.*
