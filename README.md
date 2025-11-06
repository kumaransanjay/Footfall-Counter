# Footfall-Counter
# Project Overview

This project implements a Footfall Counter using computer vision. The system detects and tracks people in a video and counts how many cross a defined Region of Interest (ROI) line  representing entries and exits.

## Objectives

Detect people in a video stream.

Track individuals across frames.

Define a virtual line or region of interest (ROI).

Count how many people enter and exit by crossing the  ROI.

Display results visually and as final totals.

## Tools & Libraries
Programming Language: Python

Detection Model: YOLOv8 (from Ultralytics, pretrained on COCO dataset)

Tracking Algorithm: DeepSORT

Visualization: OpenCV

```bash
pip install ultralytics deep-sort-realtime opencv-python numpy
```

## Approach

Detection: Use YOLOv8 to detect people in each video frame.

Tracking: Use DeepSORT to assign unique IDs to each person so their movement can be tracked across frames.

ROI Definition: Define a virtual counting line or region in the frame.

Counting Logic: Detect when a tracked person crosses the line — incrementing entry or exit counts.

Visualization: Draw bounding boxes, IDs, and counters on each frame and save the processed video.

## Counting Logic (Simplified Explanation)
Step 1: Define the ROI Line

The ROI line can be horizontal, vertical, or diagonal, defined by two endpoints:
```
p1 = (x1, y1)
p2 = (x2, y2)
```
Step 2: Compute Each Person’s Center Point

Each detected bounding box has a center:
```
cx = (x1 + x2) // 2
cy = (y1 + y2) // 2
```
Step 3: Determine Which Side of the Line They’re On

We calculate which side of the ROI line the person’s center is located on:
```
side = (x2 - x1)*(cy - y1) - (y2 - y1)*(cx - x1)

side > 0 → person is on one side

side < 0 → person is on the other side
```
Step 4: Detect Line Crossing

For each tracked person (using their unique ID), we compare the current and previous side values:
```
if side_prev > 0 and side_now < 0:
    entering += 1   # crossed one way (e.g., entered)
elif side_prev < 0 and side_now > 0:
    exiting += 1    # crossed the other way (e.g., exited)
```
This logic counts how many times a person’s movement crosses the ROI line — changing the sign of their position relative to it.

## Summary

The Footfall Counter combines AI-based person detection and object tracking with a simple but powerful geometric crossing logic. Each person is identified, tracked, and monitored relative to a line. When they cross it, the system automatically updates entry and exit totals, enabling real-time crowd analytics for smart environments.
