---
layout: project
title: Intake System
description: Screw conveyor for the Cornell Nexus Project Team robot
technologies: [SolidWorks, 3D Printing]
image: /assets/images/intake-modular-cad.png
---

Work on an intake system for the Cornell Nexus Project Team robot was initiated in Fall 2024. Rose Basch, Cleo Hamilton, Derek Yang, and I are currently working on this project. Work on this project began with brainstorming multiple ideas for individual components and integrated systems. Prototyping progress for an Archimedes screw based system has proved promising. The next steps for this project are to design a front scooper to scoop sand into the screw and integrate the design into the robot. 

**Current set of design requirements (active list):**
- Align the shaking with the cylinder rotation rate
 - Priority should be placed on ensuring proper feed rate onto filtration system, rather than a strict monolayer feed.
- Height constrained to [Insert Value] in order to fit around bucket
- Must be able to scoop sand while robot is moving → Honu with drive at intake speed to avoid build-up
 - Ramp shall take in at a width of 36 inches and deliver to belt at 12 inches
- Deliver sand to drop directly on top of the grounded cylinder
- Feed rate range still depends on filtration system's rate (TBD from Ethan and Chris)
 - Digging depth of ~0.5", based off microplastic research
 - Hopper delivery of 12" width to match length of grounded cylinder
 - Intake volume TBD
- Driving motor load capacity TBD

**General Design Notes:**
- For manufacturability, front scoop should be a planar triangle wedge. This angle will be determined through prototyping various angles of the wedge and using the scoop on test sand.
- The macro filter may be able to rake sand in order to make it easier to scoop.
- Priority should be placed on ensuring proper feed rate onto filtration system, rather than a strict monolayer feed.

**Archimedes Screw Design**
For MVP, we designed and 3D printed a modular screw out of PVA. Hexagonal press fits were reinforced with super glue. At the top of the screw, we used an adaptor to interface the scew with the pulley. A picture of the CAD can be seen at the top of this page. 

We also created an improved CAD for manufacturing the next iteration of our screw. This CAD file is improved in two main ways: 1. The profile of the sweep for the screw was 2D instead of a narrow 3D shape and 2. All dimensions are more easily modifiable. Stratasys SAF Propylene is currently our top material choice, but price will influence this choice. 

We believe that using a archimedes screw conveyor will have several benefits over alternatives: 
- Can be vertical or near vertical
- More space efficient
- No need for overarching organization changes because this fits into the current organization of the robot
- Sand can be fed in from the bottom and fed out the top in a simpler manner than a conveyor belt

**Front Scooper**
This is a design for how to get sand from the ground into the screw conveyor. Initial testing with a ramp proved that it is less effective than we thought, but more testing with different materials and angles needs to be done to conclusively decide whether a ramp is the best option. 

Design Constraints: 
- Must have a digging depth of approximately 0.5 inches
- Must be durable enough not to break or jam the screw conveyor system while the robot traverses a beach
- Preferably does not use another motor
- Must lift the sand ~2 inches into the screw conveyor

Designs: 
- Ramp lifts sand using the robot's forward movement and feeds the sand into a trough where the screws take in sand
 - Pros: can cover the width of the robot, protects screws from damage
 - Cons: losing energy when the sand goes up and down and up again
- Ramp feeds sand directly into screw
 - Pros: is not losing energy like the previous design
 - Cons: may not work since the screw is not submerged in sand
- Water Wheel/Circular Saw Design: Waterwheel lifts sand into a bucket that the screw picks up from
 - Pros: Efficient, easily expandable, somewhat easy to make, not a lot of different parts, covers width of robot, durable
 - Cons: Might require a motor, material heavy (A lot of sheet metal), might be dangerous if too fast
 