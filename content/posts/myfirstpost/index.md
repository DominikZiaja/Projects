---
title: "FilFree"
date: 2025-07-14
draft: false
summary: "PET bottle revitalization machine for 3D printing purposes."
tags: ["space"]


---


## Intorduction

The project aims to reduce plastic waste by recycling PET bottles into 1.75 mm filament for 3D printing. This approach promotes sustainability by repurposing discarded materials into usable printing resources.

## Used Project Parts List
- Nema17 - Stepper Motor
- TB6600 - Motor driver
- REX-C100 - Digital PID Temperature Controller
- Thermocouple type K 
- 3D printed Planetary Gear
## Principle of Operation
 The machine uses a NEMA 17 stepper motor controlled by TB6600 stepper driver to pull PET plastic strips through a heating block set to 217°C by REX-C100. This temperature ensures the plastic becomes pliable without melting, allowing it to be reshaped into consistent 1.75 mm filament. To handle the required torque, a 3D-printed planetary gear with a 3:1 ratio amplifies the motor’s pulling force.

 ## Encountered Problems

### 1. Preprocessing Bottles:
PET bottles require pretreatment with compressed air and external heating to achieve a smooth, uniform surface.

### 2. Filament Length Limitations:
The process yields 10–15 meter filament segments, which must be joined for longer prints. The optimal solution involves inserting both ends into a  mm metal tube and heating it to 250°C to fuse them seamlessly.

 ## Conclusion  
The system achieves a filament diameter tolerance of ±0.05 mm, meeting standard 3D printing requirements. This project demonstrates a viable method for upcycling PET waste into high-quality printing material.



## Machine Presentation
{{< youtube vki53ACSH6U>}}
The author apologizes for the lack of professionalism, this is due to the lack of an official video representing the project, the video was recorded in 2021.

## Planetary Gear presentation
{{< youtube suNA7sB0KZ0>}}
The author apologizes for the lack of professionalism, this is due to the lack of an official video representing the project, the video was recorded in 2021.
