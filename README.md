# Kelly's Criterion for Optimal Bet Sizing
The math behind the famous optimal bet size formula "Kelly's Criterion".
Please see the PDF [Kelly_Criterion.pdf](Kelly_Criterion.pdf) for an explanation of the formula and its derivation. 

Kelly's criterion provides a simple formula for the optimal bet size in a gamble with two outcomes under certain assumptions:

$$x^*=p-q/b$$

where $p=\mathbb{P}(\text{win one round})$, $q=1-p=\mathbb{P}(\text{lose one round})$, and $b>0$ is the payout from one successful bet. 

A more detailed explanation of the variables and the assumptions, along with a full derivation of the result, is in the PDF document linked above.

Here’s a GitHub README–friendly Markdown conversion of your LaTeX document. I preserved the math using standard LaTeX math syntax (`$...$` and `$$...$$`) which GitHub renders properly. I also removed document-class boilerplate and converted environments like `itemize` and `enumerate` into Markdown lists.

Source file: 


# Optimal Gambling: Deriving The Kelly Criterion

*Antonio Melacini*  
March 2026

Consider a gamble with two states: we either win or lose.

Define the following variables:

- Let $W_0>0$ be your initial wealth.
- Let $x\in[0,1]$ be the proportion of your wealth that you bet.
- Let $b>0$ be the payout from a successful bet, so if you win you receive $W_0bx$ and your total wealth becomes $W_0(1+bx)$. If the odds are "$x$-to-$y$" then $b=x/y$.
- Let $a>0$ be the proportion of your betting amount that you pay when you lose, so when you lose your wealth becomes $W_0(1-ax)$. In an ordinary gamble $a=1$ (you lose everything if you lose), but for a stock investment it might be reasonable to assume $a\in(0,1)$. Also note that if you're leveraged (e.g., buying on margin), then $a>1$.
- Let $p$ be the probability you win and $q=1-p$ be the probability you lose.

---

Now we make three key assumptions:

1. Bets compound.
2. Infinite repeated gambles are possible.
3. Gambles are independent.

Meaning that:

1. If we win once, we have $W_0(1+bx)$, and then $100x\%$ of our wealth becomes $W_0(1+bx)x$, so if we lose in the next bet, we will have $W_0(1+bx)(1-ax)$. This assumption holds up in most real-life settings.
2. We can continue playing the gamble repeatedly forever.
3. All rounds of gambling are independent. For example, whether we win or lose the next bet is not determined by whether we won or lost any of our past bets.

---

## Theorem (Kelly Criterion)

Under the setup above, the optimal bet size is

$$
x^*=p/a-q/b
$$

and in the usual case where $a=1$ we have Kelly's formula

$$
x^*=p-q/b
$$

---

# Proof

Suppose we have bet for $n$ rounds.

The number of rounds we won is a binomial random variable

$$
N_{\text{win}} \sim \text{Bin}(n,p)
$$

and the number of rounds we lost is

$$
N_{\text{lose}} = n - N_{\text{win}} \sim \text{Bin}(n,q)
$$

Hence, our wealth becomes

$$
W_n = W_0(1+bx)^{N_{\text{win}}}(1-ax)^{N_{\text{lose}}}
$$

so our wealth has grown by

$$
R_n := \frac{W_n}{W_0}
=
(1+bx)^{N_{\text{win}}}(1-ax)^{N_{\text{lose}}}
$$

which is clearly nonlinear in $(N_{\text{win}}, N_{\text{lose}})$, so $\mathbb{E}(R_n)$ is difficult to calculate.

Thanks to our assumptions about rounds of betting being IID, the strong law of large numbers guarantees that

$$
\frac{N_{\text{win}}}{n}
\xrightarrow[n\to\infty]{a.s.}
p
\qquad\text{and}\qquad
\frac{N_{\text{lose}}}{n}
\xrightarrow[n\to\infty]{a.s.}
q
$$

Then, the per-period (or "per-bet") growth rate is

$$
R_n^{1/n}
=
(1+bx)^{N_{\text{win}}/n}
(1-ax)^{N_{\text{lose}}/n}
$$

Since $R_n^{1/n}$ is a continuous function of the vector

$$
\left(
\frac{N_{\text{win}}}{n},
\frac{N_{\text{lose}}}{n}
\right),
$$

the continuous mapping theorem guarantees that

$$
R_n^{1/n}
\xrightarrow[n\to\infty]{a.s.}
(1+bx)^p(1-ax)^q
=:R
$$

So our asymptotic growth rate per round is $R$.

We wish to find the optimal bet size $x^*$ that maximizes $R$:

$$
x^*
=
\arg\max_x \{R\}
=
\arg\max_x \{\ln R\}
$$

Let

$$
L = \ln R
=
p\ln(1+bx)+q\ln(1-ax)
$$

Then

$$
\frac{dL}{dx}
=
pb(1+bx)^{-1}
-
qa(1-ax)^{-1}
$$

Now solve for the root of the derivative:

$$
\frac{dL}{dx}=0
$$

$$
\begin{aligned}
0
&=
pb(1+bx)^{-1}
-
qa(1-ax)^{-1}
\\
&\implies
pb(1-ax)=qa(1+bx)
\\
&\implies
pb-pbax=qa+qabx
\\
&\implies
xab(q+p)=pb-qa
\\
&\overset{p+q=1}{\implies}
xab=pb-qa
\\
&\implies
x=p/a-q/b
\end{aligned}
$$

Therefore, the optimal bet size is

$$
x^*=p/a-q/b
$$

and when $a=1$, this simplifies to Kelly's formula

$$
x^*=p-q/b
$$

$$
\square
$$

