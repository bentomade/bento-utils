# Custom Mouse Data Setup Guide

## Overview

Check the original SmartMouse repo if you would like general overview/steps to take, [SmartMouseV2](https://github.com/Bandit-HaxUnit/SmartMouseV2).

Custom mouse data allows your Bento scripts to use personalized, human-like mouse movements instead of the default DreamBot mouse algorithm. This feature helps:

- **Avoid Detection**: Your mouse movements are unique to you, not shared with other botters
- **Look More Human**: Uses real timing patterns from your actual mouse movements
- **Reduce Profiling**: Harder for anti-cheat systems to build patterns when everyone uses different mouse data

**This is completely optional** - your scripts work perfectly fine without custom mouse data. However, many users find it beneficial for reducing ban rates.

Note, the custom mouse only works if you:
- **Disable Menu Manipulation** in your script settings
- **Disable No Click Walk** in your script settings

Also, you can customize the mouse speed using the built in dreambot client mouse speed slider. 50% mouse speed is the baseline speed.

Just as an additional example, I've attached some example mouse data in this directory which was the output from me following these steps myself.
`
---

## Quick Start (Using Pre-Generated Data)

If you just want to try custom mouse quickly without generating your own:

### Step 1: Download Sample Data
Download the sample mouse data file from this repository:
- [mousedata.json](https://github.com/bentomade/bento-utils/blob/master/mousedata/mousedata.json) *(if available)*

Or ask in the Discord for a sample file.

### Step 2: Place the File
1. Create the directory (if it doesn't exist):
   - **Windows**: `C:\Users\YourName\DreamBot\Scripts\bento\util\`
   - **Mac**: `~/DreamBot/Scripts/bento/util/`
   - **Linux**: `~/DreamBot/Scripts/bento/util/`

2. Copy `mousedata.json` (or whatever you named it) into that directory

### Step 3: Configure Your Script
1. Open your Bento script GUI
2. Go to the **Misc** tab
3. Find the "Mouse Data File" field
4. Enter the filename: `mousedata.json` (or whatever you named your mouse data file)
5. Save your config

### Step 4: Verify It's Working
1. Start the script
2. There should be a log that says something like the following: `Successfully initialized custom mouse algorithm from: mousedata.json`
3. Watch the mouse move - it should look more natural now as it is seeded with the data you generated.

---

## Generating Your Own Profile (Recommended)

For best results, generate your own unique mouse profile. This takes about 10-15 minutes.

### Prerequisites

You'll need Python 3 installed on your computer:
- **Windows**: Download from [python.org](https://www.python.org/downloads/)
- **Mac**: Usually pre-installed, or use `brew install python3`
- **Linux**: `sudo apt install python3 python3-pip` (Ubuntu/Debian)

### Step 1: Clone the SmartMouseV2 Repository

Open your terminal/command prompt and run:

```bash
git clone https://github.com/Bandit-HaxUnit/SmartMouseV2
cd SmartMouseV2
```

If you don't have `git`, you can download the ZIP from GitHub and extract it.

### Step 2: Install Dependencies

Run this command in the SmartMouseV2 directory:

```bash
python3 -m pip install pynput
```

**Note**: On Windows, you might need to use `python` instead of `python3`:
```bash
python -m pip install pynput
```

If you run into issues you may also try other variants of installing necessary packages in the right env:
```bash
pip install pynput
```
or
```bash
pip3 install pynput
```

### Step 3: Record Your Mouse Movements

Run the recorder:

```bash
python3 recorder.py
```

A window will appear with red and blue dots. Here's how it works:

1. **Click the red dot** (start point)
2. **Move your mouse naturally** to the blue dot (like you normally would)
3. **Click the blue dot** (end point)
4. **Repeat** for different distances and directions

The program will guide you through recording movements in all 8 directions (N, NE, E, SE, S, SW, W, NW) and 11 distance categories.

**Tips for good data:**
- Move your mouse naturally - don't try to be perfect
- Use your normal mouse speed
- Complete all the prompts for best coverage
- Takes about 10-15 minutes to record everything

When finished, you'll have a file called `mousedata_raw.json`.

### Step 4: Process the Raw Data

Convert your raw recordings into usable mouse data:

```bash
python3 parser.py
```

This creates `mousedata.json` with all your mouse movement patterns organized and ready to use.

You should see output like:
```
Created 'mousedata.json' with 8 directions, including timing data.
```

### Step 5: Copy to Bento Directory

Copy your new `mousedata.json` to the Bento util directory. Make sure to create the util directory nested within bento directory if you haven't already:

**Windows:**
```bash
copy mousedata.json C:\Users\YourName\DreamBot\Scripts\bento\util\
```

**Mac/Linux:**
```bash
cp mousedata.json ~/DreamBot/Scripts/bento/util/
```

### Step 6: Configure and Test

Refer to Steps 3-4 from the Quick Start section above. Basically, add the name of your mouse data file to your desired settings in the GUI before starting your desired script.

---

## Bento Scripts Configuration

### Adding Mouse Data to Your Script

1. Open the script GUI (before starting)
2. Navigate to the **Misc** tab
3. Locate the "Mouse Data File" field
4. Enter your mouse data filename (e.g., `mousedata.json`). Note that your data must be in `bento/util` directory nested under dreambot scripts directory.
5. **Leave empty to use default DreamBot mouse**
6. Save your configuration and start your script.

---

## Multiple Profiles (Advanced)

You can create different mouse profiles for different activities:

### Creating Multiple Profiles

1. Generate mouse data as normal
2. Rename to something descriptive:
   - `mousedata_pvm.json` - for PvM (fast, aggressive)
   - `mousedata_skilling.json` - for skilling (slower, methodical)
   - `mousedata_default.json` - your general profile

3. Place all profiles in `bento/util/` directory
4. Now for different script configs you can use different mouse data file names in the settings if you would like.

## Troubleshooting

### "Mouse data file not found"

**Problem**: Script can't find your mousedata.json file

**Solutions**:
1. Verify the file is in the correct location:
   - Check: `DreamBot/Scripts/bento/util/mousedata.json`

2. Check the filename spelling in your config matches exactly

3. Make sure the file has `.json` extension (might be hidden on Windows)

### "Invalid mouse data structure"

**Problem**: Your mousedata.json file is corrupted or invalid

**Solutions**:
1. Re-run `python3 parser.py` to regenerate the file
2. Make sure you completed all recording steps in recorder.py
3. Try downloading a fresh copy of SmartMouseV2 and start over

### "Failed to parse mouse data"

**Problem**: JSON file is malformed

**Solutions**:
1. Don't manually edit the mousedata.json file
2. Re-generate using parser.py
3. Make sure you're using the latest version of SmartMouseV2

### "ModuleNotFoundError: No module named '_tkinter'" during initial recording

**Problem**: Python can't find tkinter when running recorder.py (common on macOS with Homebrew Python)

**Solutions**:

1. **Install python-tk via Homebrew** (Recommended for Mac):
   ```bash
   brew install python-tk@3.12
   ```
   Replace `3.12` with your Python version. Check with: `python3 --version`

2. **Use macOS system Python**:
   ```bash
   /usr/bin/python3 recorder.py
   ```
   The system Python usually includes tkinter by default.

3. **Install Python from python.org**:
   Download and install Python from [python.org](https://www.python.org/downloads/), which includes tkinter.

4. **Verify tkinter works**:
   ```bash
   python3 -m tkinter
   ```
   This should open a small test window if tkinter is properly installed.

### "ModuleNotFoundError: No module named 'pynput'"

**Problem**: Missing required pynput library. You need to install this dependency as detailed above.

**Solutions**:

1. Install pynput:
   ```bash
   pip3 install pynput
   ```

2. Or if using system Python:
   ```bash
   python3 -m pip install pynput
   ```

3. On Windows, use:
   ```bash
   python -m pip install pynput
   ```

## References

- **SmartMouseV2 Repository**: [SmartMouseV2](https://github.com/Bandit-HaxUnit/SmartMouseV2)
- **Python Download**: https://www.python.org/downloads/
- **Support**: Ask in the Bento scripts Discord for help
