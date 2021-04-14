# Variational CNN


*********************

Consider every CNN to be a Fredholm operator. RL network is used in the 
testing space.

$$

F * X = C

< F * X, t > = < C, t >, t \in T 

$$

The problem is then convert to CNN + RL with objective function <C, t>.

如果卷积网络在问题域中难以求解，则将其变分到测试空间中成为 RL 后再进行求解。
