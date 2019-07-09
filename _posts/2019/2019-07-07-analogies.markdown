---
layout: single
title:  "On word analogies and negative results in NLP"
date:   2019-07-07 16:00:47
last_modified_at: 2019-07-07 16:00:47 
categories: squib
tags: academia methodology negresults review
mathjax: true
toc: true
excerpt: "Negative results are hard to publish, and even harder to make well-known. Even when the disproved result is something as pervasive as Mikolov's word analogies."
header:
    og_image: /assets/images/analogy-header.png
---

<figure>
	<img src="/assets/images/analogy-header.png">
	<!--figcaption> some figure </figcaption-->
</figure>

In real world, fake news spread faster than facts. People's attention is caught by sensational, exaggerated, clickbait-y messages like "5.23 million more immigrants are moving to the UK". Any subsequent fact-checking messages look less sensational and they will not reach as many people. Once the damage is done, it's done.

Thank God this never happens in academia. Right?

Wrong. 

Experts are as susceptible as the rest of the populace - see for example Daniel Kahneman's account of an author of a statistics textbook who readily went with stereotype rather than provided base rate information {% cite Kahneman_2013_Thinking_fast_and_slow %}. Maybe we - researchers - have it even worse, because we also have to publish-or-perish. The publication treadmill demands eye-catching, breakthrough results that can't possibly be produced at the required speed. We rarely have the problem of people deliberately faking results, but... how shall I put it... there isn't exactly an incentive to triple-check things before they land on Arxiv. If you happen to be right, you get to be the first to publish that, and if you're wrong - no shame in it, you can always revise.

The readers are not necessarily triple-checking either. For an academic publication it would require much more than a google search, so we rarely bother unless we're reviewing or replicating. The worst case scenario is when the shiny but hasty result also conforms to your own intuitions about how things should work - i.e. when you're told something you want to believe anyway.

This is exactly what happened to word analogies {% cite MikolovChenEtAl_2013_Efficient_estimation_of_word_representations_in_vector_space %}. Its [over 11K citations](https://scholar.google.com/citations?hl=en&user=oBu8kMMAAAAJ) are mostly due to the hugely popular word2vec architecture, but the idea of word analogies rode the same wave. A separate paper on "linguistic regularities" {% cite MikolovYihEtAl_2013_Linguistic_Regularities_in_Continuous_Space_Word_Representations %} currently has extra 2K citations.

These citations are not just something from 2013 either. Because it's so tempting to believe that language really works this way, the word analogies are still everywhere. Only in June 2019, I heard them mentioned in the first 10 minutes of a NAACL invited talk, in a word embeddings lecture in the CISS dialogue summer school, and all over Twitter. Seriously. It just soo makes sense that language relations are all neat and regular like this: 

<figure>
	<img src="/assets/images/analogy-mikolov.png" class="width70">
	<!--figcaption>some figure</figcaption-->
</figure>

Unfortunately, they are not, and it's actually been shown a while ago that the vector offset does not work as advertised.  

## All things wrong with word analogies

To the best of my knowledge, the first suspicions about vector offset arose when it didn't work for lexicographic relations {% cite KoperScheibleEtAl_2015_Multilingual_reliability_and_semantic_structure_of_continuous_word_spaces %}. Then the still-largest-to-date BATS dataset {% cite GladkovaDrozdEtAl_2016_Analogybased_detection_of_morphological_and_semantic_relations_with_word_embeddings_what_works_and_what_doesnt %} offered a large balanced sample of 40 relations, among which the vector offset worked well only on those that happened to be included in the original Google dataset.

<figure>
	<img src="/assets/images/analogy-bats2.png">
	<figcaption>When does the vector offset work? 40 relations from the BATS dataset</figcaption>
</figure>

A later study constructed a comparable (NOT google-translated) dataset for Japanese, which showed a similar pattern even for subcharacter-based embeddings {% cite KarpinskaLiEtAl_2018_Subcharacter_Information_in_Japanese_Embeddings_When_Is_It_Worth_It %}. Roughly concurrently with BATS, Australian experiments on 15 relations in English also showed difficulties with lexical semantics {% cite VylomovaRimmelEtAl_2016_Take_and_took_gaggle_and_goose_book_and_read_evaluating_utility_of_vector_differences_for_lexical_relation_learning %}).

So why doesn't it work, if language relations are so neat and regular? Well, it turns out that it wouldn't have worked in the first place if the 3 source words were not excluded from the set of possible answers. In the original formulation, the solution to $$king-man+woman$$ should be $$queen$$, given that the vectors $$king$$, $$man$$ and $$woman$$ are excluded from the set of possible answers. Tal Linzen showed that for some relations you get considerable accuracy by simply getting the nearest neighbor of $$woman$$ word, or the one most similar to both $$woman$$ and $$king$$ (without $$man$$) {% cite Linzen_2016_Issues_in_evaluating_semantic_spaces_using_word_analogies %}. And here's what happens if you don't exclude any of them {% cite RogersDrozdEtAl_2017_Too_Many_Problems_of_Analogical_Reasoning_with_Word_Vectors %}:

<figure>
	<img src="/assets/images/analogy-honest.png" class="width60"> 
	<figcaption>Share of BATS analogy questions in which the vector the closest to the predicted vector is one of the source vectors (a,a', b), the target vector b', or some other vector. In most cases the result is simply the vector b ("woman").
	</figcaption>
</figure>

If in most cases the predicted vector is the closest to the source $$woman$$ vector, it means that the vector offset is simply too small to induce a meaning shift on its own. And that means that adding it will not get you somewhere significantly different. Which means you're staying in the neighborhood of the original vectors.   

Here are the experiments showing that if the source vectors $$a$$ ("man"), $$a'$$ (king), and $$b$$ ("woman") are excluded, your likelihood to succeed depends on how close the correct answer is to the source words:

<figure>
	<img src="/assets/images/analogy-sim-bias.png">
	<figcaption>The share of BATS analogy questions predicted successfully vs similarity of the target vector to the source vectors</figcaption>
</figure>

Does that sound like linguistic relations are reliably nice and regular, and vector offset lets us reliably control how we move in the semantic space? 

To sum up: if your formulation of vector offset excludes the source vectors, it will *appear* to work for the small original dataset, where its success is largerly explainable by basic cosine similarity. But it will fail to generalize to a larger set of linguistic relations. And no, we cannot just discard most of language as "irregular" or "suffering from polysemy", because the goal is to be able to perform verbal reasoning, and that's what natural language is like.

## The impact on further research

So much for the "linguistic regularities". But the problem is not just that the vector offset doesn't work as advertised. The focus of this post is the fact that the negative results memo never reached the same audience of thousands of researchers who were impressed by the original Mikolov's paper. 

Obviously, I'm impartial here because some of this work is mine, but isn't it just counter-productive for the field in general? If there are serious updates to a widely cited but too-good-to-be-true paper, it is in everybody's interest for those updates to travel fast - because they could save people the effort of either doing the same work again, or the wasted effort of building on the original untested assumption. Right?

Well, it's perhaps not coincidental that only one of all the above negative-results papers by 4 teams even made it to one of the main conferences. However, there are two ACL, one COLING, and one best-paper-mention ICML paper providing mathematical proofs of why the vector offset *should* work: {% cite GittensAchlioptasEtAl_2017_SkipGram_Zipf_Uniform_Vector_Additivity HakamiHayashiEtAl_2018_Why_does_PairDiff_work EthayarajhDuvenaudEtAl_2019_Towards_Understanding_Linear_Word_Analogies AllenHospedales_2019_Analogies_Explained_Towards_Understanding_Word_Embeddings %}. Go figure. Only one paper also took a quantitative perspective, but bravely arrived at the opposite conclusion {% cite Schluter_2018_Word_Analogy_Testing_Caveat %}. 

Don't take me wrong, I'm not out to shame anybody. First, research inherently involves mistakes, and I'm making them daily myself. Second, the papers offering proofs for vector offset are serious work aiming to establish important facts, and I'm sure the authors would not ignore the relevant negative evidence if they had been aware of it. And that is the general problem with low visibility of negative results that is not unique to NLP.

Btw, if any of the authors of the above papers on proofs for vector offset are reading this - I would actually be very happy to be proved wrong. Vector offset is computationally cheap and conceptually simple, it would be fantastic for the whole field if it was a reliable tool with which we could perform actual meaning shifts across a wide range of linguistic relations. But so far the negative evidence stands unchallenged.
 
An important implication of the vector offset problem is that analogies have attracted the attention of researchers on fairness/bias, where many practitioners use it in earnest. For example, here's a NIPS paper that started from accepting that the underlying vector offset mechanism works: {% cite BolukbasiChangEtAl_2016_Man_is_to_Computer_Programmer_As_Woman_is_to_Homemaker_Debiasing_Word_Embeddings %}. But this one didn't: {% cite NissimvanNoordEtAl_2019_Fair_is_Better_than_SensationalMan_is_to_Doctor_as_Woman_is_to_Doctor %}. Let me quote its authors on the impact of the blind use of the vector offset:

<figure>
	<img src="/assets/images/analogy-nissim.png">
</figure>


Analogical reasoning is an incredibly important aspect of human reasoning, and we *have* to get it right if we're ever to arrive at general AI. From what I've seen, linear vector offsets in word embeddings are not the right way to think of it. But there are plenty of other directions, including better methods for analogical reasoning {% cite DrozdGladkovaEtAl_2016_Word_embeddings_analogies_and_machine_learning_beyond_king_man_woman_queen VineGevaEtAl_2018_Unsupervised_Mining_of_Analogical_Frames_by_Constraint_Satisfaction BouraouiJameelEtAl_2018_Relation_Induction_in_Word_Embeddings_Revisited DufterSchutze_2019_Analytical_Methods_for_Interpretable_Ultradense_Word_Embeddings %} and specialized representations for analogous pairs {% cite WashioKato_2018_Neural_Latent_Relational_Analysis_to_Capture_Lexical_Semantic_Relations_in_a_Vector_Space JoshiChoiEtAl_2018_pair2vec_Compositional_Word-Pair_Embeddings_for_Cross-Sentence_Inference HakamiBollegala_2019_Learning_Relation_Representations_from_Word_Representations Camacho-ColladosEspinosa-AnkeEtAl_2019_Relational_Word_Embeddings %}. If we're not married to the ideal of natural language with impossibly regular relations, shouldn't we try to maximize the effort in those more promising directions?

## How we can encourage factchecking of widespread assumptions

The problem with vector offset is not unique. Its components are (1) a shiny result that is intuitively appealing and becomes too-famous-to-be-questioned, (2) the low visibility of negative results, even when they are available. In NLP, the latter problem is aggravated by the insane Arxiv pace. When you work on "a truth universally accepted", and you can't even keep up with the list of papers that you *want* to read, why would you bother searching for papers nobody cited?

It is admittedly hard to make negative results sexy, but in high-profile cases I think it is doable. Why don't we have an **impactful-negative-result award category at ACL conferences**, to encourage fact-checking of at least the most widely-accepted assumptions? This would:

* **increase the awareness of widespread problems, so that people do not build on shaky assumptions;**
* **identify high-profile research directions where more hands are needed next year, thus stimulating the overall progress in NLP;**
* **help with reproducibility crisis by encouraging replication studies and reporting of negative results.**

For example, in NAACL 2019 there were several interesting papers that could definitely be considered for such an award. A few personal favorites: 

* exposing the lack of transfer between QA datasets {% cite Yatskar_2019_Qualitative_Comparison_of_CoQA_SQuAD_20_and_QuAC %}, 
* limitations of attention as "explaining" mechanism {% cite JainWallace_2019_Attention_is_not_Explanation%},
* multimodal QA systems that work better by simply ignoring some of the input modalities {% cite ThomasonGordonEtAl_2019_Shifting_Baseline_Single_Modality_Performance_on_Visual_Navigation_QA %}. 

2 out of 3 of these great papers were posters, and I can not imagine how many more did not even make it through review. I would argue that it sends a message to the community, and it is the wrong message.

More visibility for impactful negative results, please. *Debunked assumption* + *more work building on it* != *progress*.

##  ***

{% include bib_footer.markdown %}

## Leave a comment (Twitter)

TBA
