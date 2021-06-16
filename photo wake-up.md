先把躯体从图片里segement出来，然后套一下SMPL模型。压缩，调整SMPL模型，使其能拟合原来segement出来的模型，然后放回去。之后再处理自我重叠的部分，填补背景就ok。


animating a human subject from a single photo   
input: 2D picture   
output: fully 3D experience, like a video   

### First: 
person detection and segmentation which using other methods to refine it 
    &&& 2d pose estimation  
### Second： 
With the two results from the above mentioned models, we fit it into a SMPL model.  
To deal with some intricate place, they propose to warp the SMPL silhouette to match the person silhouette in the original image and then apply that warp to projected SMPL normal maps and skinning maps.  

