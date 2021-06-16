三级处理    
1. 首先是一个weakly-supervised 的frame-level的语义分割，去分割出左心室     
2. 然后是用CNN去做一个clip-level的prediction      
3. 最后是将segment的结果和prediction的结合去预测

总的说就是一个是分割，一个是预测，最后是拼起来。    