---
layout: project
title: Cube Craze Competition Robot
description: Designed and built a robot to collect more cubes than the opposing robot
technologies: [Fusion, Arduino, C]
image: /assets/images/CUBE_CRAZE/completed-robot.png
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
  <img src="{{ '/assets/images/CUBE_CRAZE/milestone-2-path.PNG' | relative_url }}" width="550">
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
  <img src="{{ '/assets/images/CUBE_CRAZE/milestone-4-assembly.png' | relative_url }}" width="550">
  <br>
  <em>Figure 7: CAD assembly of the robot. Standoffs holding the frames and the cardboard flaps are not included. </em>
</p>

Later, my teammate added holes to the square frame to mount the flaps. The completed CAD for the square frame is shown in the screenshot below. 

<p align="center">
  <img src="{{ '/assets/images/CUBE_CRAZE/completed-square-frame.png' | relative_url }}" width="550">
  <br>
  <em>Figure 8: Completed CAD of the square frame. Holes were added to hold the flaps. </em>
</p>

I also coded the robot's behavior for this milestone. The finalized robot and code are shown in the next section. 

**Final Robot and Competition Day**

The completed robot is shown below. 

<p align="center">
  <img src="{{ '/assets/images/CUBE_CRAZE/completed-robot.png' | relative_url }}" width="550">
  <br>
  <em>Figure 9: Completed robot on competition day. Ultrasonic sensor is visible on the side of the robot. </em>
</p>

I wrote the code for the competition, which you can see by expanding the section below. A full description of what it does is in the report section below. 

<details>
<summary><strong>Click to view full C source code</strong></summary>

```c
  //----------- PIN ASSIGNMENTS ------------
  // 2: Color sensor output
  // 4: Right QTI sensor
  // 5: Left QTI sensor
  // 8: Left motor forward
  // 9: Left motor backward
  // 10: Right motor backward
  // 11: Right motor forward
  // A1: Ultrasonic sensor (trigger + echo)

  #define PING_PIN PC1

  // --- Global Variables ---

  volatile unsigned int period = 0;
  volatile unsigned int timer = 0;
  volatile uint32_t timer2_overflows = 0;

  // PWM calibration values
  int leftDuty = 100;
  int rightDuty = 80;

  //-------------------------- INTERRUPT FUNCTIONS ----------------------------
  // --- ISR: an interrupt function that resets and reads the timer value
  // COLOR SENSOR
  ISR(PCINT2_vect)
  {	
    if (PIND & (1 << PIND2)) {
      TCNT1 = 0;
    }
    else {
      timer = TCNT1;
    }
  }

  // ISR: Increments overflow counter every time Timer2 rolls over. 
  // Used to extend Timer2 from 8-bit to 32-bit timing resolution.
  // ULTRASONIC SENSOR
  ISR(TIMER2_OVF_vect) {
    timer2_overflows++;
  }

  // ---------------------------------- FUNCTIONS --------------------------------------------
  // --- initColor: initializes the interrupt and timer 
  // COLOR SENSOR
  void initColor()
  {
    DDRD &= ~((1 << DDD2) | (1 << DDD3));   
    DDRD &= ~0b00110000;  // pins 4 and 5 inputs for QTI
    DDRB |= 0b00001111;       

    PCICR |= (1 << PCIE2);     
    PCMSK2 &= ~(1 << PCINT18); 

    TCCR1A = 0;                
    TCCR1B = (1 << CS10);      
    TCNT1 = 0;                 

    sei();                     
  }

  // --- getColor
  // COLOR SENSOR
  int getColor()
  {
    PCMSK2 |= (1 << PCINT18);
    _delay_ms(10);
    PCMSK2 &= ~(1 << PCINT18);

    period = timer / 8;
    return period;
  }

  //Initializes Timer2 and the ultrasonic sensor pin.
  // ULTRASONIC SENSOR
  void initUltrasonic(void) {
    // Timer2 normal mode
    TCCR2A = 0;
    TCCR2B = 0;

    // Prescaler = 8
    // 16 MHz / 8 = 2 MHz, so each tick = 0.5 us
    TCCR2B |= 0b00000010;

    // Enable Timer2 overflow interrupt
    TIMSK2 |= 0b00000001;

    // Start with PING pin as input
    DDRC &= ~(1 << PING_PIN);

    sei();
  }

  //Safely resets Timer2 count and overflow counter
  // ULTRASONIC SENSOR
  void resetTimer2Count(void) {
    uint8_t oldSREG = SREG;
    cli();

    TCNT2 = 0;
    timer2_overflows = 0;
    TIFR2 |= 0b00000001;  // clear overflow flag

    SREG = oldSREG;
  }

  //Returns total elapsed Timer2 ticks (including overflows)
  //Each tick = 0.5 microseconds (with prescaler = 8)
  // ULTRASONIC SENSOR
  uint32_t getTimer2Ticks(void) {
    uint8_t oldSREG = SREG;
    cli();

    uint32_t ticks = (timer2_overflows * 256UL) + TCNT2;

    SREG = oldSREG;
    return ticks;
  }

  //Sends a trigger pulse to the ultrasonic sensor and measures echo duration
  //Returns pulse duration in microseconds
  // ULTRASONIC SENSOR
  unsigned int getUltrasonicPulseUS(void) {

    // Send trigger pulse
    DDRC |= (1 << PING_PIN);      // output

    PORTC &= ~(1 << PING_PIN);
    _delay_us(2);

    PORTC |= (1 << PING_PIN);
    _delay_us(5);

    PORTC &= ~(1 << PING_PIN);

    DDRC &= ~(1 << PING_PIN);     // input

    // Wait for echo to go HIGH
    resetTimer2Count();
    while (!(PINC & (1 << PING_PIN))) {
      if (getTimer2Ticks() > 60000UL) {
        return 0;
      }
    }

    // Measure how long echo stays HIGH
    resetTimer2Count();
    while (PINC & (1 << PING_PIN)) {
      if (getTimer2Ticks() > 60000UL) {
        return 0;
      }
    }

    uint32_t ticks = getTimer2Ticks();

    // each tick = 0.5 us
    return ticks / 2;
  }

  //Converts ultrasonic pulse duration into distance in inches
  //Returns distance in inches
  // ULTRASONIC SENSOR
  unsigned int getDistanceInches(void) {
    unsigned int pulseUS = getUltrasonicPulseUS();

    if (pulseUS == 0) {
      return 999;   // no object / timeout
    }

    return pulseUS / 148;
  }

  //Determines if an object (e.g., opposing robot) is within a given distance
  //Returns true if object is within threshold distance, false otherwise
  //ULTRASONIC SENSOR
  bool robotDetected(unsigned int thresholdInches) {
    unsigned int distance = getDistanceInches();

    return distance <= thresholdInches;
  }

  // driving functions

  // --- Software PWM forward driving ---
  void drive_forward_pwm(int duration_ms){
    int elapsed = 0;

    while(elapsed < duration_ms){

      // Start each PWM cycle clean: clear all motor control pins
      PORTB = PORTB & 0b11110000;

      // Turn both motors forward
      PORTB = (PORTB & 0b11110000) | 0b00000110;

      for(int t = 0; t < 10; t++){

        if(t >= rightDuty / 10){
          // turn off left motor: clear BOTH left motor pins PB0 and PB1
          PORTB &= 0b11111100;
        }

        if(t >= leftDuty / 10){
          // turn off right motor: clear BOTH right motor pins PB2 and PB3
          PORTB &= 0b11110011;
        }

        _delay_ms(1);
      }

      elapsed += 10;
    }

    // End cleanly
    PORTB = PORTB & 0b11110000;
  }

  void drive_backward(void){
    PORTB = (PORTB & 0b11110000) | 0b00001001;
  }

  void turn_left(void){
    PORTB = (PORTB & 0b11110000) | 0b00000101;
  }

  void turn_right(void){
    PORTB = (PORTB & 0b11110000) | 0b00001010;
  } 

  void stop(void){
    PORTB = (PORTB & 0b11110000) | 0b00000000;
  }

  //checks for black border and takes action
  //QTI SENSOR
  void avoid_border(void){
    //variable with just sensor input (pins 4 and 5)
    unsigned char sensors = PIND & 0b00110000;
    //if robot hits border head on, back up and turn around
    if (sensors == 0b00110000) { //both QTIs high
      stop();
      _delay_ms(10);
      drive_backward();
      _delay_ms(200);
      stop();
      turn_right();
      _delay_ms(1200); //180 degrees
    }
    //if robot hits border on left, turn right away from the wall
    else if (sensors == 0b00100000) { //only pin 5 high 
      turn_right();
      _delay_ms(200);
    }
    //if robot hits border on right, turn left away from the wall
    else if (sensors == 0b00010000) { //only pin 4 high
      turn_left();
      _delay_ms(200);
    }
    drive_forward_pwm(100);
  }

  // --------------------------------- PHASE 1: DRIVE FORWARD TO COLOR BORDER (HOPEFULLY) ----------------------------------
  int main(void) {

    //initialize stuff
    init();
    initColor();
    initUltrasonic();

    //find starting color
    period = getColor();
    bool blue = period > 160;

    //started on blue and still on blue OR started on yellow and still on yellow
    bool hit_color_border = !((blue && period > 160) || (!blue && period < 160));
    //loop iteration count
    int i = 0; 
    int max_time = 10; //seconds

    //while not hit color border check color and move forward
    while(!hit_color_border) {
      // drive forward
      drive_forward_pwm(100);
      
      // call getColor function to check color
      period = getColor();

      //check to make sure we didn't run into the black border
      avoid_border();

      //update loop conditions
      hit_color_border = !((blue && period > 160) || (!blue && period < 160));
      i++;

      //if max time has passed, there was interference from the other robot
      //detect if the other robot is to the side and if not try to go around that way towards the color border
      if(i >= max_time*6) {
        stop();
        _delay_ms(10);
        drive_backward();
        _delay_ms(400);
        if(robotDetected(8)) {
          turn_right();
          _delay_ms(650);
          drive_forward_pwm(600);
          turn_left();
          _delay_ms(750);
        }
        else {
          turn_left();
          _delay_ms(750);
          drive_forward_pwm(600);
          turn_right();
          _delay_ms(650);
        }
        i = 0;
      }
    }
    //-------------------------------- PHASE 2: DETECT OPPOSING ROBOT -----------------------
    if(robotDetected(10)) {
      turn_right();
      _delay_ms(650);
    }
    else {
      turn_left();
      _delay_ms(750);
    }
    // ----------------------- PHASE 3: COLLECT & PROTECT THE CUBES ------------------------------------
    //drive around the border
    while(1) {
      drive_forward_pwm(100);
      avoid_border();
    }
  }
```

</details>

Overall, our robot performed well. Our team placed second in our group of 8, advancing us to the tournament. Unfortunately, we lost our first round. 

**Report**

The sections of the report that I wrote are below. If you are interested in the full report, it can be downloaded in PDF form by clicking [here]({{ "/assets/MAE-3260-final-report.pdf" | relative_url }}).

*Robot Design and Strategy Overview*

Our robot was built with an 8 inch by 8 inch laser cut square frame to utilize all of the space available in the initial bounding box. The robot had cardboard flaps attached to the frame that only opened in towards the robot in order to let cubes in from all directions but not out. The flaps were pushed open by the motionless cubes when the robot drove over them. The frame was built in 2 parts: the top frame, which was attached to the robot, and the square bottom frame, which held the flaps. The square frame was lower to reduce the risk of the flaps bending and letting cubes out. 

The robot had 2 motors, one for each wheel. We used a color sensor to detect when we reached the center of the board, which was where the cubes were. The 2 QTI sensors were used to detect the black border. An ultrasonic sensor was used to detect the opposing robot. We decided to use the breadboard from one of our kits in order to help prevent loose connections and provide more space for wiring.

We implemented 5 driving functions, which made the robot drive forward, drive backward, turn right, turn left, and stop. The forward driving function used software PWM to prevent the robot from drifting. We did not implement PWM for the other driving functions because they were only used for short bursts of movement. The avoid_border() function read the QTI sensor input and acted accordingly in order to prevent the robot from driving out of bounds. We also wrote multiple functions to implement the ultrasonic sensor and color sensor, and used interrupts for both. During Phase 1 of the competition behavior code, the robot drove forward until it reached the color border. If 10 seconds passed and the robot did not reach the color border, this meant that there was interference by the opposing robot and our robot would try to maneuver around it to reach the middle of the board. During Phase 2, the robot used the ultrasonic sensor to sense the opposing robot and turned accordingly. During Phase 3, the robot drove around the board, collecting more cubes and avoiding the black border. 

During competition, the robot would ideally drive forward until it detected the color border. It would collect the middle third of the cubes and then use the ultrasonic sensor to see if the opposing robot was on its right side. Then, it would turn based on the sensor input and collect another third of the cubes. After that, it would drive around the board for the rest of the round, protecting its cubes and avoiding the opposing robot. With two thirds of the cubes collected, our robot would have the majority and therefore would win. 

*Competition Analysis*

Overall, our robot performed well. Our team placed second in our group of 8, advancing us to the tournament. Unfortunately, we lost our first round. 

The flaps did a good job holding the cubes in and not letting them out. Also, the robot’s path worked very well and the robot consistently collected a lot of cubes. However, we consistently had issues with loose connections, particularly the QTI wires. Also, the ultrasonic sensor was ultimately not used at all because the opposing robot had to be positioned almost exactly to the right of our robot and at the right time in order to be sensed. Another issue we faced is that the 8 inch by 8 inch initial bounding box was smaller than expected and the robot chassis took up a large portion of it. This caused cubes to get stuck in the wheels. 

During the competition, there were more collisions with opposing robots than expected. This made our design work better than expected because our robot was able to collect cubes even while it was being pushed because the robot could collect cubes from all directions. This also meant that our robot got into many pushing matches with other robots. Fortunately, our design made our robot very stable, meaning it did not tip over and often won those matches. Also, sometimes the timer meant to get the robot to the color border in case of interference went off too early, which was unexpected because that did not happen during testing. 

The robot’s main strength was that it could collect cubes from all directions, even while being pushed. It also held the cubes very well once they were collected. However, our robot could only hold a maximum of 6 cubes on each side, or 12 total. Often, all of the collected cubes were concentrated on one side. Also, our robot’s complex design meant that there were many places for small errors or loose connections. This meant that it was difficult to debug, particularly between competition rounds. 
