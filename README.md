
# Augmented Reality Terrain Display (ART-D)

This GitHub repository contains the code and resources for  **ART-D**. The goal of this project is to help private-industry and recreational pilots withstand low visual flight range (VFR) conditions during flight, including the ability to make guided emergency landings with low to zero collision probability.

## Methodology

The project first started out by using privately-generated 3D map TPK tiles of several major counties, including San Luis Obispo. These came in the form of `.obj`'s. An initial proof of concept comprised of a Unity project  built for Microsoft HoloLens (generation 1) with this TPK tile loaded. A user could walk around the map tile at a scale that was true to life.

[Watch Demo](https://youtu.be/DftBt-RP7Qc)

The next iteration involved finding a way to separate the pilot's head movement from the plane's constant orientation and elevation changes. It turns out that modern smartphones contain all the sensors necessary for this purpose: barometer for elevation, compass for heading and accelerometer for speed. By tying the Unity camera's location and origin to that of the smartphone, the headset's movement could now be an independent process. Determining a protocol for communication between the headset and smartphone yielded two options: Bluetooth and WiFi.

Considering that the headset and smartphone would stay within short distance (both located inside the airframe cabin), Bluetooth's lower power requirements and "technically" lower latencies made it desirable. However, after testing, it turned out that sending barometer, compass and accelerometer data at the fastest rates possible (120 Hz) exceeded Bluetooth's bandwidth, leaving WiFi as the only viable option. In the current implementation, the phone outputs a WiFi hotspot that the headset connects to.

On top of the internet protocol, there was also a need for a data protocol. Open sound control (OSC) was chosen for its long history of use in audio synthesizers and networked multimedia devices and is specially optimized for long streams of data. Considering that a pilot might be airborne anywhere from 2 to 8 hours, ensuring that the data protocol was error and corruption-resistant was of utmost priority. Downsides of OSC include its computational inefficiency and lack of message standardization.

At the time of this writing (03/24/2023), ART-D's implementation relies on two core connectivity technologies: 1) WiFi for stream connection between the smartphone and headset (WiFi mobile hotspot) 2) OSC for data protocol communication (using an iOS app called GyrOSC).

![GyrOSC app screenshot](https://www.bitshapesoftware.com/instruments/gyrosc/data/gyrosc-screenshots-2.5.png)
*GyrOSC is a mobile app that streams gyro and other data from the phone to a target IP address using the Open Sound Control (OSC) protocol*

## 3D Map Tiles

The first iteration of ART-D was built for the first-generation HoloLens and used a manually generated TPK tile. However, the second iteration moved onto using the ArcGIS Maps SDK because of its ease of use, optimization for augmented reality devices and high resolution. It also offers offline capability, wherein map tiles can be downloaded and saved on-device. This is useful for ART-D, as flight routes are typically planned in advance, making downloading the necessary map tiles possible in order to avoid requiring an internet connection while airborne.

![ArcGIS Maps SDK in Unity](https://www.esri.com/arcgis-blog/wp-content/uploads/2020/10/Unreal_NY_005-scaled.jpg)

## Code

The code for this project is organized into the following folders:

-   **Scenes**: Contains ArcGIS world model and XR rig.
-   **Scripts**: Contains scripts for OSC communication and camera tracking.

## Requirements

The following packages are required to run the code:

-   ArcGIS Maps SDK for Unity (generate an API token [here](https://developers.arcgis.com/dashboard/))
- Unity (2022 or later)
- Windows OS (requires [MRTK](https://github.com/microsoft/MixedRealityToolkit-Unity))

## How to Build

1.  Clone the repository
2.  Install the required packages
3.  Open in Unity
4.  Go to:
    -  File
    - Build Settings
	    - Select `Build for HoloLens (ARM)`

## Authors

- Jaron Schreiber

## License

This project is open-source and free to use.

## Acknowledgments

A very big thank you to Christian Eckhardt (the Cal Poly advisor for this project), Cal Poly President Jeffrey Armstrong (who approved the test flight) and SunWest Aviation (who provided the flight).