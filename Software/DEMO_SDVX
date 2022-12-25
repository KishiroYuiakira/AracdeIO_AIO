/*
 * Copyright © 2021 SnowsakuraCN. All rights reserved. 
 * Github: https://github.com/SnowsakuraCN
 * Website: https://snowsakura.io
 * Unauthorized use for commercial purposes is strictly prohibited.
 */
 
#include "USB.h"
#include "USBHIDKeyboard.h"
#include "ESP32Encoder.h"
#include "USBHIDMouse.h"
#include "Adafruit_NeoPixel.h"
#include "ESP32Encoder.h"

#define DELAY_TIME 10
#define ENCODR_THRESHOLD 1
#define MOUSE_MOVE_DISTANCE 5

USBHIDMouse Mouse;
USBHIDKeyboard Keyboard;
ESP32Encoder VOL_L;
ESP32Encoder VOL_R;
Adafruit_NeoPixel pixels (20, 27, NEO_GRB + NEO_KHZ800);

/*GPIO Using:
 * 
 * |Physical Pin|GPIO Pin|Usage|
 * |03|00|BTN_START|
 * |04|01|BTN_A|
 * |05|02|BTN_B|
 * |06|03|BTN_C|
 * |07|04|BTN_D|
 * |08|05|BTN_FX-L|
 * |09|06|BTN_FX-R|
 * |10|07|BTN_POINT|
 * |11|08|BTN_CARD|
 * |12|09|BTN_SERVICE|
 * |13|10|BTN_TEST|
 * 
 * |14|11|SWITCH_1|
 * |15|12|SWITCH_2|
 * |16|13|SWITCH_3|
 * |17|14|SWITCH_CONFIG|
 * 
 * |18|15|VOL-L-A|
 * |19|16|VOL-L-B|
 * |20|17|VOL-R-A|
 * |21|18|VOL-R-B|
 * 
 * |22|19|USB_N|
 * |23|20|USB_P|
 * 
 * |27|33|RGB|
 */
 char keyBind[11] = {'s','a','b','c','d','l','r','p','f','e','t'};
bool isPressed[11];

void setup(){
  initPinAll(0,11);//Init all keys
  ESP32Encoder::useInternalWeakPullResistors=UP;
  VOL_L.attachFullQuad(15, 16);
  VOL_L.clearCount();
  VOL_R.attachFullQuad(17, 18);
  VOL_R.clearCount();
  pixels.begin();
  delay(500);
  USB.begin();
  Keyboard.begin();
  Mouse.begin();
}
void loop(){
  delay(DELAY_TIME);//DELAY
  
  LoopKey(0,11,keyBind);//LOOP ALL KEYS
  //loopEncoder();
}
void loopEncoder(){
  encoderDetect(VOL_L,ENCODR_THRESHOLD,MOUSE_MOVE_DISTANCE,0);
  encoderDetect(VOL_R,ENCODR_THRESHOLD,0,MOUSE_MOVE_DISTANCE);
}

void initPinAll(int pinStart,int pinNum)
{
  for (int pinNow = pinStart; pinNow < pinStart + pinNum; pinNow++)
  initPin(pinNow);
}

void LoopKey(int pinStart, int pinNum,char keyBind[])
{
  for (int pinNow = pinStart; pinNow < pinStart + pinNum; pinNow++)
  keyDetect(pinNow,keyBind[pinNow-pinStart]);
}

void initPin(int pinNum){
  pinMode(pinNum, INPUT_PULLUP);
  isPressed[pinNum] = false;
}
bool isSelected(int pinNum){
  return (digitalRead(pinNum) == LOW);
  }
//Detect pin in keyboard mode
void keyDetect(int pinNum,char key){
    if(digitalRead(pinNum) == LOW && !isPressed[pinNum])
  {
    Keyboard.press(key);
    isPressed[pinNum] = true;
    pixelOn(pinNum,pixels.Color(0,0,255));
  }
  if (digitalRead(pinNum) == HIGH && isPressed[pinNum])
  {
    Keyboard.release(key);
    isPressed[pinNum] = false;
    pixelOff(pinNum);
  }
}

void pixelOn(int num,uint32_t color){
  pixels.setPixelColor(num, color);
  pixels.show();
}
void pixelOff(int num){
  pixels.setPixelColor(num, pixels.Color(0,0,0));
  pixels.show();
}
//Detect Encoder
void encoderDetect(ESP32Encoder encoder,int threshold,int xDistance,int yDistance){
  if ((int32_t)encoder.getCount() >= threshold)
    Mouse.move(xDistance, yDistance, 0);
  if ((int32_t)encoder.getCount() <= -threshold)
    Mouse.move(-xDistance, -yDistance, 0);
  encoder.clearCount();
}
