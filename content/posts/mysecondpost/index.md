---
title: "X Configuration Drone"
date: 2023-08-15
draft: false
summary: "Development of a Drone Using a Custom Arduino-Based Flight Controller with Simulink-Implemented Control System"
tags: ["space"]
---

## Introduction

As part of a semester project, as a group of 6 members we built X configuration drone within given budget 2000Dkk. My key responsibilities included sensor fusion, IMU calibration, observer selection, part selection, and assisting with control code development.

### Project Retrospective: Achievements and Limitations

The overall document was a 4th semester project where with given arduino BLE 33 rev 2 our group had to make a drone in hovering position. Everything was made by our group including: 
- Mathematical model
- FEA Simulation
- Simulink Simulation
- Control, Sensor and Code Developement 

Due to the report submission deadline and requirement to return parts the drone was not at it's final stage which was the hoovering position. 
#### Informations about the report. 
- 136 A4 pages written with Latex (Overleaf)
- 83 Bibliography links where a significant number are research papers
- 104 Equations
- Project and report was made within 4 months
 
<style>
  .image-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 10px; /* Small spacing between images */
    justify-items: center;
    margin-top: 20px;
  }

  .image-card {
    width: 100%;
    max-width: 450px;
    text-align: center;
  }

  .image-card img {
    width: 100%;
    border-radius: 8px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  }

  .image-card figcaption {
    margin-top: 4px;
    font-size: 0.9rem;
    color: #aaa; /* Light gray caption */
  }
</style>

<div class="image-grid">
  <figure class="image-card">
    <img src="/images/Gimbal.png" alt="Gimbal Mechanism" />
    <figcaption><em>Figure 1:</em> Gimbal Mechanism for Stability Testing</figcaption>
  </figure>
  <figure class="image-card">
    <img src="/images/Stress.png" alt="Frame Design" />
    <figcaption><em>Figure 2:</em> Stress Distribution from FEA</figcaption>
  </figure>
  <figure class="image-card">
    <img src="/images/Final.png" alt="Stress Analysis" />
    <figcaption><em>Figure 3:</em> Final Frame Design in X Configuration</figcaption>
  </figure>
  <figure class="image-card">
    <img src="/images/DeformationG.png" alt="Deformation Plot" />
    <figcaption><em>Figure 4:</em> Total Deformation Under Load</figcaption>
  </figure>
</div>

## Project Parts List
- Arduino BLE 33 rev2 
- BLDC motor iFlight XING X1504 3100KV 
- Uranus 35A BLHeli_32 Single ESC By Skystars
- Gemfan Hurricane 3520 3-Blade Propeller for 3.5 
- Gens Ace Tattu 2300mAh battery

##  Sensor Fusion
Sensor fusion is defined as the combination of two or more data sources to enhance the
consistency, accuracy, and dependability of a system. These data sources may include
mathematical models that encode knowledge from the physical world into the fusion
algorithm to improve sensor measurements. Sensor fusion improves data quality
by reducing noise using multiple sensors and varying sensor types. In addition, it
increases system reliability by compensating for the limitations of individual sensors.

## IMU Integration in Sensor Fusion
In the project the internal IMU (BMI270) and magnetometer (BMM150) on the
Arduino Nano 33 BLE Rev2 was utilized to estimate the orientation of the system.
An IMU combines an accelerometer, gyroscope and magnetometer which together
enable motion tracking. Orientation is defined as a rotational transformation that
describes the deviation of an object from a predefined reference frame.

The focus narrowed to the complementary and Kalman
filters, which were compared and adapted to the drone project’s requirements. The
advantages and disadvantages of each were [considered [1]](https://www.researchgate.net/publication/387124305_Comparing_Filter_Efficiency_Complementary_vs_Kalman_Filters_on_Accelerometer_and_Gyroscope_Signals)

##  Comparison of Advantages
### Kalman Filter 
- Handles unreliable measurements
- Adapts to system changes
- Predicts future system states
- Suitable for complex dynamic models
### Complementary Filter
- Simple implementation
- Low computational cost
- Stable and reliable in real-world conditions
- System can be non-linear
- Noise distribution does not have to be Gaussian


Ultimately, the complementary filter was selected as the primary solution due to its
computational efficiency and alignment with the drone project’s needs. Key reasons
include:
- Computational efficiency [[2] “As an advantage, the algorithmic simplicity makes the complementary filter the best choice for embedded applications, where not much computational power is available“ ](https://www.researchgate.net/publication/275957000_Experimental_comparison_of_Kalman_and_complementary_filter_for_attitude_estimation)
- Studies show that [[3] “the prediction error for all filters kept inside ±2.5 degrees indicated that these techniques performed effectively”](https://www.researchgate.net/publication/386218076_Comparative_Analysis_of_Sensor_Fusion_for_Angle_Estimation_Using_Kalman_and_Complementary_Filters) under non-disturbance
conditions (standard rotations) which can be treated as reference to hovering
of the drone. Since our drone prioritizes stable flight over complex and rabid
movements, this level of precision is acceptable.

### Additional notes 
The equations that were used in the development of a complementary filter for drone attitude estimation can be found [here](https://ahrs.readthedocs.io/en/latest/filters/complementary.html).
## Complementary Filter In Simulink
After implementing data acquisition data was executed and loaded to workspace by Matlab commands. The discrete solver with fixed step
type (set to 0.01 fundamental sample time) was selected due to the compatibility
with control development. 
<figure class="image-card" style="max-width:500px; margin: 0 auto; text-align: center;">
  <img src="/images/SensorFusion.png" alt="Pitch Estimation" 
       style="border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);" />
  <figcaption class="text-gray-600 mt-2"><em>Figure 5:</em> Pitch Estimation</figcaption>
</figure>

  ## Complementary Filter system explanation 
  #### Gyroscope
- **gyroDatafromworkspace** provides earlier gathered data from matlab.
- **Demux** splits the data to 3 separate signals.
- The gyroscope’s angular velocity is integrated over time using a discrete-timeForward Euler method with a fixed sample time, estimating pitch angle which
tends to drift.
- **DiscreteHigh** − passf ilter with gain α set to 0.01 sample rate.
   #### Accelerometer
- **LinearAccelDataformworkspace** block provides raw accelerometer data
- **MatlabFuunction** calculates the pitch angle accurate in steady-state but
noisy during motion.
- **DiscreteLow** − passf ilter with gain 1 − α set to 0.01 sample rate.
  #### Complementary Filter
- **Sum** combines the gyro-integrated and accelerometer pitch
- **out.pitch.est.** A drift-corrected, smoothed pitch estimation
### Results

In the provided images, the parameter α was set to 0.98, based on empirical evidence, this parameter value achieves the best results, complementary filter effectively
attenuated accelerometer noise, achieving a significant reduction and filter successfully mitigated drift.

Following data acquisition comprising 420 discrete angular measurements during the
terminal 5-second stabilization phase of the experimental trial, the following attitude
parameters were obtained:

<figure class="image-card" style="max-width:400px; margin: 0 auto; text-align: center;">
  <img src="/images/Metrics.png" alt="Statistical metrics visualization" 
       style="border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);" />
  <figcaption class="text-gray-600 mt-2"><em>Figure 6:</em> Statistical metrics from the stabilization phase</figcaption>
</figure>
<div class="image-grid">
  <figure class="image-card">
    <img src="/images/Roll.png" alt="Gimbal Mechanism" />
    <figcaption><em>Figure 7:</em> Accelerometer,Gyroscope,Roll Estimation</figcaption>
  </figure>
  <figure class="image-card">
    <img src="/images/Pitch.png" alt="Frame Design" />
    <figcaption><em>Figure 8:</em> Accelerometer,Gyroscope,Pitch Estimation</figcaption>
  </figure>

</div>

### Key challenges and solutions affecting sensor system performance
- Rotations induce centripetal accelerations on off-centre IMUs, corrupting attitude estimation. To minimize this, the IMU was positioned to the drone’s
centre of rotation (centre of gravity). While perfect alignment with the centre of
gravity is challenging due to payload distribution and physical constraints, geometric centre, it significantly reduces rotational artifacts in the accelerometer
data.
- Inertia Measurement System noise and structural vibrations [[4]](https://www.researchgate.net/publication/362683783_Motor_noise_reduction_of_unmanned_aerial_vehicles), [[5]](https://www.researchgate.net/publication/305635329_Noise_modeling_and_analysis_of_an_IMU-based_attitude_sensor_improvement_of_performance_by_filtering_and_sensor_fusion), [[6]](https://ntrs.nasa.gov/api/citations/20180005580/downloads/20180005580.pdf). It is
stated that [[5] “Deterministic noise arises due to such factors as misalignmentof the sensor chip during fabrication, static bias or scale factor errors”](https://www.researchgate.net/publication/305635329_Noise_modeling_and_analysis_of_an_IMU-based_attitude_sensor_improvement_of_performance_by_filtering_and_sensor_fusion), to
determine these errors the calibration procedures has been performed.
[[4] “According to the analysis of the mechanical structure and noise characteristicsof quad-rotor UAVs, the radiated noise of UAVs mainly comes from rotarywings and brushless motors”](https://www.researchgate.net/publication/362683783_Motor_noise_reduction_of_unmanned_aerial_vehicles), The complementary filter allowed for partial
noise reduction, improving the stability of orientation estimation, but full
optimization would require additional hardware and algorithmic modifications
that were not implemented as part of the project.

- The main components where magnetic fields play a significant role is an
brushless motors (motors generate time-varying electromagnetic fields during
the operation) and ESCs (high-frequency magnetic noise from rapid PulseWidth Modulation switching to control motor speed). To mitigate magnetic
interference with the magnetometer, one method [(as shown in Figure 1 of [7]](https://www.mdpi.com/1424-8220/23/8/3897)
is to maximize the distance between the magnetometer and noise sources. In
this design, individual ESCs were mounted on the drone’s arms, physically
separating them from the magnetometer. This reduces direct magnetic coupling
and improves heading accuracy

### Bias correction and calibration 

#### Gyroscope Bias Correction


Gyroscope bias correction constitutes a critical preprocessing step in inertial measurement systems, particularly for applications requiring precise orientation estimation.
These biases manifest as angular rate offsets that, when integrated over time, lead
to unbounded growth in attitude estimation error. The bias calculation algorithm implemented the
correction process through the following computational steps:

1. Collected 2000 samples per axis (x,y,z)
2. Calculated mean offset for each axis
3. Subtracted biases from real-time measurements

####  Accelerometer Bias Correction
Accelerometer bias correction is essential for accurate gravity vector estimation,
particularly in attitude determination systems where tilt accuracy directly impacts
flight stability. These biases introduce static tilt errors that propagate into orientation
estimates, compromising the drone’s ability to maintain level flight. The bias
correction algorithm implemented the calibration process through the following steps:

1. Recorded 500 stationary samples per axis
2. Compared x/y axes to 0g reference, z-axis to 1g gravity
3. Applied offsets to all incoming measurements

#### Magnetometer Calibration 
Magnetometer calibration represents a critical preprocessing step in attitude estimation systems due to the sensor’s susceptibility to both hard-iron (permanent magnetic
fields) and soft-iron (magnetic properties of materials surrounding the magnetometer)
magnetic disturbances. The BMM150 magnetometer integrated in the Arduino 33
BLE Rev2 exhibits particularly pronounced sensitivity to environmental interference, with uncompensated biases. The calibration routine was based on [8](https://se.mathworks.com/videos/sensor-fusion-part-2-fusing-a-mag-accel-and-gyro-to-estimate-orientation-1569411056638.html) and
implemented through the following procedure: 

1. Used MATLAB to stream data at 115200 baud
2. Rotated IMU in figure-8 pattern (10 sec, 1000 samples)
3. Ensured full spherical coverage for accurate calibration


<figure class="image-card" style="max-width:400px; margin: 0 auto; text-align: center;">
  <img src="/images/MAG.png" alt="Statistical metrics visualization" 
       style="border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);" />
  <figcaption class="text-gray-600 mt-2"><em>Figure 9:</em> Magnetometer data visualisation after calibration</figcaption>
</figure>


## Additional Images

<figure class="image-card" style="max-width:400px; margin: 0 auto; text-align: center;">
  <img src="/images/WHOLESIMULINK.png" alt="Pitch, Roll and Yaw estimation structure" 
       style="border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);" />
  <figcaption class="text-gray-600 mt-2"><em>Figure 10:</em> Pitch, Roll and Yaw estimation structure</figcaption>
</figure>

<div class="image-grid">
  <figure class="image-card">
    <img src="/images/Holder.png" alt="Holder" />
    <figcaption><em>Figure 11:</em> Holder </figcaption>
  </figure>
  <figure class="image-card">
    <img src="/images/Data logging.png" alt=" Data logging" />
    <figcaption><em>Figure 12:</em> Data logging</figcaption>
  </figure>
  <figure class="image-card">
    <img src="/images/Above.png" alt="Testing setup: Above caption" />
    <figcaption><em>Figure 13:</em> Testing setup: Above caption</figcaption>
  </figure>
  <figure class="image-card">
    <img src="/images/Side.png" alt="Testing setup: Side caption" />
    <figcaption><em>Figure 14:</em> Testing setup: Side caption</figcaption>
  </figure>
</div>
<div class="image-grid">
  <figure class="image-card">
    <img src="/images/L.png" alt="Linear system, controller" />
    <figcaption><em>Figure 15:</em>Linear system, controller  </figcaption>
  </figure>
  <figure class="image-card">
    <img src="/images/Motor.png" alt="Motor Mixing block" />
    <figcaption><em>Figure 16:</em> Motor Mixing block</figcaption>
  </figure>
    </figure>
</div>

<figure class="image-card" style="max-width:400px; margin: 0 auto; text-align: center;">
  <img src="/images/Non-LinearSystem.png" alt="Non-LinearSystem" 
       style="border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);" />
  <figcaption class="text-gray-600 mt-2"><em>Figure 17:</em> Non-LinearSystem</figcaption>
</figure>



<figure class="image-card" style="max-width:400px; margin: 0 auto; text-align: center;">
  <img src="/images/PythonScript.png" alt="Data acquisition python script" 
       style="border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);" />
  <figcaption class="text-gray-600 mt-2"><em>Figure 18:</em> Data acquisition python script</figcaption>
</figure>
