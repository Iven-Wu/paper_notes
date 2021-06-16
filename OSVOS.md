1. 将交叉熵加上了一个和data数量相关的权重beta   


首先是一个VGG在ImageNet上pretrain一下。称为base network。    
然后是将base network在DAVIS的binary mask上train一下，学会分离foreground和background。这个称为parent network。   
