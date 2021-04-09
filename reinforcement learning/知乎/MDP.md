<center><font size=30>MDP</font></center>

# 马尔科夫决策

马尔科夫决策过程$(S,A,P,R,\gamma)$

$S$:状态集

$A$:动作集

$P$:状态转移概率  $P_{ss'}^a=P[S_{t-1}=s'|S_t=s,A_t=a]$

$R$:回报函数

$\gamma$:折扣因子

累计回报:$G_t=\sum^\infty_{k=0}\gamma^kR_{t+k+1}$







# 状态值函数

状态值函数 $v_\pi(s)=E_\pi[\sum_{k=0}^\infty \gamma^kR_{t+k+1}|S_t=s]$



# 状态-行为值函数

$q_\pi(s,a)=E_\pi[\sum_{k=0}^\infty \gamma^kR_{t+k+1}|S_t=s,A_t=a]$



# 贝尔曼方程

$v_\pi(s)=E_\pi[R_t+\gamma v(S_{t+1})|S_t=s]$

$q_\pi(s,a)=E_\pi[R_t+\gamma p(S_{t+1},A_{t+1})|S_t=s,A_t=a]$



$v_\pi=\sum_{a\in A}\pi(a|s)q_\pi(s,a)$

$q_\pi(s,a)=R^a_s+\gamma\sum_{s'} P^a_{ss'}v_\pi(s')$

$v_\pi(s)=\sum_{a\in A}\pi(a|s)(R^a_s+\gamma\sum_{s'} P^a_{ss'}v_\pi(s'))$



