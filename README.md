# kelly-criterion
The math behind the famous optimal bet size formula "Kelly's Criterion".

\documentclass{article}
\usepackage{graphicx} % Required for inserting images
\usepackage{graphicx}
\usepackage{amsmath,amssymb}
\usepackage{bbm}
\usepackage[margin=1.3in]{geometry}

%<!--  Math symbols -->
\newcommand{\T}{^{\top}}
\newcommand{\isim}{\overset{\text{ind}}{\sim}}
\newcommand{\iid}{\overset{iid}{\sim}}
\newcommand{\deq}{\overset{d}{=}}
\newcommand{\dconv}{\overset{d}{\rightarrow}}
\newcommand{\pconv}{\overset{p}{\rightarrow}}
\newcommand{\asconv}{\overset{a.s.}{\rightarrow}}
\newcommand{\asconvn}{\xrightarrow[n\rightarrow\infty]{a.s.}}
\newcommand{\quant}{^{\leftarrow}}
\newcommand{\indep}{\perp\!\!\!\perp}
\newcommand{\ddx}{\frac{d}{dx}}
%<!-- Sets (natural numbers etc) -->
\newcommand*{\IN}{\mathbb{N}} 
\newcommand{\IZ}{\mathbb{Z}}
\newcommand{\IQ}{\mathbb{Q}}
\newcommand{\IR}{\mathbb{R}}
\newcommand{\IC}{\mathbb{C}}
%<!-- Distributions, processes -->
\newcommand{\sign}{\operatorname{sign}}
\newcommand{\se}{\operatorname{se}}
\newcommand{\Geo}{\operatorname{Geo}}
\newcommand{\Exp}{\operatorname{Exp}}
\newcommand{\Poi}{\operatorname{Poisson}}
\newcommand{\NVM}{\operatorname{NVM}}
\newcommand{\Par}{\operatorname{Par}}
\newcommand{\IG}{\operatorname{IG}}
\newcommand{\LN}{\operatorname{LN}}
\newcommand{\Cauchy}{\operatorname{Cauchy}}
\newcommand{\U}{\mathcal{U}} 
\newcommand{\B}{\operatorname{B}}
\newcommand{\Bin}{\operatorname{Bin}}
\newcommand{\Ber}{\operatorname{Ber}}
\newcommand{\Beta}{\operatorname{Beta}}
\newcommand{\NB}{\operatorname{NB}}
\newcommand{\N}{\operatorname{N}}
\newcommand{\F}{\mathcal{F}}
%<!-- Operators, functions -->
\newcommand{\I}{\mathbbm{1}} 
\renewcommand{\P}{\mathbb{P}}
\newcommand{\E}{\mathbb{E}}
\newcommand{\dummy}[1]{\mathbbm{1}\{\texttt{#1}\}}
\newcommand{\med}{\operatorname{med}}
\newcommand{\Var}{\operatorname{Var}}
\newcommand{\Cov}{\operatorname{Cov}}
\newcommand{\Corr}{\operatorname{Corr}}
\newcommand{\diag}{\operatorname{diag}}
\newcommand{\logit}{\operatorname{logit}}
\newcommand{\expit}{\operatorname{expit}}
\newcommand{\Odds}{\operatorname{Odds}}
\newcommand{\OR}{\operatorname{OR}}
\newcommand{\RR}{\operatorname{RR}}
%<!-- Estimators  -->
\newcommand{\hmu}{\hat{\mu}}
\newcommand{\hpi}{\hat{\pi}}
\newcommand{\hsigma}{\hat{\sigma}}
\title{Optimal Gambling: Deriving The Kelly Criterion}
\author{Antonio Melacini}
\date{March 2026}


\begin{document}

\maketitle

Consider a gamble with two states: we either win or lose. \newline
Define the following variables:
\begin{itemize}
    \item Let $W_0>0$ be your initial wealth.
    \item Let $x\in[0,1]$ be the proportion of your wealth that you bet.
    \item Let $b>0$ be the payout from a successful bet, so if you win you receive $W_0bx$ and your total wealth becomes $W_0(1+bx)$. If the odds are "$x$-to-$y$" then $b=x/y$.
    \item Let $a>0$ be the proportion of your betting amount that you pay when you lose, so when you lose your wealth becomes $W_0(1-ax)$. In an ordinary gamble $a=1$ (you lose everything if you lose), but for a stock investment it might be reasonable to assume $a\in(0,1)$. Also note that if you're leveraged (e.g., buying on margin), then $a>1$. 
    \item Let $p$ be the probability you win and $q=1-p$ be the probability you lose.
\end{itemize}

\vspace{0.4cm}
Now we make three key assumptions:
\begin{enumerate}
    \item Bets compound.
    \item Infinite repeated gambles are possible.
    \item Gambles are independent.
\end{enumerate}
\vspace{0.4cm}

Meaning that:
\begin{enumerate}
    \item If we win once, we have $W_0(1+bx)$, and then $100x\%$ of our wealth becomes $W_0(1+bx)x$, so if we lose in the next bet, we will have $W_0(1+bx)(1-ax)$. This assumption holds up in most real-life settings.
    \item We can continue playing the gamble repeatedly forever.
    \item All rounds of gambling are independent. For example, whether we win or lose the next bet is not determined by whether we won or lost any of our past bets.
\end{enumerate}
\vspace{0.4cm}

\textbf{Theorem (Kelly's Criterion):} Under the setup above, the optimal bet size is
$$x^*=p/a-q/b$$
and in the usual case where $a=1$ we have Kelly's formula
$$x^*=p-q/b$$

\newpage 
\textbf{Proof:} Suppose we have bet for $n$ rounds. 
The number of rounds we won is a binomial random variable $N_{win}\sim\Bin(n,p)$ and the number of rounds we lost is also a binomial random variable $N_{lose}=n-N_{win}\sim\Bin(n,q)$. Hence, our wealth becomes $$W_n=W_0(1+bx)^{N_{win}}(1-ax)^{N_{lose}}$$
so our wealth has grown by 
$$
R_n:=\frac{W_n}{W_0}=(1+bx)^{N_{win}}(1-ax)^{N_{lose}}
$$
which is clearly nonlinear in $(N_{win},N_{lose})$, so $\E(R_n)$ is difficult to calculate. % work out the exact expectation manually by summation as exercise?
Thanks to our assumptions about rounds of betting being IID, the strong law of large numbers guarantees that:
$$
\frac{N_{win}}{n}\asconvn p 
~~~~\&~~~~
\frac{N_{lose}}{n}\asconvn q
$$
Then, the per-period (or "per-bet") growth rate is:
$$
R_n^{1/n}=
(1+bx)^{N_{win}/n}(1-ax)^{N_{lose}/n}
$$
Since $R_n^{1/n}$ is a continuous function of the vector $(N_{win}/n,~N_{lose}/n)$, the continuous mapping theorem guarantees that:
$$
R_n^{1/n} \asconvn
(1+bx)^{p}(1-ax)^{q}=:R
$$
So our asymptotic growth rate per-round is $R$. We wish to find the optimal bet size $x^*$ that maximizes $R$; 
$$x^* = \arg\max_{x}\{R\} = \arg\max_{x}\{\ln R\} $$
Let $L=\ln R=p\ln(1+bx)+q\ln(1-ax)$. 
\begin{align*}
    \ddx L &= pb(1+bx)^{-1}-qa(1-ax)^{-1} \\
\end{align*}
Then we solve for the root of the derivative:
\begin{align*}
    \ddx L = 0 &\implies 0 = pb(1+bx)^{-1}-qa(1-ax)^{-1} \\
    &\implies pb(1-ax)=qa(1+bx) \\
    &\implies pb-pbax = qa +qabx \\
    &\implies xab(q+p)=pb-qa \\
    &\overset{p+q=1}{\implies} xab =  pb-qa \\
    &\implies x = p/a-q/b
\end{align*}
% add a check that this is a global minimum?
Therefore, the optimal bet size is 
$$
x^*=p/a-q/b
$$
and when $a=1$, this simplifies to Kelly's formula 
$
x^*=p-q/b
$
$$
\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\square
$$







\end{document}
