@[剑指offer](学习)

# 1. 二维数组中的查找

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

**思路**
```  
  [
    [1,2,3,4],
    [2,3,4,5],
    [3,4,5,6],
    [4,5,6,7]
  ]
```  
根据这个数组的特点，可以发现：
- （1）每一行，从左到右递增
- （2）每一列，从上到下递增

那么，我们在选择第一个数与target比较大小，第一个数的选取一定要选择数组中每行每列最后一个数。在这里我选取的是**右上角**的数和target比较。
- (a)如果array[row,col]>target，则向左走，即j--
- (b)如果array[row,col]<target，则向右走，即i++

参考代码如下：
```javascript
function Find(target, array)
{
    // write code here
     var row = 0;
    var col = array[row].length-1;
    while(row<=array.length-1&&col>=0){
        if(target == array[row][col])
            return true
        else if(target<array[row][col])
            col--;
        else
            row++
    }
}
var arr = [[1,2,3,4],[2,3,4,5],[3,4,5,6],[4,5,6,7]];
console.log(Find(7,arr));//true
```

# 2.替换空格
请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

题目说的不太严谨：
- 1.能不能允许连续出现多个空格？
- 2.若有可能连续多个空格，用多个还是单个20%进行替换？

分三种情况解答:
1.不会出现连续多个空格：
直接用空格将字符串切割成数组，在用20%进行连接。
```javascript
function replaceSpace(str)
{
    return str.split(' ').join('%20');
}
```
2.允许出现多个空格，每个空格均用一个20%替换：
用正则表达式找到所有空格依次替换

```javascript
function replaceSpace(str)
{
    return str.replace(/\s/g,'%20');
}
```
3.允许出现多个空格，多个空格用一个20%替换：
用正则表达式找到连续空格进行替换
```javascript
function replaceSpace(str)
{
    return str.replace(/\s+/g,'%20');
}
```

方法二：利用split()+join()
```javascript
function replaceSpace(str)
{
    // write code here
     // write code here
    let arr = str.split(' ');
    let newStr = arr.join('%20');
    return newStr;
}
```
# 3.从尾到头打印链表
输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。
```javascript
/*function ListNode(x){
    this.val = x;
    this.next = null;
}*/
function printListFromTailToHead(head)
{
    // write code here
   var arr=[];
    var me=head;
    while(me){
        arr.push(me.val);
        me=me.next;
    }
    return arr.reverse();
}
```
方法二：unshift():在数组前添加任意项并返回新数组的长度
```javascript
/*function ListNode(x){
    this.val = x;
    this.next = null;
}*/
function printListFromTailToHead(head)
{
    var result=[];
    while(head){
        result.unshift(head.val);
        head=head.next;
    }
    return result;
}
```
# 4.重建二叉树
# 5.用两个栈实现队列
用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

**思路**：

- 1.整体思路是元素先依次进入栈1，再从栈1依次弹出到栈2，然后弹出栈2顶部的元素，整个过程就是一个队列的先进先出
- 2.但是在交换元素的时候需要判断两个栈的元素情况：
“进队列时”，队列中是还还有元素，若有，说明栈2中的元素不为空，此时就先将栈2的元素倒回到栈1 中，保持在“进队列状态”。
“出队列时”，将栈1的元素全部弹到栈2中，保持在“出队列状态”。
所以要做的判断是，进时，栈2是否为空，不为空，则栈2元素倒回到栈1，出时，将栈1元素全部弹到栈2中，直到栈1为空。

![](https://img2018.cnblogs.com/blog/1502611/201903/1502611-20190325204459123-970878480.jpg)

```javascript
var stack1=[],stack2=[];
function push(node)
{
    // write code here
    stack1.push(node);
}
function pop()
{
    // write code here
    if(stack2.length === 0){
        if(stack1.length ===0){
            return null;
        }else{
            var len = stack1.length;
            for(var i=0;i<len;i++){
                stack2.push(stack1.pop());
            }
            return stack2.pop();
        }
    }else{
        return stack2.pop();
    }
}
```
# 6.旋转数组的最小数字
把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

剑指Offer中有这道题目的分析。这是一道二分查找的变形的题目。
旋转之后的数组实际上可以划分成两个有序的子数组：前面子数组的大小都大于后面子数组中的元素

注意到实际上最小的元素就是两个子数组的分界线。本题目给出的数组一定程度上是排序的，因此我们试着用二分查找法寻找这个最小的元素。

**思路**：

- （1）我们用两个指针left,right分别指向数组的第一个元素和最后一个元素。按照题目的旋转的规则，第一个元素应该是大于最后一个元素的（没有重复的元素）。
但是如果不是旋转，第一个元素肯定小于最后一个元素。
- （2）找到数组的中间元素。
中间元素大于第一个元素，则中间元素位于前面的递增子数组，此时最小元素位于中间元素的后面。我们可以让第一个指针left指向中间元素。

移动之后，第一个指针仍然位于前面的递增数组中。

中间元素小于第一个元素，则中间元素位于后面的递增子数组，此时最小元素位于中间元素的前面。我们可以让第二个指针right指向中间元素。

移动之后，第二个指针仍然位于后面的递增数组中。

这样可以缩小寻找的范围。

（3）按照以上思路，第一个指针left总是指向前面递增数组的元素，第二个指针right总是指向后面递增的数组元素。

最终第一个指针将指向前面数组的最后一个元素，第二个指针指向后面数组中的第一个元素。

也就是说他们将指向两个相邻的元素，而第二个指针指向的刚好是最小的元素，这就是循环的结束条件。

到目前为止以上思路很耗的解决了没有重复数字的情况，这一道题目添加上了这一要求，有了重复数字。

因此这一道题目比上一道题目多了些特殊情况：

我们看一组例子：｛1，0，1，1，1｝ 和 ｛1，1， 1，0，1｝ 都可以看成是递增排序数组｛0，1，1，1，1｝的旋转。

这种情况下我们无法继续用上一道题目的解法，去解决这道题目。因为在这两个数组中，第一个数字，最后一个数字，中间数字都是1。

第一种情况下，中间数字位于后面的子数组，第二种情况，中间数字位于前面的子数组。


也就无法移动指针来缩小查找的范围。

# 7.斐波那契数列
大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。

```javascript
function Fibonacci(n){  
    if(n<=1){
        return n;
    }else{
        var f0=0,f1=1;
   for(var i=2;i<=n;i++){
        f2=f0+f1;
        f0=f1;
        f1=f2;
    }
      return f2;}
}
```
