## 再论递归

大概是从**汉诺塔**[hanoi](https://baike.baidu.com/item/%E6%B1%89%E8%AF%BA%E5%A1%94/3468295?fr=aladdin)了解递归算法的：
````
function hanoi(n, a, b, c) {
  if(n===1) {
    console.log(`${a} ---> ${c}`)
    return
  }
  hanoi(n-1, a, c, b);
  hanoi(1, a, b, c);
  hanoi(n-1, b, a, c);
}

hanoi(10, 'A', 'B', 'C');
````
我自诩脑回路清奇，然而面对这层层递归，大脑CPU却显得不堪重负，甚至一度崩盘！并且作为一个~伪~编程人员，如果让别人知道我对递归这样的基本算法都搞不定，颜面何存呐？！！所以，对此我一直耿耿于怀。

直到昨天晚上（2017/12/21真是个伟大的日子），我在知乎看到一个回答，简单粗暴。原答案见[python中的汉诺塔递归算法的具体运算过程是怎样的？](https://www.zhihu.com/question/37152936)

@半岛 的回答：
>重点其实是：不要一开始就关心每一步怎么解决的，你只需要把函数当成一个实现你目的的神器，随时调用。也就是递归。
>
>####01
>比如说我们有一个万能神器move，只需要给它几个参数，即可自动完成一个功能：把n个盘子利用缓冲区，从起点运送到终点，期间严格遵守汉诺塔规则。
>这里你暂时不需要去了解每一个步是如何实现的。
>
>>move(N,起点，缓冲区，终点）
>>N: 盘子的个数。
>
>
>####02
>现在有个n个盘子，a,b,c三个塔。
>把n个盘子抽象成两个盘子，n-1 和 底下最大的1:`n = (n-1) + 1`


>这个最简单的玩法如何实现呢？
>* 首先：把n-1 移到 缓冲区       -------过程1
>* 然后：把1 移到 终点           -------过程2
>* 最后：把缓冲区的n-1 移到 终点  -------过程3
>
>####03
>过程1 如何实现？  
>还是召唤神器吧。
> `move(N,起点，缓冲区，终点）`
>此时，我们的起点是a,终点是b ,N=n-1,缓冲区只能是c了
> `move(n-1,a,c,b)`
>过程2呢？
> `move(1,a,b,c)`
>过程3呢？
> `move(n-1,b,a,c)`
>
>####04
>哦哦 神器的力量太大，止不住咋办。。
>>if (N == 1):        
>>  a -> c #此时我已经不需要缓冲区了
>

-----
过程1-3是整个递归算法的核心。再抽象一点，即把大象装进冰箱拢共需要三步：
* 打开冰箱门
* 把大象装进冰箱
* 关上冰箱门

oh my my，胜利的号角已经吹响，五星红旗在敌人的阵地上迎风招展！

## 实现递归的基本步骤

* 1、实现一个功能需要 n 次递归调用；
* 2、假设前面 n-1 次已经执行完毕；
* 3、执行最后一步操作
* 4、添加递归边界条件

## 判别递归使用场景
最后，什么时候需要用到递归呢？一个简单的判别方法是：当前的输入依赖上一步的输出，依次重复。

最后的最后，做一个小练习加深一下印象吧：
````
/* 
小易准备去魔法王国采购魔法神器,购买魔法神器需要使用魔法币,但是小易现在一枚魔法币都没有,但是小易有两台魔法机器可以通过投入x(x可以为0)个魔法币产生更多的魔法币。
魔法机器1:如果投入x个魔法币,魔法机器会将其变为2x+1个魔法币
魔法机器2:如果投入x个魔法币,魔法机器会将其变为2x+2个魔法币
小易采购魔法神器总共需要n个魔法币,所以小易只能通过两台魔法机器产生恰好n个魔法币,小易需要你帮他设计一个投入方案使他最后恰好拥有n个魔法币。 
输入描述:
输入包括一行,包括一个正整数n(1 ≤ n ≤ 10^9),表示小易需要的魔法币数量。

输出描述:
输出一个字符串,每个字符表示该次小易选取投入的魔法机器。其中只包含字符'1'和'2'。

输入例子1:
>10

输出例子1:
>12

*/

// TODO:






















// js实现

// 最初实现思路
function createMagic(n) {
  let arr = [];

  function magicCoin(n) {
    // 递归边界
    if (n < 1 || n > Math.pow(10,9)) return;
    if (n === 1) return arr.push(1);
    if (n === 2) return arr.push(2);

    if (n % 2 === 0) {
      magicCoin((n - 2) / 2);
      arr.push(2);
    } else {
      magicCoin((n - 1) / 2);
      arr.push(1);
    }
  }
  magicCoin(n);

  return arr.join('')
}

// 略微优化
function magicCoin(n) {

  if (n < 1) return;
  if (n % 2 === 0) {
    return magicCoin((n - 2) / 2) + '2';
  } else {
    return magicCoin((n - 1) / 2) + '1';
  }
}


// test

for(var i = 1; i<20; i++) {
  console.log(i+': ' +createMagic(i));
}

// 输出
1: 1
2: 2
3: 11
4: 12
5: 21
6: 22
7: 111
8: 112
9: 121
10: 122
11: 211
12: 212
13: 221
14: 222
15: 1111
16: 1112
17: 1121
18: 1122
19: 1211

````

敬请勘误指正！
