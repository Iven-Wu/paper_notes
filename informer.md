1. 是transformer的提升版   
2. 随着时间的增加，LSTM的预测越来越慢、能力越来越差，是因为会有一个error的累加
3. 原生的transformer会计算每个点与其余所有点的attention，本质上是一个平方，所以它是不能处理很长序列的情况的
4. 本身的attention矩阵本身比较稀疏，informer可以处理这种问题。  
5. 在self-attention里，是基于KL散度去确定相似性的。这里Query矩阵只保存基于之前KL散度选出的最大的u个值。     
6.    


## Transformer 缺点
1. self-attention 处理的是一个矩阵，是平方复杂度的  
2. Encoder 和 Decoder 里面会有layer的堆叠，内存使用很大。 O(J*L^2)
3. 预测长序列的时候，speed会快速下降。每次都是将上一次预测的结果作为input，然后去得到结果

## 对应于Transformer的处理方案

### For Chanllenge 1 

* 之前的处理方式：
  * LogTransformer等都是用了一个heuristic的办法，去判断哪个attention要用，哪个不需要         
  * 但这种不能自适应的去选择 

### 现在的处理方式
* 减少self-attention的计算量，稀疏化。     
* 发现了self-attention的高激活值都是稀疏的，是服从一个long-tail分布的，及大部分都是没用的。       
    * 有两种，一种称为active，即有峰值，有的地方attention大，有的比较小； 另一种称为lazy的，即非常接近uniform的分布。       
* 用均匀分布的变量和attention的概率作KL散度，得到对应的attention的稀疏性评估。选取出u个query，这个称为active的。      
    $$  attention: p(k_j|q_i)  \qquad  Uniform: q(k_j|q_i) = 1/ L_k$$
    * 最小化求解$KL(q||p) =$
    $$ ln \sum_{j=1}^{L_k} {e^{\frac{q_i k_j^T}{\sqrt{d}}}} - \frac{1}{L_k} \sum_{j=1}^{L_k}{\frac{q_i k_j^T}{\sqrt{d}}}$$      
    * 通过上面的KL散度去选择出最大的u个query，称为active的。    

    * 但是这样的话，仍然需要遍历，仍然需要O($L^2$)的时间复杂度; 同时LogSum会有数值稳定性问题，是因为精度截断。
    * 将前面的LogSum那一项换成max的形式$\max_j{\frac{q_i k_j^T}{\sqrt{d}}}$，其为Logsum的upper bound，处理了数值稳定性的问题    
    且以这种方式，不需要选择L个，只需要sample log L个点，就可以去拟合整一个的分布形式。所以时间复杂度下降为O($L log L$)     

### For Challenge 2

* 有一个Distill的操作      
* 级联层的输入除以2，可以将复杂度减半 
* 在每一层output的基础上，叠加上一个conv1d，再maxpooling。可以将L*L变成L/2 * L/2
* 同时为了增加模型的robustness，增加了几个replica，长度减半。最后再将几个东西concat在一起。
* Distilling的目标是将每一层上的一些特征加上attention，在下一层能够显示出来
* Long Sequence 的Input一直不停的减少，好处理了些

### For Challenge 3 
* 之前的Transformer和RNN差不多，都是Dynamic的操作
* 这里要一次性的生成所有数据，且start token不是一个特殊的标志物，只是一个从Input里sample出的一个短的序列（想要预测的序列之前的一个片段）。
    



