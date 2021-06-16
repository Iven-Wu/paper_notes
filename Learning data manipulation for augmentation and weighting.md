这篇论文主要是一个基于其前一篇论文的应用。  
说以往的论文都是将augmentation作为一个policy去train的，但是这里呢，想将data manipulation作为一个policy去train。    
上一篇的论文是 CONNECTING THE DOTS BETWEEN MLE AND RL FOR
SEQUENCE GENERATION。 其中提出了一种将supervised learning转换成Reinforcement learning的办法。

然后是一个EM算法可以进行求解  
之后根据不同的问题，还提出了参数化reward function，进而可以处理不同的问题。