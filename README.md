# 2019CVFX_Homework4_Team5

## Sequence of NTHU Campus
#### A. 雅齋宿舍 1 樓走廊

<p float="left">
    <img src="./Images/IMG_4995.jpg" width="200px" />
    <img src="./Images/IMG_4996.jpg" width="200px" />
    <img src="./Images/IMG_4997.jpg" width="200px" />
    <img src="./Images/IMG_4998.jpg" width="200px" />
    <img src="./Images/IMG_4999.jpg" width="200px" />
    <img src="./Images/IMG_5001.jpg" width="200px" />    
</p>

#### B. 雅齋宿舍 2 樓走廊
<p float="left">
    <img src="./Images/IMG_5196.JPG" width="200px" />
    <img src="./Images/IMG_5197.JPG" width="200px" />
    <img src="./Images/IMG_5198.JPG" width="200px" />
    <img src="./Images/IMG_5199.JPG" width="200px" /> 
</p>

## Feature Extraction and Comparison

#### Introduction

在做影像辨識的時候，能夠提供資訊的特性有edge、corner、blobs等等。像是下圖中 D 圖可以很容易判斷出是兔子眼睛。 <br>
但是像 C 屬於一個沒有變化的局部影像，看不出明顯的特性，就無法明確辨識出屬於圖片中的哪個區域。如果能提取好的keypoint， <br>
再針對keypoint計算feature就能用來辨識物體。總而言之，影像辨識大致上的步驟可以歸納為 <br>
keypoint detection -> feature extration -> feature matching <br>

<img src="./Images/choice.png" width="700px" /> 
<img src="./Images/rabbit.png" width="500px" /> 

下面我們會介紹並比較三種較為常見的feature extraction演算法。

#### A. ORB

ORB (Oriented Fast and rotated BRIEF) 是將FAST和BRIEF特徵描述的方式結合起來，並在他們的基礎上做改進跟優化。
首先，ORB會利用FAST來檢測keypoint，再將所搜尋除來的結果用Harris corner取出前N個最有可能是corner的keypoint。
為了能夠應對rotation的變化，ORB會對每個keypoint計算weighted centroid，也就是從keypoint到其他weighted centroid的方向。<br>

優點：速度最快、ORB descriptor的表現比SURF來得更好<br>

##### scale

<p float="left">
	<img src="./Images/scale.png" width="400px" />
	<img src="./Images/scale2.png" width="400px" />
</p>

##### rotate

<p float="left">
	<img src="./Images/rotate.png" width="400px" />
	<img src="./Images/rotate2.png" width="400px" />
</p>

##### brightness

<p float="left">
	<img src="./Images/brightness.png" width="400px" />
	<img src="./Images/brightness2.png" width="400px" />
</p>

#### B. SIFT 

SIFT (scale-invariant feature transform)會針對每個選定的keypoint取周圍16x16個像素點，再切分為4x4的cell。
接著，針對每個cell會再進行gradient magnitude和orientation的計算。得到16組8 bin 的histograms後，可以再合併為16x8維的資料。
最後對這些資料做L2-Normalizing，就可以得到代表那個keypoint的feature vector。 <br>

<img src="./Images/SIFT.png" width="600px" />

優點：對尺度具有不變性，即使改變角度、亮度、視角，都能夠得到很好的檢測效果 <br>
缺點：速度慢、產生的資料量大、不適用於realtime環境

##### scale

<p float="left">
	<img src="./Images/dragonScale.png" width="400px" />
	<img src="./Images/dormScale.png" width="400px" />
</p>

##### rotate

<p float="left">
	<img src="./Images/dragonRotate.png" width="400px" />
	<img src="./Images/dormRotate.png" width="400px" />
</p>

##### brightness

<p float="left">
	<img src="./Images/dragonBright.png" width="400px" />
	<img src="./Images/dormBright.png" width="400px" />
</p>

#### (補充) Feature Matching: FLANN-Based Matcher
FLANN(Fast Library for Approximate Nearest Neighbors)這裡是先利用SIFT提取keypoints並且計算descriptors，接著FLANN利用KNN演算法進行feature matching。並且會使用ratio test來減少錯誤的feature matching。<br>

優點：是目前最快的feature matching演算法，能夠減少計算複雜度以及時間，因此也適合應用在feature 較多的場景。<br>

#### scale

<p float="left">
	<img src="./Images/flann_scale2.png" width="400px" />
	<img src="./Images/flann_scale3.png" width="400px" />
</p>

##### rotate

<p float="left">
	<img src="./Images/flann_rotate.png" width="400px" />
	<img src="./Images/flann_rotate2.png" width="400px" />
</p>

##### brightness

<p float="left">
	<img src="./Images/flann_brightness.png" width="400px" />
	<img src="./Images/flann_brightness2.png" width="400px" />
</p>

#### C. SURF
上面提到了 SIFT 雖然效果不錯，但是他的速度慢且產生的資料量很大。而 SURF 可以解決這兩個問題。利用 box filter 對 [積分圖](https://zh.wikipedia.org/wiki/%E7%A7%AF%E5%88%86%E5%9B%BE) 進行計算，得出 Hessian matrix。這個方法的好處是，不同大小的 box filter 可以平行計算，加快速度。

<img src="./Images/SURF.png" width="600px" />

優點：速度比 SIFT 快，，資料量較少<br>
缺點：與 ORB 相比還是比較慢

##### scale

<p float="left">
	<img src="./Images/dragonScale3.png" width="400px" />
	<img src="./Images/dormScale3.png" width="400px" />
</p>

##### rotate

<p float="left">
	<img src="./Images/dragonRotate3.png" width="400px" />
	<img src="./Images/dormRotate3.png" width="400px" />
</p>

##### brightness

<p float="left">
	<img src="./Images/dragonBright3.png" width="400px" />
	<img src="./Images/dormBright3.png" width="400px" />
</p>

SURF (speeded-up robust features)是基於SIFT發展而成的，改善了其速度緩慢的缺點。他常用於偵測corner以及明顯的材質紋路。

優點：可使用於realtime環境，但速度上仍比不上FAST等keypoint detector



## Image Alignment and Infinite Zooming Effect

ORB            |  SIFT           |  SURF           |  FLANN
:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
![ORB](./Images/out_GIF_ORB.gif)  |  ![](./Images/out_GIF_SIFT.gif)|  ![](./Images/out_GIF_SURF.gif)|  ![](./Images/out_GIF_FLANN.gif)



我們的作法如下<br>
1. 將圖片進行放大
2. 放大約 1.15 倍時加入第二張圖，進行 image alignment
3. 重複 1、2

從結果中可以看到，圖片到最後都會壞掉，我們認為這可能是因為在放大與 align 的過程中，可能導致後來貼上的圖片變形，越來越歪。而上一張圖片歪了之後，下一張圖片通常就會變得更歪，導致最後一發不可收拾，直接壞掉。<br>
另外從 Feature Extraction 產生的結果也可以看出，到後面幾張圖片的時候，產生了許多交叉的線，這些找錯的 feature matching 就是讓圖片扭曲的主要原因。我們嘗試了在很多種放大倍率的情況下加入圖片，但效果都不彰。
<img src="./Images/OUTSIFT.png" width="300px" />


## Image Processing

在做image alignment的時候，我們嘗試在邊緣的地方加上羽化的效果，如此一來可以讓視覺效果變得比較柔和，不會有明顯的界線。

<p float="left">
	<img src="./Images/before.png" width="300px" />
	<img src="./Images/after.png" width="300px" />
</p>

由於上面用 Image Align 做出來的效果實在是差強人意，故我們後來使用 AE 來進行實作以下為我們產生的成果(點擊圖片可進入 youtube 連結)：
[![Youtube](./Images/Youtube.png)](https://www.youtube.com/watch?v=7wjfgfAkjJU)

我們的實作方式是參考[這個連結](https://www.youtube.com/watch?v=pOmW76fADz8&t=80s)所做的，對於圖片的邊界也進行了羽化的處理，讓邊界看起來不要那麼明顯。<br>
利用 AE 做出來的效果比 code 看起來好非常多，如果就一次摸索的人來說，我覺得使用 AE 也比用 code 容易，因為要很好的去 extract 出 feature 並正確的 align 實在太難了。但如果真的寫出來可以做出很好結果的 code，那所花費的時間就可以快上很多。

## Conclusion

|            |  ORB    | SIFT        | SURF |
| :--------: |  :--:   | :--:        | :--: |
|    速度    |  快     |  慢         |  中  |
|    用途    |  ...    | blob、corner| corner、texture|

## Reference

在[A Comparison of SIFT, PCA-SIFT and SURF](https://pdfs.semanticscholar.org/b00c/19f4c596f6b99c34ec6c612adcc61d4e6b53.pdf)這篇論文中
作者對以上三個以算法在經過縮放、旋轉、模糊、亮度等變化之後，與原影像進行匹配，並統計其效果。由上表中可以知道，SIFT在縮放和
旋轉變化的情況下效果最好，而SURF在亮度變化下的匹配效果最好。此外，SURF的速度約是SIFT的3倍。
<img src="./Images/comparison.png" width="1000px" />

