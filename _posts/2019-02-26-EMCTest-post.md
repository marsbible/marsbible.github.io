---
layout: post
title: 某知名存储公司的几道面试题目 
modified: 2019-02-26
tags: [题目]
---

这是从网上看到的某知名存储公司的面试题目，我觉得题目设计的不错，贴上来供参考。

4. What are the most reasonable two numbers to follow the sequence:  

    A. 124, 217  

    B. 125, 217  

    C. 125, 215  

    D. 126, 215  

    E. None of the above  


5. If F(n)=50!*5^n(5^n是5的n次方的意思), (n is positive integer), what number is the least value of n such that F(n) and F(n+1) have the same number of trailing 0’s (F(n) is represented in decimal)?  

    A. 34  

    B. 35  

    C. 36  

    D. 37  

    E. None of the above  

6. There are 5 balls making a ring. Each ball is red or white, with equal probability. What is the probability that no two red balls are adjacent to each other in this ring?  

    A. 7/32  

    B. 5/16  

    C. 11/32  

    D. 13/32  

    E. None of the above  

7. As shown in the following figure, the shaed region is enclose by four lines, which all start from  
a square’s vertex, and end at a midpoint of the square’s side. If the side of the square has a length
of 1, then what is the area of the shaded region?

<figure>
    <img src="/images/posts/0_1323070407HgXu.png" alt="">
</figure>

    A. 2/7  

    B. 1/4  

    C. 1/5  

    D. 1/9  

    E. None of the above  

8. A Double Tower of Hanoi contains twice the number of disk as the regular Tower of Hanoi  
problem, where each disk size appears twice. So there are 2n disks, of n different sizes (n>0). As  
usual, there are 3 pegs. The objective is to transfer the whole tower form the original peg to one of 
the other two pegs, moving only one disk at a time, without putting a larger one over a smaller one.  
Putting a same-sized disk onto another is okay. If we are required to reproduce the original  
top-to-bottom order arrangement, how many moves (minimal) does it take? Remember, disks of  
equal size need to be in original order, and cannot be inversed.  

    A. 122nn.+

    B. 221n.

    C. (1)22n+.

    D. (2)25n+.

    E. 2*+1 (1)3n.

9. A binary tree has 7 nodes, which are denoted as A, B, C, D, E, F, and G. When the tree is walked  
in pre-order, the route is A-B-D-G-C-E-F. When this tree is wealked in in-order, the route is  
D-G-B-A-E-C-F. Which of the following is the correct route when the tree is walked in  
post-order?  

    A. A-B-D-G-E-F-C  

    B. G-D-B-A-E-F-C  

    C. D-B-G-A-C-E-F  

    D. G-D-B-E-F-C-A  

    E. None of the above  

10. Which of following entities CANNOT be shared by multiple threads of a process?  

    A. Data section  

    B. Thread-local variables  

    C. Register set  

    D. Stack  

    E. None of the above  

13. Which of following numbers(in base 3) is closest to decimal number 0.8889?  

    A. 0.21  

    B. 0.211  

    C. 0.212  

    D. 0.22  

    E. None of the above  

15. For an IA-32 system, which of following choices is the correct order according to the memory  
layout of a Linux program, from lower address to higher address?  

    A. Heap, Stack, Text  

    B. Text, Heap, Stack  

    C. Heap, Text, Stack  

    D. Text, Stack, Heap  

    E. Stack, Heap, Text  


17. In C, someone writes the following function to reverse a one-dimensional array. For example,  
when the input is {1,2,3,4,5}, then the result should be {5,4,3,2,1}.   
{% highlight c %}
int reverse_array(int *list, int len){

 int *p1,*p2;  

 int temp;  

 if(len<=0) return -1;  

 p1 = list;

 p2 = list + n -1;
 while(p1!=p2){  

 temp=*p1;  

 *p1=*p2;  

 *p2=temp;  

 p1++;  

 p2--;  

 }  

 Return 0;  
}  
{% endhighlight %}

Which of the following statements is correct when you compile and run this program?  

    A. Compilation error appears 

    B. Program runs correctly  

    C. Program runs correctly when length of input array is odd.  

    D. Program runs correctly when length of input array is even.  

    E. None of the above  

18. Assume you have two singly linked lists, denoted as L1 and L2. It is possible that L1 and L2  
meet on some node and have a common tail. If L1 has m nodes and L2 has n nodes, then what is  
the best time complexity to check if the two linked lists meet and to find out the meeting point?  
(Only O(1) constant amont of extra storage space is allowed.)  

    A. O((m+n)*log(m+n))  

    B. O(m*n)  

    C. O(m+n)  

    D. O((m+n)*log(m*n))  

    E. O(log(m*n))  

19. There are n green buckets and n red buckets. Each green bucket is of a different size, but for  
every green bucket, there is a corresponding red bucket of the same size. What is the AVERAGE  
time complexity to find all matching buckets pairs(red and green bucket of the same size) if the  
comparisons between buckets of same color are forbiden. (Only O(1) constant amount of extra  
storage space is allowed.)  

    A. O(n)  
    
    B. O(log(n))  
    
    C. O(n*n)  
    
    D. O(nlog(n))  
    
    E. None of the above  

我的解答：

4. E

   125=5^3,216=6^3,实在是看不出来A-D有啥规律，有达人说一个是7D一个是D7，貌似挺有道理。

5. B

   50!=2^x*5^y*...，需要求解x-y
   求x的方法：1-50这50个数中2的奇倍数有13个,4的奇倍数有6个，8的奇倍数有3个,16的奇倍数有2个
    ,32的奇倍数有1个，所以x=13+6*2+3*3+2*4+5=47；求y的方法：类似上面y=10+2=12，最后得到多余的2的幂是35，所以答案是35

6. C

    这个用手工的方法算就行，注意算上没有红球的概率，一个红球都没有的概率1/32，有一个红球的概率是
    5/32，有两个红球但是不相邻的概率是5/32(这个手工画画就行)，三者相加结果11/32

7. C

    这个用初中的知识就不难算出，答案是C

8. 
    看着答案像乱码，我开始自己推倒的结果是4*(2^n-1)，后来证明是错误的，应该是4*2^n-3
    最原始的汉诺塔的步数为2^n-1
    首先我们计算一下简单的情况，即不要求相同大小的盘子顺序不变，我们每次移动两个相同
    大小的盘子即可，所需要的步骤为2*(2^n-1)
    然后我们需要一个递推公式来计算在保持相同大小盘子顺序的情况下需要怎么移动。    
    我们记f(n)为保持顺序的前提下移动2n个盘子需要的步数,f'(n)为不保持同大小盘子顺序移动2n个盘子需要的步数
    需要注意的是，只有经过偶数次的无序移动才能恢复有序，我们在移动时候就需要保持盘子经过偶数次移动
    我们举最简单的例子来说明,n=2：
    从上面的情况可以看出，f(n) = 4*f'(n-1) + 3 = 4*(2^n-2)+5 = 4*2^n - 3
 
9.  D

        A

      B    C

    D     E    F

      G

10. D

   对Thread Local Var和Register Set不太清楚，查了一下，应该是共享的，支持写时复制功能
 
13. D

   基本常识

15. B 

    基本常识

17. C

    很明显的错误

18. C

    网上有成熟的算法，基本思路是先计算一遍链表的长度，然后长链表先出发，保证剩下的步数和短链表一样，两者第一个相同的节点就是相遇点。

19. D