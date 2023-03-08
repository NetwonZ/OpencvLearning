# Harris角点检测

---

* ##### 角点检测原理

  图像i(x,y)在(x,y)处平移($\varDelta x,\varDelta y$),其变化的幅度可以用c表示,如下:
  
  


$$
c(x,y,\varDelta x,\varDelta y)=\sum _{(u,v)\in W(x,y)} \omega(u,v)·(I(u,v)-I(u+\varDelta x,v+\varDelta y))^2
$$

$$

$$

​		w(u,v)$$ 为加权函数,$(u,v)$代表的是选择的一个方框的所有像素点

  将平方项单独拉出来考虑

  $(I(u,v)-I(u+\varDelta x,v+\varDelta y))^2 = I^2(u,v)+2I(u,v)I(u+\varDelta x,v+\varDelta y)+I^2(u+\varDelta x,v+\varDelta y)$由泰勒展开$f(x)=f(x_0)+f'(x_0)(x-x_0)+O(x)$

  $I(u+\varDelta x,v+\varDelta y)=I_x(u,v)\varDelta x+I_y(u,v)\varDelta y + I(u,v)$其中的$I(u,v)$项与前面相减消掉

  所以$c = \sum _{w} (I_x(u,v)\varDelta x+I_y(u,v)\varDelta y)^2 = \begin{bmatrix}\varDelta x & \varDelta y \end{bmatrix}·M(x,y)·\begin{bmatrix} \varDelta x\\ \varDelta y \end{bmatrix}$

  其中 $ M(x,y)=\sum _{w} \begin{bmatrix} I_x^2(x,y)&I_x(x,y)I_y(x,y)\\ I_x(x,y)I_y(x,y)&I_y^2(x,y) \end{bmatrix} = \begin{bmatrix} A&C\\C&B \end{bmatrix}$

  以上的矩阵为对称矩阵 可以相似对角化为  $\begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix}$

  上述步骤将 c 化简为 
$$
A\Delta x^2+2C\Delta x\Delta y +B\Delta y^2 = \lambda_1\Delta x^2+ \lambda_2 \Delta y^2
$$
上述式子可以看成一个椭圆



$$
\lambda_1 x^2 + \lambda _2 y^2 = 1
$$
可以看出当$\lambda_1$和$\lambda_2$越大时,椭圆的面积越大,在$x,y$方向上的变化也就越大

越大,其所描绘的椭圆也就越大

我们定义
$$
R = \lambda_1\lambda_2 -\alpha(\lambda_1+\lambda_2)^2
$$
$\alpha$ 一般为$0.04$

$R$值越小,就越接近角点

opencv相关代码

```python
dst = cv2.cornerHarris(img,blocksize,ksize,k)
```

blocksize:检测窗口的大小,也就是上面的$W$

ksize: sobel的卷积核

k: 上式中$\alpha$的值

> sobel算子 可以用来求图像 x y 方向的梯度
> -> 唤醒记忆
>
> dx = cv2.Sobel(img,cv2.CV_64F,dx=1,dy=0,ksize=3)
>
> dy = cv2.Sobel(img,cv2.CV_64F,dx=0,dy=1,ksize=3)
>
> G = dx+dy

```python
import cv2
import numpy as np

img = cv2.imread('./class02/qiqi.jpg')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
cv2.imshow('img',gray)
dst =cv2.cornerHarris(gray,blockSize=3,ksize=3,k=0.04)
img[ dst>0.01*dst.max() ] = [0,0,255] # 将R值大于设定的阈值的点设定为红色
cv2.imshow('dst',img)
cv2.waitKey(0)

```

- ##### shi-tomasi 角点检测

  检测算法类似于harris算法,但是改变了最后的角点响应
  $$
  R = min(\lambda_1,\lambda_2)
  $$
  $\lambda_1$ $\lambda_2$ 是$\begin{bmatrix} A&C\\C&B \end{bmatrix}$计算出来的两个特征值

  在opencv中的API

  ~~~python
  dst = cv2.goodFeaturesToTrack(img,maxCorners,qualityLevel,minDistance)
  ~~~

  maxcorners: 角点最大数,0表示无限制

  qualityLevel:角点质量,小于1,一般在0.01-0.1之间

  minDistance:角点之间最小的欧氏距离

  k:默认为0.04

  ```python
  import cv2
  import numpy as np
  
  img = cv2.imread('./class02/qiqi.jpg')
  gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
  
  corners = cv2.goodFeaturesToTrack(gray, 0, 0.01, 10)
  corners = np.int0(corners)
  print(corners)
  print(corners.shape)
  for i in corners:
      x,y = i.ravel()
      cv2.circle(img, (x,y), 3, (0,0,255), -1)
   
  cv2.imshow('img',img)
  cv2.waitKey(0)
  ```

  





