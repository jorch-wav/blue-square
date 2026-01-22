# Blue Square

An interactive audio-reactive visual art piece with procedurally generated blue-themed images.

🎨 **[Live Demo](https://jorch-wav.github.io/blue-square)**

## Features

- **2 Display Modes:**
  - **Mode 1 (Uniform)**: Organized grid layout
  - **Mode 2 (Random)**: Dynamic random patterns with 7 unique presets

- **Audio Reactive**: Responds to beat detection at 120 BPM
- **230 Procedurally Generated Images**: Unique blue-themed patterns
- **Interactive Controls**: Full keyboard and mouse control
- **60 FPS Animation**: Smooth transitions and effects

## Controls

| Key | Action |
|-----|--------|
| `M` | Toggle mode (Uniform ↔ Random) |
| `B` | Toggle beat detection |
| `P` | Pause/Resume animation |
| `I` | Show/Hide images |
| `←` `→` | Navigate presets (Mode 2) |
| `↑` `↓` | Adjust animation speed |
| `+` `-` | Adjust image scale |
| `1-7` | Jump to specific preset (Mode 2) |
| **Mouse Wheel** | Adjust grid size |

## Installation

This is a single-file web application with no dependencies. Simply open `index.html` in a modern web browser.

```bash
# Clone the repository
git clone https://github.com/jorch-wav/blue-square.git
cd blue-square

# Open in browser
open index.html  # macOS
xdg-open index.html  # Linux
start index.html  # Windows
```

## Technical Details

- **Pure HTML5/CSS3/JavaScript** - No external dependencies
- **Canvas API** for rendering
- **Procedural generation** for all visual content
- **Optimized for 60 FPS** performance

## Original Platform

Originally developed for Raspberry Pi with hardware controls:
- Physical buttons (PiicoDev NeoKey)
- Rotary encoder (Adafruit Seesaw)
- Potentiometer for preset selection

## License

MIT License - Feel free to use and modify

---

Created with ❤️ by Jorge Arreola
