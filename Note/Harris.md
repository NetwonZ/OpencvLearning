Harris角点检测

---

- 角点检测原理
  图像i(x,y)在(x,y)处平移(\varDelta x,\varDelta y),其变化的幅度可以用c表示,如下:

c(x,y,\varDelta x,\varDelta y)=\sum _{(u,v)\in W(x,y)} \omega(u,v)·(I(u,v)-I(u+\varDelta x,v+\varDelta y))^2

  w(u,v) 为加权函数,(u,v)代表的是选择的一个方框的所有像素点

  将平方项单独拉出来考虑:

  (I(u,v)-I(u+\varDelta x,v+\varDelta y))^2 = I^2(u,v)+2I(u,v)I(u+\varDelta x,v+\varDelta y)+I^2(u+\varDelta x,v+\varDelta y)由泰勒展开f(x)=f(x_0)+f'(x_0)(x-x_0)+O(x)

   I(u+\varDelta x,v+\varDelta y)=I_x(u,v)\varDelta x+I_y(u,v)\varDelta y + I(u,v)其中的I(u,v)项与前面相减消掉

  所以c = \sum _{w} (I_x(u,v)\varDelta x+I_y(u,v)\varDelta y)^2 = \begin{bmatrix}\varDelta x & \varDelta y \end{bmatrix}·M(x,y)·\begin{bmatrix} \varDelta x\\ \varDelta y \end{bmatrix}

  其中  M(x,y)=\sum _{w} \begin{bmatrix} I_x^2(x,y)&I_x(x,y)I_y(x,y)\\ I_x(x,y)I_y(x,y)&I_y^2(x,y) \end{bmatrix} = \begin{bmatrix} A&C\\C&B \end{bmatrix}

  以上的矩阵为对称矩阵 可以相似对角化为  \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix}

  上述步骤将 c 化简为 

A\Delta x^2+2C\Delta x\Delta y +B\Delta y^2 = \lambda_1\Delta x^2+ \lambda_2 \Delta y^2

上述式子可以看成一个椭圆



\lambda_1 x^2 + \lambda _2 y^2 = 1

可以看出当\lambda_1和\lambda_2越大时,椭圆的面积越大,在x,y方向上的变化也就越大

越大,其所描绘的椭圆也就越大

我们定义

R = \lambda_1\lambda_2 -0.04(\lambda_1+\lambda_2)^2

R值越小,就越接近角点


