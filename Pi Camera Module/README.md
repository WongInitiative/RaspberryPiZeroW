# Plant Growth Time-Lapse Automation Script

This Python script is designed to automate the process of capturing time-lapse photographs documenting the growth of a plant using a Raspberry Pi equipped with a camera module. It sequentially captures images over a specified duration, compiles these into a time-lapse video, and organizes the output into designated directories, facilitating easy periodic captures through a **cron job** setup.

## Dependencies

- **PiCamera**: A Python library for controlling the Raspberry Pi Camera.
- **time**: A standard Python module for time-related functions.
- **os**: A Python module for interacting with the operating system.

## Configuration

Before running the script, ensure the following parameters are configured according to your needs:

- `camera.resolution = (1920, 1080)`: Set the resolution of the camera to 1920x1080 pixels.
- `SleepTimeL = 300`: Set the time interval (in seconds) between each photo capture.
- `FrameCount = 0`: Initializes the frame counter.
- `FrameStop = 240`: Total number of frames to capture.

## How It Works

### Initial Setup
The script sets up the camera's resolution and calculates the estimated total duration for the photography session based on the number of frames (`FrameStop`) and the interval (`SleepTimeL`).

### Capturing Images
The script enters a loop, capturing a single image per iteration until the `FrameCount` equals `FrameStop`. Each image file is sequentially named (`image0000.jpg`, `image0001.jpg`, etc.). The script pauses for `SleepTimeL` seconds between captures.

### Creating the Video
After capturing all images:
- The script uses `ffmpeg` to compile these images into a video. The output video file is named based on the current date and time to ensure uniqueness and is set at a frame rate of 24 fps.

### Managing Files
- The video is moved to `/home/pi/Documents/Grow1/timelapse/completed/` for organized storage.
- All captured image files are deleted to free up space.

### Completion
The script prints a message upon successful completion of the entire process.

## Setup Instructions

1. **Install Required Libraries**: Ensure that `ffmpeg` and `PiCamera` are installed on your Raspberry Pi.
   ```bash
   sudo apt update
   sudo apt install ffmpeg
   pip install picamera


2. Prepare the Storage Directory: Ensure the path /home/pi/Documents/Grow1/timelapse/completed/ exists, or modify the script to point to an existing directory.

3. Set up the Cron Job:
Edit the crontab file for your user on the Raspberry Pi by running:

crontab -e

4. Add a line to execute the script at your desired frequency. For example, to run the script every day at midnight:

0 0 * * * /usr/bin/python3 /path/to/your/timelapse/script.py


##Additional Notes
This script does not include error handling and assumes that the camera and file system are always in a ready state. For robust production use, consider adding error handling and validation steps.
