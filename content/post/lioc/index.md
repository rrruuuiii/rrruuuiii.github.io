---
title: Logical Interpretations of Continuations
date: '2024-04-01'
---

Hi, I'm Rui! This is my first blog post.

Happy April Fools' Day! But I'm not here making jokes. Today, I was reading recitation notes on continuations and would like to present some rules that could be used in a proof tree to help us construct a proof term involving continuations much more easily.

We know the correspondence between continuation and proposition:

$$
  \tau \ \texttt{cont} \leftrightarrow \neg A
$$

where we associate the type $\tau$ with the proposition $A$. If we have a proof of $A$, or equivalently, a term of type $\tau$, then we can "time travel" back to when we make the claim that no proof of $A$ exists and claim instead that a proof of $A$ exists using the term we come up with.

We present the following two rules concerning continuations:

{{< math >}}

\begin{prooftree}
    \AxiomC{$x : \neg A$}

    \noLine
    \UnaryInfC{$\vdots$}

    \noLine
    \UnaryInfC{$M : A$}
    
    \RightLabel{${\texttt{letcc}}^x$}
    \UnaryInfC{$\texttt{letcc}[A](x \ . \ M) : A$}
\end{prooftree}

where "$x : \neg A$" is our continuation, and

\begin{prooftree}
    \AxiomC{$M : \neg A$}
    \AxiomC{$N : A$}
    \RightLabel{$\texttt{throw}$}
    \BinaryInfC{$\texttt{throw}[B](M; N) : B$}
\end{prooftree}

{{< /math >}}

which basically says we can throw "$N : A$" to "$M : \neg A$" and time travel and conclude any proposition "$B$".

Now, consider the proposition $(A \supset B) \supset (\neg B \supset \neg A)$. We come up with the following proof tree without proof terms:

{{< math >}}

\begin{prooftree}
    % L
    \AxiomC{$[\neg B]_y$}

    % RL
    \AxiomC{$[A \supset B]_x$}

    % RRL
    \AxiomC{$[\neg \neg A]_z$}

    % RRR
    \AxiomC{$[\neg A]_u$}
        
    % RR
    \RightLabel{$\texttt{throw}$}
    \BinaryInfC{$A$}

    \RightLabel{$\texttt{letcc}^u$}
    \UnaryInfC{$A$}

    % R
    \RightLabel{$\supset E$}
    \BinaryInfC{$B$}

    \RightLabel{$\texttt{throw}$}
    \BinaryInfC{$\neg A$}

    \RightLabel{$\texttt{letcc}^z$}
    \UnaryInfC{$\neg A$}

    \RightLabel{$\supset I^y$}
    \UnaryInfC{$\neg B \supset \neg A$}

    \RightLabel{$\supset I^x$}
    \UnaryInfC{$(A \supset B) \supset (\neg B \supset \neg A)$}
\end{prooftree}

{{< /math >}}

A few points are worth mentioning here. First, note that $\texttt{letcc}^z$ introduces $\neg \neg A$, which we cannot directly apply double negation to obtain $A$. Also, we cannot apply the introduction rule $\supset I$ to $\neg A$ since we are currently interpreting $\neg A$ as a continuation rather than $A \rightarrow \bot$ in the constructive sense.

Therefore, the only rule that could be applied to $\neg A$ is the $\texttt{letcc}$ rule. Dually, the $\texttt{throw}$ rule replaces the $\supset E$ rule that could've been applied to $\neg A$ and $A$ in constructive logic. The $\supset E$ rule could still be applied to any propositions of the form $A \supset B$ and $A$.

Annotating the above proof tree with terms, we have:

{{< math >}}

\begin{prooftree}
    % L
    \AxiomC{$y : \neg B$}

    % RL
    \AxiomC{$x : A \supset B$}

    % RRL
    \AxiomC{$z : \neg \neg A$}

    % RRR
    \AxiomC{$u : \neg A$}
        
    % RR
    \RightLabel{$\texttt{throw}$}
    \BinaryInfC{$\texttt{throw}[A](z;u) : A$}

    \RightLabel{$\texttt{letcc}^u$}
    \UnaryInfC{$\texttt{letcc}[A](u \ . \ \texttt{throw}[A](z;u)) : A$}

    % R
    \RightLabel{$\supset E$}
    \BinaryInfC{$x(\texttt{letcc}[A](u \ . \ \texttt{throw}[A](z;u))) : B$}

    \RightLabel{$\texttt{throw}$}
    \BinaryInfC{$\texttt{throw}[\neg A](y ; x(\texttt{letcc}[A](u \ . \ \texttt{throw}[A](z;u)))) : \neg A$}

    \RightLabel{$\texttt{letcc}^z$}
    \UnaryInfC{$\texttt{letcc}[\neg A](z \ . \ \texttt{throw}[\neg A](y ; x(\texttt{letcc}[A](u \ . \ \texttt{throw}[A](z;u))))) : \neg A$}

    \RightLabel{$\supset I^y$}
    \UnaryInfC{$\lambda \ (y : \neg B)  \ \texttt{letcc}[\neg A](z \ . \ \texttt{throw}[\neg A](y ; x(\texttt{letcc}[A](u \ . \ \texttt{throw}[A](z;u))))) : \neg B \supset \neg A$}

    \RightLabel{$\supset I^x$}
    \UnaryInfC{$M : (A \supset B) \supset (\neg B \supset \neg A)$}
\end{prooftree}
where 
$$
    M = \lambda (x : A \supset B) \  \lambda \ (y : \neg B)  \ \texttt{letcc}[\neg A](z \ . \ \texttt{throw}[\neg A](y ; x(\texttt{letcc}[A](u \ . \ \texttt{throw}[A](z;u)))))
$$

{{< /math >}}

FIXME: proof tree is too big.

TODO: finish the rest of this post.