# csci6231project3
G37208826
nongxu LI

(a) 2D Pixel ⇄ 3D GPS Correspondence

The following 2D image pixel coordinates were manually annotated on the image and matched to real-world 3D locations (latitude, longitude, altitude) obtained from Google Earth / mapping tools. These correspondences were used for camera calibration and pose estimation.

Point	Pixel X (px)	Pixel Y (px)	Latitude (°N)	Longitude (°W)	Altitude (m)
P0	369	768	42.33467500	-71.16827500	59
P1	277	386	42.33458333	-71.16863611	84
P2	316	728	42.33463611	-71.16841667	59
P3	378	873	42.33470833	-71.16794722	60
P4	1750	693	42.33535556	-71.16873333	63
P5	974	313	42.33560278	-71.17042778	113
P6	623	372	42.33485000	-71.16869444	84

These points were converted from GPS → ECEF → local ENU (East-North-Up) coordinates using camera GPS as the local origin before running pose estimation.



(b) Recovered Camera Location

The camera position was unknown and therefore estimated using OpenCV camera pose estimation (solvePnP / calibrateCamera).
The initial camera GPS prior used as ENU origin was:

42°20'5.0" N, 71°10'2.5" W, 70 m
(Decimal form: 42.33472222, -71.16736111, 70.0)

After pose optimization, the camera center recovered in the ENU frame was approximately:

Camera Center (ENU):
X ≈ -22.2 m  (East)
Y ≈ -11.1 m  (North)
Z ≈  -3.1 m  (Up)


This indicates the camera is positioned roughly 22 m west, 11 m south, and 3 m below the chosen origin point. The small negative Z suggests the assumed initial altitude was slightly above the real camera height, which is expected because the camera elevation was an estimate.


(c) Insights from the Point Selection & Calibration Process

Altitude values critically impact accuracy.
Small horizontal (lat/lon) errors produce minor pixel shifts, but even a ±5 m error in altitude significantly affects reprojection and camera pose.

Choice of origin matters.
Using the approximate camera GPS as the ENU origin produced far more stable calibration than using the mean of 3D points.

Point distribution matters more than quantity alone.
Points that vary in depth, height, and field coverage improve pose estimation significantly. Increasing from 4 to 7 points reduced reprojection ambiguity substantially.

Outlier sensitivity is high in small datasets.
With only 7 correspondences, a single inaccurate pixel or height value can shift the entire estimated pose.

Calibration successfully recovered camera pose, but final accuracy is limited by input measurement uncertainty, especially elevation estimates.
