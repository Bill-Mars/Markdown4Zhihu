噪声叠加的计算方法：
两个噪声$x_1(t)$和$x_2(t)$叠加之后的功率为：
$\begin{align} P_{av}&=\lim\limits_{T \to \infty}\frac{1}{T}\int^{T/2}_{-T/2}[x_1(t)+x_2(t)]^2dt \\
&=\lim\limits_{T \to \infty}\frac{1}{T}\int^{T/2}_{-T/2}x_1(t)^2dt +\lim\limits_{T \to \infty}\frac{1}{T}\int^{T/2}_{-T/2}x_2(t)^2dt +\lim\limits_{T \to \infty}\frac{1}{T}\int^{T/2}_{-T/2}2x_1(t)x_2(t)dt \\ &=P_{av1}+P_{av2}+ \lim\limits_{T \to \infty}\frac{1}{T}\int^{T/2}_{-T/2}2x_1(t)x_2(t)dt\end{align}$

如果两个噪声是无关的，那么$\lim\limits_{T \to \infty}\frac{1}{T}\int^{T/2}_{-T/2}2x_1(t)x_2(t)dt =0$，$P_{av} = P_{av1}+P_{av2}$ 

# 热噪声
热噪声是一种白噪声。电阻热噪声的功率谱密度为 $S_v(f)=4kTR$。
![](Pasted%20image%2020231208063515.png)
等效电路为：
![](Pasted%20image%2020231208063538.png)
其中$\overline{V_n^2}=4kTR$
功率谱密度在频率上的积分就是那段频段内的功率$P_n = \int S(f)df$

MOS中的热噪声包括沟道热噪声和栅极寄生电阻产生的热噪声。
沟道热噪声模型：
![](Pasted%20image%2020231208063956.png)
$\overline{I_n^2} = 4kT\gamma g_m$
对于layout如下图的MOS：
![](Pasted%20image%2020231208064106.png)
其栅极寄生电阻是分布式的，模型如下图($R_G$为总的栅极电阻）：
![](Pasted%20image%2020231208064134.png)
其等效到单管的总的栅极热噪声为$\overline{V_n^2}=4kTR_G/3$
通过调整layout，降低栅极等效电阻，从而降低栅极热噪声，如下图：
![](Pasted%20image%2020231208064609.png)

此外MOS还存在闪烁噪声，等效到栅极的噪声电压源为$\overline{V_n^2}=\frac{K}{C_{ox}WL}\cdot\frac{1}{f}$。闪烁噪声随频率增大而降低，在闪烁噪声等于MOS热噪声的频点称为corner frequency。
![](Pasted%20image%2020231208064956.png)

# 输入参考噪声
在电路中我们关注的输出信号的信噪比而不是噪声功率的绝对值，因此直接用输出噪声的功率大小来衡量一个电路的噪声性能是不准确的，噪声功率大的电路可能因为增益大从而输出信号的信噪比也更好。因此通常采用”输入参考噪声“，即将输出噪声除以电路的增益等效到输入。这样，一个有噪声的电路与它的无噪声电路加上输入参考噪声后具有相同的输出噪声。然后对比不同电路等效到输入的噪声大小来衡量电路的噪声性能。


以CS stage为例：
![](Pasted%20image%2020231208070103.png)
计算输出噪声时将输入短接：
其中MOS的噪声考虑沟道热噪声和闪烁噪声$\overline{I_{n1}^2}=4kT\gamma g_m+\frac{K}{C_{ox}WL}\cdot\frac{1}{f}\cdot g_m^2$
电阻的等效噪声电流源为$I_{nRD}^2=\frac{4kT}{R_D}$
因此$\overline{V_{n,out}^2}=(\overline{I_{n1}^2}+I_{nRD}^2)R_D^2$
输入参考噪声为
$\overline{V_{n,in}^2}=\frac{\overline{V_{n,out}^2}}{A_v^2} =4kT\frac{\gamma}{g_m}+\frac{K}{C_{ox}WL}\cdot\frac{1}{f}+\frac{4kT}{g_m^2R_D}$
可见输出噪声随着$R_D$增大而增大，但输入参考噪声却随$R_D$增大而减小，原因是输出噪声电压与$\sqrt{R_D}$成正比，但电路增益则与$R_D$成正比，信噪比随$R_D$增大而增大。

## 输入参考噪声电压与输入参考噪声电流

前面都只讨论了CS stage的噪声可以等效到输入串联一个参考噪声电压源。事实上对于一个有有限的输入阻抗和源阻抗的电路，它同时需要串联的输入参考噪声电压和并联的输入参考噪声电流来描述输入参考噪声。
![](Pasted%20image%2020231211213500.png)
如上图，如果只考虑输入参考噪声$V_{n,in}$,在X点的噪声电压为：
$V_{n,X} = \frac{Z_{in}}{Z_{in}+Z_S}V_{n,in}$
对于输出噪声和增益满足$V_{n,out}=A\cdot V_{n,X}$
当$Z_{in}=\infty$或$Z_S=0$时由于$V_{n,X} = V_{n,in}$，因此$V_{n,out}=A\cdot V_{n,X}=A\cdot V_{n,in}$仍满足。事实上大多数MOS的输入都是栅极，不考虑$C_{GS}$时是满足$Z_{in}=\infty$这个条件的。
但当都有限（且不为0）时，$V_{n,out}=A\cdot V_{n,in}$不再成立了，前面通过直接将输出噪声除以增益来获得的输入参考噪声就不能够以串联电压源的方式来表示了（因为那得到的是$V_{n,X}$）。
为了解决这个问题，额外引入一个并联的输入参考噪声电流$\overline{I_{n,in}^2}$，这样对于任意输入阻抗和源阻抗都有：
$V_{n,X} = \frac{Z_{in}}{Z_{in}+Z_S}V_{n,in}+\frac{Z_{in}Z_S}{Z_{in}+Z_S}I_{n,in}$
而输入参考噪声电压和电流仍不能通过直接将输出噪声除以增益或跨阻得到，而是通过：
1. 令$Z_S$为0，这样$V_{n,X}=V_{n,in}$，再计算此时的输出噪声$V_{n,out1}$ 。由$V_{n,in}=V_{n,X}=V_{n,out}/A$得到输入参考噪声电压。
2. 令$Z_S=\infty$，此时$V_{n,X}=I_{n,in}Z_{in}$，再计算此时的输出噪声$V_{n,out2}$，若已知跨导R可通过$I_{n,in}=V_{n,out2}/R$直接计算输入参考噪声电流，或者可以通过电压增益来计算$I_{n,in}=V_{n,out2}/(A\cdot Z_{in})$

此外，类似CH3中的定理：电压增益可以通过输出短路时的跨导G乘以输出阻抗来计算，输出噪声电压也可以通过将【输出短路时的噪声电流】乘以【输出阻抗】来计算。

对于CG电路，由于MOS的输入为栅极（阻抗很大），因此只需要输入参考噪声电压作为输出参考噪声，$I_n=g_m\cdot V_n$时下图(a)和(b)是等效的。
![](Pasted%20image%2020231212215608.png)
书中还给出了戴维南等效和诺顿等效的证明来说明二者的等效：
首先图中$Z_L$是负载，所以要证明二者对于$Z_L$是等效的，只需证明下面两图中虚线框内的电路对于$Z_L$是等效的单端口网络。
![](Pasted%20image%2020231212215927.png)
![](Pasted%20image%2020231212215935.png)
众所周知单端口网络可以进行戴维南和诺顿等效。只要二者的等效网络有相同的输出阻抗和等效电压源或电流源即可。两个电路显然有相同的输出阻抗。通过戴维南等效来证明等效电压源相等，只需将负载开路后确认二者的输出电压相等即可。同理诺顿等效则是负载短路后确认二者的输出电流相等。

## 各种电路的噪声
### CS stage的噪声
CS 电路和其中的噪声源如下图：
![](Pasted%20image%2020231212220804.png)
可以得到输出噪声：
$\overline{V_{n,out}^2}=(4kT\gamma g_m + \frac{K}{C_{ox}WL}\cdot\frac{1}{f}\cdot g_m^2+\frac{4kT}{R_D})R_D^2$
*tips：虽然上图(b)中两个噪声源标了方向，但噪声其实是没有方向的，因此计算两个噪声的结果时只需考虑二者是无关噪声然后相加即可。*
再考虑负载是MOS的情况，电路和噪声源如下图。
![](Pasted%20image%2020231212221334.png)
可以得到输出噪声：
$\overline{V_{n,out}^2}=[4kT\gamma (g_{m1}+g_{m2}) + \frac{K}{C_{ox}WL}\cdot\frac{1}{f}\cdot (g_{m1}^2+g_{m2}^2)](r_{o1}\parallel r_{o2})^2$
由于电路增益为$g_{m1}(r_{o1}\parallel r_{o2})$
故输入参考噪声为
$\overline{V_{n,in}^2} =\frac{\overline{V_{n,out}^2}}{g_{m1}^2(r_{o1}\parallel r_{o2})^2}= 4kT\gamma (\frac{1}{g_{m1}}+\frac{g_{m2}}{g_{m1}^2}) + \frac{K}{C_{ox}WL}\cdot\frac{1}{f}\cdot (1+\frac{g_{m2}^2}{g_{m1}^2})$
输出噪声和输入参考噪声的公式表明，虽然$g_{m1}$和$g_{m2}$增加都会增大输出噪声，但是$g_{m1}$增加会降低输入参考噪声，$g_{m2}$增加却会增大输入参考噪声。
原因是M1管是input device，$g_{m1}$增大会同时提高增益，且输出热噪声电压与$\sqrt{g_{m1}}$成正比，但电路增益则与$g_{m1}$成正比，整体信噪比随$g_{m1}$增大而增大。而M2管不是input device，增大$g_{m2}$只会提高噪声而不会提高电路增益。
前面提高过增大CS stage中的$R_D$会同时提高输出噪声和信噪比，也是一样的原因。

CS stage中的噪声性能，是需要跟它的功耗，voltage swing和速度进行trade-off的。因为要提高CS stage的输入参考噪声，一方面可以通过增大$R_D$实现，但直接增大$R_D$而不改变电流则会改变静态电压偏置，从而影响voltage swing，且降低高速电路中的速度（回忆电路理论中的时间常数$\tau=RC$）；另一方面可以通过增大$g_m$实现，即增大$I_D$或者增大W/L，增大W(通常L受工艺限制不能无限减小)会增大寄生电容，从而降低速度，而增大$I_D$也会改变静态电压偏置从而影响voltage swing，而且会提高功耗。

下图是一个noise-power trade-off：
![](Pasted%20image%2020231215031200.png)
电流翻倍，负载减半，这样MOS管的噪声减半了，同时静态电压依然不变所以能保持voltage swing，输出节点的速度能保持不变（虽然电容翻倍了但是负载也减半了）。但是输入电容增大了，功耗也翻倍了（电压和电流乘积翻倍）。



### CG stage的噪声
CG 电路和其中的噪声源如下图：
![](Pasted%20image%2020231214210522.png)
由于CG电路的输入阻抗不是无穷大，CG电路的输出噪声跟源阻抗有关。但可以计算其输入参考噪声。
计算输入参考噪声电压时将输入短路，得：
$(g_m+g_{mb})^2R_D^2\cdot \overline{V_{n,in}^2}=(\overline{I_{n1}^2}+\overline{I_{n,RD}^2})R_D^2$
$\overline{V_{n,in}^2}=\frac{4kT\gamma g_m+\frac{K}{C_{ox}WL}\cdot\frac{1}{f}\cdot g_{m1}^2+\frac{4kT}{R_D}}{(g_m+g_{mb})^2}$

计算输入参考噪声电流时将输入开路，此时由于M1源极开路，所以M1及其噪声整体的电流为0，输出噪声为$\overline{V_{out}^2}=\overline{I_{n,RD}^2}R_D^2$。
![](Pasted%20image%2020231214212150.png)
CG电路的“电流增益”为1，上图(d)中等效输入参考噪声电流为$\overline{I_{n,in}^2}R_D^2=\overline{V_{n,out}^2}$。因此输入参考噪声电流为$\overline{I_{n,in}^2}=\overline{I_{n,RD}^2}=\frac{4kT}{R_D}$。

实际CG stage的源极会加入电流源进行静态偏置，如下图，其中$C_0$用来滤掉M0支路引入的噪声（后文会讨论$C_0$能在多大程度滤掉M0支路的噪声）。
![](Pasted%20image%2020231214213029.png)
对于加入电流源偏置后的CG stage，M2的噪声会影响输入参考噪声电流，但不会影响输入参考噪声电压。因为计算输入参考噪声电压时要将$V_{in}$短路，这时M2的噪声不会反映在输出噪声上。但计算输入参考噪声电流时$V_{in}$开路，这时M2的噪声$\overline{I_{n2}^2}$就全部通过$R_D$而产生噪声电压了，等效于M2的噪声直接加入输入参考噪声电流（事实上从电路上可以直接看出M2的噪声是与输入参考噪声电流并联的）。
此时要降低M2引入的输入参考噪声电流，就需要降低$g_{m2}$，但是偏置的目的是使M2支路的电流保持在一个固定值，因此M2的过驱动电压就要提高，从而意味着$V_{out}$最小值也要提高，即输出摆幅与M2引入的噪声的trade-off。
事实上，在有偏置的情况下M1的噪声对输入参考噪声电流的贡献不再是0，但通常情况下依然可以忽略。下一小节讨论Cascode电路时会谈到。

### cascode stage的噪声
电路如下图：
![](Pasted%20image%2020231214215213.png)
由于输入阻抗很大，因此只需考虑输入参考噪声电压。M1和$R_D$产生的输出噪声为$\overline{V_{n,out}^2}=4kT\gamma g_{m1}+\frac{K}{C_{ox}WL}\cdot\frac{1}{f}\cdot g_{m1}^2+4kTR_D$
除以增益$g_{m1}R_D$即为输入参考噪声电压。
在讨论M2引入的噪声，如前所述，M2的噪声可以直接等效到其栅极：
![](Pasted%20image%2020231214215846.png)
其中$\overline{V_{n2}^2}=4kT\gamma \frac{1}{g_{m1}}+\frac{K}{C_{ox}WL}\cdot\frac{1}{f}$
$V_{n2}$到$V_{out}$是degenerated CS stage，忽略$r_o2$（认为是无穷大的话），那么增益为$\frac{-R_D}{\frac{1}{g_{m2}}+r_{o1}}$由于$r_{o1}$通常很大，因此增益约为0。只有在高频要考虑X点的电容导致阻抗降低，或者$R_d$与X点向M1看的阻抗相当时（比如$R_D$是个current source load）才需要考虑M2对噪声的贡献。上小节中CG stage在有偏置时的M1对噪声的贡献与此类似。

### current mirror的噪声
前面提到，current mirror电路中的参考电流会引入噪声，因此需要在栅极加入一个电容来滤掉噪声。
电路如下：
![](Pasted%20image%2020231215013855.png)
$M_{ref}$管部分小信号等效和引入的噪声如下图：
![](Pasted%20image%2020231215014200.png)
进行小信号的戴维南等效，上图可见开路时的电压就是噪声电压$V_{n,ref}$，$M_{ref}$管是一个diode-connected MOS，所以内阻为$\frac{1}{g_m}$。因此戴维南等效后，再考虑M1管的噪声，电路如下图：
![](Pasted%20image%2020231215014453.png)
于是M1的噪声电流就是(因为是无关噪声，所以要分别计算两个噪声在栅极的电压然后叠加）：
$\overline{I_{n,out}^2}=(|\frac{1/(C_Bs)}{1/g_{m,ref}+1/(C_Bs)}|^2\overline{V_{nref}^2}+\overline{V_{n1}^2})g_{m1}^2$
由于是电流镜，假设$N\cdot I_{ref}=I_D$，那么$g_{m1}=\sqrt{2\mu_nC_{ox}W/L\cdot I_D}=\sqrt{N}g_{mref}$
因此$\overline{I_{n,out}^2}=(\frac{Ng_{m,ref}^2}{C_B^2\omega^2+g_{m,ref}^2}+1)g_{m1}^2\overline{V_{n1}^2}$
而M1自身的噪声电流就是$g_{m1}^2\overline{V_{n1}^2}$，因此$C_B$足够大时能使$\overline{I_{n,out}^2}\approx g_{m1}^2\overline{V_{n1}^2}$，但书中给出了一个例子说明正常情况下Mhz频率应用时需要nF级电容。由此书中给出了另一种方法，在栅极额外串联电容：
![](Pasted%20image%2020231215020858.png)
并给出了这时的输出电流噪声：
$\overline{I_{n,out}^2}=(\frac{g_{m,ref}^2}{(1+g_{m,ref}R_B)^2C_B^2\omega^2+g_{m,ref}^2}(\overline{V_{nref}^2}+\overline{V_{nRB}^2})+\overline{V_{n1}^2})g_{m1}^2$
可见电阻虽然减小了$C_B$需求，但也会引入自己的额外噪声。

### differential pair的噪声
由于differential pair输入也是栅极，因此也没有输入参考噪声电流。
计算输入参考噪声电压的原理图如下：
![](Pasted%20image%2020231215021748.png)
首先计算M1引入的噪声：
![](Pasted%20image%2020231215021838.png)
对于M1的噪声$\overline{I_{n1}^2}$，忽略沟长调制效应，向M1和M2看进去的阻抗都相同，因此流入两边电流相等。
但是$R_D1$的电流方向是从VDD到M1，而$R_D2$电流方向是M2到VDD，因此产生的差分噪声电压为$V_{n,out|M_1}=\frac{I_{n1}}{2}R_{D1}+\frac{I_{n1}}{2}R_{D2}=I_{n1}R_D$（因为流入两边的都是$I_{n1}$，是相关的噪声，就要用电流分量相加减得到输出噪声，而不是分别计算输出噪声然后相加）
M2引入的噪声同理。M1和M2引入的噪声都为$\overline{V_{n,out}^2}=2\overline{I_{n,12}^2}R_D^2$
再考虑$R_d$引入的噪声，得到总的输出噪声为：
$\overline{V_{n,out}^2}=2\overline{I_{n,12}^2}R_D^2+8kTR_D$
差分对的增益为$g_mR_D$，因此输入参考噪声为$\overline{V_{n,in}^2}=2\overline{I_{n,12}^2}/g_m^2+8kT/(g_m^2R_D)$

此外，尾电流源本身存在噪声，相当于尾电流源的电流在波动，在差分输入时也会引入差分噪声。
$V_{out}=(\Delta I_{D1}+\Delta I_{D2})R_D=\sqrt{2\mu_nC_{ox}W/L (I_{ss}+I_n)/2}\Delta V_{in}$
取$I_n\ll I_{ss}$近似，得$V_{out} \approx g_{m0}\Delta V_{in}R_D+g_{m0}R_D\Delta V_{in}\frac{I_n}{2I_{ss}}$
第二项就是噪声引入的差分噪声。

### differential pair with active current mirror的噪声
先计算输出短路时引入的电流，再除以跨导得到输入参考电压。
输出短路时如下图：
![](Pasted%20image%2020231215025052.png)
由于尾电流源固定，M1和M2引入的噪声电流都直接从短路的输出支路走。
M3引入的噪声电流不会进入M1（忽略M1和M2的沟长调制），而是与自己的阻抗$\frac{1}{g_{m3}}$相乘变成M4的栅压后再乘以$g_{m4}$进入输出支路。M4的噪声电流也不会进入M2，全部进入短路的输出支路。因为M1和M2电流之和是尾电流源，是固定的。
最终得到的短路电流噪声就是4个MOS的电流噪声之和。跨导为$g_{m1,2}$，因此输入参考电压$\overline{V_{n,in}^2}=\frac{\overline{I_{n1}^2}+\overline{I_{n2}^2}+\overline{I_{n3}^2}+\overline{I_{n4}^2}}{g_{m,1,2}^2}$

由于OTA是单端输出，因此尾电流源的电流噪声直接反映为输出噪声。
![](Pasted%20image%2020231215030559.png)
考虑尾电流源噪声时两边对称，$V_{out}=V_X =\frac{I_n}{2}\cdot \frac{1}{g_{m3}}$
噪声要用功率所以$V_{out}^2=(\frac{I_n}{2}\cdot \frac{1}{g_{m3}})^2$

## 噪声带宽
噪声带宽的概念如下图：
![](Pasted%20image%2020231215031655.png)
通常由于电路是由零极点的，所以噪声也有零极点。噪声带宽就是使$V_0^2\cdot B_n = \int_0^\infty\overline{V_{n,out}^2 }df$成立的带宽。
















