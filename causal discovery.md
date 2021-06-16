## Goal
Inferring the type and strength of interactions that have a causal effect on the behavior of the dynamical system


### Detail
* 一个模型去抽取图片的一些keypoint表示（perception 模型）   
$$ V^t_m = f^V_{\theta}(I^t_m)$$  
其中I是输入的图片,$V^t_m$是输出的keypoint的二维坐标
* 通过得到的keypoint去推断出一些图片的distribution（inference 模型），然后是得到之间的联系  
$$ \xi_m = f^{\xi}_{\phi}(V_m^{1:T})$$
其中V是输入的第一步得到的keypoint的二维坐标，输出的是edge set
* 去预测未来（dynamics），这一串点之后的动作    
$$ V_m^{T+1} = f_{\varphi}^T(V_m^{1:T},\xi_m)$$
通过上一时间戳的keypoint和edge set去预测下一个时间戳的



inference 和dynamics 是一起train的，基于以下的loss：
$$ min_{\phi,\varphi} \sum_m \sum_t L(V_m^{t+1},f_{\varphi}^D(V_m^{1:t},\xi_m)) +  \lambda R(\xi_m)$$
