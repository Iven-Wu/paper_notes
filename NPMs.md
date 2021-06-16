1. 这篇文章处理的是一个deformable的model
2. 整个模型主要分成了两个，一个是处理shape的，一个是处理pose的


#### Pose Space
* 是用的一个MLP，去将point从其标准形式投影到变形的位置。 
* MLP 预测了一个flow vector， conditional on both shape code $s_i$ and pose code $p_j$      
*  
