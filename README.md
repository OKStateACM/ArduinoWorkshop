# Arduino
## What is Arduino?

Arduino is an open-source electronics platform based on easy-to-use hardware and software. The core of arduino lies in its easily modifiable code and hardware.

An Arduino Board is a microcontroller on a circuit board reads inputs and drives outputs from compiled code. Below are some examples of both inputs and outputs available:
  * Inputs: Buttons, Switches, etc.
  * Outputs: LEDs, LCD Screen, etc.

### How to Start
The first step is to plug in your Arduino Board to your computer using the USB it is supplied with. Once you have done that, the drivers will begin to install and you are ready to start your first project in Arduino!

First off, we will need to access the online editor [Here](https://create.arduino.cc/editor/) and create a new Arduino account.

Once the editor is set up and ready to go, you should see something similar to this:
```
/*

*/

void setup() { // Initialization function
    
}

void loop() { // Main loop - continuously executes our code
    
}
```
Our Arduino program has two main functions:
  * setup() - Initializes variables and other things
  * loop() - Main loop which continuously executes our code
  
### Hello World!
Our first project will involve us taking an LED we have plugged into our board and turning it on. To achieve this, we use the code below:
```
const int ledPin = 13;

void setup()
{
  pinMode(ledPin, OUTPUT);
}
void loop()
{
  digitalWrite(ledPin, HIGH);
}
```
In the first line, all we are doing is creating a variable named *ledPin* and assigning it to pin 13 on our Arduino Board.

* pinMode tells the compiler what type of module we are using and takes 2 parameters:
  * ledPin (The pin we are writing to)
  * OUTPUT (The type of module we are using. Can be OUTPUT or INPUT)
  
* digitalWrite writes to our module, telling it what we want it to do. It also has 2 parameters:
  * ledPin (Same as before)
  * HIGH (Tells the module to switch ON, in our case lighting the LED. Can be HIGH or LOW)

Before we compile and run our code, we still need to hardwire the arduino board itself.

<img src="https://www.arduino.cc/en/uploads/Tutorial/ExampleCircuit_bb.png" alt="Blink Setup" width="250" /><img src="https://www.arduino.cc/en/uploads/Tutorial/ExampleCircuit_sch.png" alt="Blink Setup Diagram" width="250"/>

Now we can compile our code in the web editor and run it...

### Goodbye World!
We can also turn the LED off by changing our digitalWrite function:
```
const int ledPin = 13;

void setup()
{
  pinMode(ledPin, OUTPUT);
}
void loop()
{
  digitalWrite(ledPin, LOW); // Notice all we did was change the output mode to LOW
}
```
We now have the ability to turn our LED on and off whenever we want, but what if we need it to blink?

`delay(1000)`

This will cause our code to delay by 1000ms or 1 second. Plugging this into our code we have:
```
const int ledPin = 13;

void setup()
{
  pinMode(ledPin, OUTPUT);
}
void loop()
{
  digitalWrite(ledPin, HIGH);
  delay(1000);
  digitalWrite(ledPin, LOW);
  delay(1000);
}
```
Great! We can now switch the light on and off using a timer, but what if we want to do it using something else - say a button.

### Input Modules
Now that we have the basics down for how to output to a module, it's time to move on to getting input from the environment.

Let's set up a button that will turn on our light when we press it.
```
// Assigning button and led pin values
const int buttonPin = 2;
const int ledPin = 13;

// Sets initial button state to 'Not Pressed'
int buttonState = 0;

void setup() {
  // The only setup difference being type OUTPUT or INPUT and pin number
  pinMode(ledPin, OUTPUT);
  pinMode(buttonPin, INPUT);
}

void loop() {

}
```
Seems familiar?

Next, we will need to set up our logic for when the button is pressed in our loop() function.
```
const int buttonPin = 2;
const int ledPin =  13;

int buttonState = 0;

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(buttonPin, INPUT);
}

void loop() {
  /* Changes the buttonState when we press it
     digitalRead() takes in the button pin as input and outputs either 1 or 0 */
     
  buttonState = digitalRead(buttonPin);
  
  /* HIGH or LOW for buttonState is the same as it was for our LED
     While the button is being pressesd, the light will remain on
     The LED will shut off once we release it */
  if (buttonState == HIGH) {
    digitalWrite(ledPin, HIGH);
  } else {
    digitalWrite(ledPin, LOW);
  }
}
```

<img src="https://cdn.instructables.com/FKR/XEB3/IADH24QW/FKRXEB3IADH24QW.LARGE.jpg?auto=webp&width=600&crop=3:2" alt="Button1" width="350" /> <img src="https://4c3q4z2euxqn29mk0e34ayce-wpengine.netdna-ssl.com/wp-content/uploads/2014/01/13-Debounce-Resized.jpg" alt="Button2" width="350" />

Congratulations, you have now set up a working module with inputs and everything! Let's apply some of this knowledge to something we can use.
### Game Buzzer
The following code will allow us to set up a buzzer system like those seen in game shows. We have two contestants, each with their own button, and we want to know who pressed it first. Here's the setup:
```
// Setup 2 LEDs and 2 Buttons for input and output
const int buttonPin1 = 2;
const int buttonPin2 = 3;
const int ledPin1 =  12;
const int ledPin2 = 13;

// Stores the state of each button and buttonPressed stores which button was pressed, 1 or 2 (0 for none)
int buttonState1 = 0;
int buttonState2 = 0;
int buttonPressed = 0;

void setup() {
    /* We configure our input and outputs as normal and set ledPin1 and ledPin2 to off. 
       When we begin a new round, we don't want a buzz in before starting! */
       
    pinMode(ledPin1, OUTPUT);
    pinMode(ledPin2, OUTPUT);
    pinMode(buttonPin1, INPUT);
    pinMode(buttonPin2, INPUT);

    digitalWrite(ledPin1, LOW);
    digitalWrite(ledPin2, LOW);
}

void loop() {
    // If we haven't pressed a button, keep checking
    if (buttonPressed == 0) {
        buttonState1 = digitalRead(buttonPin1);
        buttonState2 = digitalRead(buttonPin2);
    }
    
    /* If we haven't pressed a button before, we will check if we just did
       Otherwise we can continue the loop */
       
    if (!buttonPressed) {
        if (buttonState1 == HIGH) {
            buttonPressed = 1;
        } else if (buttonState2 == HIGH) {
            buttonPressed = 2;
        }
    }
    // Now we light up the LED from whichever button we pressed
    if (buttonPressed == 1) {
        digitalWrite(ledPin1, HIGH);
    } else if (buttonPressed == 2) {
        digitalWrite(ledPin2, HIGH);
    }
}
```
### Other Uses for Arduino
#### Wall-E Robot
<img src="https://www.bonjourlife.com/wp-content/uploads/2014/01/Arduino-Wall-E-Robot-With-Voice-Commands-6.jpg" alt="Wall-E" width="750" />

#### Arduino Flappy Bird
<img src="https://howtomechatronics.com/wp-content/uploads/2015/12/Arduino-TFT-LCD-Touch-Sceen-Tutorial-Example-03.jpg?x57244" alt="Flappy Bird Arduino" width="750" />

#### Light Cube
<img src="https://images-na.ssl-images-amazon.com/images/I/81w47oHSviL._SX425_.jpg" alt= "Light Cube" width="750" />
