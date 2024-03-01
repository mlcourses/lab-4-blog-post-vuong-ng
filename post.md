# Lab 4: Sensors and Actuators
Utsav, Vuong, Long

## Overview and Motivation
This week we'll explore how an Arduino can interact with the external environment, through means of sensors and actuators. Using an ultrasonic sensor and a buzzer actuator, we will design a Distance Detector device that can make different sound frequencies based on the distance between the ultrasonic sensor and an external object. We will walk through the new hardware components introduced first, and the design of the Distance Detector will follow. Let's begin!

## Materials
Like other labs, we will need a breadboard, an Arduino microcontroller, and different colored wires. We also need a Buzzer, an actuator that can be programmed to send out different sound frequencies, as well as an Ultrasonic sensor, which sends out ultrasonic sound waves and based on the sound waves reflected back, determine an object’s proximity to the sensor.

## Design Challenge

### Overview and Requirements
We built a proximity detector that emits a sound whose pitch correlates with the distance to a nearby object. There were a few requirements we needed to meet for a proper **Distance Detector**.

 -  We need to make sure that our device (Distance Detector) should be sensitive to and respond correspondingly to distances that span from vary close (1 cm or less) to a distance of 100 cm (1 meter).

 - We also need to make sure our device emits a sound tone in relation to the distance. For example, we used a low-pitched tone to indicate a far distance (100cm). Similarly, we used a high-pitched tone to indicate a near distance (1 cm or less). This means, we also need to make sure that we are emiting sound in relation to the distance i.e. how far an object is or how close it is, should have different and unique sound tones in relation to one another.

 
### Arduino Code and Explanation

**Code Implementation:** One of the most important part of this **Distance Detector** was to implement the correct Arduino Code written in **C** to make sure it functions as intended and fulfills the requirements of this lab. To write this code, we used the help of the code we implemented previously to make sure our Buzzer and our Ultrasonic Sensor functions properly. 



**C Code For Distance Detector:** This is the following code we used in Arduino IDE, comments are added as explanations for what the line of code does:


```
// Define buzzer and ultrasonic sensor pins

#define BUZZER_PIN 9 
#define TRIG_PIN 12 
#define ECHO_PIN 11  

void setup() {


  pinMode(BUZZER_PIN, OUTPUT);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  



  // Initialize serial communication for debugging
  Serial.begin(9600);
  Serial.println("Distance Detector Initialized");


}


void loop() {


  digitalWrite(TRIG_PIN, LOW); 
  delayMicroseconds(2);  
  digitalWrite(TRIG_PIN, HIGH); 
  delayMicroseconds(10); 
  digitalWrite(TRIG_PIN, LOW); 
  

  // measure the duration of the pulse sent back by the sensor (built in function)
  long duration = pulseIn(ECHO_PIN, HIGH); 


  // Convert duration to distance in cm (speed of sound is 0.034 cm/microsecond)
  int distance_cm = duration * 0.034 / 2; 
  
  // Check if distance measurement is valid (within range)
  if (distance_cm > 0 && distance_cm <= 100) {


    // map = (value, fromLow, fromHigh, toLow, toHigh). range from low to high = range to low, to high.
    int frequency = map(distance_cm, 1, 100, 5000, 500); // Adjust frequency range as needed
    

    tone(BUZZER_PIN, frequency);

  
    // Print distance and frequency for debugging
    Serial.print("Distance: ");
    Serial.print(distance_cm);
    Serial.print(" cm, Frequency: ");
    Serial.println(frequency);


  } else {

    Serial.println("Error: Distance measurement out of range");
  }
  
  // Add delay between readings
  delay(500);
}
```

**Code explanation:** 
- In this code, we take in the `TRIG` of the sensor from the `PIN 19`, `ECHO` from the `PIN 11`. The buzzer is connected via `PIN 9`. 

- Out of the 3 pins we take in, `ECHO` is the distance input we take from the sensor. `TRIG` is the triggers it send out to signal corresponding to the distance input. The buzzer will be triggered by the output and send out the sound via `BUZZER_PIN`. 

- We used `pulseIn` to take in the duration of the pulse sent by the sensor. This has information about the distance of the object that the sensor is sensing. Once we have the duration, we need to convert it into `cm` distance. For this program, we only take in distance within 100 cm (which can be higher value if we use another power supply such as a battery). Hence, we will need to check if our input is in range using `if (distance_cm > 0 && distance_cm <= 100)`

- Now, let's map our distance value with the sound we want the buzzer to emit. We want the sound to be high when we are close to the sensor and low when the objetc is far away. For this step to be easier to implement and concise, we use `map` function in C. `map` re-maps the a number from one range to another range. In this prgram, we have the distance ranging from 1 to 100. We will map this range to the frequency range from 5000 to 500 since we want the sound to be high when the object is close to the sensor and high otherwise. This function really help with making the code neat and easy to read. 

- After each reading, we add a delay `delay(500)` where the program will delay for 500 icrosecond before changing its sound as the distance changes. 

![Picture of the Code](resources/C-Code-DistanceDetector.png)

### Wiring Steps
Now let's proceed to wiring the circuit. WAe will first wire the buzzer:
- Plug the buzzer onto the breadboard so that the 2 pins span two distict rows. According to our code, we will connect the buzzer `+` to `pin 9` on the Arduino. 

- Then, wire the other end of the buzzer to the Ground (GND).

Next, we will wire the Ultrasonic sensor. The Ultrasonic sensor will be wired on an auxiliary breadboad. Hence, take another smaller breadboad and pin the Ultrasonic sensor on. Now, let's start wire it into the circuit:

- Wire the `Vcc` end to the HIGH hole on the breadboard.

- Wire the `TRIG` end to `PIN 12` on the Arduino. Tak eanother wire, connect the `ECHO` end of the Ultrasonic to the `PIN 11` on the Arduino.

[How the Ultrasonic sensor should be wired to the Arduino](https://drive.google.com/file/d/1QpiMEBEWocV_we-ZWhOsj1qht8jwiiL3/view)

- Wire the Ground to a LOW hole on the breadboard. 

[This is our complete circuit](https://drive.google.com/file/d/1NMJiRnznTmDyTALjPS83z3xSZCcIcmI7/view)

That's all we have to do to wire the circuit together. Now let't test our result. 


### Testing
Now, let's see if your code and your circuit work well. To avoid noise during the testing, this circuit, you should choose a flat surface (e.g., a mini board, wall surface, etc.,). First, hold the Ultrasonic sensor close to the surface. Now, the sound from the buzzer must be high. Now, move the Ultrasonic a bit farther, you should notice that the sound turns lower. 

- Now, let's turn our Ultrasonic a static flat surface. Or you can place the Ultrasonic on the egde of a table, hold a flat surface so that we can move the surface closer mor farther from the sensor more smoothly. 

- Once, we have our testing set up. Start keep our objetc close to the sensor and then move the object slowly farther. We should hear the sound changing from low to high quite smoothly. We can make the sound changes more smoothly by changing the `delay` parameter in our code so that there will less delayed time between each reading. 

[This is how the program and the circuit should work](https://drive.google.com/file/d/1z2CD9IMTs7_TmeRBwPw5kUn49wr6djpJ/view?t%253D9)

- In this lab, we also change the supply from our laptop to a battery. We see that after connected with the laptop (or the code), when connected with the battery, the program still work as it does when connected with the laptop.

[This is when we change the laptop to the battery](https://drive.google.com/drive/folders/1qT6_cgMYxM4oddeN7Ivh13Xp4cR28Ogr)

## Conclusion
Sensors and Actuators are essential components of any hardware design since they determine how a machine interacts and responses to external factors from the environment. This design challenge gave us a glimpse of how to leverage these devices, as well as how useful they are.



