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
I define the movies watched and rated by a user as items. I refer to the union of the items and rating values as a user’s data. Consider a set of users U, their corresponding rating values R and a set of items M. Mu ⊂ M is the set of items watched by user u ∈ U and Ru their rating values. I denote the data of user u by Hc = Mc ∪ Rc. My model define as :

f : (u, Hu) → M × [0, 1]

, where u is a user, Hu as inputs, and returns a probability distribution PuM over the set of items M corresponding to the probability of user u watching them over the next period. For t = [0, ..., T ], where T is the length of the sequence, consider a sequence of inputs $H_u^t$. In order to estimate the likelihood of choosing items in M during period T + 1, my model aims to learn a function f : (u, Hu0, ..., HuT ) that captures the sequential information. I just refer to the user’s data sequence as their history to keep things simple. The input and output of function f for the current dataset are shown in Figure 3.2.


You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

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
