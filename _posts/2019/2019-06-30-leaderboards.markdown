---
layout: single
title:  "How the Transformers broke NLP leaderboards"
date:   2019-06-30 22:00:47 -0400
categories: squib
tags: academia methodology 
mathjax: true
redirect_from: "/leaderboards/"
toc: true
excerpt: "With the huge Transformer-based models such as BERT, GPT-2, and XLNet, are we losing track of how the state-of-the-art performance is achieved?"
header:
    og_image: /assets/images/compete.png
---

<figure>
	<img src="/assets/images/compete.png">
</figure>

> This post summarizes some of the recent XLNet-prompted discussions on Twitter and offline. Idea credits go to Yoav Goldberg, Sam Bowman, Jason Weston, Alexis Conneau, Ted Pedersen, fellow members of Text Machine Lab, and many others. Any misconfiguration of those ideas is my own. 

A big reason why NLP is such an actively developed area is the leaderboards: they are the core of multiple shared tasks, benchmark systems like GLUE, and individual datasets such as SQUAD and AllenAI datasets. Leaderboards stimulate competitions between engineering teams, helping them to develop better and better models to tackle human language. 

Or do they? 

## So what's wrong with the leaderboards?

Typically a leaderboard for an NLP task X looks roughly as follows:

<div class="table-wrapper" markdown="block">

|---
| System | Citation | Performance 
|:-:|:-:|:-:
| System A | Smith et al. 2018 | **76.05** 
| System B | Li et al. 2018 | 75.85
| System C | Petrov et al. 2018 | 75.62  

</div>

This format is followed both by online leaderboards (such as the GLUE benchmark), and academic papers (when comparing the proposed model to the baselines). 

Now, the test performance of the model is far from the only thing that make it novel or even interesting, but it is the only thing that is in the leaderboard. Since DL is such a big zoo with different architectures, there is no standard way to present additional information such as model parameters and training data. In the papers, sometimes these details are in the methodology section, sometimes in the appendices, sometimes in the comments on github repo or nowhere at all. In an online leaderboard, the details of each system can only be retrieved from the link to the paper (if one is available), or by going through the code in the repository.  

In an increasingly busy world, how many of us actually look for those details, unless we are reviewing or re-implementing? The simple leaderboard already gives us the information we most care about: who SOTA-ed. Generally, our minds are lazy and tend to receive such messages uncritically, ignoring any caveats even they are immediately present {% cite Kahneman_2013_Thinking_fast_and_slow %}. And if we have to actively hunt for the caveats... well, no chance. The winner receives all the Twitter hype, potentially [gaining unfair advantage in the blind review](https://medium.com/@ryancotterell/we-should-anonymize-model-names-during-peer-review-bcab0cc78946).

There has been a lot of discussion of the dangers of the SOTA-centric approach. If the reader's main takeaway is going to be the leaderboard, that increases the perception that the publication-worthiness is only achieved by beating the SOTA. That perception results in a flood of papers with marginal and often unreproducible performance gains {% cite Crane_2018_Questionable_Answers_in_Question_Answering_Research_Reproducibility_and_Variability_of_Published_Results %}. It also creates a huge problem for shared tasks, when non-winners feel like it's not even worth their while to write the paper on their work {% cite EscartinReijersEtAl_2017_Ethical_Considerations_in_NLP_Shared_Tasks %}, see also [the recent discussion of the issue by Ted Pedersen](https://medium.com/@tpederse/semeval-discussions-naacl-2019-4b73cd6734c0)). 

The focus of this post is yet another problem with the leaderboards that is relatively recent. Its cause is simple: fundamentally, **a model may be better than its competitors by building better representations from the available data - or it may simply use more data, and/or throw a deeper network at it**. When we have a paper presenting a new model that also uses more data/compute than its competitors, credit attribution becomes hard. 
 
The most popular NLP leaderboards are currently dominated by Transformer-based models. BERT {% cite DevlinChangEtAl_2019_BERT_Pre-training_of_Deep_Bidirectional_Transformers_for_Language_Understanding %} received the best paper award at NAACL 2019 after months of holding SOTA on many leaderboards. Now the hot topic is XLNet {% cite YangDaiEtAl_2019_XLNet_Generalized_Autoregressive_Pretraining_for_Language_Understanding %} that is said to overtake BERT on GLUE and some other benchmarks. Other Transformers include GPT-2 {% cite RadfordWuEtAl_2019_Language_models_are_unsupervised_multitask_learners %}, ERNIE {% cite ZhangHanEtAl_2019_ERNIE_Enhanced_Language_Representation_with_Informative_Entities %}, and the list is growing.

The problem we're starting to face is that these models are HUGE. While the source code is available, in reality it is beyond the means of an average lab to reproduce these results, or to produce anything comparable. For instance, XLNet is trained on 32B tokens, and the price of using 500 TPUs for 2 days is over $250,000. Even fine-tuning this model is getting expensive.

## Wait, this was supposed to happen!

On the one hand, this trend looks predictable, even inevitable: people with more resources *will* use more resources to get better performance. One could even argue that a huge model proves its scalability and fulfils the inherent promise of deep learning, i.e. being able to learn more complex patterns from more information. Nobody knows how much data we actually need to solve a given NLP task, but more should be better, and limiting data seems counter-productive. 

On that view - well, from now on top-tier NLP research is going to be something possible only for industry. Academics will have to somehow up their game, either by getting more grants or by collaborating with high-performance computing centers. They are also welcome to switch to analysis, building something on top of the industry-provided huge models, or making datasets. 

However, in terms of overall progress in NLP that might not be the best thing to do. Here is why.

## Why huge models + leaderboards = trouble 

The chief problem with the huge models is simply this: 

> **"More data & compute = SOTA" is NOT research news**. 

If leaderboards are to highlight the actual progress, we need to incentivize new architectures rather than teams outspending each other. Obviously, huge pretrained models are valuable, but unless the authors show that their system consistently behaves differently from its competition with comparable data & compute, it is not clear whether they are presenting a model or a resource.

Furthermore, much of this research is not reproducible: nobody is going to spend $250,000 just to repeat XLNet training. Given the fact that its ablation study showed only 1-2% gain over BERT in 3 datasets out of 4 {% cite YangDaiEtAl_2019_XLNet_Generalized_Autoregressive_Pretraining_for_Language_Understanding %}, we don't actually know for sure that its masking strategy is more successful than BERT's. 

At the same time, the development of leaner models is dis-incentivized, as their task is fundamentally harder and the leaderboard-oriented community only rewards the SOTA. That, in its turn, prices out of competitions academic teams, which will not result in students becoming better engineers when they graduate.

Last but not the least, huge DL models are often overparametrized {% cite FrankleCarbin_2019_Lottery_Ticket_Hypothesis_Finding_Sparse_Trainable_Neural_Networks WuFanEtAl_2019_Pay_Less_Attention_with_Lightweight_and_Dynamic_Convolutions %}. As an example, the smaller version of BERT achieves better scores on a number of syntax-testing experiments than the larger one {% cite Goldberg_2019_Assessing_BERTs_Syntactic_Abilities%}. The fact that DL models require a lot of compute is not necessarily a bad thing in itself, but *wasting* compute is not ideal for the environment {% cite StrubellGaneshEtAl_2019_Energy_and_Policy_Considerations_for_Deep_Learning_in_NLP %}.

## Possible solutions

NLP leaderboards are in real danger of turning into something where we give up on reproducibility and just watch one Google model outperform another Google model every couple of months. To avoid that, **the leaderboards need to change**. 

{::comment}
However, it is hard to do fairly, because DL architectures are such a big zoo: the impact of the training time, size and nature of the training data, number and impact of individual parameters may vary widely across architectures, so fixing any one of these may disadvantage a class of models. 
{:/}

In principle, there are two possible solutions:

1) For a specific task, it should be possible to **provide a standard training corpus, and limit the amount of compute to that used by a strong baseline**. If the baseline is itself something like BERT, this will incentivize the development of models that make better use of resources. If a system uses pre-trained representations (word embeddings, BERT, etc.), the size of pre-training data should be factored into the final score.

2) For a suite of tasks like GLUE, we could **let the participants use however much data&compute they wanted, but factor that into the final score**. The leaderboard itself should make it immediately clear what is the performance of a model over the baseline relative to the amount of resources it consumed. 

Both of these approaches require a reliable way to estimate the computation cost. At the minimum, it could be the inference time as estimated by the task organizers. Aleksandr Drozd (RIKEN CCS) suggests the best way is to just report the FLOPs count, which seems to be already possible for both [PyTorch](https://github.com/Lyken17/pytorch-OpCounter) and [TensorFlow](https://medium.com/@fanzongshaoxing/model-flops-measurement-in-tensorflow-a84084bbb3b5). Perhaps it would also be possible to build a general service for shared tasks that would receive a DL model, train it for one epoch on one batch of data, and provide the researchers with the estimate. 

Estimating the training data is also not straightforward: a plain text corpus should be worth less than an annotated corpus or Freebase. However, this should be possible to weigh. For example, unstructured data could be estimated as raw token count $$N$$, augmented/parsed data - as $$aN$$, and structured data such as dictionaries - as $$N^2$$.

One counter-argument to the above is that some models may inherently require more data than others, and can only be fairly evaluated in large-scale experiments. But even in this case, a convincing paper would need to show that the new model can "hold" more data than its competitors, and so multiple rounds of training all models on the same data are still necessary. 

## Summing up

This is the leaderboard discussion so far, and it's far from over. If you have anything to add, especially any other possible solutions - please let me know on Twitter or in the comments below. I'll update the post with any major developments.

Let me stress that huge pretrained models like BERT are an undeniable achievement, and did help to push the state-of-the-art on numerous tasks. Obviously, there is nothing wrong methodologically with *using* any `<muppetName>` as pretrained representations, as long as the paper is about something else and does not rest on any properties of `<muppetName>` that have not been fully validated. There is also nothing wrong with analysing `<muppetName>`: the steady stream of BERTology papers by itself suggests how little we understood about BERT while it was all over the leaderboards {% cite VoitaTalbotEtAl_2019_Analyzing_Multi-Head_Self-Attention_Specialized_Heads_Do_Heavy_Lifting_Rest_Can_Be_Pruned ClarkKhandelwalEtAl_2019_What_Does_BERT_Look_At_Analysis_of_BERTs_Attention CoenenReifEtAl_2019_Visualizing_and_Measuring_Geometry_of_BERT JawaharSagotEtAl_What_does_BERT_learn_about_structure_of_language LinTanEtAl_2019_Open_Sesame_Getting_Inside_BERTs_Linguistic_Knowledge %}.

But we do have a methodological problem if a paper introduces another `<muppetName>` without factoring in its stability and the resources it took to train vs competition, and then everybody takes the leaderboard performance as indicator of a breakthrough architecture.

Imagine that tomorrow we wake up to a paper presenting a Don't-Even-Try-Net (model name © Olga Kovaleva), a new architecture that achieves superhuman performance on every NLP task after being trained for a year on every computer in North America. Even with the source code we would not be able to verify that claim. We could use the pretrained weights, but without multiple runs for ablation and stability evaluation the authors would *not* have proven the superiority of their approach. In a sense, they would be presenting a resource rather than a model. 

If we are to make actual progress, we need to make sure new systems get fame and awards only with rigorous proofs - including the multiple runs of training on the same data as the baselines, ablation studies, estimates of compute and stability. This would inherently encourage more hypothesis-driven research. For instance, the dependency objective in XLNet looks really interesting, and I would love to know how much advantage it actually confers on different tasks, given that dependency-based word embeddings turned out to be of limited use {% cite LiLiuEtAl_2017_Investigating_Different_Syntactic_Context_Types_and_Context_Representations_for_Learning_Word_Embeddings LapesaEvert_2017_Large-scale_evaluation_of_dependency-based_DSMs_Are_they_worth_the_effort %}.

## Update of 22.07.2019

Oh wow, this post was retweeted over 100 times and [made it to Sebastian Ruder's NLP newsletter](http://newsletter.ruder.io/)! Clearly, the issue of fair evaluation of huge models resonates with the community deeply. 

Sebastian points out that Transformers make an important contribution in showing us the limitations of more-data-and-compute approach, and, ironically, also starting to encourage research on the leaner models. I fully agree with both points, and of course the Transformer in itself is an undeniable breakthrough. My point is simply that the current leaderboards implicitly encourage a blend of architectures, data and compute that are impossible to disentangle and replicate. If we are on a quest for the best possible NLP *model*, this is a problem we are going to have to solve.

Another update from a later discussion with Sam Bowman: leaderboards where you win by whatever combination of means do have a place in the world. Like Kaggle, they stimulate competition in ML engineering for NLP, and the results they showcase may in themselves be interesting and useful. But by themselves they are not a proof of architecture superiority, which they are commonly mistaken for. It seems to me that it would be the easiest for the most influential leaderboards such as GLUE to change so as to help to correct this perception, since all eyes are on them, but I can also see why they may want to remain Kaggle-style.

*Model training cost clarification*. the price of training XLNet was estimated as follows: the paper states that it was trained on 512 TPU v3 chips for 2.5 days, i.e. 60 hours. [Google on-demand price for TPU v-3](https://cloud.google.com/tpu/pricing) is currently $8, which amounts to $245,760 before fine-tuning. [James Bradbury points out](https://twitter.com/jekbradbury/status/1143397614093651969) that authors could actually mean "devices" or "cores", which would bring it down to $61,440 or $30,720, respectively. I would add that even in this most optimistic scenario the model would still cost [more than the stipend of the graduate student working on it](https://www.glassdoor.com/Salaries/phd-student-salary-SRCH_KO0,11.htm), and still be unrealistic for most labs. 

##  ***

{% include bib_footer.markdown %}

## Leave a comment (Twitter)

[https://twitter.com/annargrs/status/1152194347942731776](https://twitter.com/annargrs/status/1152194347942731776)
