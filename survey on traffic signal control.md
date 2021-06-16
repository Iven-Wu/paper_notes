传统的control 是将问题考虑成一个有约束的最优化任务。    
并且一般会有一些将问题简单化的条件。    
在传统的模型里面，func(data) 是静止的

Special Considerations:
1. 在黄灯之后，每次应有3-6秒的全部红灯时间  
2. 绿灯的最小时间需要可以让行人能通过   
3. Left turn phase. Usually, a left-turn phase is added when the left-turn volume is above certain threshold.


经典的基于最优化的transportation method：
1. Webster
2. GreenWave,Maxband
3. Actuated,SOTL
4. Max-pressure
5. SCATS

### Webster
Usage: calculate the cycle length and phase split for a single intersection     
假定一段时间里，这个traffic flow是uniform的  
期望的cycle length $C_{des}$ 为:    
$$ C_{des}(V_c) = \frac{N * t_L}{1-\frac{V_c}{\frac{3600}{h} * PHF * v/c}}$$

* 这里面N是phase的数量    
* $t_L$是每个phase总的loss time，可以认为是全红+汽车加速和减速的时间（个人觉得在city brain里似乎并没有什么关系）  
* h是饱和headway time，就是连续两辆车通过同一点的最小时间间隔。     
* PHF是peak hour factor的缩写，衡量高峰期traffic demand的参数       
* v/c是去衡量一个路口拥挤程度的，v是volume，c是capacity
* $V_c = \sum_i^N{V_c^i}$ 是临界车道量

会有一个比例：
$$\frac{t_i}{t_j} = \frac{V_c^i}{V_c^j}$$
也就是时间和volume成正比    
利用上面的公式，可以得到
$$(1-\frac{N * t_L}{C_{des}}) \frac{3600}{h} = V_c$$

### GreenWave
这个考虑了相邻intersection，因为不考虑的话，一个地方最优了，另一个地方就爆了    
每个intersection的cycle length要相等，是选取intersection中最大的那个    
考虑两个相邻intersection中的offset(两个路口间开始的时间)：
$$\triangle t_{i,j} = \frac{L_{i,j}}{v}$$
$L_{i,j}$ 是i,j 之间的 road length， v是期望速度

### Maxband
同样是单个intersection的作为输入，同时要优化offset。    
与GreenWave不同，这个希望最小化number of stops for vehicles traveling along two opposite directoins(基于signal plan 去寻找一个最大的bandwidth)  
这里bandwidth是一个cycle length中绿灯的比例。   

* 每个方向上，绿灯时间比bandwidth大，约束：
  $$w_i + b \leq 1- r_i , w_i > 0 $$
  $$\hat{w_i} + \hat{b} \leq 1- \hat{r_i} , \hat{w_i} > 0 $$

  这里，$w_i/\hat{w_i}$ 是红灯结束，bandwidth开始的时间间隔，b是bandwidth变量，$r_i/\hat{r_i}$ 是红灯时间   
  对于in和out，基本上对应的b都是设置为相等的        

  这里有一个temporal的约束
    $$\phi(i,j) + \hat{\phi(i,j)} + \Delta_i - \Delta_j = m(i,j)$$

  还有一个Spatial的约束：
  $$\phi(i,j) + 0.5* r_j + +w_j + \tau_j = 0.5 * r_i + w_i + t(i,j)$$
  $$\hat{\phi(i,j)} + 0.5* \hat{r_j} +\hat{w_j} = 0.5 * \hat{r_i} + \hat{w_i} - \hat{\tau} + \hat{t(i,j)}$$

  在上述约束下，去最大化b

### Actuated Control  
  本方法是评估当前的phase和其他完成的phase间绿灯的requests，并基于一些rules去判断keep或者change现在的状态   
  评估request的方法：
  * duration for the current phase 没到最小的time，或者有个车在incoming上，并且与intersection在close distance
  * 等着车的数量比一个阈值大

### Self-organizing Traffic Light Control
与Actuated Control很类似，只不过需要与intersection距离近的车过一个阈值。

### Max-Pressure
Pressure of a movement signal 被定义为incoming lanes上车的数量减去outgoing lanes上车的数量。    
Pressure of a phase 是incoming上总的queue length 减去outgoing上的。 
每次都是找最大pressure的phase。 

### SCATS
pre-defined signal plans作为input   
迭代的从通过一个pre-defined performance measurements上选择traffic signal。  
Measurement是这样的(degree of saturation):
$$DS = \frac{g_E}{g} = \frac{g-(h'-h * n)}{g}$$
g是可行的绿灯时间。$g_E$是有效绿灯时间，绿灯时间-被浪费的绿灯时间   
$g_E$是通过,h'检测到的总时间gap，n是检测到的车的数量，h是unit saturation headway between vechicles（就是通过一点的最小时间间隔）    
phase split（phase time的比例），cycle length 和offset都是从pre-define的模型里面弄出来的。  
这里还有一个别的DS：
$$\hat{DS_p^j = \frac{DS_p * g_p}{g_p^j}}$$
这里DS是上面定义的，$g_p$是phase p在当前的signal plan里的绿灯时间（应该还是比例）,$g_p^j$是phase p在signal plan j里的DS


