# SP-404SX VST Plugin

A VST3/AU plugin that emulates classic Roland SP-404SX effects.

## Effects Included

| Section        | Parameters                                    |
|----------------|-----------------------------------------------|
| **Lo-Fi**      | Bit Depth, Sample Rate Crush                  |
| **Vinyl Sim**  | Noise, Wow, Flutter                           |
| **Filter/ISO** | Cutoff, Resonance, Mode (LP/HP/BP/Isolator)   |
| **Reverb**     | Room Size, Damping, Mix                       |
| **Delay**      | Time, Feedback, Mix                           |
| **Scatter**    | Rate, Depth, ON/OFF button                    |
| **Tape Stop**  | Speed (decay), ON/OFF button                  |

---

## Build Instructions (Beginner-Friendly)

### Step 1 — Install Prerequisites

#### Windows
1. Install **Visual Studio 2022 Community** (free): https://visualstudio.microsoft.com/
   - During install, check: **"Desktop development with C++"**
2. Install **CMake**: https://cmake.org/download/
   - During install, select **"Add CMake to PATH"**
3. Install **Git**: https://git-scm.com/download/win

#### macOS
1. Install **Xcode** from the App Store (free)
2. Open Terminal, run: `xcode-select --install`
3. Install **Homebrew**: https://brew.sh then run `brew install cmake git`

#### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install cmake git build-essential \
    libasound2-dev libfreetype6-dev libx11-dev \
    libxcomposite-dev libxcursor-dev libxext-dev \
    libxinerama-dev libxrandr-dev libxrender-dev \
    libwebkit2gtk-4.0-dev libglu1-mesa-dev mesa-common-dev
```

---

### Step 2 — Build the Plugin

Open a terminal (or Command Prompt on Windows) in this folder.

```bash
# Create a build folder
mkdir build
cd build

# Configure (downloads JUCE automatically ~500MB, takes a few minutes)
cmake ..

# Build (use -j4 to use 4 CPU cores, replace with your count)
cmake --build . --config Release -j4
```

---

### Step 3 — Install the Plugin

#### Windows
After building, find `SP404SX.vst3` in:
```
build/SP404SX_artefacts/Release/VST3/SP404SX.vst3
```
Copy it to:
```
C:\Program Files\Common Files\VST3\
```

#### macOS
```
build/SP404SX_artefacts/Release/VST3/SP404SX.vst3
```
Copy to:
```
/Library/Audio/Plug-Ins/VST3/
```
For AU:
```
/Library/Audio/Plug-Ins/Components/
```

#### Linux
Copy to:
```
~/.vst3/
```

---

### Step 4 — Load in Your DAW

- **Ableton Live**: Options → Preferences → Plug-ins → Rescan
- **FL Studio**: Options → Manage Plugins → Find More → Rescan
- **Reaper**: Options → Preferences → Plug-ins/VST → Rescan
- **Bitwig**: Dashboard → Settings → Plug-ins → Rescan

---

## How to Use Each Effect

### Lo-Fi / Vinyl
- **BITS**: Reduces bit depth (16 = clean, 1 = extreme crunch)
- **S.RATE**: Reduces sample rate (1 = clean, 32 = robotic/crunchy)
- **NOISE**: Adds vinyl crackle and hiss
- **WOW**: Slow pitch wobble (like a warped record)
- **FLUTTER**: Fast pitch wobble (like tape speed variation)

### Filter / Isolator
- **CUTOFF**: Filter frequency
- **RESO**: Filter resonance / peak
- **MODE**: Choose Low Pass, High Pass, Band Pass, or Isolator

### Reverb
- **SIZE**: Room size
- **DAMP**: High-frequency absorption
- **MIX**: Dry/wet blend

### Delay / Echo
- **TIME**: Delay time in seconds
- **FEEDBACK**: How many echoes (keep below 0.9 to avoid runaway)
- **MIX**: Dry/wet blend

### Scatter (Beat Repeat)
- **RATE**: Loop length (fraction of a second, 1/16 to 1 bar)
- **DEPTH**: How much scatter replaces the original signal
- **SCATTER button**: Toggle ON to activate stutter loop

### Tape Stop
- **SPEED**: How fast the "tape" slows down
- **TAPE STOP button**: Toggle ON to trigger the effect

---

## Troubleshooting

**CMake can't find JUCE / download fails**
- Check your internet connection (FetchContent downloads JUCE automatically)
- Or manually clone JUCE: `git clone https://github.com/juce-framework/JUCE.git`
  Then change `FetchContent_Declare` to `add_subdirectory(/path/to/JUCE)`

**Plugin not showing in DAW**
- Make sure you copied to the correct VST3 folder
- Always rescan plugins in your DAW after installing

**Crackling audio**
- Increase your DAW's audio buffer size (512 or 1024 samples)

---

## Project Structure

```
SP404SX_VST/
├── CMakeLists.txt              ← Build configuration
├── README.md                   ← This file
└── Source/
    ├── PluginProcessor.h       ← DSP engine header
    ├── PluginProcessor.cpp     ← All audio effects DSP
    ├── PluginEditor.h          ← UI header
    └── PluginEditor.cpp        ← UI rendering & layout
```

---

## Customizing

To change the UI size, edit this line in `PluginEditor.cpp`:
```cpp
setSize (820, 540);
```

To add a new parameter, add it in `createParameterLayout()` in `PluginProcessor.cpp`
and register a listener for it.

---

## License

Free to use and modify for personal and commercial projects.
JUCE is used under the JUCE Personal/Startup license (free for revenue < $50k/yr).
For commercial release, check JUCE licensing at https://juce.com/get-juce/
