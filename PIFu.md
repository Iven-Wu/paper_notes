1. 有encoder和decoder，然后每一个的输入是图片中某一位置的特征向量和该点在摄像机坐标系下的深度值。输出是该点在那个模型内的概率值，用0到1去决定。 
2. 试图去拟合一个 当X在surface内，为1；X在surface外为0的function。  
同时将0.5作为一个threshold去抽取等值面（iso-surface）   
3. 关于sampling的method。其中uniform的话，没法抽取出一些很有用的信息。但如果都是从iso-surface上抽取的话，会导致overfit。     
所以考虑的是使用在geometry surface上抽取，并且加上扰动的方式。与直接抽取点的比例大约为16：1。   
对于loss function用的是每个pixel上的MSE。
4. 对于texture的预测，将之前函数输出的概率值修改为RGB向量，即可。    
Loss function 用的是L1 error的average。 
5. multi-view的一个情况。处理方式是将各个single-view的结果再过一个函数去结合一下       