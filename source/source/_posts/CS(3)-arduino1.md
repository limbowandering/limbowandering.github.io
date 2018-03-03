---
title: 用arduino自制电子琴
date: 2017-12-26
categories: CS
tags: [音乐, arduino]
---

这是一个课程设计作业.

## 材料

面包板*1

无缘蜂鸣器*3

线*n

微控开关*n

键皮(你开心就好)

## 实现效果

![](../../../../images/mine/arduino2.jpg)

![](../../../../images/mine/arduino3.jpg)

玩起来感觉很好玩. 可是确实感觉有点…成本太高.

## 源代码

``` C
int NOTE_D[] = {220,233,246,261,277,293,311,329,369,349,391,415,440,466,493,523,554};
int NOTE_Down[] = {262, 293, 329, 349, 392, 440, 494};
int NOTE_DL[] = {147, 165, 175, 196, 221, 248, 278};
int NOTE_DH[] = {523, 586, 658, 697, 783, 879, 987};
double rhythm[] = {1, 0.5, 0.25, 0.125, 0.0625};
int tonepin[] = {9, 10, 11}; //设置控制蜂鸣器的数字引脚
int inputPinA[] = {0, 1, 2, 3, 4, 5, 6,
7, 8, 12, 13, 14, 15, 16, 17, 18};
int inputPin[] = {0, 1, 2,3, 4,5,6,7,8,12,13,14,15,16,17,19};

int getTonePin[] = {-1, -1, -1}; //-1代表蜂鸣器没有发声
void setup() {
  // put your setup code here, to run once:
    for(int i = 0; i < sizeof(tonepin)/sizeof(tonepin[0]); i++){
        pinMode(tonepin[i], OUTPUT);
    }
    for(int i = 0; i < sizeof(inputPin)/sizeof(inputPin[0]); i++){
        pinMode(inputPin[i], INPUT);
    }
}

void loop() {
  int lengthofInputPin = sizeof(inputPin)/sizeof(inputPin[0]);
  int lengthofgetTonePin = sizeof(getTonePin)/sizeof(getTonePin[0]);
  int needClose = -1;
  for(int i = 0; i < lengthofInputPin; i++){ 
        if(digitalRead(inputPin[i]) == LOW){
            if(i <= int(lengthofInputPin / 3) ){
                tone(tonepin[0], NOTE_D[i]);
                getTonePin[0] = i; 
            }
            if(i> int(lengthofInputPin/3) && i <= int(2 * lengthofInputPin/3)){
                tone(tonepin[1], NOTE_D[i]);
                getTonePin[1] = i; 
            }
            if(i> int(2 * lengthofInputPin/3) && i <=  int(lengthofInputPin)){
                tone(tonepin[2], NOTE_D[i]);
                getTonePin[2] = i; 
            }
        }
      }
  
  for(int i = 0; i < lengthofgetTonePin; i++){
      if(getTonePin[i] != -1){
          int temp = getTonePin[i];
          if(digitalRead(inputPin[temp]) == HIGH){
              if(temp <= int(lengthofInputPin / 3) ){
                  needClose = 0; 
              }
              if(temp> int(lengthofInputPin/3) && temp <= int(2 * lengthofInputPin/3)){
                  needClose = 1;
              }
              if(temp> int(2 * lengthofInputPin/3) && temp <=  int(lengthofInputPin)){
                  needClose = 2;
              }
              noTone(tonepin[needClose]);
              getTonePin[i] = -1;
              needClose = -1;
              }
          }
      }
  }


```



