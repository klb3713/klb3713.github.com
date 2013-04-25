===================================
USACO 1.1 Broken Necklace
===================================

题目描述
--------
You have a necklace of N red, white, or blue beads (3<=N<=350) some of which are red, others blue, and others white, arranged at random. Here are two examples for n=29::

                1 2                               1 2 
            r b b r                           b r r b 
          r         b                       b         b 
         r           r                     b           r 
        r             r                   w             r 
       b               r                 w               w 
      b                 b               r                 r 
      b                 b               b                 b 
      b                 b               r                 b 
       r               r                 b               r 
        b             r                   r             r 
         b           r                     r           r 
           r       r                         r       b 
             r b r                             r r w 
            Figure A                         Figure B 
    
    r red bead 
    b blue bead 
    w white bead 

The beads considered first and second in the text that follows have been marked in the picture. 

The configuration in Figure A may be represented as a string of b's and r's, where b represents a blue bead and r represents a red one, as follows::

    brbrrrbbbrrrrrbrrbbrbbbbrrrrb . 

Suppose you are to break the necklace at some point, lay it out straight, and then collect beads of the same color from one end until you reach a bead of a different color, and do the same for the other end (which might not be of the same color as the beads collected before this).

Determine the point where the necklace should be broken so that the most number of beads can be collected. 

**Example**

For example, for the necklace in Figure A, 8 beads can be collected, with the breaking point either between bead 9 and bead 10 or else between bead 24 and bead 25. 

In some necklaces, white beads had been included as shown in Figure B above. When collecting beads, a white bead that is encountered may be treated as either red or blue and then painted with the desired color. The string that represents this configuration will include the three symbols r, b and w. 

Write a program to determine the largest number of beads that can be collected from a supplied necklace. 

**PROGRAM NAME** : beads 

**INPUT FORMAT** ::

    Line 1:  N, the number of beads 
    Line 2:  a string of N characters, each of which is r, b, or w 

**SAMPLE INPUT** (file beads.in) ::

    29 
    wwwbbrwrbrbrrbrbrwrwwrbwrwrrb 

**OUTPUT FORMAT**

A single line containing the maximum of number of beads that can be collected from the supplied necklace. 

**SAMPLE OUTPUT** (file beads.out) ::

    11 

**OUTPUT EXPLANATION**

Consider two copies of the beads (kind of like being able to runaround the ends). The string of 11 is marked. ::

    wwwbbrwrbrbrrbrbrwrwwrbwrwrrb wwwbbrwrbrbrrbrbrwrwwrbwrwrrb 
                           ****** ***** 

解题报告
--------
这个题目我的思路和网上 `这篇 <http://zqynux.blog.163.com/blog/static/1674995972010631968443/>`_ 类似, 只是在最终实现卡住了，编程能力还是不过关。

用a表示前一段的连续长度, b表示当前段的连续长度, w表示在当前出现的w的连续数目，结果取最大的a+b值。对于对于循环的话, 就把字符串f再复制一份到f后面。详情见代码：

.. code-block:: c

    /*
    ID: klbgyx71
    LANG: C
    PROG: beads
    */
    
    #include <stdio.h>
    #include <string.h>

    int main (void) {
        int i, limit;
        int a = 0, b = 0, w = 0;
        char c = '0';   /*当前段的珠子颜色 b or r*/
        int m = 0;
        int n;
        char f[701];

        FILE *fin  = fopen ("beads.in", "r");
        FILE *fout = fopen ("beads.out", "w");
        fscanf (fin, "%d %s", &n, f);
        memcpy(f+n, f, n);
        limit = 2*n;

        for(i = 0; i < limit; i++){
            if(f[i] == 'w'){
                b++;
                w++;
            }else if(f[i] == c){
                b++;
                w = 0;
            }else{
                if(b + a > m){
                    m = b + a;
                }
                a = b - w;
                b = w + 1;
                w = 0;
                c = f[i];
            }
        }
        if(a + b > m){
            m = b + a;
        }
        fprintf(fout, "%d\n", m > n ? n : m);
        return 0;
    }

