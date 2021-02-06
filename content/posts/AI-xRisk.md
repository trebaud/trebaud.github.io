---
title: "AGI and existential risk"
date: 2023-10-19T05:56:32-04:00
draft: false
tags: [AGI]
---

"[The Creator](https://www.rottentomatoes.com/m/the_creator_2023)" is the latest AI blockbuster sensation to grace our screens this year. In it we are introduced to a future where AI robots and "simulants" are a more compassionate and peaceful version of their human counterparts and have to fight for their freedom and defeat American imperialism. Although the film is visually stunning with impressive photography and special effects, the storytelling is superficial, with poor character development and an unoriginal scenario imo. But even the best movies of the last decades on the topic (Her, Automata, Ex Machina) all seem to be missing something big in the way they describe our interactions with AGI.

Decades of bad Sci-Fi movies have conditioned our minds to think of AGI in terms of human archetypes. It is usually depicted as an anthropomorphized projection of the smartest human among us into silicon form. Be it as a cute Wall-E or a psychopathic Skynet, AI is something (or someone) we can more or less understand and maybe even reason with.

## Human cognitive biases

If we believe the [vulnerable world hypothesis](https://onlinelibrary.wiley.com/doi/full/10.1111/1758-5899.12718) described by Nick Bostrom we will at some point encounter a technological "black ball", that is to say a technology that will present an immediate and obvious existential threat to civilization. Humans are subject to a range of cognitive biases that make them bad at evaluating black swan events. Continuity or normalcy bias, ie. the belief that the short or medium term future won't be too different from the present, is strongly at play here. Moreover there is an ingrained [techno-optimism](https://a16z.com/the-techno-optimist-manifesto/) that implicitly rejects or downplays the vulnerability thesis.

So should we worry then? Is AGI likely to arrive any time soon? And if it does, is there really a problem?

[Metaculus](https://www.metaculus.com/questions/3479/date-weakly-general-ai-is-publicly-known/) places the median of arrival of AGI at August 2026. Many experts in the field from the likes of Geoffrey Hinton, Yoshua Bengio, Stuart Russel or Eliezer Yudkowsky have all expressed deep concern with the current rate of progress in AI. This progress is being led primarily by transformer models, neural network architectures that were originally developed as a way to improve performance on natural language translation tasks. Fast forward a few years and these models have now been scaled to billions of parameters and are exhibiting impressive and unforeseen emergent behaviours.


It is useful when approaching unintuitive subjects like these, to try and think from first principles and avoid some obvious cognitive biases as much as possible. In one of her articles, macroeconomist [Lynn Alden](https://www.lynalden.com/) describes the difference between intelligence and rationality. She equates intelligence to computing power, but rationality to the ability of avoiding reasoning biases. Think like the rational investor, don't assume you can predict what is going to happen based on what feels familiar and intuitive, instead pay attention to the logical structure of an argument and from that try to derive some sense about the risk constraints of a possible future event.

In order to avoid lengthy philosophical regressions I will use the following definition of intelligence: property of a system that maximizes the value over the expected future of some measure of utility. As an important corollary of this definition, intelligence is associated with the ability to map actions to goals, the more intelligent you are as an agent the better you are at planning your next moves to get to a desired set of outcomes. This is the property that makes an AI potentially dangerous. But what about self awareness, consciousness and qualia? Sufficiently intelligent systems might possess all of those but as far as I know it is currently not possible to know and is probably irrelevant for our current discussion. Superintelligent agents are dangerous because they are really good (possibly many orders of magnitude better than humans) at optimizing actions to goals mapping.

Most people think it is obvious that AGI will align itself with human values by default. Like in "The Creator", the robots and simulants look just like humans and display very human-like traits and behaviours. We are never given an explanation during the movie as to how this happens, it is not even a central component of the plot. There are many good reasons to believe it is highly unlikely that this will happen.

## AGI motivation and alignment

To understand why alignment work is necessary we can refer to two pieces of contemporary reasoning on AI motivation, although there are other lines of reasoning that lead to this conclusion. The first one is often referred to as the "orthogonality thesis" and is described by Nick Bostrom in this very good [paper](https://nickbostrom.com/superintelligentwill.pdf) as the following:

> Intelligence and final goals are orthogonal axes along which possible agents can freely
vary. In other words, more or less any level of intelligence could in principle be
combined with more or less any final goal. 

> The orthogonality thesis suggests that we cannot assume that a superintelligence will
necessarily share any of the final values stereotypically associated with wisdom and intellectual
development in humans—scientific curiosity, benevolent concern for others, spiritual
enlightenment and contemplation, renunciation of material acquisitiveness, a taste for refined
culture or for the simple pleasures in life, humility and selflessness, and so forth

Many people find it hard to accept this thesis. Intuitively it feels as though a sufficiently advanced intelligence will naturally converge to final goals that are similar to those of humans. [Paperclip maximizer](https://terbium.io/2020/05/paperclip-maximizer/) goals are seen as "stupid" and pointless from our reference point, hence incompatible with high intelligence.

The second piece of reasoning is what is known as "instrumental convergence", an obtuse term at first sight but actually pretty intuitive to understand. It just means that the AI will converge to a set of "instrumental" goals which will help it achieve its final goal. Those might be things like resource acquisition or goal preservation. To quote Bostrom again:

> An agent with such a final goal (calculate the digits of pi) would have a convergent instrumental reason,
in many situations, to acquire an unlimited amount of physical resources and, if possible, to eliminate potential threats to itself and its goal system.

Now let's come back quickly to the alignment problem which we should try to solve in one form or another in order to control these systems. Why is it hard to solve in principle?

Without getting too technical, one of the first issues is that even among humans we are not capable of coming to a consensus as to what constitutes a good set of moral principles we can ground ourselves upon. Although the fact that we all share a similar evolutionary past made it easier for stable social rules to have emerged. The second problem is that even if we came up with a set of rules we could all agree upon, like Asimov's famous three laws of robotics, how do we transmit them to the agent in a safe and reliable way? This is sometimes referred to as the value loading problem, in short how to encode these principles into an agent evolving in a complex and unpredictable environment?

One potential way to do that and which is deployed heavily in most LLM systems today is through the use of reinforcement learning techniques, like reinforcement learning from human feedback (RLHF). The agent receives a reward signal as it interacts with the environment, and tries to maximize it. Perhaps we could reward a reinforcement learner for aligning with our values, and it could learn and respect them. Will that strategy continue to work well as systems are further scaled? And also how to operationalize such a task in detail?

Another way would be to use some form of associative value accretion: have the AI acquire values in the way that humans appear to, starting out with some internal structure for synthesizing appropriate new values as we interact with our environments.

Besides the value loading problem there are other properties that we would want an AGI to possess like corrigibility, the willingness of the agent to be course corrected, or interpretability, the possibility for humans to figure out what's really happening internally with the system and know for example when the AI is trying to deceive us.

Needless to say this is an active area of research, with probably decades of human speed progress still ahead.

## Governance risk

Finally the most difficult challenges to solve might not even be technical in nature but instead related to governance or human coordination problems. Let's say we solve the alignment problem in time, who is to say that this knowledge won't be used by some malicious or greedy actor to bypass all controls and create an unaligned agent? This is even more likely to occur if we continue on the open sourcing trajectory. Llamas and AutoGPTs are currently not strong enough to do real damage but we should not expect this to remain the case for long. Open sourcing this kind of tech is a bad idea since it increases the number of participants in the race and makes it harder to regulate AI safety measures.

Most experts now evaluate their [p(doom)](https://forum.effectivealtruism.org/posts/THogLaytmj3n8oGbD/p-doom-or-agi-is-high-why-the-default-outcome-of-agi-is-doom) at a non trivial number greater than 10%. To equate it to something close to 0% is just a lack of epistemic humility imo. Even leadership from the big research labs at OpenAI, Anthropic or DeepMind have all made public statements that seem to indicate they are worried to some degree and feel some form of regulation is desirable (although some form of regulatory capture may be at play here). The Marc Andreessens of the world are clearly relying on some form of ideological reasoning.

It's interesting to note though that some companies don't seem that worried. A good example is Meta. They "leaked" their Llama models probably in a desperate attempt to stay relevant, without any kind of consideration in regards to the impact of open sourcing models and without any safety measures. Their leadership doesn't seem to care and arrogantly denies any sort of short or long term existential risk, acting in a way that is honestly very reminiscent of the tobacco and oil industries.

Aligning humans with each other on how to use AGI tech might end up being an even more intractable task. [Connor Leahy](https://www.express.co.uk/comment/expresscomment/1816498/Ai-anthropic-ai-artificial-intelligence-definition-will-ai-kill-us) offers some potential solutions here, they all boil down to boring government control, treaties on computing hardware and processes between states and enterprise to reduce reaction time in the case of an unexpected event. Boring is probably what it will take, not everything can be solved by tech.

## Closing thoughts

We can derive the following inference chain from the previous points:

```
orthogonality thesis is true +
instrumental convergence is true +
alignment problem is hard =

AGI poses an existential risk (or p(doom) >> 0)
```

If you don't like the conclusion then with what premise do you disagree with? Or maybe you think the conclusion doesn't follow from the premises, or that some implicit premises are missing. If you don't find anything wrong with the argument in principle but are still unable to accept the conclusion, try to identify the cognitive biases at play with your line of thinking: normalcy bias, optimistic bias, sunk cost fallacy, etc.

AI safety is a vast and fascinating field and there are many topics I didn't cover here like capability control, kinetics of capabilities development (FOOM or not FOOM), multipolar scenarios (refer to "Superintelligence" by Nick Bostrom for a more in depth discussion on these topics) or outer and inner alignment (most of what we talked about here could be considered as outer alignment).

AGI might still be decades away and might require more than just scaling of the current transformer architectures, [here](https://arxiv.org/abs/2305.18654) is a good paper that explores limits of current systems. [Bill Gates](https://multiplatform.ai/bill-gates-suggests-that-generative-ai-may-have-reached-a-plateau-despite-openais-optimism-about-gpt-5/) is already anticipating a plateau for generative AI and doesn't think next gpt iterations in the next 5 years will bring the same kind of breakthroughs that we saw this year. But again at the risk of sounding repetitive, we don't need to get to "real" AGI, whatever that means, to get to powerful actions to goals optimizers capable of real damage.

It is only a matter of time untill we get to general powerful optimizers and it is entirely possible that building AGI safely is the equivalent of trying to build a perpetual motion machine in the informational realm. Some people will find this idea absurd, but impossibility results exist everywhere. Something like the 2nd law of thermodynamics might prevent a lesser optimizer from achieving alignment with a more powerful one.
