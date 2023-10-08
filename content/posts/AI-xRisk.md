---
title: "AGI and existential risk"
date: 2023-09-30T00:00:00-0:00
draft: false
tags: [AI]
---

"The Creator" is the latest AI blockbuster sensation to grace our screens this year. In it we are introduced to a future where AI robots and "simulants" are a more compassionate and peaceful version of their human counterparts and have to fight for their freedom and defeat american imperialism. Although the film is visually stunning with impressive photography and special effects, the story telling is superficial, with poor character development and an unoriginal scenario. But even the best movies of the last decades on this topic (Her, Automata, Ex Machina) all seem to be missing something big in the way they describe our encounter with AGI.

Decades of bad Sci-Fi movies have conditionned our minds to think of AI in terms of human archetypes. It is usually depicted as an anthropomorphized projection of the smartest human among us into silicon form. Be it as a cute Wall-E or a psychopathic Skynet, AI is something (or someone) we can more or less understand and maybe even reason with. But in reality its behaviour will probably break our intuitions and look more alien than anything else.

If we believe the "vulnerable world hypothesis" described by Nick Bostrom we will at some point encounter a technological black ball that will present an immediate and obvious existential threat. Humans are subject to a range of cognitive biases that make them bad at evaluating black swan events, continuity or normalcy biases are strongly at play here, ie. the belief that the future won't be too different from the present or the past. This is obviously false when you zoom out in history, be it to 300 years ago before the industrial revolution, 100,000 years before the emergence of homo sapiens, or even 20 years ago before the internet. But our comfy lives specially in Western societies which were enabled by technological progress have strongly conditionned us to only expect good things from the future.

Many experts in the field from the likes of Geoffrey Hinton, Yoshua Bengio, Stuart Russel or Eliezer Yudkowsky have all expressed deep concern with the current rate of progress in AI. This progress is being lead primarily by transformer models, neural network architectures that were originally developed primarily as a way to improve performance on natural language translation tasks. Fast forward a few years and these models have now been scaled to billions of parameters and are exibiting impressive and unforseen emergent behaviours.

## Should we worry then?

It is useful I believe when approaching unintuitive subjects like these, to try and think from first principles and avoid cognitive biases as much as possible. In one of her blog articles, macroeconomist Lynn Alden describes the difference between intelligence and rationality, intelligence is sheer computing power, but rationality is the ability to avoid reasoning or cognitive biases. Think like the rational investor, don't assume you can predict what is going to happen based on what feels familiar and intuitive, instead pay attention to the logical structure of an argument and from that try to derive some sense of the risk constraints of a possible future event.

In order to avoid lengthy philosophical regressions I will use the following definition of intelligence: property of a system that maximizes the value over the expected future of some measure of utility. As an important corrolary of this definition, intelligence is associated with the ability to map actions to goals, the more intelligent you are as an agent the better you are at planning your next moves to get to a desired set of outcomes. That is the property of a superintelligent system that is really dangerous. But what about self awareness, consciousness and qualia? Sufficiently intelligent systems might possess all of those but as far as I know it is currently not possible of knowing and is irrelevant for our current discussion. Superintelligent agents are dangerous because they are really good (possibly many orders of magnitude better than humans) at optimizing actions to goals mapping.

Many people think it is obvious that AGI will align itself with human values by default. Like in "The Creator", the robots and simulants look just like humans and display very human like traits and behaviours. We are never given an explanation during the movie as to how did this happen, it is not even a central component of the plot.

To understand why alignment work is necessary we can refer to two pieces of contemporary reasoning on AI motivation. The first one is the "orthogonality thesis" and is described by Nick Bostrom as the following:

```
The orthogonality thesis suggests that we cannot assume that a superintelligence will
necessarily share any of the final values stereotypically associated with wisdom and intellectual
development in humans—scientific curiosity, benevolent concern for others, spiritual
enlightenment and contemplation, renunciation of material acquisitiveness, a taste for refined
culture or for the simple pleasures in life, humility and selflessness, and so forth
```

Many people find it hard to accept this thesis. Intuitively it feels as though a sufficiently advanced intelligence will naturally converge to final goals that are similar to those of humans. "Paperclip maximizer" goals are seen as "stupid" and pointless from our reference point, hence incompatible with high intelligence.

The second piece of reasoning is what is known as "instrumental convergence", an obtuse term at first sight but actually pretty intuitive to understand. It just means that the AI will converge to a set of "instrumental" goals which will help it achieve its final goal. Those might be things like resource acquisition or goal preservation. Humans do the same thing, if our final goal is self reproduction (for most of us it is, even if unconsciously) then good instrumental goals coul be self preservation (ie. not dying) and resource acquisition (ie. acquiring money and social status in order to better raise kids). To quote Nick Bostrom again:

```
An agent with such a final goal (calculate the digits of pi) would have a convergent instrumental reason,
in many situations, to acquire an unlimited amount of physical resources and, if possible, to eliminate potential threats to itself and its goal system.
```

Now let's come back quickly to the alignment problem which we should try to solve in one form or another in order to control these systems. Why is it hard in principle?

Without getting into too many technical details, one of the first issues is that even among humans we are not capable of coming to a consensus as to what constitutes a good set of moral principles we can ground ourselves on. Altough the fact that we all share a similar evolutionary past made it easier for stable social rules to have emerged. The second problem is that even if we came up with a set of rules we could all agree upon and give them to the AI, like Asimov's famous three laws of robotics, how do we do transmit them to the agent in a safe way? This is sometimes referred to as the value loading problem, in short how to encode these principles into an agent evolving in a complex and unpredictable environment. One potential way to do that and which is deployed heavily in most LLM system today is through reinforcement learning from human feedback. The agent receives a reward signal as it interacts with the environment, and tries to maximize it. Perhaps we could reward a reinforcement learner for aligning with our values, and it could learn them. Will that strategy continue working well as systems are further scaled? Another way would be to use some form of associative value accretion: have the AI acquire values in the way that humans appear to, starting out with some internal structure for synthesizing appropriate new values as we interact with our environments.

Besides the value loading problem there are other properties that we would want an AGI to possess like corrigibility, the willingness of the agent to be course corrected, or interpretability, the possibility for humans to figure out what's really happening internally with the system and know for example when the AI is trying to deceive us.

Needless to say this is an active area of research, with probably decades of progress still to make (at human speed).

Finally the most difficult problems might not even be technical in nature but instead governance or human coordination problems. Let's say by some miracle we solve the alignment problem in time, who is to say that this knowledge won't be used by some malicous or greedy actor to bypass all controls and create an unaligned agent? This is even more likely to occur if we continue on the open sourcing trajectory. Llamas and AutoGPTs are currently not strong enough to do real damage but we should not expect this to remain the case. Open sourcing this kind of tech is a bad idea since it increases the number of participants in the race and makes it harder to regulate AI safety measures.

The p(doom) is the term used by experts in the field to place some probability on the risk of extinction due to AI. Some evaluate it to close to 99% like E. Yudkowsky, others to 50% or 30% or even less than 1%. But to place it at close to or equal to 0% is just lack of epistemic humility. Research labs' leadership at OpenAI, Anthropic or DeepMind have all made public statements that seem to indicate they are worried to some degree and feel some form of regulation is desirable (although it more feels like regulatory capture more than anything else).

Some companies like Meta don't seem too worried though. They "leaked" their Llama models probably in a desperate attempt to stay relevant, without any kind of consideration in regards to the impact of open sourcing models and without any safety measures. And their leadership doesn't seem to care and denies any sort of short or long term existential risk, acting in a way that is honestly very reminiscent of the tobacco and oil industries. Meta's (or Facebook's) famous moto of "Move fast and break things" seems incredibly childish and cynical in this context.

To summarize the previous points we get the following inference chain:

```
orthogonality thesis is true +
instrumental convergence is true +
control/alignement problem is hard =

AGI poses an existential risk (or p(doom) >> 0)
```

If you don't like the conclusion then with what premise do you disagree with? Or maybe you think the conclusion doesn't follow from the premises, or that some there are some missing pieces to this argument. You are probably right. If you don't find anything wrong with the argument in principle but are still unable to accept the conclusion, try to identify the cognitve bias at play in your line of thinking: normalcy bias, optimistic bias, sunk cost fallacy, etc. 

AI safety is a vast and fascinating subject and there are many topics to cover like capability control, kinetics of capabilities development (FOOM or not?) or multipolar scenarios (refer to "SuperIntelligence" by Nick Bostrom for more in depth discussions of these topics). Also the difference between outer and inner alignment (most of what we talked about here could be considered as outer alignment).

It is possible real AGI is still decades away and that it will necessitate more than just scaling of the current transformer architectures, a good paper on this is Faith and Fate, limits of Transformers on Compositionality.
It is also plausible that alignment research will pan out some solid foundations on which to build safe AI soon or at least before any FOOM takes place.

But what about aligning humans on how to use these powerful systems? This might possibly be an even more difficult task. Conor Leahy offers some potential solutions, they all boil down to boring government control, treaties on computing hardware and processes between states and entreprise to reduce reaction time in the case of an unexpected event. Boring is probably what it will take, not everything can be solved by tech.

In the end it is possible all these worries will appear outdated as we merge more and more with machines. LLMs were trained on human acquired knowledge, so there is probably a very real sense in which they represent not a discontinuity but rather an evolution of the human condition. Although this is a bit easier to accept if our successors won't be trying to maximize the amount of some arbitrary physical shape for the next millenias.
