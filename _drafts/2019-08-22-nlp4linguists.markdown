layout: single
title:  "How to teach NLP to non-CS-majors in 2 weeks?"
date:   2019-06-23 17:00:47 -0400
categories: squib
tags: academia 
mathjax: false
toc: false
excerpt: "What I learned from teaching a 2-week course at ESSLLI 2019."
header:
    og_image: /assets/images/writing.png
---

I strongly believe that getting machines to understand natural language, if at all possible, will require much interdisciplinary collaboration. We do not know for sure that our models need to be biologically plausible or explicitly encode any linguistic structures, but no discipline has been able to solve all the problems on its own.

The problem is, of course, that true interdisciplinarity requires working knowledge of more than one field - at the time when we can barely keep up with the literature even within one narrow sub-field. And it's not about reading more literature, but about learning a different mindset, a different way to set and solve problems. Also, it requires acquiring a whole lot of technical knowledge that should have come from a large series of udnergraduate courses - all in your spare time. Somebody willing to do this must have dedication and time, as well as unusual willingness to look beyond the basic tenets of one's original discipline.

This year I came to ESSLLI to teach a 2-week introductory NLP courses aimed at theoretical linguists with basic Python skills. In week 1 my goal was to provide the minimal skill set for getting, processing, and experimenting with textual data with Python, as well as fundamentals of machine learning. In week 2 I aimed to cover the basic neural architectures and the latest issues in evaluation and creation of datasets. I assumed that the students would have only the basic Python skills from an online course (such as this excellent edX course that I started with myself).

Sounds doable, right?

Of course, the Murphy law did not fail to apply. Here are some things I learned the hard way.

## The pool of students is incredibly diverse.

ESSLLI is a very unique environment where you can meet literally anybody: I was preparing with the linguists in mind, but I found that my class had also physicists, mathematicians, philosophers (at least 2 of each). Some of the linguists also had much more coding experience than others. However, this probably would apply to any elective NLP or data science course. For example, I have just taught an introductory Data Science course aimed at business majors, and I had everybody from chemists to political science.  

This means two things for the lecturer:

* at any point you can safely assume that a part of the audience is either bored or confused.
* at any point you can get any kind of question, from something very basic to something you can't answer on the spot.

For the balance of boredom and confusion, and also to acknowledge the impossibility of attending to just thing for 90 minutes straight, my strategy was to split the class into a short lecture (~30-40 min out of 90) and a hands-on tutorial in Jupyter. I aimed the lectures at the confused part of the audience, during which the bored part will just dive into Jupyter and keep themselves occupied.

## It's not at all easy to really start from the basics

I assumed that the students would have taken an online Python course, which in my mind implied familiarity with functions and classes. 

What I should have realized is that object-oriented programming is not something that a beginner is going to really pick up and use after just a set of exercises. I know many non-CS researchers who rely on Python heavily for daily experimentation - and manage to do most of it procedurally, in Jupyter notebooks. They might write a few functions to load/save their data that they copy-paste routinely, but they're not going to get anywhere near classes. They really don't need it for what they do. 

What this means for an NLP course is that if your deep learning framework relies on relatively complex programming constructs like classes and decorators, your students will be stuck with programming before they can even start to get stuck with partial derivatives. I chose vanilla PyTorch, with the goal of showing the students all the building blocks of a simple feed-forward network. My code was heavily commented, and they were able to run it, but I honestly have no idea how much of it could possibly sink in. 

I still debate whether I should have just gone with fast.ai. If the goal is to just give people tools to run pre-packaged models, then it would definitely be achieved much more efficiently than what I did. But if the goal is, eventually, research, then awareness of all the individual components is certainly to be encouraged, and I am hoping that it helped to understand the whole concept of a neural net better than any lecture could. 

## 




One particular direction of this transfer: theoretical linguistics to NLP. 

Try to think of an NLP practitioner who would routinely keep up and cite recent linguistic and/or neuroscience publications. 