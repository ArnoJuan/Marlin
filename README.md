# Marlin
MKS Gen 1.4 with BL-Touch customized version.

# Description
Model：<br/>
Prusa i3 DIY with Aluminum extruded line track

Firmware：<br/>
Marlin version 1.1.8

Electrical and Mechanical：<br/>
MKS Gen v1.4 一體板 (Arduino Mega 2560 R3 + Ramps 1.4)<br/>
TMC2130 5顆 (XYZZE，備用A4988 8顆)<br/>
馬達整流濾波器 4個 (XYZZ)<br/>
步進馬達 1.3A 5顆<br/>
MK8擠出機 1組<br/>
TFT3.5吋觸碰螢幕 2個 (備用1個)<br/>
斷料偵測 1個<br/>
限位開關 3個<br/>
TL-Touch 1個<br/>
熱敏電阻溫度偵測 2個 (噴嘴、熱床)v
300W電源供應器 1個<br/>

Organization：<br/>
歐規2020鋁材<br/>
三軸線軌 (XYYZZ共5條)<br/>
梅花聯軸器 2顆<br/>
T8螺桿8mm導程 兩根<br/>
T8螺桿消間隙 2顆<br/>
5125迷你鼓風機 1個 (TMC2130散熱)<br/>
4010靜音風扇 3個 (主板2個、擠出機1個)<br/>
3010靜音風扇 2個 (噴嘴1個、備用1個)<br/>

# Custom parameters
X&Y axis:<br/>
GT2皮帶 + 16齒同步輪<br/>
GT2皮帶齒距2mm，一圈16齒表示轉一圈移動32mm<br/>
步進馬達步進角1.8度，晶片驅動16微步<br/>
表示轉一圈需要(360/1.8)*16=3200步<br/>
此軸移動1mm需要3200/32=100步<br/>

Z axis:<br/>
T8螺桿導程8mm，表示轉一圈會移動8mm<br/>
步進馬達步進角1.8度，晶片驅動16微步<br/>
表示轉一圈需要(360/1.8)x16=3200步<br/>
此軸移動1mm需要3200/8=400步<br/>

Extruder:<br/>
MK8擠出齒輪，齒輪直徑11mm<br/>
圓周為11xPI=34.54mm<br/>
步進馬達步進角1.8度，晶片驅動16微步<br/>
表示轉一圈需要(360/1.8)x16=3200步<br/>
此軸移動1mm需要3200/34.54=92.646步<br/>
公式:(3200x擠出機齒輪比)/(pix擠出齒輪直徑)<br/>

# IC chip:<br/>
步進馬達1.3A，A4988 Rs=0.1Ω、電流上限2A，Vref應設定成1.6V<br/>
(1.4/0.7)x8x0.1Ω = 1.6<br/>
目前實際設定值為0.65V/(8x0.1Ω) = 0.8125A 轉換RMS電流 => 0.815A/1.41 = 0.578A<br/>
步進馬達1.3A，TMC2130晶片<br/>
目前實際設定值為1.0Vx1.77/2.5 = 0.71A，加速度設為1000<br/>

# BL-Touch auto-leveling related commands:<br/>
X_PROBE_OFFSET_FROM_EXTRUDER 2  // X offset: -left  +right  [of the nozzle]<br/>
Y_PROBE_OFFSET_FROM_EXTRUDER -50  // Y offset: -front +behind [of the nozzle]<br/>
Z_PROBE_OFFSET_FROM_EXTRUDER 0   // Z offset: -below +above  [of the nozzle]<br/>
G28 (X、Y歸零，Z回中間歸零)<br/>
G29 (產生3x3偵測矩陣)<br/>
M851 Z-1.7 (調整Z軸高度)<br/>
M420 V (檢視偵測矩陣)<br/>
M420 S1 (啟用自動調平功能)<br/>
M500 (儲存參數至EEPROM)<br/>
M501 (載入EEPROM儲存參數)<br/>
M502 (清除EEPROM參數)<br/>
M503 (檢視EEPROM參數)<br/>

# Extruder head, heat bed temperature control auto tuning PID parameters:<br/>
M303 E0 C8 S200 (自動調整擠出頭PID參數，溫度震盪8次後停止，目標溫度200度)<br/>
M303 E-1 C8 S70 (自動調整熱床PID參數，溫度震盪8次後停止，目標溫度70度)<br/>
若收到錯誤訊息"PID Autotune failed! Temperature too high"，代表初始測試條件會讓溫度超出目標溫度20度。<br/>
這時候需要修正韌體中的PID_MAX(加熱頭最高電流上限)。<br/>
若收到 "PID Autotune finished! Put the Kp, Ki and Kd constants into Configuration.h" 的訊息後，<br/>
用測試過程中最後一輪的 Kp, Ki, Kd 值替換掉Configuration.h中的DEFAULT_Kp, DEFAULT_Ki 和 DEFAULT_Kd。<br/>
