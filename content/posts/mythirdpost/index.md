---
title: "Bionic Prosthetic Hand"
date: 2023-08-16
draft: false
summary: "Development of a Bionic Prosthetic Hand Controlled by a Velostat-Based Sensor Glove Using Schlesinger Grasp Classification for Biomechanical Accuracy"
tags: ["space"]

---

<style>
  .image-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 0px; /* Small spacing between images */
    justify-items: center;
    margin-top: 20px;
  }

  .image-card {
    width: 100%;
    max-width: 150px;
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



### Project Overview: Bionic Robotic Hand with Glove-Based Control Using Velostat Sensors
Project integrates two distinct components: a bionic robotic prosthetic hand and a sensor equipped control glove. The prosthetic hand is designed to replicate the six primary human grasp patterns as defined by the Schlesinger classification system, ensuring biomechanically accurate functionality.

The second component consists of a glove embedded with velostat based flex sensors which enables intuitive, real time manipulation of the robotic hand by translating finger movements into corresponding actuation signals.

### Development Status & Iterations 
Currently in the prototyping and refinement phase, the project has undergone significant structural and functional revisions since its inception on June 2, 2022 (02.06.2022). Initially focused on basic phalangeal (finger joint) movements, the scope has expanded considerably to incorporate advanced grasp patterns and improved sensor responsiveness.


<figure class="image-card" style="max-width:500px; margin: 0 auto; text-align: center;">
  <img src="/images/List.png" alt="Statistical metrics visualization" 
       style="border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);" />
  <figcaption class="text-gray-600 mt-2"><em>Figure 1:</em> The six basic prehensile patterns defined by Schlesinger (1919)</figcaption>
</figure>


## First Prototype
{{< youtube kmtExL9DDlg>}}
The author apologizes for the lack of professionalism, this is due to the lack of an official video representing the project, the video was recorded in 2023.

### Principle of operation



The initial prototype implements a direct tendon driven actuation mechanism for finger flexion, consisting of three primary components:

1. Actuation System
- A compact servomotor provides the primary motive force
- Rotary motion is converted to linear displacement through a disk.
- Position feedback enables precise angular control
2. Tendon Transmission
- A high strength synthetic line serves as the flexion tendon
- The tendon routes through low friction guide channels along the phalanges
- Distal termination maintains secure attachment while allowing force distribution
3. Biomechanical Implementation
- The linkage system provides anatomical joint alignment
- Passive elastic elements assist in extension return

## Developement of the pchlanges
The mechanical function and desing of the phalanges was considered in order to find the most reliabie prototype.
<figure class="image-card" style="max-width:500px; margin: 0 auto; text-align: center;">
  <img src="/images/Intro.png" alt="Statistical metrics visualization" 
       style="border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);" />
  <figcaption class="text-gray-600 mt-2"><em>Figure 2:</em> Previous prototypes of the phalanges</figcaption>
</figure>

### First Prototype: Mechanical Implementation of Flexion Return Mechanism.


The initial prototype utilized a linkage based actuation system, where metal rods were connected through the phalangeal joints via drilled holes. Movement was achieved through a dual spring mechanism:

1. Extension Spring (Dorsal Side):

Positioned along the posterior aspect of the phalanx, this spring provided the restoring force necessary to return the proximal and intermediate phalanges to their default extended configuration.

2. Compression Spring (Palmar Side):

Mounted anteriorly along the distal phalanx, this spring assisted in resetting the finger’s flexed position, ensuring consistent return to the neutral anatomical arrangement.

This mechanical configuration enabled basic flexion extension cycles while maintaining structural stability.



<figure class="image-card" style="max-width:360px; margin: 0 auto; text-align: center;">
  <img src="/images/Paliczek1.png" alt="Statistical metrics visualization" 
       style="border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);" />
  <figcaption class="text-gray-600 mt-2"><em>Figure 3:</em> Extension and compression spring phalanx movement</figcaption>
</figure>

#### Alternative Flexion Return Mechanism

#### Torsional spring  Implementation 
During the design phase, the implementation of a torsional spring system was evaluated as an alternative to the linear spring based return mechanism. However, this approach was ultimately abandoned due to the unavailability of a commercially viable torsional spring with an appropriate spring index that met the design’s torque and spatial constraints.

#### Elastic Band Implementation

Currently the elastic bands are udner research as a compact alternative to extension springs for passive finger extension. Key advantages include easier integration, tunable tension, and reduced mechanical complexity. Current studies focus on durability under cyclic loading, force consistency, and attachment reliability. This solution maintains existing flexion systems while simplifying the return mechanism.

#### Silicone tendon Implementation 
The current development phase focuses on advancing biomimetic articulation through a tendon system embedded in cast silicone. Utilizing RTV2 SF33 silicone (Shore hardness: 33A), this iteration addresses prior mechanical limitations by leveraging the material’s key properties:
- High Tear Resistance
- Excellent Flow Characteristics
- Satisfactory for intricate molds and skin-safe applications.

### Second Prototype: Principle of operation
Another implementation of movement system between the joints instead of shaft between them is the the rolling contact/compliant.
Rolling contact and compliant joints are mechanisms that combine the benefits of rolling contact (low friction), with the flexibility of compliant mechanisms, which use elastic deformation rather than rigid parts. These joints are designed to allow movement with minimal friction and wear, often by using surfaces that roll against each other, held together by compliant elements like flexible bands or strips.


<figure class="image-card" style="max-width:300px; margin: 0 auto; text-align: center;">
  <img src="/images/Paliczki2.png" alt="Statistical metrics visualization" 
       style="border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);" />
  <figcaption class="text-gray-600 mt-2"><em>Figure 4:</em> Rolling contact joints</figcaption>
</figure>


## Glove Controller
The glove controller was designed to enhance user immersion during prosthetic hand operation. It integrates flex sensors constructed from Velostat layered between conductive copper traces, enabling motion detection. Future iterations will incorporate tactile vibration actuators to provide haptic feedback, further improving sensory integration and control realism.
The system calculates finger bend angle by measuring Velostat resistance changes, where maximum resistance corresponds to a straight finger and minimum resistance to full flexion, enabling continuous angle mapping through proportional interpolation between these calibrated endpoints.

## Robotic hand controlled by a Velostat sensor glove
The author apologizes for the lack of professionalism, this is due to the lack of an official video representing the project, the video was recorded in 2023.
{{< youtube lJXUBwVkYWk>}}


{{< youtube fO0Vjp68EgY>}}
Additional information: Velostat sensor glove can be seen at the bottom of the video.



