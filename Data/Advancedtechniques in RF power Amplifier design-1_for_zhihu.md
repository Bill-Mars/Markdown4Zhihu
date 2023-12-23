---
imageNameKey : Advanced1
---

*tips:个人对Cripps这本书总体收获不大，但看都看了，还是挑有用的记录一下吧~*



# CH1 Class AB

## 1.3  dog-leg region

理想Class B如前所述，是线性器件。
事实上，平方律器件：$$ I_D=V_i^2$$ 偏置在$$ \frac{1}{2}I_{max}$$ 时也是线性器件。

对于$$ v_i=0.5+v_scos\theta$$ , 有$$ I_d =(\frac{1}{4}+\frac{1}{2}v_s^2)+v_scos\theta+\frac{1}{2}v_s^2cos2\theta$$ 。第一个括号项是直流分量，$$ cos\theta$$ 项是其基波分量，显然基波分量也是输入信号$$ v_i$$ 的线性函数，因此也是线性器件。

同理，立方律器件：$$ I_d=v_i^3$$ 偏置在$$ \frac{1}{2}I_{max}$$ 时就不是线性的。因为输出的基波分量为$$ \frac{3}{4}(v_s+v_s^2)$$ 不是$$ v_s$$ 的线性函数。

下图给出了一系列线性的传输函数，以Class B（虚线）和平方律器件（n=1）为界的区域内（书上称之为狗腿区域, dog-leg region）都是线性的，它们的波形只包含偶次谐波，n越大，电流波形越接近Class B。

![kk](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-1.png)

![kkk](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-2.png)



但书中也没有指出如何求得n = 2, 4, 6… 时的传输函数（只给出了不同n对应的电流波形表达式$$ I_D(\theta) = cos\theta+k_2cos2\theta+...+k_{2n}cos2n\theta$$ ），大约是求出来也意义不大的缘故罢。

## 1.4 BJT中的trade off

BJT的传输特性为$$ I_c=\beta I_b=\beta e^{k(v_b-1)}$$ 。由于是电流控制的电流源，跟FET的一个区别是输入阻抗小，基极电压$$ v_b$$ 不等于信号源$$ v_{in}$$ ：
![](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-3.png)

基极电压$$ v_b=v_{in}-i_bR$$ ，因此$$ i_b=e^{k((v_{in}-i_bR)-1)}$$ ，从而$$ v_{in}=i_bR+1+\frac{1}{k}lni_b$$ 

可由此画出不同源阻抗对应的BJT传输特性：
![](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-4.png)
上图可见源阻抗R为0时BJT有“增益膨胀”的问题，而源阻抗远大于输入阻抗时就能认为BJT接近线性（可以想象横轴上$$ V_{be}$$ 有两个不同幅度的正弦波摆动，对应的纵轴的$$ i_b$$ 得到的波形的基波分量幅度是否与$$ V_{be}$$ 的成比例）。

但是通过增大源阻抗的方法来实现BJT的线性放大有两个问题：
1. 源阻抗越大阻抗失配越严重，增益越低。
2. 线性传输时，若要求集电极电流$$ i_c$$ 为不失真的单音，那么基极电流$$ i_b$$ 也须是单音，但相应的基极电压就不是单音了，包含谐波成分。因此通过增大源阻抗实现线性，要求源阻抗在谐波上也足够大。（考虑上文的$$ v_{in}=i_bR+1+\frac{1}{k}lni_b$$ 在二次谐波的频率下，由于$$ v_{in}$$ 和$$ i_b$$ 都为单音，在二次谐波上就都为0，那么等式就化为$$ 0=0\cdot R+1+\frac{1}{k}ln0$$ ，此时只有R为无穷大才能保证在二次谐波上的谐波平衡。）

考虑Class A的FET，其输入为单音时，输出也为单音。但类似地如果要实现Class A的BJT，就同样要求集电极电流ic为单音，因此就要求BJT的源阻抗在谐波上足够大，即“谐波开路”。同时基波上的阻抗不能太大以避免降低增益，但又不能太小因为BJT会呈现增益膨胀特性恶化线性。

前级的射频匹配通常是基波附近的窄带的，谐波上的阻抗没有控制。通过串联谐振可实现“谐波开路”电路：
![](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-5.png)

下图是基极的电压和电流波形：  
![](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-6.png)
通过加入串联谐振和加大源阻抗，可以实现线性的Class A BJT。

再来考虑Class AB BJT的实现。Class AB的FET通过降低栅压导通角使漏极电流为削去一部分的正弦波，因此同样的BJT的集电极电流也应为削去一部分的正弦波，因此是包含谐波分量的。而上图的串联谐振导致基极电流只存在基波分量，不会有高次谐波分量，因此无法用于Class AB的BJT。

一种Class AB的BJT电路实现如下：
![](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-7.png)
通过在输入加入串联电阻的四分之波长短路线，可以实现基波和偶次谐波上的阻抗均为有限电阻值，从而在基极和集电极电流中加入偶次谐波分量。（但这种电路没有引入Class AB波形所包含的奇次谐波分量，因此只是近似地模拟Class AB）

下图给出了不同静态偏压下的电流波形（电流最大值归一化为1）：
![](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-8.png)

同时给出了增益曲线和效率曲线：  
![](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-9.png)

上图BJT的增益曲线类似于FET的，Class A与Class B均为线性，而Class AB有增益压缩。此外Class B的增益比Class A低约6dB。效率曲线也与FET的相似。

基于上图中Class B的曲线，考虑提高其增益。
由于BJT实现共轭匹配需要的阻抗很低，因此降低源阻抗可以提高增益，但相应的其Ib-Vin曲线就会偏离线性。
可以通过提高静态来保持线性。如下图标记的几个静态工作点实际上有相似的线性。
![](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-10.png)
而提高静态改善效率的代价是降低效率。下图是采用不同源阻抗时，改变静态来保持线性，得到的增益和效率曲线。
![](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-11.png)‘

实际BJT的基极-发射极通常有一个寄生电容，其在基波上的影响可以通过射频匹配吸收。但它会导致基极电流的谐波成分短路，从而基极电流的波形与上述分析偏离。下图是考虑寄生电容后性能变化的一个例子：
![](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-12.png)
可见只有电流没有谐波分量的Class A能保持跟理想无寄生情况相似的性能，Class B原本的线性会变成“增益膨胀”。

可以采取更先进的工艺来降低寄生参数。                                                      

## 增益膨胀器件的IM notch

假设一个器件的Id-Vin曲线如下：
$$ I_d= I_{max}(g_zV_{in}+(1-g_z)V_{in}^2)$$ 
不同gz的传输特性曲线为
![](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-13.png)
适当选择静态电流时，它们（双音输入时的）IM3会呈现一个”notch”，如下图：
![](https://raw.githubusercontent.com/Bill-Mars/Markdown4Zhihu/master/Data/Advanced techniques in RF power Amplifier design-1/Advanced1-14.png)