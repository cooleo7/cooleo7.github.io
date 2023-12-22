---
layout: post
title:  "Recommender systemd based on self-attention mechanism"
date:   2018-05-29 18:05:55 +0300
image:  10.png
tags:   
---
In this project, I implemented the recommender system based on self-attention mechanism from movie dataset. The purpose of this project is to learn something new through careful data analysis such as self-attention mechanism from transformer.

The goal of this project is finding out the accuracy of prediction depending on the condition in experiments. The project will be consist of 6 parts:

* Basic concept
* Data Presentation
* Transformer Encoder
* Experiment - Data preprocess & Model learning
* Result
* Conclusion


## Basic concept
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    displayMath: [['$$','$$'], ['\[','\]']],
    processEscapes: true,
    processEnvironments: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code'],
	ignoreClass: ["gist-file"],
    TeX: { equationNumbers: { autoNumber: "AMS" },
         extensions: ["AMSmath.js", "AMSsymbols.js"] }
  }
});
</script>
I define the movies watched and rated by a user as items. I refer to the union of the items and rating values as a user’s data. Consider a set of users $U$, their corresponding rating values $R$ and a set of items $M$. $M_u \subset M$ is the set of items watched by user $u \in U$ and $R_u$ their rating values. I denote the data of user $u$ by $H_c = M_c \cup R_c$. My model define as :
\begin{center} $f : (u, H_u) \to M \times [0, 1]$
\end{center} ,
where $u$ is a user, $H_u$ as inputs, and returns a probability distribution $P^M_u$ over the set of items $M$ corresponding to the probability of user $u$ watching them over the next period. For $t = [0,...,T]$, where $T$ is the length of the sequence, consider a sequence of inputs $H^t_u$. In order to estimate the likelihood of choosing items in $M$ during period $T + 1$, my model aims to learn a function $f: (u, H^0_u,..., H^T_u)$ that captures the sequential information. I just refer to the user's data sequence as their history to keep things simple. The input and output of function $f$ for the current dataset are shown the image below.
<p align="center"><img src="{{ site.baseurl }}/images/63.png" width="100%" height="100%"></p>


## Data Presentation
I express the time periods as words and the data for a particular period as letters because the task is comparable to one involving language modelling(BERT). The history created by $T$ sequences of data values can be represented as the input to the model as a sentence of $T$ words with a set number of letters in each word. I demonstrate how a word from a phrase is represented in the context of the current dataset in the image below.
<p align="center"><img src="{{ site.baseurl }}/images/64.png" width="100%" height="100%"></p>


## Transformer Encoder
I briefly explain the transformer encoder methodology, which is the main method in my model, and I provide a detailed illustration below.
<p align="center"><img src="{{ site.baseurl }}/images/65.png" width="100%" height="100%"></p>
*Trnasformer Layer, Scaled Dot-Product Attention and Multi- Head Attention consists of several attention layers running in parallel*
As I mentioned previously, a feed-forward neural network uses the embedding representation that the transformer encoder learns to forecast probabilities over the item set. The transformer encoder is defined as :
$E : (u, H^0_u, ..., H^T_u) \to e_u = (e^0_u, ..., e^D_u)$
, generates a representation $e_u$ of the history of user u in embedding space of dimension $D$. The feed-forward neural network defined as :
$n : (e_u) \to \sigma(M)$
is made up of a single layer with dimension $M$ and a softmax activation function, which takes the embedding $e_u$ as an input and produces a probability distribution over the items as an output. The output is defined as :
$\sigma(M) = softmax(We_u + b)$
, where $W$ and $b$ are a weight matrix and a bias vector respectively. Following the encoder layers is a feed-forward layer, which is then followed by a softmax layer. This final component is used to forecast the likelihood that each item will be chosen in the subsequent period $T + 1$. I mask the items that have already been chosen in the last period from $P^M_u$ because my objective is to anticipate which additional items users will select in addition to the ones they have already chosen.


## Experiment - Data preprocess & Model learning
1. Data preprocess
   To apply my model, I preprocess the original dataset. For each value in the movieId column, the column name needs to be changed. This dataset creation is produced by a pivot. Given that movieId can take one of 9066 potential values, this dataset should have a dimension of 9068. UserId and timestamp are two more columns. I decrease the dimension from 9066 to the previously mentioned 15, 43, and 57, with the exception of UserId and timestamp in two columns. As a result, for each experiment, the input data has the following shapes : $10251\times17, 10897\times45,$ and $10931\times59$.
<p align="center"><img src="{{ site.baseurl }}/images/66.png" width="100%" height="100%"></p>
2. Model learning
   A binary cross entropy loss function, Adam\cite{Adam} as the optimizer, and ReLU as the activation function are used in the model's learning process.
Users are divided into batches of $k = 256$ during training. The final probability distribution is $k \times dim(M)$, the encoder's output embedding space is $k \times dim(D)$, and the input of the encoder has form $k \times dim(T) \times dim(H)$. $T$ stands for the collection of timestamps, $H$ for the total number of items, $D$ for the embedding dimension, and $M$ for the total number of anticipated items.


## Result
1. Condition
   I experiment six cases. The hyper-parameter's state is as follows:
   \begin{table}[h]
%\begin{center}
\begin{tabular}{|c|c|c|c|c|c|c|c|}
\hline
          \multicolumn{1}{m{1.8cm}|}{}& 
          \multicolumn{1}{m{1.3cm}|}{batch size}  & \multicolumn{1}{m{1.3cm}|}{Head number}  & \multicolumn{1}{m{1.3cm}|}{trans former layer}  & \multicolumn{1}{m{1.3cm}|}{dimen sion}  & \multicolumn{1}{m{1.3cm}|}{dropout}      &
          \multicolumn{1}{m{1.3cm}|}{learning rate} & \multicolumn{1}{m{1.3cm}|}{rating scale}\\
\hline
\multicolumn{1}{m{1.8cm}|}{CASE1} & 256 & 2 & 2  & 56 & 0.2 & 0.001 & scale\\
\hline
\multicolumn{1}{m{1.8cm}|}{CASE2}  & 256 & 4 & 2 & 56 & 0.2 & 0.001 & scale\\
\hline
\multicolumn{1}{m{1.8cm}|}{CASE3}  & 256 & 7 & 2 & 56 & 0.2 & 0.001 & scale\\
\hline
\multicolumn{1}{m{1.8cm}|}{CASE4}  & 256 & 7 & 2 & 28 & 0.2 & 0.001 & scale\\
\hline
\multicolumn{1}{m{1.8cm}|}{CASE5}  & 256 & 7 & 2 & 70 & 0.2 & 0.001 & scale\\
\hline
\multicolumn{1}{m{1.8cm}|}{CASE6}  & 256 & 7 & 2 & 70 & 0.2 & 0.001 & no scale\\
\hline
\end{tabular}
\caption{Hyper-parameters in models}
%\end{center}
\end{table}





3. 
Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/


\bibliographystyle{plain}
\begin{thebibliography}{OOO}
\bibitem[47]{BERT}
	Jacob Devlin, Ming-Wei Chang, Kenton Lee and Kristina Toutanova, 
	{\it BERT:
Pre-training of Deep Bidirectional Transformers for Language Understanding},
	CoRR abs/1810.04805 (2018), 2018
