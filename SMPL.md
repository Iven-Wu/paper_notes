SMPL 构建了一个人体的参数化三维模型，人体可以理解成一个基础模型和在该模型上进行形变的总和，这个思想和photo wake-up中使用的观念差不多。      
在形变基础上进行了PCA，得到刻画形状的低维参数————形状参数。同时，使用运动树表示人体的姿态，即运动树每个关节点和父节点的旋转关系，该关系可以表示为三维向量，最终每个关节点的局部旋转向量构成了smpl模型的姿态参数。       
### 模型细节
模型中$\beta$, $\theta$ 就是其中的参数，$\beta$ 表示了人体高矮胖瘦、头身比例等的10个参数，$\theta$表示人体整体运动位姿和24个关节相对角度的75（24*3+3；每个关节三个，加3个根结点）个参数。       
多个参数点，多个变量去构建了一个模型。





represent any body shape in any pose
compatible------based on standard skinning methods

$T,J,W,\theta$      
$T$ template  mesh  
$J$ joint positions      
$W$ skin weight     
$\theta$ pose       
$M$ skin function


$T$ template mesh       
    shape blend shapes      
Simple predicts the joint positions     


From pose training scans, learn pose blend shapes   

Given a pose, "simple" computes the near contribution of the blend shapes       
the correct skinny errors and the producee realistic pose dependent deformations