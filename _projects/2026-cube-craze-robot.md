---
layout: project
title: Cube Craze Competition Robot
description: Designed and built a robot to collect more cubes than the opposing robot
technologies: [Fusion, Arduino, C]
image: /assets/images/heat-transfer-work.jpg
---

**Overview**

The objective of the "Cube Craze" robot project was to design and build a robot that moves cubes around an arena in a fun competition where we faced off against our classmates. The goal of each match was to gather more cubes than the opponent's robot by the end of a one minute period. Each team was made up of 3 students. My individual contributions to my team are outlined below. 

**Milestone 1: Design Pitch**

With my teammates, I researched mechanisms other people have used in the past to collect cubes and brainstormed a list of potential designs. We compared them based on their projected cost, complexity, and effectiveness and eventually settled on a design I came up with, which is pictured in the sketches below. 

<p align="center">
  <img src="{{ '/assets/images/CUBE_CRAZE/cube-craze-sketch.png' | relative_url }}" width="550">
  <br>
  <em>Figure 1: Sketch of the final design of the robot. </em>
</p>

During competition, the plan was for the robot to drive forward until it detects the color border, collecting the middle third of the cubes. Then, it uses the ultrasonic sensor to detect whether the opposing robot is on our robot's right side. If so, the robot turns 90 degrees to the left. If not, the robot turns right. Then, it drives straight, collecting another third of the cubes. Finally, it turns 90 degrees again and then drives until it hit the black border, at which point it stops.

While building the robot, the design and algorithm underwent a few changes, which are outlined in later sections. 

**Milestone 2: Mobility**

The objective of this milestone was to make our robot follow the movements in this diagram:

<p align="center">
  <img src="{{ '/assets/images/CUBE_CRAZE/milestone-2-path.png' | relative_url }}" width="550">
  <br>
  <em>Figure 2: Robot path for milestone 2. </em>
</p>

Since Milestones 1 and 2 were due on the same day, the team's focus was split during this time and until Milestone 1 was finished I focused on that. However, after we pitched our design, I took over the software component of this milestone, debugging my teammate's code and eventually creating the final version that was used to complete the milestone. I also assisted with wiring, mounting, and finding loose connections. 

**Milestone 3: Color Sensing and Border Detection**

The objective of this milestone was to make our robot drive on a narrow blue and yellow track, immediately stop when it hits the other color, turn 180 degrees, drive to the back of the starting color, and stop again. The initial color, position, and orientation were random. A diagram of the track is below.  

<p align="center">
  <img src="{{ '/assets/images/CUBE_CRAZE/milestone-3-track.png' | relative_url }}" width="550">
  <br>
  <em>Figure 3: Two-color track for Milestone 3. </em>
</p>

To test our color sensor and get a better sense of how it worked, I recreated Lab 4 from earlier in the class. I wired and coded a system to light up a red LED if the color sensor sensed blue and to turn it off if it detected yellow. Once I got it to work, I recreated the circuit on our robot and adapted the code to make the robot stop when it detected the color border. A photo of the robot wiring up until this point is shown below. 

<p align="center">
  <img src="{{ '/assets/images/CUBE_CRAZE/milestone-3-wiring.png' | relative_url }}" width="550">
  <br>
  <em>Figure 4: Wiring for the motors and color sensor for Milestone 3. </em>
</p>

For the border detection part of the milestone, I partly wired the two QTI sensors with the help of one of my teammates. I also wrote the code for the completion of the milestone. At this point, my team faced many challenges and spent a lot of time debugging. I wrote a program to print whether each QTI sensor was reading high or low, and using this I figured out that one of our QTI sensors was broken. The code I wrote is shown below: 

```c
#define F_CPU 16000000UL
#include <avr/io.h>
#include <util/delay.h>

// --- motor functions --- 
void drive_forward(void){ 
  PORTB = (PORTB & 0b11110000) | 0b00001001; 
} 

void stop(void){ 
  PORTB = (PORTB & 0b11110000) | 0b00000000; 
} 

int main(void)
{ 
  // Motor pins (PB0–PB3) as outputs 
  DDRB |= 0b00001111; 

  // Pins 4 and 5 (PD4, PD5) as inputs 
  DDRD &= ~0b00110000; 

  Serial.begin(9600);

  while(1){ 
    // Read only PD4 and PD5 
    unsigned char sensors = PIND & 0b00110000; 
    
    if (sensors == 0b00010000) { 
      Serial.println("4 high"); 
    } 
    else if (sensors == 0b00100000) { 
      Serial.println("5 high"); 
    } 
    else if (sensors == 0b00110000) { 
      Serial.println("both high"); 
    } 
    else { 
      Serial.println("both low"); 
    } 

    _delay_ms(10); 
  }   
}
```

After replacing the QTI sensor, the milestone went smoothly and my team was able to complete it. 

**Milestone 4: Cube Clearing**

At the beginning of the milestone, there were 10 cubes on each side of the board, randomly scattered within the middle third of the field. The objective of the mielstone was to make our robot gather at least 5 cubes within one minute.

At this point, a couple changes were made to our design from the design pitch. Since the initial bounding area was a square, we changed the frame to be a square to utilize all of the space we were given. Also, we changed the frame so that it hung down to reduce the length of the flaps and therefore reduce the risk that they would bend and let cubes out. 

I CADed the top frame and the square frame. Screenshots of the CAD I did are shown below. 

<p align="center">
  <img src="{{ '/assets/images/CUBE_CRAZE/milestone-4-top-frame.png' | relative_url }}" width="550">
  <br>
  <em>Figure 5: CAD of the top frame, which is mounted to the robot and holds up the square frame and the flaps. </em>
</p>

<p align="center">
  <img src="{{ '/assets/images/CUBE_CRAZE/milestone-4-square-frame.png' | relative_url }}" width="550">
  <br>
  <em>Figure 6: CAD of the square frame, which attaches to the top frame and holds up the the flaps. </em>
</p>

<p align="center">
  <img src="{{ '/assets/images/CUBE_CRAZE/milestone-4-square-frame.png' | relative_url }}" width="550">
  <br>
  <em>Figure 7: CAD assembly of the robot. Standoffs holding the frames and the cardboard flaps are not included. </em>
</p>

Later, my teammate added holes to the square frame to mount the flaps. The completed CAD for the square frame is shown in the screenshot below. 

<p align="center">
  <img src="{{ '/assets/images/CUBE_CRAZE/completed-square-frame.png' | relative_url }}" width="550">
  <br>
  <em>Figure 8: Completed CAD of the square frame. Holes were added to attach the flaps to. </em>
</p>

I also coded the robot's behavior for this milestone. The finalized robot and code are shown in the next section. 

**Final Robot and Competition Day**

The completed robot is shown below. 

<p align="center">
  <img src="{{ '/assets/images/CUBE_CRAZE/completed-robot.png' | relative_url }}" width="550">
  <br>
  <em>Figure 9: Completed robot on competition day. Ultrasonic sensor is visible on the side of the robot. </em>
</p>

I wrote the code for