# Traffic Monitoring and Analysis System

This project implements a computer vision-based system for real-time traffic monitoring and analysis using MATLAB.  
It detects vehicle presence across four lanes (North, South, East, and West) through background subtraction and region-of-interest (ROI) masking, and uploads processed traffic data to the ThingSpeak cloud platform for visualization.

---

## Key Features

- Live Video Capture — Captures real-time frames using a connected webcam.  
- ROI Masking — Defines and isolates four traffic lanes for focused detection.  
- Background Subtraction — Detects motion by comparing current frames with a static background image.  
- Traffic Quantification — Computes normalized movement area as a measure of lane congestion.  
- Cloud Integration — Sends real-time data to ThingSpeak for visualization and logging.  
- Visual Feedback — Displays annotated video feed (red for heavy traffic, cyan for low traffic).

---

## Software and Tools

| Tool | Purpose |
|------|----------|
| MATLAB | Primary development environment |
| Image Processing Toolbox | For image manipulation and detection operations |
| MATLAB Support Package for Webcam | Enables live video input |
| ThingSpeak IoT Platform | Cloud-based data logging and visualization |

---

## Project Workflow

### Stage 1: Capture Background (`ex1.m`)

Captures a reference image of the traffic intersection (without vehicles).

```matlab
cam = webcam(2);   % Connect to webcam (index 2)
img = snapshot(cam);
imwrite(img, 'background.bmp');
Output: background.bmp (static background image)

Stage 2: Define Monitoring Zones (ex2.m)
Identifies four traffic lanes using a mask image and crops them for focused detection.

Requirements:

background.bmp

mask.bmp (white cross-shaped image defining lanes)

Output:

1.bmp, 2.bmp, 3.bmp, 4.bmp — background models for each lane.

Stage 3: Real-Time Detection and Reporting (ex3.m)
Performs live monitoring, detects movement, and sends traffic metrics to ThingSpeak.

Dependencies:

background.bmp, mask.bmp, 1.bmp, 2.bmp, 3.bmp, 4.bmp

External function:

matlab
Copy code
[detectionFlag, normalizedTrafficArea] = videodetection(backgroundFrame, currentFrame, Thre, Area);
(Must be implemented separately for motion detection.)

ThingSpeak Configuration:

matlab
Copy code
thingSpeakWrite(1562240, {north, south, east, west}, 'WriteKey', '5PPWSEBP10NDFEOJ');
Field	Description
Field 1	North_Traffic_Area
Field 2	South_Traffic_Area
Field 3	East_Traffic_Area
Field 4	West_Traffic_Area

Data is uploaded every 20 seconds to ThingSpeak.

ThingSpeak Setup
Create a ThingSpeak Channel.

Define 4 fields:

Field 1: North_Traffic_Area

Field 2: South_Traffic_Area

Field 3: East_Traffic_Area

Field 4: West_Traffic_Area

Copy your Channel ID and Write API Key.

Replace them in ex3.m.

Folder Structure
bash
Copy code
Traffic-Monitoring-Analysis/
│
├── ex1.m                 # Capture background image
├── ex2.m                 # Mask and crop traffic zones
├── ex3.m                 # Real-time detection and upload
├── mask.bmp              # ROI mask (cross-shaped)
├── background.bmp        # Static reference image
├── 1.bmp, 2.bmp, 3.bmp, 4.bmp  # Individual lane backgrounds
└── README.md             # Project documentation
Future Enhancements
Integrate object detection for vehicle counting.

Add AI-based traffic prediction using time-series analysis.

Implement a GUI dashboard for real-time monitoring.

Author
Siddharth Chandra Prabhakar
B.Tech (Electronics and Communication Engineering)
National Institute of Technology, Sikkim
