/*  RenchTech
 *  https://github.com/renchtech/Arduino-Numeric-Keypad 
 *  3/2/2018
 *  *Arduino_Numeric_Keypad_Mac_001
 *  v1.0
 *  
 *  22 Button Numeric Keypad (ADB), converison on Arduino Pro Micro
 *  Main key presses are mostly stock functionality
 *  LED 1 Blinks ON for Alternate Mode
 *  LED 2 Fades In and Out
 *  
 *  ///Keypad Keys///
 *  Esc, Tab, Clear, Backspace
 *  Num Lock(HOLD), = , / , *
 *  7, 8, 9, -
 *  4, 5, 6, +
 *  1, 2, 3, Return
 *  0, .
 *  
 *  ///Default Mode///
 *  Keys function as keypad numbers, functions, esc, tab, etc 
 *  Clear = Windows + D, Hide all Windows
 *  
 *  ///Secondary Mode///
 *  Top row is unchanged
 *  =, /, * changes to CUT, COPY, PASTE; Must be CTRL + _ for Windows
 *  +, - changes to Vol Up/Down
 *  7, 8, 9 - changes to F13, F14, F15, F16
 *  4, 5, 6 - changes to F12, F3, F4, F5
 *  1, 2, 3 - Pg Up, Dwn, Pg Dwn
 *  0, . - F17, F18
 *  
 *  ///Config///
 *  Row Pins: 9, 18, 15, 14, 16, 10
 *  Col Pins: 9, 8, 7, 6
 *  
 *  
 *  ///References///
 *  Arduino Keyboard Commands https://www.arduino.cc/en/Reference/KeyboardModifiers
 *  HID/Consumer Commands from https://github.com/NicoHood/HID/blob/d4938ddcff7970bc1d32a040a08afeac4915e4a9/src/HID-APIs/ConsumerAPI.h
 *  https://github.com/NicoHood/HID/blob/master/src/HID-APIs/ImprovedKeylayouts.h
*/


#include <Keypad.h>
#include "HID-Project.h"
#include "HID-Settings.h"

const byte ROWS = 6; // 6 rows
const byte COLS = 4; // 4 columns

// Define the keymaps.
char charKeys[ROWS][COLS] = {
  {'a','b','c','d'},
  {'e','f','g','h'},
  {'i','j','k','l'},
  {'m','n','o','p'},
  {'q','r','s','t'},
  {'u','v'}
};

// Create keypad

byte rowPins[ROWS] = {19, 18, 15, 14, 16, 10}; //connect to the row pinouts of the keypad
byte colPins[COLS] = {9, 8, 7, 6}; //connect to the column pinouts of the keypad

Keypad keyPad( makeKeymap(charKeys), rowPins, colPins, sizeof(rowPins), sizeof(colPins) );

unsigned long loopCount;
String msg;
int newIndex = 0;



// LED Config
unsigned long startTime;
const byte led1Pin = 3;                            // Use the LED on pin 3
const byte led2Pin = 5;                            // Use the LED on pin 5.

// define directions for LED fade
#define UP 0
#define DOWN 1
 
// constants for min and max PWM
const int minPWM = 0;
const int maxPWM = 255;
 
// State Variable for Fade Direction
byte fadeDirection = UP;
 
// Global Fade Value
// but be bigger than byte and signed, for rollover
int fadeValue = 0;
 
// How smooth to fade?
byte fadeIncrement = 5;
 
// millis() timing Variable, just for fading
unsigned long previousFadeMillis;
 
// How fast to increment?
int fadeInterval = 30;



void setup() {
    Serial.begin(9600);
    pinMode(led1Pin, OUTPUT);
    pinMode(led2Pin, OUTPUT);
    digitalWrite(led1Pin, LOW);
    // digitalWrite(led2Pin, HIGH);
    analogWrite(led2Pin, fadeValue);

    keyPad.setHoldTime(1000);                   // Default is 1000mS
    // loopCount = 0;
    startTime = millis();
    msg = "";
    
    Consumer.begin();
}

char key;



void loop() {
    checkButtons();

    // get the current time, for this time around loop
    // all millis() timer checks will use this time stamp
    unsigned long currentMillis = millis();
    doTheFade(currentMillis);
}

static byte kpadState;

int modeCount = 0;  // Mode selection variable, Num Lock Off = 0, Num Lock On = 1

void checkButtons() 
{
  if (keyPad.getKeys()) 
  {
    for (int i=0; i<LIST_MAX; i++)  // Scan whole key list
    {
      if(keyPad.key[i].stateChanged)  // Only find keys that have changed state
      {
        switch(keyPad.key[i].kstate)  // Report active key state : IDLE, PRESSED, HOLD, OR RELEASED
        {
          case PRESSED:
            msg = " PRESSED";
            
            // Serial.print(keypad.getKey());
            // Serial.print(keypad.key[i].kchar);
            
            //Serial.print([i]);
            // Serial.println(msg);
            Serial.print(keyPad.key[i].kchar);
            Serial.println(msg);

            switch(keyPad.key[i].kchar)
            {
              //
              // 1st Row
              //
                case 'a':
                  Serial.println("press Esc");
                  Keyboard.press(KEY_ESC);
                  break;
                  
                case 'b':
                  Serial.println("press Tab");
                  Keyboard.press(KEY_TAB);
                  break;
                  
                case 'c':
                  Serial.println("GUI + D = Hide Windows");
                  Keyboard.press(KEY_LEFT_GUI);
                  Keyboard.press(KEY_D);
                  break;
                case 'd':
                  Serial.println("press Backspace/Delete");
                  Keyboard.press(KEY_BACKSPACE);
                  break;
    
                //
                // 2rd Row
                //
                /*
                 * No Short Press Num Lock Action
                case 'e':
                  if (modeCount == 0){
                    Serial.println("Num Lock ON  *Mode Switch");
                    // press modeCount
                    modeCount = 1;
                    blinkLed();
                  }
                  else if  (modeCount == 1){
                    Serial.println("Num Lock OFF");
                    modeCount = 0;  // *Set to default mode
                    blinkLed();
                  }
                  break;
                  */
                case 'f':
                  if (modeCount == 0){
                    Serial.println("press Keypad =");
                    Keyboard.press(KEY_EQUAL);
                    // Keyboard.releaseAll();
                  }
                  else if (modeCount == 1){
                    // No alternate
                    Serial.println("Cut");
                    Keyboard.press(KEY_LEFT_CTRL);
                    Keyboard.press(KEY_X);
                    Keyboard.releaseAll();
                  }
                  break;
                case 'g':
                  if (modeCount == 0) {
                    Serial.println("press Keypad Divide");
                    Keyboard.press(KEYPAD_DIVIDE);
                  }
                  else if (modeCount == 1){
                    Serial.println("Copy");
                    Keyboard.press(KEY_LEFT_CTRL);
                    Keyboard.press(KEY_C);
                    Keyboard.releaseAll();
                  }
                break;
                case 'h':
                  if (modeCount == 0){
                    Serial.println("press Keypad Multiply");
                    Keyboard.press(KEYPAD_MULTIPLY);
                  }
                  else if (modeCount == 1){
                    // No alternate
                    Serial.println("Paste");
                    Keyboard.press(KEY_LEFT_CTRL);
                    Keyboard.press(KEY_V);
                    Keyboard.releaseAll();
                  }
                break;
    
                //
                // 3rd Row
                //
                case 'i':
                  if (modeCount == 0){
                    Serial.println("press Keypad 7");
                    Keyboard.press(KEYPAD_7);
                    // Keyboard.releaseAll(); 
                  }
                  else if  (modeCount == 1){
                    Serial.println("F13");
                    Keyboard.press(KEY_F13);
                  }
                break;
                case 'j':
                  if (modeCount == 0){
                    Serial.println("press Keypad 8");
                    Keyboard.press(KEYPAD_8);
                    // Keyboard.releaseAll(); 
                  }
                  else if (modeCount == 1){
                    Serial.println("F14");
                    Keyboard.press(KEY_F14);
                  }
                break;
                case 'k':
                  if (modeCount == 0) {
                    Serial.println("press Keypad 9");
                    Keyboard.press(KEYPAD_9);
                    // Keyboard.releaseAll(); 
                  }
                  else if (modeCount == 1){
                    Serial.println("F15");
                    Keyboard.press(KEY_F15); 
                  }
                break;
                case 'l':
                  if (modeCount == 0){
                    Serial.println("press Keypad -");
                    Keyboard.press(KEYPAD_SUBTRACT);
                    // Keyboard.releaseAll();
                  }
                  else if (modeCount == 1){
                    Serial.println("F16");
                    Keyboard.press(KEY_F16);
                  }
                  break;
                
              //
              // 4th Row
              //
                case 'm':
                  if (modeCount == 0){
                    Serial.println("press Keypad 4");
                    Keyboard.press(KEYPAD_4);
                  }
                  else if  (modeCount == 1){
                    Serial.println("press F2");
                    Keyboard.press(KEY_F2); 
                  }
                break;
                case 'n':
                  if (modeCount == 0){
                    Serial.println("press Keypad 5");
                    Keyboard.press(KEYPAD_5); 
                  }
                  else if (modeCount == 1){
                    Serial.println("press F3");
                    Keyboard.press(KEY_F3);
                  }
                break;
                case 'o':
                  if (modeCount == 0) {
                    Serial.println("press Keypad 6");
                    Keyboard.press(KEYPAD_6); 
                  }
                  else if (modeCount == 1){
                    Serial.println("press F4");
                    Keyboard.press(KEY_F4);
                  }
                  break;
                case 'p':
                  if (modeCount == 0){
                    Serial.println("press Keypad +");
                    Keyboard.press(KEYPAD_ADD);
                    // Keyboard.releaseAll();
                  }
                  else if (modeCount == 1){
                    Serial.println("press F5");
                    Keyboard.press(KEY_F5);
                  }
                  break;
    
              ///
              // 5th Row
              //
                case 'q':
                  if (modeCount == 0){
                    Serial.println("press Keypad 1");
                    Keyboard.press(KEYPAD_1);
                    // Keyboard.releaseAll(); 
                  }
                  else if  (modeCount == 1){
                    Keyboard.press(KEY_PAGE_UP);
                  }
                  break;
                case 'r':
                  if (modeCount == 0){
                    Serial.println("press Keypad 2");
                    Keyboard.press(KEYPAD_2);
                  }
                  else if (modeCount == 1){
                    Keyboard.press(KEY_DOWN_ARROW);
                  }
                  break;
                case 's':
                  if (modeCount == 0) {
                    Serial.println("press Keypad 3");
                    Keyboard.press(KEYPAD_3);
                    // Keyboard.releaseAll(); 
                  }
                  else if (modeCount == 1){
                    Keyboard.press(KEY_PAGE_DOWN);
                  }
                  break;
                case 't':
                  if (modeCount == 0){
                    Serial.println("press Keypad Return");
                    Keyboard.press(KEYPAD_ENTER);
                    // Keyboard.releaseAll();
                  }
                  else if (modeCount == 1){
                    Keyboard.press(KEYPAD_ENTER);
                  }
                  break;
    
              //
              // 6th Row
              //
                case 'u':
                  if (modeCount == 0){
                    Serial.println("Press Keypad 0");
                    Keyboard.press(KEYPAD_0);
                  }
                  else if  (modeCount == 1){
                    Serial.println("Windows Show Apps");
                    Keyboard.press(KEY_LEFT_CTRL);
                    Keyboard.press(KEY_LEFT_ALT);
                    Keyboard.press(KEY_TAB);
                  }
                  break;
                case 'v':
                  if (modeCount == 0){
                    Serial.println("press Keypad .");
                    Keyboard.press(KEYPAD_DOT);
                    // Keyboard.releaseAll(); 
                  }
                  else if (modeCount == 1){
                    Serial.println("GUI + CTRL + D: Add virtual desktop");
                    Keyboard.press(KEY_LEFT_GUI);
                    Keyboard.press(KEY_LEFT_CTRL);
                    Keyboard.press(KEY_D);
                  }
                  break;    
            }  // end-of-PRESSED-key-switch-case  

            break;
           
          case HOLD:
            msg = " HOLD";

            switch(keyPad.key[i].kchar)
            {
              case 'e':
                  if (modeCount == 0){
                    Serial.println("Num Lock ON  *Mode Switch");
                    // press modeCount
                    modeCount = 1;
                    blinkLed();
                  }
                  else if  (modeCount == 1){
                    Serial.println("Num Lock OFF");
                    modeCount = 0;  // *Set to default mode
                    blinkLed();
                  }
                  break;
            }
            break;
            
          case RELEASED:
            msg = " RELEASED";
            
            // Joystick.button(i+1,0);

            Serial.print(keyPad.key[i].kchar);
            Serial.println(msg);

           
            switch(keyPad.key[i].kchar)
            {
                case 'a':
                  Serial.println("release Esc");
                  Keyboard.release(KEY_ESC);
                  
                case 'b':
                  Serial.println("release Tab");
                  Keyboard.release(KEY_TAB);
                  break;
                  
                case 'c':
                  
                  Serial.println("Released GUI + D = Hide Windows");
                  Keyboard.release(KEY_LEFT_GUI);
                  Keyboard.release(KEY_D);
                  break;
                  
                case 'd':
                  Serial.println("release Backspace/Delete");
                  Keyboard.release(KEY_BACKSPACE);
                  break;
    
                //
                // 2rd Row
                //
                /*
                case 'e':
                  if (modeCount == 0){
                    Serial.println("Num Lock ON  *Mode Switch");
                    // release modeCount
                    modeCount = 1;
                    blinkLed();
                    // Keyboard.release(KEY_NUM_LOCK);
                    // // Keyboard.releaseAll();
                  }
                  else if  (modeCount == 1){
                    Serial.println("Num Lock OFF");
                    modeCount = 0;  // *Set to default mode
                    blinkLed();
                    // Keyboard.release(KEY_NUM_LOCK);
                    // // Keyboard.releaseAll();
                  }
                  break;
                  */
                case 'f':
                  if (modeCount == 0){
                    Serial.println("release Keypad =");
                    Keyboard.release(KEY_EQUAL);
                    // Keyboard.releaseAll();
                  }
                  else if (modeCount == 1){
                    Serial.println("Cut Release");                 
                  }
                  break;
                case 'g':
                  if (modeCount == 0) {
                    Serial.println("release Keypad Divide");
                    Keyboard.release(KEYPAD_DIVIDE);
                  }
                  else if (modeCount == 1){
                    Serial.println("Copy Release");
                  }
                break;
                case 'h':
                  if (modeCount == 0){
                    Serial.println("release Keypad Multiply");
                    Keyboard.release(KEYPAD_MULTIPLY);
                  }
                  else if (modeCount == 1){
                    Serial.println("Paste Release");
                  }
                break;
    
                //
                // 3rd Row
                //
                case 'i':
                  if (modeCount == 0){
                    Serial.println("release Keypad 7");
                    Keyboard.release(KEYPAD_7);
                  }
                  else if  (modeCount == 1){
                    Serial.println("F13");
                    Keyboard.release(KEY_F13);
                  }
                break;
                case 'j':
                  if (modeCount == 0){
                    Serial.println("release Keypad 8");
                    Keyboard.release(KEYPAD_8);
                  }
                  else if (modeCount == 1){
                    Serial.println("F14");
                    Keyboard.release(KEY_F14);
                  }
                break;
                case 'k':
                  if (modeCount == 0) {
                    Serial.println("release Keypad 9");
                    Keyboard.release(KEYPAD_9);
                  }
                  else if (modeCount == 1){
                    Serial.println("F15");
                    Keyboard.release(KEY_F15); 
                  }
                break;
                case 'l':
                  if (modeCount == 0){
                    Serial.println("release Keypad -");
                    Keyboard.release(KEYPAD_SUBTRACT);
                  }
                  else if (modeCount == 1){
                    Serial.println("F16");
                    Keyboard.release(KEY_F16);
                  }
                  break;
                
              //
              // 4th Row
              //
                case 'm':
                  if (modeCount == 0){
                    Serial.println("release Keypad 4");
                    Keyboard.release(KEYPAD_4);
                    // Keyboard.releaseAll(); 
                  }
                  else if  (modeCount == 1){
                    Serial.println("release F2");
                    Keyboard.release(KEY_F2);
                  }
                break;
                case 'n':
                  if (modeCount == 0){
                    Serial.println("release Keypad 5");
                    Keyboard.release(KEYPAD_5);
                    // Keyboard.releaseAll(); 
                  }
                  else if (modeCount == 1){
                    Serial.println("release F3");
                    Keyboard.release(KEY_F3);
                  }
                break;
                case 'o':
                  if (modeCount == 0) {
                    Serial.println("release Keypad 6");
                    Keyboard.release(KEYPAD_6);
                    // Keyboard.releaseAll(); 
                  }
                  else if (modeCount == 1){
                    Serial.println("release F4");
                    Keyboard.release(KEY_F4);
                  }
                  break;
                case 'p':
                  if (modeCount == 0){
                    Serial.println("release Keypad +");
                    Keyboard.release(KEYPAD_ADD);
                    // Keyboard.releaseAll();
                  }
                  else if (modeCount == 1){
                    Serial.println("release F5");
                    Keyboard.release(KEY_F5);
                  }
                  break;
    
              ///
              // 5th Row
              //
                case 'q':
                  if (modeCount == 0){
                    Serial.println("release Keypad 1");
                    Keyboard.release(KEYPAD_1);
                  }
                  else if  (modeCount == 1){
                    Keyboard.release(KEY_PAGE_UP);
                  }
                  break;
                case 'r':
                  if (modeCount == 0){
                    Serial.println("release Keypad 2");
                    Keyboard.release(KEYPAD_2);
                  }
                  else if (modeCount == 1){
                    Keyboard.release(KEY_DOWN_ARROW);
                  }
                  break;
                case 's':
                  if (modeCount == 0) {
                    Serial.println("release Keypad 3");
                    Keyboard.release(KEYPAD_3);
                  }
                  else if (modeCount == 1){
                    Keyboard.release(KEY_PAGE_DOWN);
                  }
                  break;
                case 't':
                  if (modeCount == 0){
                    Serial.println("release Keypad Return");
                    Keyboard.release(KEYPAD_ENTER);
                  }
                  else if (modeCount == 1){
                    Keyboard.release(KEYPAD_ENTER);
                  }
                  break;
    
              //
              // 6th Row
              //
                case 'u':
                  if (modeCount == 0){
                    Serial.println("release Keypad 0");
                    Keyboard.release(KEYPAD_0); 
                  }
                  else if  (modeCount == 1){
                    Serial.println("Windows Show Apps");
                    Keyboard.release(KEY_LEFT_CTRL);
                    Keyboard.release(KEY_LEFT_ALT);
                    Keyboard.release(KEY_TAB); 
                  }
                  break;
                case 'v':
                  if (modeCount == 0){
                    Serial.println("release Keypad .");
                    Keyboard.release(KEYPAD_DOT);
                    // Keyboard.releaseAll(); 
                  }
                  else if (modeCount == 1){
                    Serial.println("Release: GUI + CTRL + D: Add virtual desktop");
                    Keyboard.release(KEY_LEFT_GUI);
                    Keyboard.release(KEY_LEFT_CTRL);
                    Keyboard.release(KEY_D);
                  }
                  break;    
            }  // end-of-key-switch-case         
            break;
          case IDLE:
            msg = " IDLE";
            Serial.println(msg);
        }
      }    
    }
  }  // close keyPad.getKeys()
}  // close void checkButtons {


void blinkLed(){
  for (int i = 0; i <=4; i++){
    // digitalWrite(led2Pin,!digitalRead(led2Pin));
    digitalWrite(led1Pin,!digitalRead(led1Pin));
    delay(100);
  }
}

void doTheFade(unsigned long thisMillis) {
  // is it time to update yet?
  // if not, nothing happens
  if (thisMillis - previousFadeMillis >= fadeInterval) {
    // yup, it's time!
    if (fadeDirection == UP) {
      fadeValue = fadeValue + fadeIncrement;  
      if (fadeValue >= maxPWM) {
        // At max, limit and change direction
        fadeValue = maxPWM;
        fadeDirection = DOWN;
      }
    } else {
      //if we aren't going up, we're going down
      fadeValue = fadeValue - fadeIncrement;
      if (fadeValue <= minPWM) {
        // At min, limit and change direction
        fadeValue = minPWM;
        fadeDirection = UP;
      }
    }
    // Only need to update when it changes
    analogWrite(led2Pin, fadeValue);  
 
    // reset millis for the next iteration (fade timer only)
    previousFadeMillis = thisMillis;
  }
}
