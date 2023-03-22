# robot-control
README-robot-control
## fire extinguisher robot
This is a basic Arduino robot project. components used are Arduino UNO, connecting wires, IR sensors, Servo Motor, Breadboard, L293D motor Driver module. The Arduino Uno can be used to connect with sensors that detect the presence of fire and then regulate the movement of the robot to approach and extinguish the fire using a pump or other mechanisms in the case of a fire extinguisher robot. The microcontroller can also be used to regulate the speed and direction of the robot's motors, as well as the placement of any servos needed to aim the water or other extinguishing agent at the fire. The IR sensors are used to detect fire. 3 infra red sensors are used here. one at the front and other two on both sides. We can use the motors to move in the proper direction of the fire by operating the motors through the L293D module. water can be carried in a small can to put out the fire.

### The fire sensor will output HIGH when there is no fire and output LOW when there is fire. If no fire is there we ask the motors to remain to stop by making all the pins high as shown below :
    if (digitalRead(LS) ==1 && digitalRead(RS)==1 && digitalRead(Front) ==1) //If Fire not detected all sensors are zero
    {
    //Do not move the robot
    digitalWrite(LM1, HIGH);
    digitalWrite(LM2, HIGH);
    digitalWrite(RM1, HIGH);
    digitalWrite(RM2, HIGH);
     }
#### if there is any fire we can ask the robot to move in that direction by rotating the respective motor.

    else if (digitalRead(Front) ==0) //If Fire is straight ahead
    {
     //Move the robot forward
     digitalWrite(LM1, HIGH);  LM1 is leftside motor
     digitalWrite(LM2, LOW);   LM2 is leftside motor
    digitalWrite(RM1, HIGH);  RM1 is right side motor 
    digitalWrite(RM2, LOW);   RM2 is right side motor
    fire = true;
    }
The above code sets the LM1 and RM1 pins to high and the LM2 and RM2 pins to low. This causes the robot's left and right wheels to move forward, driving it ahead in a straight path.

once the variable fire became true the code will execute put off fire function. the code used for doing that is given below

     while (fire == true)
     {
    put_off_fire();
     }
Inside the put_off_fire() we just have to stop the robot by making all the pins high.

#### After this we can turn on the pump.
    void put_off_fire()
    {

    delay (600);               pause the program execution for 600 milliseconds
 
    digitalWrite(LM1, HIGH);    Setting the LM1 digital output pin to a high value, which will stop the left motor from moving.
    digitalWrite(LM2, HIGH);    Setting the LM2 digital output pin to a high value, which will stop the left motor from moving.
    digitalWrite(RM1, HIGH);    Setting the RM1 digital output pin to a high value, which will stop the right motor from moving
    digitalWrite(RM2, HIGH);    Setting the RM2 digital output pin to a high value, which will stop the right motor from moving.
    digitalWrite(pump, HIGH);    //The pump will operate if the digital output pin for the pump is set to a high value
    delay(600); 
    for (pos = 50; pos <= 130; pos += 1) {      Starting a for loop that will run from pos = 50 to pos = 130 in increments of 1.
    myservo.write(pos);                         Moveing the servo to the position specified by the pos variable.
    delay(20); 
     }              end of for loop
    for (pos = 130; pos >= 50; pos -= 1) {          Starting a for loop that will run from pos = 130 to pos = 50 in decrements of 1.
    myservo.write(pos);
     delay(20); 
     }
    digitalWrite(pump,LOW); setting the pump digital output pin to a low value, which will turn off the pump.
    myservo.write(90);
    fire=false;     the fire boolean variable is set to false which indicates the fire has been extinguished
    }
#### link for datasheet of L293D motor:
  https://www.bing.com/aclk?ld=e8fK-PV_HL4PYP61kJaqjpbDVUCUxEgswJv9qFg2plRHNtz4QnS1ZBoqt4jUIdmzeq6MJgN19WSYIO6jp0HHKyCp_ES5MkFIMKRtE2FWvxAUM9UZyJ_NnXziLhMDu6GsE4L2ZLL6fn1-DEUzAf80e1ctuaWbjAfP9dKIMqiYZUefbUbWkJqfgRgIDrmd_CMEnrCRR0YQ&u=aHR0cHMlM2ElMmYlMmZvY3RvcGFydC5jb20lMmZkYXRhc2hlZXQlMmZsMjkzZC1zdG1pY3JvZWxlY3Ryb25pY3MtNDY4MjY3JTNmdXRtX3NvdXJjZSUzZGJpbmclMjZ1dG1fbWVkaXVtJTNkY3BjJTI2dXRtX2NhbXBhaWduJTNkYl9jcGNfaW50bF9zZWFyY2hfZHNhX2VuZ2xpc2hfZW5fdXNkX2RhdGFzaGVldHMlMjZ1dG1fdGVybSUzZGRhdGFzaGVldCUyNnV0bV9jb250ZW50JTNkSW50bCUyNTIwRGF0YXNoZWV0JTI1MjBEU0E&rlid=8bf5d061522310232de1b47b53cfe451&ntb=1
