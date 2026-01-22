# Blue Square Project - Handover Document

## Current Status: BLOCKED - Syntax Error in Deployed Code

**Last Updated:** 22 January 2026  
**User:** Jorge  
**Project:** Blue Square - Web version of Raspberry Pi image grid project

---

## Problem Summary

The Blue Square web app is stuck on "Loading Blue Square... 0%" with a **JavaScript syntax error on line 188**. The error is:
```
Uncaught SyntaxError: Invalid or unexpected token
```

### Root Cause
The **deployed version on GitHub Pages is corrupted** with line breaks in the middle of code. The local file is correct, but GitHub Pages keeps serving a broken version with this malformed code:
```javascript
// Line 188-190 (BROKEN in deployed version):
nextUniformImage = Math.floor(Math.random() * T
OTAL_IMAGES);  // Line break splits "TOTAL_IMAGES"
```

### What We've Tried (All Failed)
1. ✗ Fixed typo `MathbackgroundColorm` → `Math.random()` (commit 828e3cf)
2. ✗ Changed loading strategy (start immediately, load images in background)
3. ✗ Added blue placeholder squares while images load
4. ✗ Converted to Unix line endings
5. ✗ Deleted and re-added index.html to GitHub (commits 3654577, c622fb7)
6. ✗ Multiple force rebuilds and empty commits
7. ✗ **Result:** GitHub Pages now **CANCELLING ALL DEPLOYMENTS** (rate-limited)

### Current Files
- **Local file:** `/home/jorge/blue-square-web/index.html` - **WORKS CORRECTLY**
- **GitHub repo:** `jorch-wav/blue-square` - Contains correct code
- **GitHub Pages:** `https://jorch-wav.github.io/blue-square/` - **SERVING CORRUPTED VERSION**
- **Images:** 230 blue JPG files in `/home/jorge/blue-square-web/images/` (blue-1.jpg to blue-231.jpg, missing blue-57.jpg)

---

## What This Project Is

### Original (Raspberry Pi)
- Python app displaying grid of 230 blue abstract images
- Changes images every 8 beats at 120 BPM
- Two modes:
  - **Mode 1 (Uniform):** ALL cells show SAME image (changes every 8 beats)
  - **Mode 2 (Random):** Each cell independent, 7 presets

### Web Version Requirements
- Grid of cells (start with 1 cell)
- **Mode 1:** All cells must show identical image (matching Pi behavior)
- **Mode 2:** Random patterns with 7 presets
- **Controls:**
  - Left/Right arrows: Add/remove cells (1 at a time, long-press for rapid)
  - Scroll: Zoom in/out
  - D: Toggle background on/off
  - M: Switch modes
  - P: Pause
  - 1-7: Jump to preset (Mode 2 only)

---

## Technical Details

### File Structure
```
/home/jorge/blue-square-web/
├── index.html          (426 lines, 16.8KB)
├── images/             (230 files, ~115MB total)
│   ├── blue-1.jpg
│   ├── blue-2.jpg
│   └── ... (blue-231.jpg, missing blue-57.jpg)
├── test.html           (Simple test page)
└── HANDOVER.md         (This file)
```

### Key Code Structure (index.html)
- **Lines 1-49:** HTML structure, CSS
- **Lines 50-100:** Constants and variables
- **Lines 117-139:** `loadImages()` - Starts app immediately, loads in background
- **Lines 141-165:** `initializeGrid()` - Sets up grid state
- **Lines 175-202:** `updateUniform()` - Mode 1 logic (uniform image)
- **Lines 205-232:** `updateRandom()` - Mode 2 logic (random cells)
- **Lines 235-303:** `draw()` - Canvas rendering
  - Mode 1: All cells draw same `uniformImage`
  - Mode 2: Each cell draws individual `grid[i].imageIndex`

### Critical Variables
```javascript
const TOTAL_IMAGES = 230;
const DEFAULT_CELLS = 1;        // Start with 1 cell
const FRAMES_PER_BEAT = 30;     // 120 BPM

let mode = 1;                    // 1=Uniform, 2=Random
let cellCount = DEFAULT_CELLS;
let uniformImage = 0;            // Mode 1: current image index (ALL cells)
let grid = [];                   // Mode 2: array of cell states
let images = [];                 // Array of Image objects
let loadedImages = new Set();    // Tracks which images loaded
```

---

## Git History (Last 10 Commits)
```
c622fb7 - Re-add index.html fresh
3654577 - Remove broken index.html
31e88a8 - Force rebuild
686eb35 - Add test file
828e3cf - Fix critical typo: MathbackgroundColorm -> Math.random
9b2a06d - Fix loading: load first 10 images then start immediately
717d067 - Fix: Set DEFAULT_CELLS to 1 to start with 1 cell
0a09bed - Update controls: arrows add/remove cells, scroll zooms, d changes background
513f36f - Fix Mode 1: All cells now show same uniform image
3b7f84a - Implement progressive loading (batches of 30)
```

---

## How to Test Locally

### Option 1: Python Server (RECOMMENDED)
```bash
cd /home/jorge/blue-square-web
python3 -m http.server 8001
# Open browser to: http://localhost:8001
```

### Option 2: Direct File
```
file:///home/jorge/blue-square-web/index.html
```
(May have CORS issues with images)

### Expected Behavior
1. App starts immediately
2. Shows 1 blue square cell (or blue placeholder if image not loaded yet)
3. Cell changes image every 4 seconds (8 beats at 120 BPM)
4. Left/Right arrows add/remove cells
5. All cells show SAME image in Mode 1

---

## Next Steps to Fix

### Immediate Actions
1. **Wait 2-3 hours for GitHub Pages rate limit to clear**
2. Check if https://jorch-wav.github.io/blue-square/ works after rate limit clears
3. If still broken, **SKIP GITHUB PAGES** and use alternative deployment:

### Alternative Deployment Options

#### Option A: Vercel (RECOMMENDED)
```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
cd /home/jorge/blue-square-web
vercel --prod
```

#### Option B: Netlify Drop
1. Go to https://app.netlify.com/drop
2. Drag `/home/jorge/blue-square-web` folder
3. Get instant URL

#### Option C: Keep Local Only
```bash
# Add to user's cron or startup script
cd /home/jorge/blue-square-web && python3 -m http.server 8001 &
```

### If Editing Code
The local `index.html` is **CORRECT**. Key things to preserve:
- `DEFAULT_CELLS = 1` (line 55)
- `uniformImage` logic in Mode 1 (lines 175-202, 246-262)
- Blue placeholder drawing when `!loadedImages.has(imageIndex)` (lines 254, 298)
- `loadImages()` starts app immediately (line 119-125)

---

## Known Issues

### 1. GitHub Pages Corruption
- **Symptom:** Line 188 has `Math.random() * T` then newline then `OTAL_IMAGES)`
- **Cause:** Unknown (possibly GitHub Pages Jekyll processing or git corruption)
- **Fix:** Use different hosting service

### 2. Image Load Performance
- **Size:** 230 images × 500-770KB each = ~115MB total
- **Strategy:** App starts immediately with placeholders, images load in background
- **Works:** Yes (locally verified)

### 3. Missing Image
- **File:** `blue-57.jpg` doesn't exist
- **Impact:** None (code handles missing images gracefully)

---

## Important URLs

- **GitHub Repo:** https://github.com/jorch-wav/blue-square
- **GitHub Pages (BROKEN):** https://jorch-wav.github.io/blue-square/
- **Local Test:** http://localhost:8001
- **Original Pi Project:** `/home/jorge/BLUE SQUARE/` (Python files)

---

## Contact & Context

**User:** Jorge  
**System:** Raspberry Pi 5, Linux  
**Browser:** Chromium  
**Editor:** VS Code

**Related Projects:**
- Boids Playground: `/home/jorge/Boids Playground/boids_gpu.html` (GPU-accelerated particle simulation)
- Original Blue Square: `/home/jorge/BLUE SQUARE/blue_square_v*.py`

---

## Quick Diagnosis Commands

```bash
# Check if file is correct locally
sed -n '188,192p' /home/jorge/blue-square-web/index.html
# Should show: nextUniformImage = Math.floor(Math.random() * TOTAL_IMAGES);

# Check what GitHub Pages is serving
curl -s https://jorch-wav.github.io/blue-square/ | sed -n '188,192p'
# Will show corrupted line break

# Test local server
cd /home/jorge/blue-square-web && python3 -m http.server 8001 &
# Open http://localhost:8001

# Check git status
cd /home/jorge/blue-square-web && git status && git log --oneline -5
```

---

## Success Criteria

The project is complete when:
- ✓ User can access app via URL (not just local)
- ✓ App starts immediately (no loading screen stuck)
- ✓ Grid displays blue square images
- ✓ Mode 1: All cells show same image
- ✓ Images change every 8 beats
- ✓ Controls work (arrows, zoom, mode switch)

**Current Status:** 5/6 complete (blocked on deployment only, all code works locally)

---

## Final Notes

**The code is CORRECT.** The issue is purely deployment/hosting. The local file at `/home/jorge/blue-square-web/index.html` works perfectly when served via Python's HTTP server. 

**Do NOT edit the JavaScript** - the syntax error only exists in the deployed version. Any edits should focus on deployment strategy, not code fixes.

**GitHub Pages is rate-limited** - wait several hours or switch to Vercel/Netlify for immediate results.

Good luck! 🚀
