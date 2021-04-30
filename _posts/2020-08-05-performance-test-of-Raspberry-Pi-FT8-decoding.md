---
layout: post
title: "Performance test of Raspberry Pi FT8 decoding"
author: BI1EIH, Translated by BG6LH and Google Translate.
language: en
tags: RadioPi RPi FT8
---

After we launched the first version of RadoPi, many OMs would have such a question: Can the performance of a small Raspberry Pi meet the FT8 QSO? For this reason, we designed a test as close as the actual scene , and used the same data to make a comparison between different versions of RPis and PCs.


Let's start with the conclusion:

- Fixed/remote/QRO: 
    - Raspberry Pi 4B 2G is preferred.
    - It can basically guarantee normal communication with deep decoding.
    - However, when there are many parallel signals, your QSO partner may need to repeat tx multiple times.
- Portable/QRP:
    - At least Raspberry Pi 3B should be used.
    - Without deep decoding (ignoring weak signals), it can basically communicate normally when there are less than 10 parallel signals.
    - When there are many parallel signals, your QSO partner may need to repeat tx multiple times.
- SWL:
    - Without deep decoding, Raspberry Pi 2B can work well.
    - Open depth decoding requires 3B or above model to ensure basically complete decoding.


## Test design:

<small>{ Below is translated by Google Translate from our Chinese article }</small>  
A series of dedicated amateur radio communication protocols such as JT65 and FT8 invented by Professor Joe Taylor, through a series of complex information compression, redundancy, error correction, analysis and prediction, to achieve reliable communication on weak signals on high-noise links. Approaching the theoretical limit of Shannon's theorem. The complex algorithm of this superior mode determines that it relies more heavily on computing performance than traditional digital modes such as RTTY/AFSK/PSK31.  
<small>{ Above is translated by Google Translate from our Chinese articale }</small>

As you know, a full FT8 RX/TX cycle is 30 seconds, one of 15 seconds is the receiving and the other 15 seconds is the transmitting. The actual signal carrier time in each cycle is about 12 ~ 13 seconds. 

In the case of normal RX/TX, the decoding starts at the end of the 12.6 seconds of carrier. There is about 2.4 seconds to decode and make preparations for the next 15 seconds to transmission.

Because FT8 decodes all signals in the 0 ~ 3kHz audio bandwidth in most cases, the more parallel signals, the slower decoding will be. Generally, signals with a high SNR will be decoded faster, while signals with a low SNR will be slower. If it fails to decode within 2.4 seconds, the decoding process will continue to run. If the entire decoding process is completed within 15 seconds, it can catch up with the next transmission cycle. If it is too long to decode, your QSO partner may lose patience and halt TX. 


Therefore, to achieve a relatively normal communication, the decoding of most signals in one cycle should ideally not exceed 2.4 seconds. Weak signals require more computing, and a faster CPU help to deep decoding of weak signals.


In order to have a stable benchmark, we have selected 12 signal segments recorded by WSJT-X that are actually received and distributed at different times in a day, all of which are .wav files. The FT8 signals contained in each file range from 3 to 21 (receiving equipment is IC-7300, filter bandwidth 3.2kHz, a 22.4 meters long wire antenna, 20m band), using WSJT-X 2.2.2 built-in jt9 decoding Engine (consistent with the background call of the WSJT-X‘s GUI).

Each model of hardwares runs the following two test cases.  

### Case 1. Normal Decode, the average time to decode all 12 signal segments:

First, use the following command to decode all 12 signal segments.

`~/ft8_samples $ jt9 -8 -d 1 *.wav`


![Use the command to decode 12 signal segments](/img/case1.png)
<small>Above: Use the command to decode 12 signal segments</small>  


When decoding is done, a summary file of timer.out will be generated in the directory, and the data in the red circle is the total decoding time we need. Divide this value by 12 to get the average decoding time per segment.


![View the total decoding time of 12 signal segments](/img/case1_result.png)
<small>Above: View the total decoding time of 12 signal segments</small>  




### Case 2. Deep Decode, the single segment with the most parallel signals

First, delete the decoding prediction file jt9_wisdom.dat generated by the previous case:

`~/ft8_samples $ rm jt9_wisdom.dat`

Then, deep decode the signal segment 200614_153000.wav:

`~/ft8_samples $ jt9 -8 -d 3 200614_153000.wav`
 


![Use the command to decode a single signal segment](/img/case2.png)
<small>Above: Use the command to decode a single signal segment</small>  





![View the total decoding time of a single signal segment](/img/case2_result.png)
<small>Above: View the total decoding time of a single signal segment</small>  

 

## Summary of test results:


We tested the above cases in:
- 5 Raspberry Pi.
- 2 Thinkpad, as reference.


The following is a summary of the results (the red words "秒", pronounce "miǎo", is "Second" in Chinese.) . "21个" In the last column means there are 21 calls were decoded when deep decoding the single segment.


![Summary of FT8 decoding performance of each model (unit: second)](/img/table_rpi_ft8_decode.png)
<small>Above table: Summary of FT8 decoding performance of each model (unit: second).</small>  


There is a Chart of the 7 computers decoding time.
- The Bule bar means Average Decoding Time of 12 segments on each computer.
- The Orange bar means Total Decoding Time of single segment on each computer.
- The longer bar means slower decoding performance .


![Comparison of FT8 decoding performance of various models (unit: second)](/img/chart_rpi_ft8_decode.png)
<small>Above: Comparison of FT8 decoding performance of various models (unit: second). Note: The longer bar means more time to decode, an not suitable for FT8 decoding.</small>  


It can be seen that:
- For normal decoding, the RPi 4B, 2G RAM version, is pretty suitable. 
- The performance of the RPi 4B, 4G RAM version, is better than Thinkpad X61.


We don't have a Raspberry Pi 3B+ , and there is no result about it. However, we uploaded the 12 signals' wav files to [https://radiopi.club](https://radiopi.club). If you are interested in this test, you can download and test it by yourself.


### Downloading link:

[https://radiopi.club/downloads/ft8_samples/ft8_samples.tar.xz](https://radiopi.club/downloads/ft8_samples/ft8_samples.tar.xz)  
SHA256: df20f47858ee8c28cd2c722d46c9963dc4e7d0b4fe3edff1610d5ba5ff7d2420

73

BI1EIH, Translated by BG6LH and Google Translate.
