文章介绍了一个模型  
模型的主要结构是LSTM，其中有两个重点  
1.attention机制，在LSTM出去之后，连一个Dense，同时进行了attention操作，具体的话，其实就是进行了一个线性变化（和一个新的矩阵），然后是和之前的内容的一个拼接，现在想想有点像LSTM里的隐元  
2.adversarial training， 在上面提到的过了Dense得到loss之后，通过loss的gradient返回，作为一个扰动，添加到Dense上面，去重新过一下最后的Dense（和上面提到的那个不是一个）  

Idea:  
1、这里的LSTM可以尝试别的RNN变种，例如GRU，或者BidirectLSTM（这个没搞过）  
2、对于本身预测的涨跌修改为涨跌幅，那样更可能拟合过去？虽然测试的效果不佳，但这也印证了那个价格不可预测的理论（但我觉得这个点的可行性还是有的？）  
3、在扰动添加的时候，修改为基于梯度的随机扰动，现在的这个只是加上了epsilon*gradient，单一方向，效果其实应该不佳  
4、在embedding的时候，考虑别的方法去想办法得到一些相关性，比如用CNN去抽取特征？或者多加点Dense