输入一个3D model,然后可以得到几个。根据那几个点进行聚类，聚出来多个点之后，当作关节点。之后通过一个BoneNet去输出每个点之间连接的概率，然后通过一个RootNet去输出整个model的中心点位置。  
之后利用MST去根据各个节点去延展整棵树，延展得到整个model的骨架。然后去得到一个skin weight的分布，作为皮肤。