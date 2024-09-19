# **Video/Audio Conversion Scripts**

## **Overview**
These shell scripts are designed to manipulate video files using `ffmpeg`, a powerful command-line tool for handling video, audio, and other multimedia files. The scripts allow you to:
1. **Increase the frame rate of a video.**
2. **Adjust the speed of a video and change the frame rate.**
3. **Compress the videos/audio files.**
While also having the benefits of being able to do so in an easier way

## **Requirements**
To run these shell scripts, you need the following:
1. **Bash Shell**: A shell environment to execute the script (typically pre-installed on macOS and Linux, and available on Windows via tools like Git Bash or Windows Subsystem for Linux (WSL)).
2. **FFmpeg**: The core software for video processing.

### **Installing Bash Shell**
- **macOS**: Bash is pre-installed. You can access it through the Terminal.
- **Linux**: Bash is pre-installed. You can access it through the Terminal.
- **Windows**:
  - **Git Bash**: Download and install [Git Bash](https://gitforwindows.org/).
  - **Windows Subsystem for Linux (WSL)**: Install WSL and a Linux distribution (e.g., Ubuntu) from the Microsoft Store. Follow the instructions [here](https://docs.microsoft.com/en-us/windows/wsl/install).

### **Installing FFmpeg**
- **macOS**:
  1. Install [Homebrew](https://brew/) if you don't have it.
  2. Run the following command in Terminal:
     ```bash
     brew install ffmpeg
     ```
- **Linux**:
  ```bash
  sudo apt update
  sudo apt install ffmpeg
  ```
- **Windows**:
  1. Download FFmpeg from the [official website](https://ffmpeg.org/download.html).
  2. Follow the installation guide for Windows [here](https://www.geeksforgeeks.org/how-to-install-ffmpeg-on-windows/).

## **Scripts**
Make sure you add execute permissions to each of these shell scripts, so it run on your terminal. You can do this by running this in the terminal:
```
chmod +x <filename>
```

### **1. Script for Increasing Frame Rate**

#### **Filename**: `converter`

#### **Description**:
This script takes a video file and saves the output in the specified format, without the need to know how to use the commands in ffmpeg.

#### **Usage**:
```
./converter <input_video_path> <output_path> <new_fps>
```

#### **Parameters**:
- `input_video_path`: Path to the input video file (e.g., `input_video.mp4`).
- `output_path`: Path where the output video will be saved (e.g., `output_video.mp4`).
- `new_fps`: Desired frame rate for the output video (e.g., `60`).

#### **Example**:
```
./converter input_video.mov output_video.mp4 60
```

### **2. Script for Adjusting Speed and Frame Rate**

#### **Filename**: `speedup_vid`

#### **Description**:
This script adjusts the speed of a video and increases its frame rate simultaneously.

#### **Usage**:
```bash
./speedup_vid <input_video_path> <output_path> <speed_factor>
```

#### **Parameters**:
- `input_video_path`: Path to the input video file (e.g., `input_video.mp4`).
- `output_path`: Path where the output video will be saved (e.g., `output_video.mp4`).
- `speed_factor`: Factor by which to speed up the video (e.g., `1.75` for 1.75x speed).
- `new_fps`: Desired frame rate for the output video (e.g., `60`).

#### **Example**:
```bash
./adjust_speed_and_fps input_video.mp4 output_video.mp4 2.0 60
```

#### **Script Content**:
```bash
#!/bin/bash

# Check if the correct number of arguments is provided
if [ "$#" -ne 4 ]; then
    echo "Usage: $0 <input_video_path> <output_path> <speed_factor> <new_fps>"
    exit 1
fi

# Assign arguments to variables
input_video_path="$1"
output_path="$2"
speed_factor="$3"
new_fps="$4"

# Calculate the setpts value
setpts_value=$(echo "scale=2; 1/$speed_factor" | bc)

# Apply speed adjustment and frame rate increase using FFmpeg
ffmpeg -i "$input_video_path" -filter_complex "[0:v]setpts=${setpts_value}*PTS,minterpolate='mi_mode=mci:mc_mode=aobmc:vsbmc=1:fps=$new_fps'[v]" -map "[v]" -map 0:a -r "$new_fps" "$output_path"
```

## **Running the Scripts**

### **macOS/Linux:**
1. **Navigate** to the directory where the script is located:
   ```bash
   cd /path/to/your/scripts
   ```
2. **Make the script executable**:
   ```bash
   chmod +x increase_fps
   chmod +x adjust_speed_and_fps
   ```
3. **Run the script** using the appropriate parameters:
   ```bash
   ./increase_fps input_video.mp4 output_video.mp4 60
   ```

### **Windows (Git Bash or WSL):**
1. **Navigate** to the directory where the script is located:
   ```bash
   cd /path/to/your/scripts
   ```
2. **Make the script executable** (if using WSL):
   ```bash
   chmod +x increase_fps
   chmod +x adjust_speed_and_fps
   ```
3. **Run the script**:
   ```bash
   ./increase_fps input_video.mp4 output_video.mp4 60
   ```

## **Tips for Cross-Platform Usage**
- **Line Endings**: Ensure that your shell scripts use Unix-style line endings (LF) rather than Windows-style (CRLF). Tools like `dos2unix` can help convert line endings.
- **FFmpeg Path**: On Windows, make sure that the `ffmpeg` executable is in your system’s PATH or specify the full path to `ffmpeg` in the script.

## **Common Issues**
- **Permission Denied**: Ensure that the script has executable permissions (`chmod +x script`).
- **Command Not Found**: Ensure `ffmpeg` is installed and accessible from the command line.


