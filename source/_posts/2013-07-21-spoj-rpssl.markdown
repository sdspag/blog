---
layout: post
title: "SPOJ RPSSL"
date: 2013-07-21 22:10
comments: true
categories: [Editorial, SPOJ, Question of the week, Probability] 
---

Problem Link : [http://www.spoj.com/problems/RPSSL/](http://www.spoj.com/problems/RPSSL/)

Prerequisites : [Bayes Theorem](http://en.wikipedia.org/wiki/Bayes'_theorem)
 
Here comes the editorial for [previous week's question](http://www.spoj.com/problems/RPSSL/) as posted on [our group.](https://www.facebook.com/groups/sdspag/)

Let the probability that Rajesh beats Sheldon in a single round be $$ w $$ and let $$ d $$ be the probability that a single round results in a draw and $$ l $$ be the probability that Sheldon beats Rajesh in a single round. $$ w ,\ d ,\ l $$ can be calculated simply as follows :


$$

w = R_R*(S_L +S_{Sc}) + R_{Sc}*(S_P + S_L) + R_P*( S_R + S_{Sp}) + R_L*( S_{Sp}+ S_P) + R_{Sp}*( S_R + S_{Sc}) \\

d =  (R_R*S_R) + (R_{Sc}*S_{Sc}) + (R_P*S_P) + (R_L*S_L)+ (R_{Sp}*S_{Sp}) \\
              
l=1-d-w\\         

$$ 

Now Rajesh will win the game if he wins two rounds first.
The following are the different cases : 

$$

ww \: or \: wdw \: or \: dww \: or \: ddww \: or \: dwdw \: or \: wddw \: ... \\
OR\\
lww \: or \: wlw \: or \: lwdw \: or \: wdlw \: or \: wldw \: ... \\

$$

Thus we need to sum up all the above cases which result in
victory for rajesh. Both the above series  can be simplified by using some maths tricks. (Refer to [Arithmetico-geometric series](http://en.wikipedia.org/wiki/Arithmetico-geometric_sequence).)
 
The final expression is :

$$

P = w*w*(1-d)^{-2} + 2*l*w*w*(1-d)^{-3}

$$

**Caution:** Check for the cases where d=1.