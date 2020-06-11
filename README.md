## 双指针--LeetCode第十一题
双指针法能快速遍历数组，得到简化代码，且在提高代码效率有很大裨益，下面我们以leetcode上的第11题为例来讲解该方法。

 #### 题目：
 给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

#### 说明：你不能倾斜容器，且 n 的值至少为 2。
（来源：力扣（LeetCode）链接：[https://leetcode-cn.com/problems/container-with-most-water](https://leetcode-cn.com/problems/container-with-most-water)![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOS85LzI0LzE2ZDYyM2RhMGFkNDczYWM?x-oss-process=image/format,png#pic_center)
#### 暴力法的原理：
通过**计算柱子中由任意两个互异且固定的柱子构成的容器的体积**（我把线认为是柱子），并进行比较，得到最优解，用两次for循环来**排除已参与构成容器的柱子**，直到只剩下一根柱子。
#### 双指针法原理：
同样利用暴力法的原理--**排除柱子**，不同的是通过巧妙的遍历快速排除柱子，并且也遍历了所有能够构成容器的柱子组合。
1. 比较两端柱子的高低并**选择低柱子**，那么低柱子与其他任何的柱子之间形成的容器体积最大的就是由该柱子与最末尾的那个柱子构成的，这样既得到了该柱子与其他任意一个柱子所构成的体积最大的容器，又**排除了一根柱子**。
2. 接下来，比较剩下的柱子群两端柱子的高低，并**选择低柱子**，方法与上面类似，得到最大体积，与上面得到的体积进行比较，保留大的。就这样一直循环，得到最大体积，并且还遍历所有柱子的组合构成的容器体积（中间体积较小的容器的不用计算，因为我们已经人为排除了）。

#### 常规暴力方法求解简单但低效：
![](https://img-blog.csdnimg.cn/20200611212518467.png)
代码如下：

```c
/*max是最大体积，height是数组a的指针，heightSize是数组长度*/
int maxArea(int* height, int heightSize){
    int max=height[1]>height[0]?height[0]:height[1],tem=0;
    for(int i=0;i<heightSize;i++){
        for(int t=i+1;t<heightSize;t++){
            tem=(height[i]>height[t]?height[t]:height[i])*(t-i);
            if(tem>max){
                max=tem;
            }
        }
    }
    return max;
}
```

#### 双指针遍历：要得到正确结果不难，难就难在如何快速得到结果--即排除柱子。
![](https://img-blog.csdnimg.cn/20200611212658463.png)

```c
/*max是最大体积，height是数组a的指针，heightSize是数组长度*/
int maxArea(int* height, int heightSize){
    int max=0;
    for(int i=0,t=heightSize-1;t-i>=1;){
        if(height[i]<height[t]){//判断柱子群两端柱子的高低
            int tem=height[i]*(t-i);//得到由该柱子组成的容器中体积最大的
            max=max>tem?max:tem;
            i++;
        }
        else{
            int tem=height[t]*(t-i);
            max=max>tem?max:tem;
            t--;
        }
    }
    return max;
}
```
