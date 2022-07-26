# Notes for Tree rooting

I think it is useful to categorize the methods available to root a tree into 3 categories. The first, and probably the
best, is including extra information when the tree is initially inferred. The second method category are methods which
assume a, possibly stochastic and varying, molecular clock. And the final method is to use some model of evolution which
is not time reversible.

A couple of notes about myself before I talk in more detail about these categories, and that is that I am not an expert
on the specific application of all the methods I will talk about. Some of these methods can look more like art than
science, and I have no ability to decide what is a good solution. The second note is that even though I have published a
rooting tool, I think I am fairly agnostic as to which method is the best. My opinion is that all the methods have
challenges and issues, including my method, and so there is no single best method.

"Outgroup rooting works best" - Ziheng Yang

## Information included during tree inference

This category of methods really has one entry: outgroups. If an outgroup is included during the initial tree inference,
then rooting the tree is quite simple, as the branch between the ingroup and the outgroup must be where the root is
located. Generally, this method performs well, as it is just an extension of classical phylogenetic inference, which is
well understood. 

However, there are plenty of challenges with this method. Indeed, the first question is: what is a good outgroup. It
turns out this is a difficult question. First, adding an outgroup increases the time required to infer a tree, as it
increases the amount of candidate trees. Furthermore, poorly chosen outgroups can actually degrade the ingroup
phylogeny [1] [2].

Anecdotally, the outgroup should be in a "Goldilocks zone", which is not too close (so that the outgroup doesn't get
mixed with the ingroup) and not too far away (which can influence the topology of the ingroup). Such an outgroup does
not exist for all clades, further complicating this method. However, despite these shortcomings outgrouping is usually
the best option when it is available. It is more of an art than a science, as outgroup selection is increasingly
becoming more complicated.

## Molecular Clock Methods

These methods either assume a molecular clock, or attempt to infer a clock in such a way that also gives us a root. The
simplest of these is midpoint rooting, which places the root in the center of path between the two taxa with the longest
distance. While simple, this method is very sensitive to error and violation of the clock assumption. For example if a
single branch has a relatively rapid rate of evolution, for even random reasons, the root will be "attracted" toward
this branch. 

A better method which can handle branches with clock rates which vary randomly is Minimal Ancestor Deviation (MAD)[3].
Here, the random variation in branch length due to varying clock rates is averaged out. This ability to cancel out the
variation in clock rates gives MAD very robust performance, even in the case of fairly large clock assumption
violations. However, if the rates are not uncorrelated, for example an entire clade has an accelerated rate of
evolution, then the root will be misplaced under MAD. Nonetheless, I consider MAD to be strictly better than midpoint
rooting.

The final group of clock methods is the class of molecular clock analysis. Unfortunately, I can't give these methods the
explanation they deserve, but in short these methods, like midpoint rooting, try to find the root by finding the center
of the longest path between taxa, but instead allow for the clock to vary by some model. A paper [4] analyzing the
performance of these methods suggest that as long as the clock model is the true model, then these methods perform well.
However, it is often the case that the clock model is not true.

## Rooting Non-Reversible Models

Non-reversible models infer a root by finding the root or topology which maximizes the likelihood under that model.
Examples of Non-reversible models include the UNREST[5] [8], the undated model of duplication, transfer and loss [6], and
gene tree distributions under the multi-species coalescent model [7]. 

Tools which implement these models specifically for rooting are not very common, however this is common. There is
RootDigger, which is my tool, which will root with the UNREST model is one of the first. Since this has been published,
other tools have been developed such as Quintet Rooting [7]. Other tools developed for other purposes can also be used
for this task however. Examples include IQ-TREE, and ALE [9].

These methods, and I say that as somebody who has published one, are hit or miss. The assumptions that they need to make
are quite bad. For example, for RootDigger the model needs to be non-reversible, but not so non-reversible that the tree
inference goes poorly. Quintet rooting can only root species trees. I would describe these methods as methods of last
resort. If there is any other method that will work, that should be preferred.

## Conclusion

Tree rooting is a black art. What method should be used to root a given phylogeny depends on the data/species at hand,
the question to be answered, and the tolerance for error. Due to this, it is difficult to make general statements about
which method is better for what. However, I hope that these notes have helped readers understand the trade-offs involved
in root placement. 

A final parting bit of advice though: a bit of common sense goes a long way. It seems, anecdotally, that the right root
is the one that seems right to a human. Because of this, it can be helpful to consult an expert in the clade of
interest and ask them instead of trying to find the "one true rooting method".

[1]: Outgroup misplacement and phylogenetic inaccuracy under a molecular clock--a simulation study. Holland, Penny &
Hendy

[2]:  Relationships among the major lineages of Dictyoptera: the effect of outgroup selection on dictyopteran tree
topology. Ware, Litman, Klass and Spearman

[3]: Phylogenetic rooting using minimal ancestor deviation. Tria, Landan and Dagan

[4]: Performance of Relaxed-Clock Methods in Estimating Evolutionary Divergence Times and Their Credibility Intervals.
Battistuzzi, Filipski, Hedges, and Kumar.

[5]: Estimating the pattern of nucleotide substitution. Yang.

[6]: https://github.com/BenoitMorel/GeneRax

[7]: Quintet Rooting: rooting species trees under the multi-species coalescent model. Yabatabaee, Sarker, Warnow.

[8]: https://github.com/computations/root_digger

[9]: https://github.com/ssolo/ALE
