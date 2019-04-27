# 2019CVFX_Homework4_Team5

## Test
![dorm](Images/out_GIF_ORB.gif)


### Feature Extraction

#### 1. SIFT 

SIFT (scale-invariant feature transform)會針對每個選定的keypoint取周圍16x16個像素點，再切分為4x4的cell。接著，針對每個cell會再進行gradient magnitude和orientation的計算。<br>
得到16組8 bin 的histograms後，可以再合併為16x8維的資料。最後對這些資料做L2-Normalizing，就可以得到代表那個keypoint的feature vector。 <br>

<img src="./Images/SIFT.png" width="600px" />

##### scale

<img src="./Images/scale.png" width="600px" />
<img src="./Images/scale2.png" width="600px" />

##### rotate

<img src="./Images/rotate.png" width="600px" />
<img src="./Images/rotate2.png" width="600px" />

##### brightness

<img src="./Images/brightness.png" width="600px" />
<img src="./Images/brightness2.png" width="600px" />

優點：對尺度具有不變性，即使改變角度、亮度、視角，都能夠得到很好的檢測效果
缺點：速度慢、產生的資料量大

#### 2. SURF



