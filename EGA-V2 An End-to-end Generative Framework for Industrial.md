::: {#page0 style="width:612.0pt;height:792.0pt"}
[arXiv:2505.17549v3 \[cs.IR\] 1 Jun
2025]

**[EGA-V2: An End-to-end Generative Framework for
Industrial]**

# Advertising**

[Zuowu
Zheng]^[∗]^[,
Ze
Wang]^[∗]^[,
Fan
Yang]^[†]^[,
Jiangke Fan, Teng Zhang, Yongkang Wang, Xingxing
Wang]

[Meituan, Shanghai,
China]

[{zhengzuowu,wangze18,yangfan129,jiangke.fan,zhangteng09,wangyongkang03,wangxingxing04}\@meituan.com]

# Abstract**

[Traditional online industrial advertising systems suffer from
the]

[limitations of multi-stage cascaded architectures, which often
dis-]

[card high-potential candidates prematurely and distribute
decision]

[logic across disconnected modules. While recent generative
recom-]

[mendation approaches provide end-to-end solutions, they fail
to]

[address critical advertising requirements of key components
for]

[real-world deployment, such as explicit bidding, creative
selection,]

[ad allocation, and payment
computation.]

[To bridge this gap, we introduce End-to-End Generative
Adver-]

[tising (EGA-V2), the first unified framework that holistically
models]

[user interests, point-of-interest (POI) and creative generation,
ad]

[allocation, and payment optimization within a single
generative]

[model. Our approach employs hierarchical tokenization and
multi-]

[token prediction to jointly generate POI recommendations and
ad]

[creatives, while a permutation-aware reward model and
token-level]

[bidding strategy ensure alignment with both user experiences
and]

[advertiser objectives. Additionally, we decouple allocation
from]

[payment using a differentiable ex-post regret minimization
mecha-]

[nism, guaranteeing approximate incentive compatibility at the
POI]

[level. Through extensive offline evaluations we demonstrate
that]

[EGA-V2 significantly outperforms traditional cascaded systems
in]

[both performance and practicality. Our results highlight its
poten-]

[tial as a pioneering fully generative advertising solution,
paving]

[the way for next-generation industrial ad
systems.]

**[CCS
Concepts]**

[•]**[
Information
systems]**[
→]**[Display
advertising]# .

# Keywords**

[Generative Advertising, Recommendation System, Preference
Align-]

[ment]

[∗][Equal
contribution.]

[†][Corresponding
author.]

[Permission to make digital or hard copies of all or part of this work
for personal
or]

[classroom use is granted without fee provided that copies are not made
or distributed]

[for profit or commercial advantage and that copies bear this notice and
the full
citation]

[on the first page. Copyrights for components of this work owned by
others than
the]

[author(s) must be honored. Abstracting with credit is permitted. To
copy otherwise,
or]

[republish, to post on servers or to redistribute to lists, requires
prior specific
permission]

[and/or a fee. Request permissions from
permissions\@acm.org.]

*[Woodstock '18, Woodstock,
NY]*

[©][ 2018
Copyright held by the owner/author(s). Publication rights licensed to
ACM.]

[ACM ISBN
978-1-4503-XXXX-X/2018/06]

[https://doi.org/XXXXXXX.XXXXXXX]

**[ACM Reference
Format:]**

[Zuowu
Zheng]^[∗]^[,
Ze
Wang]^[∗]^[,
Fan
Yang]^[†]^[,
Jiangke Fan, Teng Zhang,
Yongkang]

[Wang, Xingxing Wang. 2018. EGA-V2: An End-to-end Generative
Frame-]

[work for Industrial Advertising.
In]*[
Proceedings of Woodstock '18: ACM
Sym-]*

*[posium on Neural Gaze Detection (Woodstock
'18).]*[ ACM,
New York, NY,
USA,]

[10 pages.
https://doi.org/XXXXXXX.XXXXXXX]

# 1**

# Introduction**

[Recall]

[10]^[4]^

[10]^[3]^

[10]^[1]^

[Ad Corpus]

[\~10]^[10]^

![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAAEwAAABgCAIAAAA4mwMxAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAAKAklEQVR4nO1ce0yN/x+PXLvQ3SWqjU2SyyZLpSlZKyvKpTCXFKaN%0AqabCFrmEyKUsMorThdbawlyyNJeYhrlEIpJbmko3pNzO9zVtzz7nc855zjmf5zlH%0Av369/mLP87w/r9fzeX/el8/nOelJ/w+g968J6AI9IrsLekR2F/SI7C7oEdld0CNS%0AGP78+VNRUZGRkbF+/Xo/Pz97e3sTE5M+ffoYGBgMHTrUyckpODg4MTGxqKjo69ev%0A2qMh1Z7IvLw8qNJTD0ZGRiEhIeXl5VoiI75IcJ05c6aa8khgkuPi4n79+iU6JTFF%0Awj+TkpIGDBjAoJCDu7v7u3fvRGQlFVFkW1vbvHnzlFHv1avXkCFDsA6nT5/u4uIC%0ATzY2NlZ2s4WFxY0bN8QiJhVLZEtLC2ZAoQd6e3unp6dXVVX9/v2bfKSjo+POnTuI%0ASRAv/yCC0/nz50XhJhVFZG1t7cSJEymWCKTx8fFv3rxR+Xh7e3tBQYGHhwdmm7TQ%0Ar1+/tLQ04fSkoogMCAigFNra2j579kxTO9nZ2QMHDqScXCKRCGcoVGR+fj41A5Mm%0ATfr48SObtZycnN69e5PWkF2ExyFBIpubm62trUlODg4O9fX1Qmzu3r2b8ouoqCgh%0ABqUCRW7cuJFkY2pqigAjkBCwcuVK0uzgwYMR2IQYZBfZ2NiI4Uk2qHKEUOGAbERV%0AS3v37hVikF3k9u3bSR5LliwRwoPC3bt3EV0546h1v337xmyNUSSSno2NDUcCVQ5z%0AsFGG0NBQ8iWePn2a2RSjSLQXZFCdM2cOMwNlKCkpIUWGhYUxm2IUefz4cZIB/svM%0AQBlQqaO+44bAKmU2xSgyOjqaFKmlLsnX15cbAnXC9+/f2ewwipw7dy43fN++fYVE%0ABR5ERkaSr/L169dsdhhFotTkxjY3N2czohKJiYmkyIcPH7LZYRTp6urKjY02gs2I%0AShw8eJAUWVpaymaHUaSXlxc3NgodtMtsdviRkJBAiiwrK2OzwygyKCiIGxtNY2tr%0AK5sdfqxbt44bBRnr7du3bHYYRW7evFmU1cKPGTNmcEOgHfnx4webHUaRmZmZpMiU%0AlBQ2OzxAM03WxujLmU0xikQRh8zBMXBychJ9WZ45c4Z8jzExMcym2At0at/x4sWL%0AzKbkgdp4/Pjx5IJ88OABszV2kRkZGaTIKVOmiLhlmpOTQxp3dHQUYo1dJKocdEAk%0AleTkZCFUODQ0NAwfPpy0LLA2FrQzsG/fPpIKyst79+4JMSj9W5fPmjWLNItXiSAk%0AxKYgkRh77NixFKHKykpmg4he4eHherJAdy6EpFT4bt3NmzdRDJCcrK2tmUsTqljV%0A+1szwnsFkhRh33XLli0UM0NDw9TUVI3iUHV19YoVK/T19Uk7lpaWwv1fKopIhPvF%0AixfryQHx9sKFC9TpgDw+fPgQERHRv39/6nEzMzNmj6AgzllIR0eHQp3A6NGjUQMW%0AFRXB67iCAQUaCtHs7Gw/Pz95eYCVlZUoc9gJ0U61MGMoSqj9bxJwRQsLCxsbm2HD%0AhsGfqX13Evb29s+fPxeLmFT0Q9iCggIsJGXsVQLKly5dKnpPI/5Jc319/erVq8nK%0AVk2gHD916pTofKTa+2agoqIiNjZ28uTJ/Go7D2cXLlyYlZWlpaZUKlAk4g3CSV1d%0AHc89TU1NhYWF6PGXL1/u7e3t6urq4eExe/bstWvXHj169MWLF/ztC4aora1taWkR%0A0uVoLLK5ufnEiROLFi0aM2aMgYFB778AdShhJqEQeDvR0dHm5uaYbQQtxFs3Nzd4%0Ax61bt1SmJQoaiPzy5QsmhNzwpRxv/vz5NTU1GmpRjNzcXARhZU4+derU69evq29N%0AXZFnz56lOgOFMDU1xTwLca3379/DmVUOBPcJCgpS8yxUtUhUZ3AbnrQmDy8vL3W+%0AFpBHfn6+iYmJ+gONHDlSnWZahUi4KNX4dKIzKmIpBgYG2trayt8waNCgVatWYf7V%0AOT8tLy/fv38/lpzCWsLd3d3Hx2fcuHEKayPUFSoPvFSIXLNmDWUUKQFpEO+P80mE%0AomXLlimbagQn1G6HDh3CI3hl8As8+PPnT8RMxKqoqCgEMGUThQWCueXI4H1JJBJy%0AW6QTCEtohhhFlpaWUlkO+frq1asKb758+bKdnZ0yutwLgjcidPGXdXp/l1xoaGhj%0AY6P8QHhBKOip+52dnXkCgVKRqKEnTJhAGkJQefToEc9LwURhZhQ6lUZwcHBAQc8z%0AEPQgzVJPZWZmaiwyLS2NNAHqan4K9vLly7CwMDapKN/T09NRAKgcBW6PcEA+i2a9%0Ara1NA5Ht7e1UONm2bZs6CkmpcKoRI0aoow1u7Onpidyj0REgnJnKalj5Gojcs2cP%0A+TAYYCVoJLITKE0qKyuR2ePj49FeILW4uLigmUbA9Pf3Dw8PP3z4cHFxMUIXg3Hg%0A2rVr5OYLVvvnz5/lb1MgEt5CVhuw8uTJEzYSOgACOzkfSUlJ8vcoEHnu3DnyMW18%0A9CAinj59SmZXR0dH+TCrQCT1bQkSuk7YsgOlLMcWmUl+T5QWiVWEWol7BolRnVj3%0Ab5GcnEzOCtY5dQMtEhUWmabR++mKKjvQlPJ/VESLpI5xNmzYoCuq7MAiJHMJEia1%0ALGmRISEhpMicnBwdsmUH2UVgVqnNPlokin1SJGKXDqmyY9OmTSTtrKws8qqMSNSr%0AKJ25W1Hda29zSVwcO3aMFBkXF0delRFZU1ND3grn1i1Vdly6dIlkvmDBAvKqjEg0%0AGeSt06ZN0y1VdpSVlZHMUTySV2VEolckbw0ODtYtVXZQPojOlrwqIzIvL4+8NTIy%0AUrdU2YH2hazUjY2NyYNDGZFUkqSWb1cGQqaRkRHHHL0b+XNFGZFHjhwhRSYkJOic%0ALSNQjZqZmXHMkSqbmpq4qzIiU1JSSJEHDhzQOVt2UL/5Ig/hZURSn15iYnVOlR3k%0Ah//Ap0+fuEt8M5mamqpzquwgmye+mTx58uT/buAhf5DJF3iuXLlCivT19dU5W0a8%0AevWKZI5ZJa/KiKyuriZvxbvR0hf0oiM7O5tk7u/vT16VEYkESm3ykbv0XRnkrxsA%0ARFDyKt1qUTvTrq6uWvq+XETcv3+fPM7Av6kPuWmRaDepz8hiYmK6sk6sRip5UC2I%0AVOFu3datW/Vk4enpWVxcrOkhtrZRX1+/a9cuU1NTkqqVlZX89/gKRGLeFJ71jho1%0AKjY2trCwsKqqqq6uruFfAN3G48ePJRJJYGCg/B80gKMqPGZXfEzQ2trq7Owsr7Mr%0AQ19fH3leoRylp1otLS0+Pj7/mrm6wKzm5uYq08J3CIuMEhERwfO5XBeBpaUlyhge%0AIao/jCgpKQkICCB/fNt1gLi6Y8cOsqtiFNkJFPVZWVnIom5ububm5lSa0Q3QJRoa%0AGtrZ2SEu7ty58/bt22oeKPb8ZaXugh6R3QX/AUupL+Zcy9oxAAAAAElFTkSuQmCC)

[User]

[POI List]

[Creative List]

[Payment List]

[Ad Slot List]

[Pay Net]

[POI
Decoder]

[MTP]

[Creative
Decoder]

[User
Encoder]

[Pre-ranking]

[Ranking]

[Creative]

[Auction]

[10]^[2]^

[10]^[3]^

[10]^[1]^

**[(a) Multi-stage Cascading Advertising
System]**

**[(b) End-to-end Generative Advertising
System]**

**[Figure 1: (a) A typical cascade advertising system. (b)
Our]**

**[proposed unified architecture for end-to-end
generation.]**

[Online advertising has become a critical revenue engine
for]

[major internet platforms, directly supporting the
sustainability]

[of a wide range of digital services. Each user request initiates
a]

[complex decision process, where the platform must select,
rank,]

[and display a mixture of ads and organic content.
Traditionally,]

[industrial advertising systems have relied on multi-stage
cascading]

[architecture, which is typically structured as: recall,
pre-ranking,]

[ranking, creative selection, and auction respectively. As
shown]

[in Figure 1 (a), each stage selects the
top-]*[𝑘]*[candidates
from its]

[input and forwards them to the subsequent stage. This
paradigm]

[effectively balances performance and efficiency by
progressively]

[narrowing down the optimal set of candidate items under
strict]

[latency
constraints.]

[However, it still suffers from intrinsic limitations. Earlier
stages]

[in the cascade inherently constrain the performance upper
bound]

[of downstream modules
\[][4][\].
For example, ads filtered out in
up-]

[stream recall or ranking stages cannot be recovered, even if
they]

[would be highly valuable in the final allocation, resulting in
reduced]

[platform revenue and failure to achieve global optimality.
Despite]

[various efforts to enhance overall recommendation performance
by]

[promoting interaction between ranking modules
\[][11][,][
36][,][
39][\],
these]

[approaches continue to operate within the traditional
cascaded]

[ranking
framework.]

[The emergence of generative recommendation frameworks
of-]

[fers a new perspective on these longstanding challenges.
Recent]

[advances have demonstrated that transformer-based sequence
mod-]

[els can unify retrieval, ranking, and generation in an
end-to-end]
:::

::: {#page0 style="width:612.0pt;height:792.0pt"}
[Woodstock '18, June 03--05, 2018, Woodstock,
NY]

[Zheng et al.]

[manner, delivering personalized content and capturing deep
user-]

[item dependencies
\[][4][,][
20][,][
31][\].
However, while such
approaches]

[have shown remarkable promise in organic recommendation
sce-]

[narios, the complexity of industrial advertising presents a
unique]

[set of obstacles. Advertising systems must satisfy strict
business]

[constraints, including bidding, creative selection, ad slot
alloca-]

[tion, and payment rules, while optimizing both user and
platform]

[objectives. These requirements introduce complex
dependencies]

[and practical challenges that cannot be fully addressed by
directly]

[applying existing generative recommendation
models.]

[To address above challenges, we propose End-to-end
Generative]

[Advertising (EGA-V2), a novel generative framework that
unifies]

[all decision-making stages into a single model. EGA-V2 bridge
the]

[gap between generative modeling and the practical requirements
of]

[industrial advertising, which directly outputs the final ad
sequence,]

[as well as corresponding creatives, positions, and payment
end-to-]

[end.]*[
Firstly]*[,
inspired by generative recommendation techniques,
we]

[leverage Residual Quantized Variational AutoEncoder (RQ-VAE)
to]

[encode user behavior and item features into hierarchical
semantic]

[tokens, and employ an encoder-decoder architecture with
multi-]

[token prediction to jointly generate candidate ad sequences
and]

[creative
content.]*[
Secondly]*[,
we propose a token-level bidding
and]

[generative allocation mechanism, enhanced by
permutation-aware]

[reward modeling. This strategy decouples allocation from
payment,]

[allowing the model to effectively reflect business objectives
during]

[generation while approximately preserving incentive
compatibility]

[(IC) with a differentiable payment
network.]*[
Last but not
least]*[,
we]

[introduce a multi-phase training paradigm. A pre-training
phase]

[learns user interests from actual exposure sequences that
contain]

[ad and organic items. Then auction-based post-training is
applied]

[to fine-tune the model with ad-specific constraints,
dynamically]

[optimizing ad allocation based on auction signals and
platform]

[objectives.]

[Our contributions are as
follows:]

[•][ We introduce
EGA-V2, which surpasses traditional
multi-stage]

[cascading architectures by an unified single generative
model.]

[To the best of our knowledge, this is one of the first
end-to-end]

[generative advertising framework in
industry.]

[•][ We propose a novel
multi-phase training strategy
consisting]

[of interest-based pre-training and auction-based
post-training.]

[This design effectively balances user interests modeling
with]

[advertising-specific constraints, leverages diverse data
sources]

[during training, and aligns model outputs with final
platform]

[objectives.]

[•][ Extensive offline
experiments in large-scale industrial
datasets]

[demonstrate the effectiveness and efficiency of our
approach.]

# 2**

**[Related
Works]**

[To achieve end-to-end optimization in industrial advertising, it
is]

[crucial to unify user behavior modeling, auction mechanisms,
and]

[ad allocation strategies. Motivated by this integration, we
review]

[recent advances in each area in this
section.]

# 2.1**

**[Generative
Recommendation]**

[In recent years, there has been growing interest in applying
genera-]

[tive paradigms to recommendation systems. A particularly
promis-]

[ing direction is to formulate recommendation as a sequence
genera-]

[tion task, where user-item interactions are modeled using
transformer-]

[based autoregressive architectures. These methods aim to
deeply]

[capture user behavioral context and generate personalized
item]

[sequences in an end-to-end fashion \[3, 4, 20, 23, 24, 31,
34\].]

[Tiger
\[][20][\]
is a pioneering approach that introduces RQ-VAE
to]

[encode item content into hierarchical semantic IDs, allowing
knowl-]

[edge sharing across semantically similar items. Building upon
this,]

[COBRA
\[][31][\]
proposes a two-stage generation framework that
first]

[produces sparse IDs and then refines them into dense vectors,
en-]

[abling a coarse-to-fine retrieval process. Another stream of
research]

[explores general-purpose generative recommenders. HSTU
\[][34][\]]

[reformulates recommendation as a sequential transduction task
and]

[designs a Transformer architecture tailored for
high-cardinality,]

[non-stationary streaming data. OneRec
\[][4][\]
further advances this
by]

[unifying retrieval, ranking, and generation within a single
encoder-]

[decoder framework, while incorporating session-level
generation]

[and preference alignment strategies to enhance output quality.
Be-]

[sides, several methods focus on enhancing semantic
tokenization]

[and representation. LC-Rec
\[][40][\]
aligns semantic IDs with
collabora-]

[tive filtering signals via auxiliary objectives. IDGenRec
\[][22][\]
lever-]

[ages large language models to generate dense textual
identifiers,]

[showing strong generalization in zero-shot scenarios. SEATER
\[][21][\]]

[introduces tree-structured token spaces trained with
contrastive]

[and multi-task objectives to ensure consistency, while ColaRec
\[][26][\]]

[bridges content and interaction spaces for better
alignment.]

[While these works demonstrate strong general
recommendation]

[performance, they are insufficient for online advertising
systems,]

[which require additional modeling of bidding, payment, and
alloca-]

[tion constraints not captured in pure user interests
modeling.]

# 2.2**

**[Auction
Mechanism]**

[Traditional auction mechanisms such as GSP
\[][8][\]
and its
variants]

[like uGSP
\[][1][\]
are widely deployed in online advertising due to
their]

[simplicity, interpretability, and strong revenue guarantees.
How-]

[ever, they operate under the assumption of independence
among]

[ads, failing to account for externalities, that is, the influence
of]

[other ads \[9,
12\].]

[To address this, recent advances in computation have
motivated]

[the development of learning-based auction frameworks
\[][35][\].
For]

[example, DeepGSP
\[][37][\]
and DNA
\[][19][\]
extend classical auctions
by]

[incorporating online feedback into end-to-end learning
pipelines.]

[However, DNA suffers from the evaluation-before-ranking
dilemma,]

[where the rank score must be predicted before knowing the final
se-]

[quence, limiting its capacity to model set-level externalities.
Other]

[approaches attempt to integrate optimality into auction
design.]

[NMA
\[][14][\]
tackles this by exhaustively enumerating all
possible]

[allocations to ensure global optimality, but its computational
cost]

[renders it impractical for real-time applications. CGA
\[][41][\]
addresses]

[the limitations of traditional and learning-based ad auctions by
ex-]

[plicitly modeling permutation-level externalities through an
autore-]

[gressive allocation model and a gradient-friendly reformulation
of]
:::

::: {#page0 style="width:612.0pt;height:792.0pt"}
[EGA-V2: An End-to-end Generative Framework for Industrial
Advertising]

[Woodstock '18, June 03--05, 2018, Woodstock,
NY]

[incentive compatibility, enabling end-to-end optimization of
both]

[allocation and
payment.]

# 2.3**

**[Ad
Allocation]**

[Platforms initially assigned fixed positions to ads and organic
items,]

[but dynamic ad allocation strategies are now gaining attention
for]

[their potential to optimize overall page-level performance
\[][15][,][
27][--]

[30][,][
38][\].
CrossDQN
\[][16][\]
introduces a DQN architecture to
incor-]

[porate the arrangement signal into the allocation model
without]

[modifying the relative ranking of ads. HCA2E
\[][2][\]
proposes hi-]

[erarchically constrained adaptive ad exposure that possesses
the]

[desirable game-theoretical properties and computational
efficiency.]

[MIAA
\[][13][\]
presents a deep automated mechanism that integrates
ad]

[auction and allocation, which simultaneously decides the
ranking,]

[payment, and display position of the
ad.]

# 3**

# Preliminary**

[This section provides the necessary preliminaries for our
approach.]

[We define the core task in online advertising, and present the
auc-]

[tion mechanism design that connects interests modeling with
busi-]

[ness objectives. The main notations are summarized in Table
1.]

**[Table 1: Summary of
Notation]**

# Symbol**

# Description**

*[𝑢]*

[User]*[
𝑢]*

*[𝑋]*[=][
{]*[𝑥]*[1]*[,
. . .
,𝑥][𝑁]*[}]

[Candidate set
of]*[
𝑁]*[ads]

*[𝑂]*[=][
{]*[𝑜]*[1]*[,
. . .
,𝑜][𝑀]*[}]

[Candidate set
of]*[
𝑀]*[organic
contents]

[Y][
=][
(]*[𝑦]*[1]*[,
. . .
,𝑦][𝐾]*[)]

[Final ranked list
of]*[
𝐾]*[selected
items]

[S]^*[𝑢]*^[=][
{]*[𝑦]*[1]*[,
\...,𝑦][𝐵]*[}]

[User historical behaviors of
length]*[
𝐵]*

[V]

[Codebooks]

*[𝐶]*

[The number of codebook
layers]

*[𝑊]*

[Codebook size of each
layer]

[pCTR]*[𝑖]*

[Predicted click-through rate
of]*[
𝑖]*[-th
item]

*[𝑏][𝑖]*

[Bid value submitted
by]*[
𝑖]*[-th
advertiser]

*[𝑣][𝑖]*

[Private value
of]*[
𝑖]*[-th
advertiser]

*[𝑝][𝑖]*

[Payment charged
to]*[
𝑖]*[-th
advertiser]

*[𝑢][𝑖]*

[Utility of]*[
𝑖]*[-th
advertiser]

*[𝒃]*[−]*[𝑖]*

[Bids profile of ads
except]*[
𝑖]*[-th
ad]

[M⟨A]*[,]*[
P⟩]

[Auction
mechanism]

[Rev]

[Platform expected
revenue]

*[𝒆]*^*[𝑝𝑜𝑖]*^

*[𝑖]*

[,]*[
𝒆]*^*[𝑖𝑚𝑔]*^

*[𝑖]*

[POI and creative image embedding of
item]*[
𝑖]*

*[𝒂]*^*[𝑝𝑜𝑖]*^

*[𝑖]*

[,]*[
𝒂]*^*[𝑖𝑚𝑔]*^

*[𝑖]*

[POI and creative image token of
item]*[
𝑖]*

[F]*[,]*[
R]

[Pre-training
model][
F][ and reward
model][
R]

*[𝑃]*[(]*[𝒂][𝑖]*[)]

[Probability of generating
token]*[
𝒂][𝑖]*

[ˆ]*[𝑟]*

[Estimated reward by reward
model][
R]

# 3.1**

**[Task
Formulation]**

[We formalize a typical task of joint ad generation and allocation
in]

[online advertising systems. Given a page view (PV) request
from]

[user]*[
𝑢]*[, there
are]*[
𝑁]*[candidate
ads and]*[
𝑀]*[candidate
organic items]

[(non-sponsored). The organic
sequence]*[
𝑂]*[is
assumed to be
pre-]

[ranked by an upstream module based on estimated GMV, and
its]

[internal order will remain
fixed]^[1]^[.
The system selects a final
ranked]

[list of]*[
𝐾]*[items
(]*[𝐾]*[≪(]*[𝑁]*[+]*[
𝑀]*[)][)
to display:]

[Y][
=][
{]*[𝑦]*[1]*[,𝑦]*[2]*[,
. . .
,𝑦][𝐾]*[}]*[,]*

*[𝑦][𝑖]*[∈]*[𝑋]*[∪]*[𝑂,]*

[(1)]

[where the output
list][
Y][ is generated by
making the following
deci-]

[sions jointly under both user engagement and business
constraints:]

[•]**[ Ad
ranking:]**[
decide the permutation of selected ads
from]*[
𝑋]*[;]

[•]**[ Creative
selection:]**[
for each chosen ad, generate the
most]

[appropriate creative
image;]

[•]**[
Payment:]**[
compute the
payment]*[
𝑝][𝑖]*[for
each exposed ad
based]

[on its bid]*[
𝑏][𝑖]*[and
allocated
position;]

[•]**[ Ad
slot:]**[
determine the optimal display position for each
ad]

[when completing with organic
contents.]

[Each
advertiser]*[
𝑖]*[submits a
bid]*[
𝑏][𝑖]*[corresponding
to its private
click]

[value]*[
𝑣][𝑖]*[.
Our objective is maximize the expected platform
revenue]

[with
sequence][
Y][:]

[max]

*[𝜃]*

[E][Y][
\[][Rev][\]][
=][
max]

*[𝜃]*

[E][Y]

[
]*[𝐾]*

[∑︁]

*[𝑖]*[=][1]

*[𝑝][𝑖]*[·][
pCTR]*[𝑖]*

[!]

*[.]*

[(2)]

# 3.2**

**[Auction Mechanism
Design]**

*[3.2.1]*

*[Auction
Constraints.]*[
Unlike traditional recommender
sys-]

[tems, advertising platforms must not only optimize platform
rev-]

[enue but also ensure advertiser utility. Given an auction
mechanism]

[M⟨A]*[,]*[
P⟩][with allocation
rule][
A][ and payment
rule][
P][, the
expected]

[utility]*[
𝑢][𝑖]*[for
an
advertiser]*[
𝑖]*[is
defined as:]

*[𝑢][𝑖]*[(]*[𝑣][𝑖]*[;]*[𝒃]*[)][
=][
(]*[𝑣][𝑖]*[−]*[𝑝][𝑖]*[)
·][
pCTR]*[𝑖][,]*

[(3)]

[Two key economic constraints in auction mechanism must
be]

[satisfied in
Equation][
(2)][:
incentive compatibility (IC) and
individual]

[rationality (IR). IC requires that truthful bidding maximizes
the]

[advertiser's utility. For
ad]*[
𝑥][𝑖]*[,
it holds that]

*[𝑢][𝑖]*[(]*[𝑣][𝑖]*[;]*[𝑣][𝑖][,][
𝒃]*[−]*[𝑖]*[)
≥]*[𝑢][𝑖]*[(]*[𝑣][𝑖]*[;]*[𝑏][𝑖][,][
𝒃]*[−]*[𝑖]*[)]*[,]*

[∀]*[𝑣][𝑖][,𝑏][𝑖]*[∈][R]^[+]^*[.]*

[(4)]

[IR requires that no advertiser pays more than their bid, that
is,]

*[𝑝][𝑖]*[≤]*[𝑏][𝑖][,]*

[∀]*[𝑖]*[∈\[]*[𝑁]*[\]]*[.]*

[(5)]

*[3.2.2]*

*[Learning-based
Auction.]*[ To
ensure incentive
compatibility]

[(IC) in our model, we adopt the concept
of]*[ ex-post
regret]*[ \[7,
41\] to]

[quantify the potential gain an advertiser could obtain by
untruth-]

[fully reporting their bid. This formulation enables us to
enforce]

[IC constraints in a differentiable manner, suitable for
end-to-end]

[optimization.]

[Formally, given the generated
sequence][
Y][,
ad]*[
𝑥][𝑖]*[∈Y][
with]

[true
valuation]*[
𝑣][𝑖]*[and
contextual user
input]*[
𝑢]*[, the
ex-post regret
is]

[defined as:]

[rgt]*[𝑖]*[(]*[𝑣][𝑖][,]*[
Y]*[,𝑢]*[)][
=][
max]

*[𝑏]*^[′]^

*[𝑖]*

[]

*[𝑢][𝑖]*[(]*[𝑣][𝑖]*[;]*[𝑏]*^[′]^

*[𝑖]*^*[,]# \ 𝒃*[−]*[𝑖]# ,*[\ Y]*[,𝑢]*[)\ −]*[𝑢]# 𝑖*[(]*[𝑣]# 𝑖*[;]*[𝑏]# 𝑖# ,# \ 𝒃*[−]*[𝑖]# ,*[\ Y]*[,𝑢]*[)]^

[ ]

*[,]*

[(6)]

[where]*[
𝑏][𝑖]*[is
the truthful
bid,]*[
𝑏]*^[′]^

*[𝑖]*^[is\ a\ potential\ misreport,\ and]*[\ 𝒃]*[−]*[𝑖]*^

[represents bids excluding the
item]*[
𝑥][𝑖]*[.
The IC constraint is
satisfied]

[if and only
if][
rgt]*[𝑖]*[=][
0][ for all
advertisers. In practice, we
approximate]

[1][This
reflects common platform design constraints where organic content is
ranked]

[independently and must preserve user
experience.]
:::

::: {#page0 style="width:612.0pt;height:792.0pt"}
[Woodstock '18, June 03--05, 2018, Woodstock,
NY]

[Zheng et al.]

[Embedding Layer]

[POI Output Head]

[POI 32]

[P\_32]

[SEP]

**[User Behavior
Sequences]**

[Embedding Layer]

[Cross-Entropy
Loss]

[BOI]

[C\_32]

[P\_68]

[SEP]

[C\_68]

[\...]

[P\_92]

[P\_37]

[P\_16]

[P\_21]

[P\_92]

[P\_37]

[P\_16]

[P\_21]

[M]

[Creative Decoder
Block]

[Creative Output
Head]

[Embedding Layer]

[Cross-Entropy
Loss]

[BOC]

[BOC]

[BOC]

[BOC]

[C\_92 C\_37 C\_16
C\_21]

[Linear
Projection]

[RMSNorm]

[RMSNorm]

[User Encoder
Block]

[POI Decoder Block     
 ]

**[Next POI
Prediction]**

**[Next Creatvie
Prediction]**

[P\_32]

[SEP]

[C\_32]

[P\_68]

[SEP]

[C\_68]

[\...]

[P\_32]

[C\_32]

[Creative 32]

[Pretrained POI & Creative Quantization
Model]

# Main Module**

# MTP Module**

*[shared]*

*[shared]*

**[Figure 2: Overview of the interest-based pre-training architecture.
The pre-training model consists of encoder for
modeling]**

**[historical behavior sequences, followed by a two-stage decoder: a POI
decoder for next POI token prediction and a
creative]**

**[decoder for multi-modal next creative prediction. Both decoders are
trained jointly using cross-entropy loss in MTP
framework.]**

[this using]*[
𝑁][𝑣]*[sampled
valuations from
distribution][
F][, the
empirical]

[ex-post regret for
ad]*[
𝑥][𝑖]*[is]

[c]

[rgt]*[𝑖]*[=][
]^[1]^

*[𝑁][𝑣]*

*[𝑁][𝑣]*

[∑︁]

*[𝑗]*[=][1]

[rgt]*[𝑖]*[(]*[𝑣]*^*[𝑗]*^

*[𝑖]*^*[,]*[\ Y]*[,𝑢]*[)]*[.]*^

[(7)]

[We then formulate the auction design problem as minimizing
the]

[expected negative revenue under the constraint that the
empirical]

[ex-post regret remains zero for each
ad]*[
𝑥][𝑖]*[:]

[min]

*[𝒘]*

[−][E]*[𝒗]*[∼][F]

[h]

[Rev]^[M][i]^

*[,]*

[s.t.]

[c]

[rgt]*[𝑖]*[=][
0]*[,]*[
∀]*[𝑖]*[∈\[]*[𝑁]*[\]]*[,]*

[(8)]

# 4**

# Methodology**

[We introduce EGA-V2, an end-to-end generative advertising
frame-]

[work. This section describes how we learn user interests via
vector]

[tokenization and encoder-decoder design, leverage
permutation-]

[aware reward modeling for business objectives, incorporate
auction-]

[based preference alignment, and integrate all components
through]

[a unified multi-phase optimization
strategy.]

# 4.1**

**[Interest-based
Pre-training]**

[The goal of the pre-training stage is to capture user interests
based]

[on their full historical behaviors, including both ads and
organic]

[contents. This stage serves as the foundation for learning a
unified]

[representation that reflects the interests of the users.
Mathemat-]

[ically, the objective of the pre-training
model][
F][ is to generate
a]

[interest-aware output
sequence][
Y][ conditioned on the
input user]

[behavior
sequence][
S]^*[𝑢]*^[:]

[Y][
:][=][
F
(S]^*[𝑢]*^[)]*[.]*

[(9)]

*[4.1.1]*

*[Feature
Representations.]*[
We represent the user-side
context]

[using][
S]^*[𝑢]*^[=][
{]*[𝑦]*[1]*[,𝑦]*[2]*[,
. . .
,𝑦][𝐵]*[}][,
where
each]*[𝑦][𝑖]*[is
a previously
interacted]

[item (e.g., click, purchase, like),
and]*[
𝐵]*[is the
sequence length.
The]

[target label][
Y][
=][
{]*[𝑦]*[1]*[,𝑦]*[2]*[,
. . .
,𝑦][𝐾]*[}][
corresponds to high-value
items]

[actually exposed in the current PV session. Each candidate
item]*[
𝑦][𝑖]*

[is described by a multi-modal representation that includes
Point]

[of Interest (POI) feature
embeddings]*[
𝒆]*^*[𝑝𝑜𝑖]*^

*[𝑖]*

[(e.g., sparse ID
features]

[and dense features), and creative image
embeddings]*[
𝒆]*^*[𝑖𝑚𝑔]*^

*[𝑖]*

[extracted]

[from visual content. The final input can be represented
as][
S]^*[𝑢]*^[=]

[{(]*[𝒆]*^*[𝑝𝑜𝑖]*^

[1]

*[,][
𝒆]*^*[𝑖𝑚𝑔]*^

[1]

[)]*[,]*[
(]*[𝒆]*^*[𝑝𝑜𝑖]*^

[2]

*[,][
𝒆]*^*[𝑖𝑚𝑔]*^

[2]

[)]*[,
\...,]*[
(]*[𝒆]*^*[𝑝𝑜𝑖]*^

*[𝐵]*^*[,]# \ 𝒆# 𝑖𝑚𝑔*^

*[𝐵]*

[)}][.]

*[4.1.2]*

*[Vector
Tokenization.]*[
Inspired by existing generative
recom-]

[mendation models
\[][4][,][
18][,][
20][,][
31][\], we
employ Residual
Quantized]

[Variational Autoencoder (RQ-VAE)
\[][33][\]
to encode dense
embed-]

[dings into semantic tokens. Each user behavior in the
historical]

[sequence is represented as a POI-creative
pair:]

[S]^*[𝑢]*^[=][
{(]*[𝒂]*^*[𝑝𝑜𝑖]*^

[1]

*[,][
𝒂]*^*[𝑖𝑚𝑔]*^

[1]

[)]*[,]*[
(]*[𝒂]*^*[𝑝𝑜𝑖]*^

[2]

*[,][
𝒂]*^*[𝑖𝑚𝑔]*^

[2]

[)]*[,
\...,]*[
(]*[𝒂]*^*[𝑝𝑜𝑖]*^

*[𝐵]*^*[,]# \ 𝒂# 𝑖𝑚𝑔*^

*[𝐵]*

[)}]*[.]*

[(10)]

[and the prediction target is a similarly structured
sequence:]

[Y][
=][
{(]*[𝒂]*^*[𝑝𝑜𝑖]*^

[1]

*[,][
𝒂]*^*[𝑖𝑚𝑔]*^

[1]

[)]*[,]*[
(]*[𝒂]*^*[𝑝𝑜𝑖]*^

[2]

*[,][
𝒂]*^*[𝑖𝑚𝑔]*^

[2]

[)]*[,
\...,]*[
(]*[𝒂]*^*[𝑝𝑜𝑖]*^

*[𝐾]*^*[,]# \ 𝒂# 𝑖𝑚𝑔*^

*[𝐾]*

[)}]*[.]*

[(11)]

[Each pair of
tokens][
(]*[𝒂]*^*[𝑝𝑜𝑖]*^

*[𝑖]*

*[,][
𝒂]*^*[𝑖𝑚𝑔]*^

*[𝑖]*

[)][ combines
high-level
categorical]

[intent and fine-grained visual semantics. For simplicity, we
assume]

[that both POI and creative image are each represented by a
single-]

[level token]*[
𝒂]*[, though
the framework can be extended to
hierarchical]

[token representations if needed. For example, given the
codebooks]

[V][ that consists
of]*[
𝐶]*[layers,
each of size]*[
𝐾]*[, the
token]*[
𝒂][𝑖]*[is
denoted]

[as]

*[𝒂][𝑖]*[=][
(]*[𝑎]*^[1]^

*[𝑖]*^*[,𝑎]*[2]^

*[𝑖]*^*[,\ \...,𝑎]# 𝐶*^

*[𝑖]*^[)]*[,]*^

*[𝑎]*^*[𝑗]*^

*[𝑖]*^[∈{V]*[𝑗,]*[1]*[,]*[\ V]*[𝑗,]*[2]*[,\ \...,]*[\ V]*[𝑗,𝐾]*[}]*[.]*^

[(12)]

*[4.1.3]*

*[Probabilistic
Decomposition.]*[
The modeling of the target
item's]

[probability distribution is decomposed into two stages,
leveraging]

[the complementary advantages of POI-level and creative-level
rep-]

[resentations. Rather than directly predicting the next
item]*[
𝒂][𝑡]*[+][1]

[from the historical interaction
sequence][
S]^*[𝑢]*^

[1:]*[𝑡]*^[,\ EGA-V2\ first\ predicts]^
:::

::: {#page0 style="width:612.0pt;height:792.0pt"}
[EGA-V2: An End-to-end Generative Framework for Industrial
Advertising]

[Woodstock '18, June 03--05, 2018, Woodstock,
NY]

[the POI]*[
𝒂]*^*[𝑝𝑜𝑖]*^

*[𝑡]*[+][1]^[,\ then\ determines\ the\ creative\ image]*[\ 𝒂]# 𝑖𝑚𝑔*^

*[𝑡]*[+][1][
]^[.]^

*[𝑃]*[(]*[𝒂]*^*[𝑝𝑜𝑖]*^

*[𝑡]*[+][1]^*[,]# \ 𝒂# 𝑖𝑚𝑔*^

*[𝑡]*[+][1][
]^[\|\ S]*[𝑢]*^

[1:]*[𝑡]*^[)][\ =]*[\ 𝑃]*[(]*[𝒂]# 𝑝𝑜𝑖*^

*[𝑡]*[+][1][
]^[\|\ S]*[𝑢]*^

[1:]*[𝑡]*^[)\ ·]*[\ 𝑃]*[(]*[𝒂]# 𝑖𝑚𝑔*^

*[𝑡]*[+][1][
]^[\|]*[\ 𝒂]# 𝑝𝑜𝑖*^

*[𝑡]*[+][1]^*[,]*[\ S]*[𝑢]*^

[1:]*[𝑡]*^[)]^

[(13)]

[where]*[
𝑃]*[(]*[𝒂]*^*[𝑝𝑜𝑖]*^

*[𝑡]*[+][1][
]^[\|]*[\ 𝑆]# 𝑢*^

[1:]*[𝑡]*^[)][\ denotes\ the\ probability\ of\ generating\ the\ next]^

[POI]*[
𝒂]*^*[𝑝𝑜𝑖]*^

*[𝑡]*[+][1][
]^[based\ on\ the\ historical\ sequence]*[\ 𝑆]# 𝑢*^

[1:]*[𝑡]*^[,\ capturing\ the\ cate-]^

[gorical identity of the next item.
Meanwhile,]*[
𝑃]*[(]*[𝒂]*^*[𝑖𝑚𝑔]*^

*[𝑡]*[+][1][
]^[\|]*[\ 𝒂]# 𝑝𝑜𝑖*^

*[𝑡]*[+][1]^*[,]*[\ S]*[𝑢]*^

[1:]*[𝑡]*^[)]^

[models the probability of generating the creative
image]*[
𝒂]*^*[𝑖𝑚𝑔]*^

*[𝑡]*[+][1][
]^[con-]^

[ditioned on the POI and the history, capturing fine-grained
multi-]

[modal details of the
item.]

*[4.1.4]*

*[Encoder-decoder.]*[
The overall generative architecture
follows]

[an encoder-decoder design aligned with the modular blocks
shown]

[in Figure 2. The encoder module first encodes the user
interaction]

[sequence][
S]^*[𝑢]*^[using
stacked self-attention and feed-forward
layers:]

[S]^*[𝑒]*^[=][
Encoder][(S]^*[𝑢]*^[)]*[,]*

[(14)]

[where][
S]^*[𝑒]*^[represents
the latent contextualized embedding of
user]

[interests.]

[The decoder generates the target
sequence][
Y][ in an
autoregres-]

[sive manner, conditioned on the encoded user
context][
S]^*[𝑒]*^[.
Each]

[decoding
step]*[
𝑡]*[consists
of two sub-stages: POI token
generation]

[followed by creative token generation. To enable joint
learning,]

[inspired by Multi-Token Prediction (MTP)
\[][10][,][
17][\], we
integrate]

[a MTP module that supervises both heads simultaneously
using]

[cross-entropy loss on POI and creative predictions. All
modules]

[share the same token embedding space and quantization
backbone,]

[facilitating efficient training and consistent generation
quality.]

[At
step]*[𝑡]*[+][1][,
the POI decoder predicts the next POI token based
on]

[previously generated tokens and the encoded historical
sequences:]

*[𝒂]*^*[𝑝𝑜𝑖]*^

*[𝑡]*[+][1][
]^[=][\ POI-Decoder][(Y][1:]*[𝑡]# ,*[\ S]*[𝑒]*[)]*[.]*^

[(15)]

[Then, the creative decoder predicts the corresponding
creative]

[token by attending to both the POI prediction and the
encoded]

[context:]

*[𝒂]*^*[𝑖𝑚𝑔]*^

*[𝑡]*[+][1][
]^[=][\ Creative-Decoder][(Y][1:]*[𝑡]# ,# \ 𝒂# 𝑝𝑜𝑖*^

*[𝑡]*[+][1]^*[,]*[\ S]*[𝑒]*[)]*[.]*^

[(16)]

[The generation of the entire target
sequence][
Y][ is modeled as
a]

[product of conditional
probabilities:]

*[𝑃]*[(Y \|
S]^*[𝑢]*^[)][
=]

*[𝑇]*

[Ö]

*[𝑡]*[=][1]

*[𝑃]*[(]*[𝒂]*^*[𝑝𝑜𝑖]*^

*[𝑡]*

[\|
Y][1:]*[𝑡]*[−][1]*[,]*[
S]^*[𝑒]*^[)
·]*[
𝑃]*[(]*[𝒂]*^*[𝑖𝑚𝑔]*^

*[𝑡]*

[\|
Y][1:]*[𝑡]*[−][1]*[,][
𝒂]*^*[𝑝𝑜𝑖]*^

*[𝑡]*

*[,]*[
S]^*[𝑒]*^[)]*[.]*

[(17)]

# 4.2**

**[Permutation-aware Reward
Model]**

[To ensure that the generated ad sequences align with real
user]

[interests, we introduce a permutation-aware reward model
(RM)]

[to guide iterative optimization. Unlike NLP tasks where
interests]

[signals are typically annotated by humans, the advertising
domain]

[benefits from more accurate, point-wise feedback derived from
user]

[interactions such as clicks and
conversions.]

[Let]*[
𝑅]*[(Y)][
denote the reward model that estimates reward
signals]

[for a candidate target
items][
Y][
=][
{]*[𝒂]*[1]*[,][
𝒂]*[2]*[,
. . . ,][
𝒂][𝐾]*[}][,
where each]*[
𝒂][𝑖]*[is
a]

[generated token. Besides, each generated token does not
necessarily]

[correspond to a unique item, which complicates the assignment
of]

[supervision signals in reward model. To address this, we enrich
each]

[token
representation]*[
𝒂][𝑖]*[by
concatenating raw item
embeddings]

*[𝒆]*^*[𝑝𝑜𝑖]*^

*[𝑖]*

[, which is represented
as:]

*[𝒉][𝑖]*[=][
\[]*[𝒂][𝑖]*[;]*[
𝒆]*^*[𝑝𝑜𝑖]*^

*[𝑖]*

[\]]*[.]*

[(18)]

[where][
\[·][;][
·\]][ denotes
concatenation. The target items
becomes]*[
𝒉]*[=]

[{]*[𝒉]*[1]*[,][
𝒉]*[2]*[,
. . . ,][
𝒉][𝐾]*[}][.]

[The target
items]*[
𝒉]*[are then
processed by self-attention
layers,]

[which enable interaction among them to capture contextual
depen-]

[dencies and aggregate relevant information across the
sequence.]

*[𝒉][𝑓]*[=][
SelfAttention][(]*[𝒉][𝑊]*^*[𝑄]*^*[,][
𝒉][𝑊]*^*[𝐾]*^*[,][
𝒉][𝑊]*^*[𝑉]*^[)]*[.]*

[(19)]

[For making fine-grained predictions such as pCTR for POI
and]

[creative image respectively, and pCVR, we augment the
reward]

[model with multiple task-specific
towers:]

[ˆ]*[𝑟]*^[pctr][\ ]^[=][
Tower]^[pctr][\ ∑︁]^

*[𝒉][𝑓]*

[]

*[,]*

[ˆ]*[𝑟]*^[pcvr][\ ]^[=][
Tower]^[pcvr][\ ∑︁]^

*[𝒉][𝑓]*

[]

[(20)]

[where][
Tower][(·)][
=][
Sigmoid][(][MLP][(·))][.
After obtaining the
estimated]

[rewards][
ˆ]*[𝑟]*^[pcxr][\ ]^[and
the corresponding ground-truth
labels]*[
𝑦]*^[pcxr][\ ]^[for]

[each item and reward type, we train the reward model by
directly]

[minimizing the binary cross-entropy loss. The detailed
training]

[procedure is described in the Section
4.4.]

# 4.3**

**[Auction-based Preference
Alignment]**

[In the industrial advertising scenario, generative
recommendation]

[models need to fulfill not only user interests but also critical
busi-]

[ness constraints. Specifically, two main business constraints
must]

[be simultaneously satisfied: i) advertiser demands for effective
ex-]

[posure, bidding compatibility, and payment consistency; and
ii)]

[platform constraints for balancing revenue with user
experience,]

[such as controlling ad exposure ratios. To address these
challenges,]

[we propose an integrated generative allocation and payment
strat-]

[egy to do preference alignment, consisting of a token-level
bidding]

[based allocation module and a decoupled POI-level payment
net-]

[work. The overall structure is illustrated in Figure
3.]

*[4.3.1]*

*[Token-level
Bidding.]*[
After applying RQ-VAE for item
tok-]

[enization, the original item-level bids no longer align directly
with]

[the token space due to a many-to-many mapping between items
and]

[latent tokens. Inspired by Google's token-level bidding theory
\[][6][\],]

[we introduce a token-level allocation mechanism that
aggregates]

[bids across items associated with each token. As mentioned
in]

[Equation][
(12)][, each
token]*[
𝒂][𝑖]*[=][
(]*[𝑎]*^[1]^

*[𝑖]*^*[,𝑎]*[2]^

*[𝑖]*^*[,\ \...,𝑎]# 𝐶*^

*[𝑖]*^[)][.\ To\ maintain\ differ-]^

[entiation in bids across different tokens, the bid of
token]*[
𝑎]*^*[𝑗]*^

*[𝑖]*^[is]^

*[𝑏]*[(]*[𝑎]*^*[𝑗]*^

*[𝑖]*^[)][\ =][\ max][(]*[𝑏]*[1]*[,𝑏]*[2]*[,\ \...,𝑏]# 𝑁# 𝑖*[)][,\ where]*[\ 𝑁]# 𝑖*[is\ the\ number\ of\ items\ asso-]^

[ciated with
token]*[
𝑎]*^*[𝑗]*^

*[𝑖]*^[.\ Formally,\ the\ aggregated\ bid\ weight\ for\ token]^

*[𝑎]*^*[𝑗]*^

*[𝑖]*^[is\ defined\ as:]^

*[𝑤]*[(]*[𝑎]*^*[𝑗]*^

*[𝑖]*^[)][\ =]^

[h]

*[𝑏]*[(]*[𝑎]*^*[𝑗]*^

*[𝑖]*^[)]^

[i]*[𝛼]*

[+]*[
𝛽]*[=]

[]

[max][(]*[𝑏]*[1]*[,𝑏]*[2]*[,
\...,𝑏][𝑁][𝑖]*[)]

[]*[𝛼]*[+]*[
𝛽]*

[(21)]

[where]*[
𝛼]*[and]*[
𝛽]*[are
hyperparameters. By
adjusting]*[
𝛼]*[, the
influence of]

[bids on the allocation process can be flexibly controlled in
real-time;]

[meanwhile, dynamically
tuning]*[
𝛽]*[enables
effective balancing
of]

[the proportion between ads and organic content in the
generated]

[results.]
:::

::: {#page0 style="width:612.0pt;height:792.0pt"}
[Woodstock '18, June 03--05, 2018, Woodstock,
NY]

[Zheng et al.]

**[POI-level Pay
Net]**

[\...]

[\...]

[P\_1]

[P\_2]

[P\_3][
]^[\...]^

[\...]

[P\_92 P\_87
P\_16]

[Reward Models]

[P\_26 P\_63
P\_42]

[P\_49 P\_51
P\_03]

[\... ]

**[All Valid
POIs]**

**[Token-level
Bidding]**

[\... ]

*[Chosen]*

[EGA]

[Weighted POI
Head]

**[Generative
Allocation]**

[PG Loss]

[\... ]

# Winning List**

# Payment**

[\...]

[\...]

**[Figure 3: Illustration of the proposed generative ad allocation and
payment architecture. Token-level bidding aggregates
item]**

**[bids for generative allocation, while a dedicated POI-level payment
network ensures incentive-compatible (IC) constraint.
The]**

**[integrated framework supports dynamic trade-off between revenue and
user
experience.]**

*[4.3.2]*

*[Generative
Allocation.]*[
The probability of selecting
token]*[
𝑎]*^*[𝑗]*^

*[𝑖]*

[in the generative allocation is given by a softmax
normalization:]

*[𝑧]*[(]*[𝑎]*^*[𝑗]*^

*[𝑖]*^[)][\ =]^

*[𝑤]*[(]*[𝑎]*^*[𝑗]*^

*[𝑖]*^[)\ ·]*[\ 𝑒]# 𝑎# 𝑗*^

*[𝑖]*

[Í]*[𝑊]*

*[𝑘]*[=][1]

[h]

*[𝑤]*[(]*[𝑎]*^*[𝑗,𝑘]*^[)
·]*[
𝑒]*^*[𝑎]# 𝑗,𝑘*[i]*[\ ,]*^

[(22)]

[where]*[
𝑎]*^*[𝑗,𝑘]*^[is
the]*[
𝑘]*[-th code
of]*[
𝑗]*[-th
layer. Based on the
generative]

[probabilities]*[
𝒛]*[∈][R]^*[𝐶]*[×]*[𝑊]*^[,
where]*[
𝐶]*[is the
codebook layers
and]*[
𝑊]*[is]

[the codebook size of each layer, we apply beam search to
generate]

*[𝑁][𝑆]*[candidate
sequences of
length]*[
𝐾]*[,
ensuring both diversity
and]

[high-quality selection in the
allocation:]

[S]^[(][1][)]^*[,]*[
S]^[(][2][)]^*[,
. . . ,]*[
S]^[(]*[𝑁]# 𝑆*[)][\ ]^[=][
BeamSearch][(]*[𝒛][,
𝑁][𝑆]*[)]*[,]*

[(23)]

[where][
S]^[(]*[𝑗]*[)][\ ]^[represents]*[
𝑗]*[-th
generated candidate sequence of
tokens,]

[and]*[
𝑁][𝑆]*[denotes
both the beam width and the number of
generated]

[candidate sequences. We evaluate each sequence using the
reward]

[model][
R][ to measure its
expected business value, which outputs
the]

[reward][
ˆ]*[𝑟][𝑗]*[for]*[
𝑗]*[-th
sequence:]

[ˆ]*[𝑟][𝑗]*[=][
R(]*[𝑆]*^[(]*[𝑗]*[)]^[)]*[.]*

[(24)]

[The final output is the
sequence][
S]^[∗]^[with
the highest reward. It
is]

[worth noting that RM provides a flexible interface to
accommodate]

[diverse reward signal combinations as needed by the
platform.]

*[4.3.3]*

*[POI-level Payment
Network.]*[
While allocation operates at
the]

[token level, payment is calculated at the item or POI level,
which]

[aligns better with traditional advertiser expectations and
business]

[logics. The decoupled payment network is specifically designed
to]

[satisfy IC and IR constraints, which computes payments based
on]

[item
representations.]

[Formally, the payment network inputs include item
representa-]

[tions][
S]^[∗]^[=][
{]*[𝑦]*[1]*[,𝑦]*[2]*[,
\...,𝑦][𝐾]*[}
∈][R]^*[𝐾]*[×]*[𝑑]*^[,
the self-exclusion bidding
pro-]

[file][
B]^[−]^[=][
{]*[𝒃]*[−][1]*[,][
𝒃]*[−][2]*[,
\...,][
𝒃]*[−]*[𝐾]*[}
∈][R]^*[𝐾]*[×(]*[𝐾]*[−][1][)]^[,
and the expected
value]

[profile][ Z
·][
Θ][
∈][R]^*[𝐾]*[×][1]^[,
where][
Z][
=][
{]*[𝑧]*[1]*[,𝑧]*[2]*[,
\...,𝑧][𝐾]*[}][
denotes the
alloca-]

[tion probability defined in
Equation][
(22)][
and][
Θ][
=][
{][ˆ]*[𝑟]*^*[𝑝𝑐𝑡𝑟]*^

[1]

*[,]*[
ˆ]*[𝑟]*^*[𝑝𝑐𝑡𝑟]*^

[2]

*[, \...,]*[
ˆ]*[𝑟]*^*[𝑝𝑐𝑡𝑟]*^

*[𝐾]*

[}]

[denotes the permutation-aware pCTR estimated by the
reward]

[model in Equation (20). The payment rate is defined
as:]

[ˆ]*[𝑝]*[=]*[
𝜎]*[(][MLP][(S]^[∗]^[;][
B]^[−]^[;][
Z ·][
Θ][))
∈\[][0]*[,]*[
1][\]]^*[𝐾]*^*[,]*

[(25)]

[where]*[
𝜎]*[denotes
the sigmoid activation to satisfy IR constraint,
and]

[the final POI-level
payment]*[
𝑝]*[is
calculated as:]

*[𝑝]*[=][
ˆ]*[𝑝]*[⊙]*[𝑏.]*

[(26)]

[It should be noted that payments are calculated exclusively for
ads]

[if]*[
𝑦][𝑖]*[∈]*[𝑋]*[,
while the payment for organic content is zero
if]*[
𝑦][𝑖]*[∈]*[𝑂]*[.]

# 4.4**

**[Optimization and
Training]**

![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACACUAFIDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDyWGF55VjjGWY8VqjT7K2VTcy8%0AsMgyEhW+igFsdear6Qyi4kDA8pgkdQNwziqc8zXEzyv1Y/l6CgC/nTvSP85KM6d6%0AR/nJWZSqQHBZdwB5GetAGlnTvSP85KM6d6R/nJStqFqWO23QDPA8lKT+0Lb/AJ4J%0A/wB+EoAM6d6R/nJRnTvSP85KoXDpJcO8SbEJyF9KjoA2Fs7C6wIX+fHIiJJH/AWA%0Az+BrMubd7WYxuQeMhl6MPUVGrMjhlJDKcgjsa09YP7q14HzLv4HTcqtj6DJoAy6K%0AKKALul/69/8Ac/qKlmuDaxQKijDRqfTsKi0v/Xv/ALn9RSX/ANy2/wCuS/8AoIoA%0AX+03/uD8zR/ab/3B+ZqlRQBd/tN/7g/M0f2m/wDcH5mqVFAF3+03/uD8zR/ab/3B%0A+ZqlRQBqWd0bq5SKRF2see+aNY/1Vn/1xT/0WlV9K/4/4/r/AFqxrH+qs/8Arin/%0AAKLSgDLooooAu6X/AK9/9z+opL/7lt/1yX/0EUul/wCvf/c/qKS/+5bf9cl/9BFA%0AFOiiigAooooAKKKKALmlf8f8f1/rVjWP9VZ/9cU/9FpVfSv+P+P6/wBasax/qrP/%0AAK4p/wCi0oAy6KKKALul/wCvf/c/qKS/+5bf9cl/9BFLpf8Ar3/3P6ikv/uW3/XJ%0Af/QRQBTooooAKKKKACiiigC5pX/H/H9f61Y1j/VWf/XFP/RaVX0r/j/j+v8AWrGs%0Af6qz/wCuKf8AotKAMuiiigC7pf8Ar3/3P6ikv/uW3/XJf/QRT9IAa7IPQqAecfxC%0AmX5BW3HpEv8A6CKAKdFFFABRRRQAUUUUAXNLz9vjxjOR1+tWNX/1Vn/1xT/0WlV9%0ALGb6MAkcjkfUVNqrborTjpEg/wDIaUAZtFFFAF/SFDXRUsFBUDJ6D5hTL4ALb47x%0Arn/vkU/SDtuyxzgKDx/vCmX/ANy2/wCuS/8AoIoAp0UUUAFFFFABRRRQBc0sBr6M%0AH1H86sax/qrP/rin/otKr6V/x/x/X+tWNY/1Vn/1xT/0WlAGXRRRQBf0jH2s7sY2%0AjOTj+IUy/wDuW3/XJf8A0EU7SWK3LMOoXI/76FNv/uW3/XJf/QRQBTooooAKKKKA%0ACiiigC5pX/H/AB/X+tWNY/1Vn/1xT/0WlV9K/wCP+P6/1qxrH+qs/wDrin/otKAM%0AuiiigC7pf+vf/c/qKS/+5bf9cl/9BFLpf+vf/c/qKfdwvNHbmMZxGueR/dFAGfRU%0A/wBjn/ufqKPsc/8Ac/UUAQUVP9jn/ufqKPsc/wDc/UUAQUVP9jn/ALn6ij7HP/c/%0AUUAS6V/x/wAf1/rVjWP9VZ/9cU/9FpTNOt5Ir2NnXC5HOR60/WP9VZ/9cU/9FpQB%0Al0UUUAX9JRnuH2jPygfiWFVVuJ4xtSaRQOwYjFSWNyLacl8+W42Pgcgdcj6EA1oX%0AOmLdk3EMqoG5ZsFkY9yCAcH2I4/SgDM+2XP/AD8S/wDfZo+2XP8Az8S/99mrDaXI%0ADxNE3uN3+FJ/Zkn/AD0j/wDHv8KAJrS5dIHnlmlYr0G4n/63enTXbXFqZklkRx1A%0AYgfpx3p1pBJAjRSNG0bexOPwIwe3HtT7iFmh8mBo1Q9SQRn8APpz7UAZf2y5/wCf%0AiX/vs0fbLn/n4l/77NT/ANmSf89I/wDx7/Cj+zJP+ekf/j3+FAFc3VwwIaeQg9QX%0ANX9XIMVnjtEg/wDIaU+20gKyyzSLKi8lVyq/8CZgAB/n3qtqd2txIscbBkjJO7GN%0AzHGT9OAPwoAo0UUUAFOSR4m3RuyN6qcGiigCT7Zc/wDPxL/32aPtlz/z8S/99mii%0AgA+2XP8Az8S/99mj7Zc/8/Ev/fZoooAPtlz/AM/Ev/fZo+2XP/PxL/32aKKAGSSy%0ASkGR2cjgFjmmUUUAFFFFAH//2Q==)
![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACACUAFIDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDyWGF55VjjGWY8VqjT7K2VTcy8%0AsMgyEhW+igFsdear6Qyi4kDA8pgkdQNwziqc8zXEzyv1Y/l6CgC/nTvSP85KM6d6%0AR/nJWZSqQHBZdwB5GetAGlnTvSP85KM6d6R/nJStqFqWO23QDPA8lKT+0Lb/AJ4J%0A/wB+EoAM6d6R/nJRnTvSP85KoXDpJcO8SbEJyF9KjoA2Fs7C6wIX+fHIiJJH/AWA%0Az+BrMubd7WYxuQeMhl6MPUVGrMjhlJDKcgjsa09YP7q14HzLv4HTcqtj6DJoAy6K%0AKKALul/69/8Ac/qKlmuDaxQKijDRqfTsKi0v/Xv/ALn9RSX/ANy2/wCuS/8AoIoA%0AX+03/uD8zR/ab/3B+ZqlRQBd/tN/7g/M0f2m/wDcH5mqVFAF3+03/uD8zR/ab/3B%0A+ZqlRQBqWd0bq5SKRF2see+aNY/1Vn/1xT/0WlV9K/4/4/r/AFqxrH+qs/8Arin/%0AAKLSgDLooooAu6X/AK9/9z+opL/7lt/1yX/0EUul/wCvf/c/qKS/+5bf9cl/9BFA%0AFOiiigAooooAKKKKALmlf8f8f1/rVjWP9VZ/9cU/9FpVfSv+P+P6/wBasax/qrP/%0AAK4p/wCi0oAy6KKKALul/wCvf/c/qKS/+5bf9cl/9BFLpf8Ar3/3P6ikv/uW3/XJ%0Af/QRQBTooooAKKKKACiiigC5pX/H/H9f61Y1j/VWf/XFP/RaVX0r/j/j+v8AWrGs%0Af6qz/wCuKf8AotKAMuiiigC7pf8Ar3/3P6ikv/uW3/XJf/QRT9IAa7IPQqAecfxC%0AmX5BW3HpEv8A6CKAKdFFFABRRRQAUUUUAXNLz9vjxjOR1+tWNX/1Vn/1xT/0WlV9%0ALGb6MAkcjkfUVNqrborTjpEg/wDIaUAZtFFFAF/SFDXRUsFBUDJ6D5hTL4ALb47x%0Arn/vkU/SDtuyxzgKDx/vCmX/ANy2/wCuS/8AoIoAp0UUUAFFFFABRRRQBc0sBr6M%0AH1H86sax/qrP/rin/otKr6V/x/x/X+tWNY/1Vn/1xT/0WlAGXRRRQBf0jH2s7sY2%0AjOTj+IUy/wDuW3/XJf8A0EU7SWK3LMOoXI/76FNv/uW3/XJf/QRQBTooooAKKKKA%0ACiiigC5pX/H/AB/X+tWNY/1Vn/1xT/0WlV9K/wCP+P6/1qxrH+qs/wDrin/otKAM%0AuiiigC7pf+vf/c/qKS/+5bf9cl/9BFLpf+vf/c/qKfdwvNHbmMZxGueR/dFAGfRU%0A/wBjn/ufqKPsc/8Ac/UUAQUVP9jn/ufqKPsc/wDc/UUAQUVP9jn/ALn6ij7HP/c/%0AUUAS6V/x/wAf1/rVjWP9VZ/9cU/9FpTNOt5Ir2NnXC5HOR60/WP9VZ/9cU/9FpQB%0Al0UUUAX9JRnuH2jPygfiWFVVuJ4xtSaRQOwYjFSWNyLacl8+W42Pgcgdcj6EA1oX%0AOmLdk3EMqoG5ZsFkY9yCAcH2I4/SgDM+2XP/AD8S/wDfZo+2XP8Az8S/99mrDaXI%0ADxNE3uN3+FJ/Zkn/AD0j/wDHv8KAJrS5dIHnlmlYr0G4n/63enTXbXFqZklkRx1A%0AYgfpx3p1pBJAjRSNG0bexOPwIwe3HtT7iFmh8mBo1Q9SQRn8APpz7UAZf2y5/wCf%0AiX/vs0fbLn/n4l/77NT/ANmSf89I/wDx7/Cj+zJP+ekf/j3+FAFc3VwwIaeQg9QX%0ANX9XIMVnjtEg/wDIaU+20gKyyzSLKi8lVyq/8CZgAB/n3qtqd2txIscbBkjJO7GN%0AzHGT9OAPwoAo0UUUAFOSR4m3RuyN6qcGiigCT7Zc/wDPxL/32aPtlz/z8S/99mii%0AgA+2XP8Az8S/99mj7Zc/8/Ev/fZoooAPtlz/AM/Ev/fZo+2XP/PxL/32aKKAGSSy%0ASkGR2cjgFjmmUUUAFFFFAH//2Q==)

**[Step 1 
Interest-based]**

**[       
Pre-training]**

**[Step 2 
Auction-based Post-training]**

![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAAB8AAAAkCAIAAAD+cXYAAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABDElEQVR4nLWVURLDIAhEtcn9z1wzThi6QWCx4aPTIDwQEVvLyUdJ%0A7z3pFUi/xdTvojVIcp/K4zjO85yfdXRoQwfIoKfIVjh63qFyBkkfMeMChFv+Axfa%0A5skdMjqHpptovVRJvC1qAvnCUiX3V+xZ+ri0lP0lVL/TXJZOxHj3rrJ9tjsJVv7F%0AQR9WcyxVBiQEMP3n67H1Qpl3Uoesz1792rXf8TCynm+3KFNhVicJegkJ+/NiwJDS%0Aps40Nh2XaFMvf6BJHPsU+lmocPwaZma9VgXJXAWPC+nrABDSoV+/GTqAnLqjY+Y6%0AdEsyLll6qNmiQ9Zhq9D01edu7u1xqhzdPDezHUUzB5nvksm7Ll8tbwNzyNXOBQAA%0AAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABUAAAAUCAIAAADtKeFkAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABn0lEQVR4nLXTS4/BYBQGYL9dXUtRl4SdxsKOWEjqthA2ItKwkRIq%0AQSLikhLRUMybnkk7LjOYyZyFtPTp955zwnb5W9n+y5/P53a7PRwOcYHP5XL5nt/v%0A96lUql6vDwaDWCwmiuLxeByPx5qmveRR6/X6cDhsNhtgRVEymYzP58vlcs/96XSa%0AzWa9Xm86neIa3+x2O47jPB7Pc49j8RAyh4xKJBLNZlPX9VarVa1W5/M53rtarR57%0ApBUEIRwO8zwPzBmFYyHx62KxCAQCDMMEg0HKdeUx6nw+DxyJRPAcumVZ1uv1wuMW%0Ak6tUKsB2u93hcGA0t77T6QDjWLze7/cDQ7rdbpfLBYDhNxoN8rVa7TY/2o7H44hN%0AmQmTZIyaTCYIiLmqqvpgfuVymbCZGSc7nU7C0WgUI7wf9qffbrfomTLfY0Todrv3%0A2PKSJNHACCM2Ych0Oo3OH2LLY+GsUWbPKLxxNBp9J698Mpmk5ObAkAXb/hlbPpvN%0AmnuinmVZfootj78HDqeesd5SqfQKvnzdH9ZbKBSKxWK/38ee3/a/qw9HMD5S7hNe%0AEgAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABEAAAATCAIAAAD5x3GmAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABkElEQVR4nJWS6WrCYBBF8/5vImo1sS7RaF0DEhVBlIA7CuKOKCFx%0A7Wk+qzSoxfkRArln7p2ZSJf3S3r2wXGcxWIxGo3G4/F6vT6fz/8wk8mkXC6XSqVi%0AsVgoFLLZrGEYy+XyFdNsNpEC5PP5XC4H8+UWhg+Y3W5HKnxgAFBnMpl0Op1MJlVV%0AxZmQXqbdbne73ePxSBgcADRNA4jH459ukdDLIMVhtVptNhvCoE6lUjjEYjEARVH6%0A/b6X0XUdKeR+vx8MBqpbiUQiGo1GIpFwONzpdP4wTEIeGAZoNBqHw6HX65FKAKFQ%0AKBgMsv07czqdarUaCgagNy+VSsW27dlsRjyAQCBANvyvDNswTROpLMtEJwxPeuPJ%0ADiFbrRZdCHbfNcdGx5QkBqMfAE8xN+Ntt1uiir9BEjdhP4pbMB9uEUb4MFu1WmWT%0AtykkTsGl5N9CKhjU9H74l0hEvPVmSr/fz3JIz0oeAj8MOxVqATD6fD5/pr4yKPDx%0A+XwAHMeyrNfAdQfT6bRerw+Hwxd5vMy79Q177FfQwumNeAAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABIAAAAUCAIAAAAP9fodAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABpUlEQVR4nJWT2WrCUBCGff8ncBdt2rhWFEUbd1zBDSUXKiiCIIgS%0AENzwQvslR1PtRWv+i3DOyXyZmX9ObNe7LpfL9WXZzNV0Og0amkwmFrButyvLcigU%0AajQaFrDNZgPZbreXy6UF7HA4ACwWC03TLGCz2Ywi6W08HlvAOp2ObKharb6Kzedz%0AzJAk6d3QaDT6H1uv1+FwGObtLsg/jGHCOtZqtQiV7mLt8/kKhQKvdrtdNptNpVKJ%0AROLTUDwe3+/3OlYul0WSaDT6ZSiTydTrdV7hKqGcUw5uCc+2262OFYvFQCDg9/uT%0AyWSpVGKbz+ebzabATObDEOQNIwiGwgRGeSZGBJjIQ8NUBHk8HnUsl8vBeL1eGiAV%0AW0VRxBWje05giKZtvs725iRxMB6Ph3ZJBYMNtVpN+MbtYS0qpITT6XTDiINxuVyx%0AWIzyhCW/ht7r9QaDwdPc+Lbb7XY6nZFIRKRKp9OVSuURGw6Hqqo+YWRwOBx2ux2Y%0ApnGVJ7w5XArj6vX7fcz4wVarFV0Ju2gAx6iWX0FEMHHGjZ/cPs7P5/P18Spb0jfd%0ALqa0BRKC4AAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABAAAAAPCAIAAABiEdh4AAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABf0lEQVR4nH1S6+vBYBh9/3S3Ym5p0j6Q5JJC+7R8Eh8UkSK22Epq%0AK3IJm1yy4Xc0zdvK7/m03p3zPOc55yEvqp7P5+VyGY/HzWaz0WgMBoPdbodHGkOc%0AL0Dr9XogEPBQ5fV6c7ncarVyaB/CZrNhWdbzo9BlOBx+CYfDIRKJ0IhisTidTpfL%0A5Ww2K5fL9ihJkj6EarVKo3mev9/vnU4HMyuVyvF4FAQB79Fo9Ha7EV3XfT6fgw6F%0AQlhmv9/jEe05juv1esCEw2H8HY1GBFbQ7TOZjGVZ5/MZzHw+HwwGYdr1ei0UCvZw%0AAvtoQiqVMk1zu936/X5RFDGh3W6fTqd0Om3vRlqtlsvHxWJhGEY8HscEhmEmkwm0%0A2bJLpRJRFMVlYiKR0DQNkXW7XVmW5/O54zjkEAiIxWIuDvRks1lYhNToKFVVfdva%0A7/dtMb+Cswt6kPeb8Hg8arXa/2johLnf0wAH+pxAXNOwPYxyHx9qvV4j1GQyiR1A%0Axr3gLuASfbB/ZA7y1rGsdFsAAAAASUVORK5CYII=)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABEAAAATCAIAAAD5x3GmAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABkElEQVR4nJWS6WrCYBBF8/5vImo1sS7RaF0DEhVBlIA7CuKOKCFx%0A7Wk+qzSoxfkRArln7p2ZSJf3S3r2wXGcxWIxGo3G4/F6vT6fz/8wk8mkXC6XSqVi%0AsVgoFLLZrGEYy+XyFdNsNpEC5PP5XC4H8+UWhg+Y3W5HKnxgAFBnMpl0Op1MJlVV%0AxZmQXqbdbne73ePxSBgcADRNA4jH459ukdDLIMVhtVptNhvCoE6lUjjEYjEARVH6%0A/b6X0XUdKeR+vx8MBqpbiUQiGo1GIpFwONzpdP4wTEIeGAZoNBqHw6HX65FKAKFQ%0AKBgMsv07czqdarUaCgagNy+VSsW27dlsRjyAQCBANvyvDNswTROpLMtEJwxPeuPJ%0ADiFbrRZdCHbfNcdGx5QkBqMfAE8xN+Ntt1uiir9BEjdhP4pbMB9uEUb4MFu1WmWT%0AtykkTsGl5N9CKhjU9H74l0hEvPVmSr/fz3JIz0oeAj8MOxVqATD6fD5/pr4yKPDx%0A+XwAHMeyrNfAdQfT6bRerw+Hwxd5vMy79Q177FfQwumNeAAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABIAAAAUCAIAAAAP9fodAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABpUlEQVR4nJWT2WrCUBCGff8ncBdt2rhWFEUbd1zBDSUXKiiCIIgS%0AENzwQvslR1PtRWv+i3DOyXyZmX9ObNe7LpfL9WXZzNV0Og0amkwmFrButyvLcigU%0AajQaFrDNZgPZbreXy6UF7HA4ACwWC03TLGCz2Ywi6W08HlvAOp2ObKharb6Kzedz%0AzJAk6d3QaDT6H1uv1+FwGObtLsg/jGHCOtZqtQiV7mLt8/kKhQKvdrtdNptNpVKJ%0AROLTUDwe3+/3OlYul0WSaDT6ZSiTydTrdV7hKqGcUw5uCc+2262OFYvFQCDg9/uT%0AyWSpVGKbz+ebzabATObDEOQNIwiGwgRGeSZGBJjIQ8NUBHk8HnUsl8vBeL1eGiAV%0AW0VRxBWje05giKZtvs725iRxMB6Ph3ZJBYMNtVpN+MbtYS0qpITT6XTDiINxuVyx%0AWIzyhCW/ht7r9QaDwdPc+Lbb7XY6nZFIRKRKp9OVSuURGw6Hqqo+YWRwOBx2ux2Y%0ApnGVJ7w5XArj6vX7fcz4wVarFV0Ju2gAx6iWX0FEMHHGjZ/cPs7P5/P18Spb0jfd%0ALqa0BRKC4AAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABAAAAAPCAIAAABiEdh4AAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABf0lEQVR4nH1S6+vBYBh9/3S3Ym5p0j6Q5JJC+7R8Eh8UkSK22Epq%0AK3IJm1yy4Xc0zdvK7/m03p3zPOc55yEvqp7P5+VyGY/HzWaz0WgMBoPdbodHGkOc%0AL0Dr9XogEPBQ5fV6c7ncarVyaB/CZrNhWdbzo9BlOBx+CYfDIRKJ0IhisTidTpfL%0A5Ww2K5fL9ihJkj6EarVKo3mev9/vnU4HMyuVyvF4FAQB79Fo9Ha7EV3XfT6fgw6F%0AQlhmv9/jEe05juv1esCEw2H8HY1GBFbQ7TOZjGVZ5/MZzHw+HwwGYdr1ei0UCvZw%0AAvtoQiqVMk1zu936/X5RFDGh3W6fTqd0Om3vRlqtlsvHxWJhGEY8HscEhmEmkwm0%0A2bJLpRJRFMVlYiKR0DQNkXW7XVmW5/O54zjkEAiIxWIuDvRks1lYhNToKFVVfdva%0A7/dtMb+Cswt6kPeb8Hg8arXa/2johLnf0wAH+pxAXNOwPYxyHx9qvV4j1GQyiR1A%0Axr3gLuASfbB/ZA7y1rGsdFsAAAAASUVORK5CYII=)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABUAAAAUCAIAAADtKeFkAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABn0lEQVR4nLXTS4/BYBQGYL9dXUtRl4SdxsKOWEjqthA2ItKwkRIq%0AQSLikhLRUMybnkk7LjOYyZyFtPTp955zwnb5W9n+y5/P53a7PRwOcYHP5XL5nt/v%0A96lUql6vDwaDWCwmiuLxeByPx5qmveRR6/X6cDhsNhtgRVEymYzP58vlcs/96XSa%0AzWa9Xm86neIa3+x2O47jPB7Pc49j8RAyh4xKJBLNZlPX9VarVa1W5/M53rtarR57%0ApBUEIRwO8zwPzBmFYyHx62KxCAQCDMMEg0HKdeUx6nw+DxyJRPAcumVZ1uv1wuMW%0Ak6tUKsB2u93hcGA0t77T6QDjWLze7/cDQ7rdbpfLBYDhNxoN8rVa7TY/2o7H44hN%0AmQmTZIyaTCYIiLmqqvpgfuVymbCZGSc7nU7C0WgUI7wf9qffbrfomTLfY0Todrv3%0A2PKSJNHACCM2Ych0Oo3OH2LLY+GsUWbPKLxxNBp9J698Mpmk5ObAkAXb/hlbPpvN%0AmnuinmVZfootj78HDqeesd5SqfQKvnzdH9ZbKBSKxWK/38ee3/a/qw9HMD5S7hNe%0AEgAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABMAAAARCAIAAACw+gCQAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABkUlEQVR4nJ2TzYuCUBTF+9PNpsySvkiqCSKFgoo+Nn0RgrRooUuD%0AwNoFRaCFiyiwbA4+qalei5mzEV/3987p3mvo9l+FyMPzvPF43Ol0dF3/M1kqlRiG%0AaTQax+PxcDhsNpvlcjmbzWRZpl4XkPP5vFgschwXjUYjkUg4HGZ+qVqt0snz+dxs%0ANkGKopjJZFKpVCKRiMfjsVjsy1c+n3cch0IikiRJ5XK5UCjkcjlCwp+YE9XrdQq5%0A3W6z2Ww6nRYEged5YsU8yzRNCnm9XlHNfBaSe75eSRwlk8lPGKIiwrevfr+/3++f%0ASPSAiiFLu90mfbr727b9mEqlUqGStVqt1+vBlmVZ0jCMDcUwC0i07h1DNeINh8Nu%0At4upWJaF/pGfkDkgcfEnUlEUwzBgcrlcMDPYgt/tdgE5nU6paVut1mQyUVV1sVjg%0AFnII0nXdgMSiYvTvJOY8GAwAj0YjbAteUaZp2qND0Hq9xgK+kDhZrVbYeGTGxwRz%0AvJLBPkiyEjC3fKH16Ar+Gxkbdvt0OqHgXvwDL9LULUwCiN0AAAAASUVORK5CYII=)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABMAAAAQCAIAAAB7ptM1AAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABpElEQVR4nJ2T2atBURTG/elkyJAhD4a8UFceyJjpwRCRxOUkXnhA%0AMmVIoSTd+8s59zikW/euh93ae5/vW9/61j6qr/+GSs5Wq1UoFPq4RzAYbLVak8lk%0APp9fLpffkNfrNRwOq98FRMvlEl7WzWZzu90eyP1+73a7XwA6nc5isWi12pdzn893%0AOp0kZLlcVt7p9fp6vX44HM7n83a7zefzIr5YLI5GI5fL1W63JWQqlVLCxuMx7cVi%0AMY/HE41GF4tFr9dDgt/vhwUhUEjIZDIpI0ul0nq9NpvN5BqNhtXhcNAkFPI3hUJB%0AQiYSCbk35KXTafJcLjedTiORCHk2mx0MBjLyUVNGmkwmevN6vWjGBmzc7XZUDgQC%0AaBYlPCHj8bh4hBP4zjDJm80mLJVKhZw5D4fDNzUxQz5tNBoYKJpJZerQAjCZ/X1N%0Awmq14i0PCGMMBoPNZiMXBAGWN8hMJqOcp91uZ2I0hqWz2axardK/8oNarSYhmZ7R%0AaFTeIZK5OZ1OVvVzwMube7xbNv1+v9vtIqzT6Xzegy0rapEg/MTxeHz9V/4a34YY%0AoqeC/KWEAAAAAElFTkSuQmCC)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABMAAAASCAIAAAA2bnI+AAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAAB1UlEQVR4nJ2Tx6piURBF36ebGzMGDGBAcKYDcaYIigNFRDHnhBFE%0AxIy5e/GufQ09eXSNbqhVtWvXOV+//ze+fpi32Wx6vd50Or3f758kn0ajUS6XKxQK%0Au93ugwyHw2azWa/Xr9frTzKTyXi9XrvdbrVanU7nYrF4JUOh0K/vOB6Pb+RqtQKz%0A2WwWi8VoNOp0OlJfyWg0KpVKDQbDp9pGo0ErgVGr1SqVymQyXa9XkUylUhKJxOVy%0AvTlEmWw2ywww6FEoFDKZjGeEiHn7/Z6cyWTyRp7PZ0qKDKooz+tsNhO10R9v4N9I%0Aho7H43K5XGCEQPBwOHwl5/P5drt9Iw+HQywWe8UIpVLJAgVyPB6zqkQikUwm8/k8%0AGp9kOp2WvAc2ttvt2+0GHAgE3G43zmMbdrD2B8neqRqJRLAXb/lHHn50Oh1EQvp8%0APrZFLcHCfr//IFFPmcFggLzO36jVavV6/XK5QDocDq1WK1rY7XYfJO7zIgAobDab%0AYOVyuVKpMBIk3RhbtJ20B7lcLgFarRYMR0LAisViqVQSSI1G8+rfk+QegAlMtVoF%0Aw0lI1ApzMt6reQz1PH2n04nOrH74HdwmRsBY8WwGg0GPx+P3+9mNcCp/ej//jT9a%0AwllxGI5+1gAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABUAAAAUCAIAAADtKeFkAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABn0lEQVR4nLXTS4/BYBQGYL9dXUtRl4SdxsKOWEjqthA2ItKwkRIq%0AQSLikhLRUMybnkk7LjOYyZyFtPTp955zwnb5W9n+y5/P53a7PRwOcYHP5XL5nt/v%0A96lUql6vDwaDWCwmiuLxeByPx5qmveRR6/X6cDhsNhtgRVEymYzP58vlcs/96XSa%0AzWa9Xm86neIa3+x2O47jPB7Pc49j8RAyh4xKJBLNZlPX9VarVa1W5/M53rtarR57%0ApBUEIRwO8zwPzBmFYyHx62KxCAQCDMMEg0HKdeUx6nw+DxyJRPAcumVZ1uv1wuMW%0Ak6tUKsB2u93hcGA0t77T6QDjWLze7/cDQ7rdbpfLBYDhNxoN8rVa7TY/2o7H44hN%0AmQmTZIyaTCYIiLmqqvpgfuVymbCZGSc7nU7C0WgUI7wf9qffbrfomTLfY0Todrv3%0A2PKSJNHACCM2Ych0Oo3OH2LLY+GsUWbPKLxxNBp9J698Mpmk5ObAkAXb/hlbPpvN%0AmnuinmVZfootj78HDqeesd5SqfQKvnzdH9ZbKBSKxWK/38ee3/a/qw9HMD5S7hNe%0AEgAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABMAAAAQCAIAAAB7ptM1AAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABpElEQVR4nJ2T2atBURTG/elkyJAhD4a8UFceyJjpwRCRxOUkXnhA%0AMmVIoSTd+8s59zikW/euh93ae5/vW9/61j6qr/+GSs5Wq1UoFPq4RzAYbLVak8lk%0APp9fLpffkNfrNRwOq98FRMvlEl7WzWZzu90eyP1+73a7XwA6nc5isWi12pdzn893%0AOp0kZLlcVt7p9fp6vX44HM7n83a7zefzIr5YLI5GI5fL1W63JWQqlVLCxuMx7cVi%0AMY/HE41GF4tFr9dDgt/vhwUhUEjIZDIpI0ul0nq9NpvN5BqNhtXhcNAkFPI3hUJB%0AQiYSCbk35KXTafJcLjedTiORCHk2mx0MBjLyUVNGmkwmevN6vWjGBmzc7XZUDgQC%0AaBYlPCHj8bh4hBP4zjDJm80mLJVKhZw5D4fDNzUxQz5tNBoYKJpJZerQAjCZ/X1N%0Awmq14i0PCGMMBoPNZiMXBAGWN8hMJqOcp91uZ2I0hqWz2axardK/8oNarSYhmZ7R%0AaFTeIZK5OZ1OVvVzwMube7xbNv1+v9vtIqzT6Xzegy0rapEg/MTxeHz9V/4a34YY%0AoqeC/KWEAAAAAElFTkSuQmCC)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABEAAAATCAIAAAD5x3GmAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABkElEQVR4nJWS6WrCYBBF8/5vImo1sS7RaF0DEhVBlIA7CuKOKCFx%0A7Wk+qzSoxfkRArln7p2ZSJf3S3r2wXGcxWIxGo3G4/F6vT6fz/8wk8mkXC6XSqVi%0AsVgoFLLZrGEYy+XyFdNsNpEC5PP5XC4H8+UWhg+Y3W5HKnxgAFBnMpl0Op1MJlVV%0AxZmQXqbdbne73ePxSBgcADRNA4jH459ukdDLIMVhtVptNhvCoE6lUjjEYjEARVH6%0A/b6X0XUdKeR+vx8MBqpbiUQiGo1GIpFwONzpdP4wTEIeGAZoNBqHw6HX65FKAKFQ%0AKBgMsv07czqdarUaCgagNy+VSsW27dlsRjyAQCBANvyvDNswTROpLMtEJwxPeuPJ%0ADiFbrRZdCHbfNcdGx5QkBqMfAE8xN+Ntt1uiir9BEjdhP4pbMB9uEUb4MFu1WmWT%0AtykkTsGl5N9CKhjU9H74l0hEvPVmSr/fz3JIz0oeAj8MOxVqATD6fD5/pr4yKPDx%0A+XwAHMeyrNfAdQfT6bRerw+Hwxd5vMy79Q177FfQwumNeAAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABMAAAATCAIAAAD9MqGbAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAAB/0lEQVR4nH2TyY8BURDG/b0isTu6SRwc3YiDiIMEifViiZ0QIYh1%0A7MIQsUbQZszX3abymKUOdFe9X9Wrr6ol92/7fLYPwegZv/dnk4jMdrtdLpeLxeJd%0AsNlsNh6PR6PRcDgcDAZvgp3P51dyv9+fTqefWVm7XC7VapU9w5OoxrrW63WhUMjl%0Acv1+H/ckf61WY8vy5Hw+p/fj8RiLxcR7Am61WhSq1+s2my0SieDMg0R7FO52u+12%0AG7Df71+tVqFQiEKVSkWr1apUKqPReL1eX0mUKpfLSB+NRqFTOBymUKlU0ul0IBUK%0AxXQ65Un8URjJcJ9isQg9fD4frk0hNI+aGo0G5OFw4EkMgJURMvR6vU6nA6lYfz6f%0AFzGTyQRFfyebzSa62mw2rB+CqdVqkIFA4KEQpkxhzC0YDKZSKXTldDqxBhTKZDJo%0AUi6XQ8IHCT0pjGo4AdLtdgNzuVwsiZqAkf1BoiVWeijh9XqtViuUczgcFEI6YBip%0AuDY8yY4bvdntdlSG0+Px4DSFkskkmkRe8ZUnG40Gu32TyQStoiwGy3Gc6MQB7AZq%0A0gLyJPYTQ/9/43e7ncFgMJvN5OFJrDWul81moWdRMIwunU4nEgmsIZYJ1bBxuCq7%0AUhJ6Qk2OMXwG8XjcYrEolUqZTCaVSvV6PSr/Qv5lt9sNOwgtxGGQfQHv7MB/Z8ZR%0ADwAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABUAAAAUCAIAAADtKeFkAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABn0lEQVR4nLXTS4/BYBQGYL9dXUtRl4SdxsKOWEjqthA2ItKwkRIq%0AQSLikhLRUMybnkk7LjOYyZyFtPTp955zwnb5W9n+y5/P53a7PRwOcYHP5XL5nt/v%0A96lUql6vDwaDWCwmiuLxeByPx5qmveRR6/X6cDhsNhtgRVEymYzP58vlcs/96XSa%0AzWa9Xm86neIa3+x2O47jPB7Pc49j8RAyh4xKJBLNZlPX9VarVa1W5/M53rtarR57%0ApBUEIRwO8zwPzBmFYyHx62KxCAQCDMMEg0HKdeUx6nw+DxyJRPAcumVZ1uv1wuMW%0Ak6tUKsB2u93hcGA0t77T6QDjWLze7/cDQ7rdbpfLBYDhNxoN8rVa7TY/2o7H44hN%0AmQmTZIyaTCYIiLmqqvpgfuVymbCZGSc7nU7C0WgUI7wf9qffbrfomTLfY0Todrv3%0A2PKSJNHACCM2Ych0Oo3OH2LLY+GsUWbPKLxxNBp9J698Mpmk5ObAkAXb/hlbPpvN%0AmnuinmVZfootj78HDqeesd5SqfQKvnzdH9ZbKBSKxWK/38ee3/a/qw9HMD5S7hNe%0AEgAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABEAAAATCAIAAAD5x3GmAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABkElEQVR4nJWS6WrCYBBF8/5vImo1sS7RaF0DEhVBlIA7CuKOKCFx%0A7Wk+qzSoxfkRArln7p2ZSJf3S3r2wXGcxWIxGo3G4/F6vT6fz/8wk8mkXC6XSqVi%0AsVgoFLLZrGEYy+XyFdNsNpEC5PP5XC4H8+UWhg+Y3W5HKnxgAFBnMpl0Op1MJlVV%0AxZmQXqbdbne73ePxSBgcADRNA4jH459ukdDLIMVhtVptNhvCoE6lUjjEYjEARVH6%0A/b6X0XUdKeR+vx8MBqpbiUQiGo1GIpFwONzpdP4wTEIeGAZoNBqHw6HX65FKAKFQ%0AKBgMsv07czqdarUaCgagNy+VSsW27dlsRjyAQCBANvyvDNswTROpLMtEJwxPeuPJ%0ADiFbrRZdCHbfNcdGx5QkBqMfAE8xN+Ntt1uiir9BEjdhP4pbMB9uEUb4MFu1WmWT%0AtykkTsGl5N9CKhjU9H74l0hEvPVmSr/fz3JIz0oeAj8MOxVqATD6fD5/pr4yKPDx%0A+XwAHMeyrNfAdQfT6bRerw+Hwxd5vMy79Q177FfQwumNeAAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABIAAAAUCAIAAAAP9fodAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABpUlEQVR4nJWT2WrCUBCGff8ncBdt2rhWFEUbd1zBDSUXKiiCIIgS%0AENzwQvslR1PtRWv+i3DOyXyZmX9ObNe7LpfL9WXZzNV0Og0amkwmFrButyvLcigU%0AajQaFrDNZgPZbreXy6UF7HA4ACwWC03TLGCz2Ywi6W08HlvAOp2ObKharb6Kzedz%0AzJAk6d3QaDT6H1uv1+FwGObtLsg/jGHCOtZqtQiV7mLt8/kKhQKvdrtdNptNpVKJ%0AROLTUDwe3+/3OlYul0WSaDT6ZSiTydTrdV7hKqGcUw5uCc+2262OFYvFQCDg9/uT%0AyWSpVGKbz+ebzabATObDEOQNIwiGwgRGeSZGBJjIQ8NUBHk8HnUsl8vBeL1eGiAV%0AW0VRxBWje05giKZtvs725iRxMB6Ph3ZJBYMNtVpN+MbtYS0qpITT6XTDiINxuVyx%0AWIzyhCW/ht7r9QaDwdPc+Lbb7XY6nZFIRKRKp9OVSuURGw6Hqqo+YWRwOBx2ux2Y%0ApnGVJ7w5XArj6vX7fcz4wVarFV0Ju2gAx6iWX0FEMHHGjZ/cPs7P5/P18Spb0jfd%0ALqa0BRKC4AAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABMAAAASCAIAAAA2bnI+AAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAAB1UlEQVR4nJ2Tx6piURBF36ebGzMGDGBAcKYDcaYIigNFRDHnhBFE%0AxIy5e/GufQ09eXSNbqhVtWvXOV+//ze+fpi32Wx6vd50Or3f758kn0ajUS6XKxQK%0Au93ugwyHw2azWa/Xr9frTzKTyXi9XrvdbrVanU7nYrF4JUOh0K/vOB6Pb+RqtQKz%0A2WwWi8VoNOp0OlJfyWg0KpVKDQbDp9pGo0ErgVGr1SqVymQyXa9XkUylUhKJxOVy%0AvTlEmWw2ywww6FEoFDKZjGeEiHn7/Z6cyWTyRp7PZ0qKDKooz+tsNhO10R9v4N9I%0Aho7H43K5XGCEQPBwOHwl5/P5drt9Iw+HQywWe8UIpVLJAgVyPB6zqkQikUwm8/k8%0AGp9kOp2WvAc2ttvt2+0GHAgE3G43zmMbdrD2B8neqRqJRLAXb/lHHn50Oh1EQvp8%0APrZFLcHCfr//IFFPmcFggLzO36jVavV6/XK5QDocDq1WK1rY7XYfJO7zIgAobDab%0AYOVyuVKpMBIk3RhbtJ20B7lcLgFarRYMR0LAisViqVQSSI1G8+rfk+QegAlMtVoF%0Aw0lI1ApzMt6reQz1PH2n04nOrH74HdwmRsBY8WwGg0GPx+P3+9mNcCp/ej//jT9a%0AwllxGI5+1gAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABUAAAAUCAIAAADtKeFkAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABn0lEQVR4nLXTS4/BYBQGYL9dXUtRl4SdxsKOWEjqthA2ItKwkRIq%0AQSLikhLRUMybnkk7LjOYyZyFtPTp955zwnb5W9n+y5/P53a7PRwOcYHP5XL5nt/v%0A96lUql6vDwaDWCwmiuLxeByPx5qmveRR6/X6cDhsNhtgRVEymYzP58vlcs/96XSa%0AzWa9Xm86neIa3+x2O47jPB7Pc49j8RAyh4xKJBLNZlPX9VarVa1W5/M53rtarR57%0ApBUEIRwO8zwPzBmFYyHx62KxCAQCDMMEg0HKdeUx6nw+DxyJRPAcumVZ1uv1wuMW%0Ak6tUKsB2u93hcGA0t77T6QDjWLze7/cDQ7rdbpfLBYDhNxoN8rVa7TY/2o7H44hN%0AmQmTZIyaTCYIiLmqqvpgfuVymbCZGSc7nU7C0WgUI7wf9qffbrfomTLfY0Todrv3%0A2PKSJNHACCM2Ych0Oo3OH2LLY+GsUWbPKLxxNBp9J698Mpmk5ObAkAXb/hlbPpvN%0AmnuinmVZfootj78HDqeesd5SqfQKvnzdH9ZbKBSKxWK/38ee3/a/qw9HMD5S7hNe%0AEgAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABEAAAATCAIAAAD5x3GmAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABkElEQVR4nJWS6WrCYBBF8/5vImo1sS7RaF0DEhVBlIA7CuKOKCFx%0A7Wk+qzSoxfkRArln7p2ZSJf3S3r2wXGcxWIxGo3G4/F6vT6fz/8wk8mkXC6XSqVi%0AsVgoFLLZrGEYy+XyFdNsNpEC5PP5XC4H8+UWhg+Y3W5HKnxgAFBnMpl0Op1MJlVV%0AxZmQXqbdbne73ePxSBgcADRNA4jH459ukdDLIMVhtVptNhvCoE6lUjjEYjEARVH6%0A/b6X0XUdKeR+vx8MBqpbiUQiGo1GIpFwONzpdP4wTEIeGAZoNBqHw6HX65FKAKFQ%0AKBgMsv07czqdarUaCgagNy+VSsW27dlsRjyAQCBANvyvDNswTROpLMtEJwxPeuPJ%0ADiFbrRZdCHbfNcdGx5QkBqMfAE8xN+Ntt1uiir9BEjdhP4pbMB9uEUb4MFu1WmWT%0AtykkTsGl5N9CKhjU9H74l0hEvPVmSr/fz3JIz0oeAj8MOxVqATD6fD5/pr4yKPDx%0A+XwAHMeyrNfAdQfT6bRerw+Hwxd5vMy79Q177FfQwumNeAAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABIAAAAUCAIAAAAP9fodAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABpUlEQVR4nJWT2WrCUBCGff8ncBdt2rhWFEUbd1zBDSUXKiiCIIgS%0AENzwQvslR1PtRWv+i3DOyXyZmX9ObNe7LpfL9WXZzNV0Og0amkwmFrButyvLcigU%0AajQaFrDNZgPZbreXy6UF7HA4ACwWC03TLGCz2Ywi6W08HlvAOp2ObKharb6Kzedz%0AzJAk6d3QaDT6H1uv1+FwGObtLsg/jGHCOtZqtQiV7mLt8/kKhQKvdrtdNptNpVKJ%0AROLTUDwe3+/3OlYul0WSaDT6ZSiTydTrdV7hKqGcUw5uCc+2262OFYvFQCDg9/uT%0AyWSpVGKbz+ebzabATObDEOQNIwiGwgRGeSZGBJjIQ8NUBHk8HnUsl8vBeL1eGiAV%0AW0VRxBWje05giKZtvs725iRxMB6Ph3ZJBYMNtVpN+MbtYS0qpITT6XTDiINxuVyx%0AWIzyhCW/ht7r9QaDwdPc+Lbb7XY6nZFIRKRKp9OVSuURGw6Hqqo+YWRwOBx2ux2Y%0ApnGVJ7w5XArj6vX7fcz4wVarFV0Ju2gAx6iWX0FEMHHGjZ/cPs7P5/P18Spb0jfd%0ALqa0BRKC4AAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABAAAAAPCAIAAABiEdh4AAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABf0lEQVR4nH1S6+vBYBh9/3S3Ym5p0j6Q5JJC+7R8Eh8UkSK22Epq%0AK3IJm1yy4Xc0zdvK7/m03p3zPOc55yEvqp7P5+VyGY/HzWaz0WgMBoPdbodHGkOc%0AL0Dr9XogEPBQ5fV6c7ncarVyaB/CZrNhWdbzo9BlOBx+CYfDIRKJ0IhisTidTpfL%0A5Ww2K5fL9ihJkj6EarVKo3mev9/vnU4HMyuVyvF4FAQB79Fo9Ha7EV3XfT6fgw6F%0AQlhmv9/jEe05juv1esCEw2H8HY1GBFbQ7TOZjGVZ5/MZzHw+HwwGYdr1ei0UCvZw%0AAvtoQiqVMk1zu936/X5RFDGh3W6fTqd0Om3vRlqtlsvHxWJhGEY8HscEhmEmkwm0%0A2bJLpRJRFMVlYiKR0DQNkXW7XVmW5/O54zjkEAiIxWIuDvRks1lYhNToKFVVfdva%0A7/dtMb+Cswt6kPeb8Hg8arXa/2johLnf0wAH+pxAXNOwPYxyHx9qvV4j1GQyiR1A%0Axr3gLuASfbB/ZA7y1rGsdFsAAAAASUVORK5CYII=)

[r][1]

[r][2]

[r][3]

[r][4]

[Greedy]

[ Masked
Greedy]

![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABUAAAAUCAIAAADtKeFkAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABn0lEQVR4nLXTS4/BYBQGYL9dXUtRl4SdxsKOWEjqthA2ItKwkRIq%0AQSLikhLRUMybnkk7LjOYyZyFtPTp955zwnb5W9n+y5/P53a7PRwOcYHP5XL5nt/v%0A96lUql6vDwaDWCwmiuLxeByPx5qmveRR6/X6cDhsNhtgRVEymYzP58vlcs/96XSa%0AzWa9Xm86neIa3+x2O47jPB7Pc49j8RAyh4xKJBLNZlPX9VarVa1W5/M53rtarR57%0ApBUEIRwO8zwPzBmFYyHx62KxCAQCDMMEg0HKdeUx6nw+DxyJRPAcumVZ1uv1wuMW%0Ak6tUKsB2u93hcGA0t77T6QDjWLze7/cDQ7rdbpfLBYDhNxoN8rVa7TY/2o7H44hN%0AmQmTZIyaTCYIiLmqqvpgfuVymbCZGSc7nU7C0WgUI7wf9qffbrfomTLfY0Todrv3%0A2PKSJNHACCM2Ych0Oo3OH2LLY+GsUWbPKLxxNBp9J698Mpmk5ObAkAXb/hlbPpvN%0AmnuinmVZfootj78HDqeesd5SqfQKvnzdH9ZbKBSKxWK/38ee3/a/qw9HMD5S7hNe%0AEgAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABEAAAATCAIAAAD5x3GmAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABkElEQVR4nJWS6WrCYBBF8/5vImo1sS7RaF0DEhVBlIA7CuKOKCFx%0A7Wk+qzSoxfkRArln7p2ZSJf3S3r2wXGcxWIxGo3G4/F6vT6fz/8wk8mkXC6XSqVi%0AsVgoFLLZrGEYy+XyFdNsNpEC5PP5XC4H8+UWhg+Y3W5HKnxgAFBnMpl0Op1MJlVV%0AxZmQXqbdbne73ePxSBgcADRNA4jH459ukdDLIMVhtVptNhvCoE6lUjjEYjEARVH6%0A/b6X0XUdKeR+vx8MBqpbiUQiGo1GIpFwONzpdP4wTEIeGAZoNBqHw6HX65FKAKFQ%0AKBgMsv07czqdarUaCgagNy+VSsW27dlsRjyAQCBANvyvDNswTROpLMtEJwxPeuPJ%0ADiFbrRZdCHbfNcdGx5QkBqMfAE8xN+Ntt1uiir9BEjdhP4pbMB9uEUb4MFu1WmWT%0AtykkTsGl5N9CKhjU9H74l0hEvPVmSr/fz3JIz0oeAj8MOxVqATD6fD5/pr4yKPDx%0A+XwAHMeyrNfAdQfT6bRerw+Hwxd5vMy79Q177FfQwumNeAAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABIAAAAUCAIAAAAP9fodAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABpUlEQVR4nJWT2WrCUBCGff8ncBdt2rhWFEUbd1zBDSUXKiiCIIgS%0AENzwQvslR1PtRWv+i3DOyXyZmX9ObNe7LpfL9WXZzNV0Og0amkwmFrButyvLcigU%0AajQaFrDNZgPZbreXy6UF7HA4ACwWC03TLGCz2Ywi6W08HlvAOp2ObKharb6Kzedz%0AzJAk6d3QaDT6H1uv1+FwGObtLsg/jGHCOtZqtQiV7mLt8/kKhQKvdrtdNptNpVKJ%0AROLTUDwe3+/3OlYul0WSaDT6ZSiTydTrdV7hKqGcUw5uCc+2262OFYvFQCDg9/uT%0AyWSpVGKbz+ebzabATObDEOQNIwiGwgRGeSZGBJjIQ8NUBHk8HnUsl8vBeL1eGiAV%0AW0VRxBWje05giKZtvs725iRxMB6Ph3ZJBYMNtVpN+MbtYS0qpITT6XTDiINxuVyx%0AWIzyhCW/ht7r9QaDwdPc+Lbb7XY6nZFIRKRKp9OVSuURGw6Hqqo+YWRwOBx2ux2Y%0ApnGVJ7w5XArj6vX7fcz4wVarFV0Ju2gAx6iWX0FEMHHGjZ/cPs7P5/P18Spb0jfd%0ALqa0BRKC4AAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABAAAAAPCAIAAABiEdh4AAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABf0lEQVR4nH1S6+vBYBh9/3S3Ym5p0j6Q5JJC+7R8Eh8UkSK22Epq%0AK3IJm1yy4Xc0zdvK7/m03p3zPOc55yEvqp7P5+VyGY/HzWaz0WgMBoPdbodHGkOc%0AL0Dr9XogEPBQ5fV6c7ncarVyaB/CZrNhWdbzo9BlOBx+CYfDIRKJ0IhisTidTpfL%0A5Ww2K5fL9ihJkj6EarVKo3mev9/vnU4HMyuVyvF4FAQB79Fo9Ha7EV3XfT6fgw6F%0AQlhmv9/jEe05juv1esCEw2H8HY1GBFbQ7TOZjGVZ5/MZzHw+HwwGYdr1ei0UCvZw%0AAvtoQiqVMk1zu936/X5RFDGh3W6fTqd0Om3vRlqtlsvHxWJhGEY8HscEhmEmkwm0%0A2bJLpRJRFMVlYiKR0DQNkXW7XVmW5/O54zjkEAiIxWIuDvRks1lYhNToKFVVfdva%0A7/dtMb+Cswt6kPeb8Hg8arXa/2johLnf0wAH+pxAXNOwPYxyHx9qvV4j1GQyiR1A%0Axr3gLuASfbB/ZA7y1rGsdFsAAAAASUVORK5CYII=)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABIAAAAQCAIAAACUZLgLAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABnElEQVR4nJ2SN4sCYRBA/elrTpjFgAiKCREDojYWWoiFraERxF5U%0AzFnvwdzt6mJ1U+wOM/MmfobXv8Qgv8PhMJlMlstlrVbb7Xbom81GXPP5fDQaoUyn%0A0/F4/IHV63Wz2dzv9xVFGQ6HLper0WiIq1QqxWIxlEKhkEqlNOz5fCYSCbBOpwM2%0AGAwcDkc8HpcIokOhEDEwwmuYx+MBa7fbYNS02+1+vx873lwuJzqJwuGwhj0eD5vN%0AZjKZisUiWLVaJQWJBEun06JHo9FgMKhh9/sdTPkTo9HIlwoSQf9Op5PUtOr1eiXX%0Ab5OZTEb5lGw2KxjzMCqpfT6f2+3WMGQ2mzGPylitVtYtrkgkgut2u9EqZT8wZLVa%0AsTSYfD6/WCzEjbAGspzPZ0oxix5Dut0uGN93YyAQYEPH45GaFovlC9ZqtcCazaZq%0AIYjdsKT9fk9NFLrVY5VKBaxcLr9jjIRxu91SE+x6veoxpiKC+75jPDSM6/Waw4Kd%0ATic9xuOi+16vp1o4VzKZ5MpE80rY6pcmyX25XNQdqiQiCtcT4w9lFMJ4cnVaWgAA%0AAABJRU5ErkJggg==)

**[2.1 Reward
Model]**

**[2.2 Generative
Allocation]**

![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABUAAAAUCAIAAADtKeFkAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABn0lEQVR4nLXTS4/BYBQGYL9dXUtRl4SdxsKOWEjqthA2ItKwkRIq%0AQSLikhLRUMybnkk7LjOYyZyFtPTp955zwnb5W9n+y5/P53a7PRwOcYHP5XL5nt/v%0A96lUql6vDwaDWCwmiuLxeByPx5qmveRR6/X6cDhsNhtgRVEymYzP58vlcs/96XSa%0AzWa9Xm86neIa3+x2O47jPB7Pc49j8RAyh4xKJBLNZlPX9VarVa1W5/M53rtarR57%0ApBUEIRwO8zwPzBmFYyHx62KxCAQCDMMEg0HKdeUx6nw+DxyJRPAcumVZ1uv1wuMW%0Ak6tUKsB2u93hcGA0t77T6QDjWLze7/cDQ7rdbpfLBYDhNxoN8rVa7TY/2o7H44hN%0AmQmTZIyaTCYIiLmqqvpgfuVymbCZGSc7nU7C0WgUI7wf9qffbrfomTLfY0Todrv3%0A2PKSJNHACCM2Ych0Oo3OH2LLY+GsUWbPKLxxNBp9J698Mpmk5ObAkAXb/hlbPpvN%0AmnuinmVZfootj78HDqeesd5SqfQKvnzdH9ZbKBSKxWK/38ee3/a/qw9HMD5S7hNe%0AEgAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABEAAAATCAIAAAD5x3GmAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABkElEQVR4nJWS6WrCYBBF8/5vImo1sS7RaF0DEhVBlIA7CuKOKCFx%0A7Wk+qzSoxfkRArln7p2ZSJf3S3r2wXGcxWIxGo3G4/F6vT6fz/8wk8mkXC6XSqVi%0AsVgoFLLZrGEYy+XyFdNsNpEC5PP5XC4H8+UWhg+Y3W5HKnxgAFBnMpl0Op1MJlVV%0AxZmQXqbdbne73ePxSBgcADRNA4jH459ukdDLIMVhtVptNhvCoE6lUjjEYjEARVH6%0A/b6X0XUdKeR+vx8MBqpbiUQiGo1GIpFwONzpdP4wTEIeGAZoNBqHw6HX65FKAKFQ%0AKBgMsv07czqdarUaCgagNy+VSsW27dlsRjyAQCBANvyvDNswTROpLMtEJwxPeuPJ%0ADiFbrRZdCHbfNcdGx5QkBqMfAE8xN+Ntt1uiir9BEjdhP4pbMB9uEUb4MFu1WmWT%0AtykkTsGl5N9CKhjU9H74l0hEvPVmSr/fz3JIz0oeAj8MOxVqATD6fD5/pr4yKPDx%0A+XwAHMeyrNfAdQfT6bRerw+Hwxd5vMy79Q177FfQwumNeAAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABIAAAAUCAIAAAAP9fodAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABpUlEQVR4nJWT2WrCUBCGff8ncBdt2rhWFEUbd1zBDSUXKiiCIIgS%0AENzwQvslR1PtRWv+i3DOyXyZmX9ObNe7LpfL9WXZzNV0Og0amkwmFrButyvLcigU%0AajQaFrDNZgPZbreXy6UF7HA4ACwWC03TLGCz2Ywi6W08HlvAOp2ObKharb6Kzedz%0AzJAk6d3QaDT6H1uv1+FwGObtLsg/jGHCOtZqtQiV7mLt8/kKhQKvdrtdNptNpVKJ%0AROLTUDwe3+/3OlYul0WSaDT6ZSiTydTrdV7hKqGcUw5uCc+2262OFYvFQCDg9/uT%0AyWSpVGKbz+ebzabATObDEOQNIwiGwgRGeSZGBJjIQ8NUBHk8HnUsl8vBeL1eGiAV%0AW0VRxBWje05giKZtvs725iRxMB6Ph3ZJBYMNtVpN+MbtYS0qpITT6XTDiINxuVyx%0AWIzyhCW/ht7r9QaDwdPc+Lbb7XY6nZFIRKRKp9OVSuURGw6Hqqo+YWRwOBx2ux2Y%0ApnGVJ7w5XArj6vX7fcz4wVarFV0Ju2gAx6iWX0FEMHHGjZ/cPs7P5/P18Spb0jfd%0ALqa0BRKC4AAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABAAAAAPCAIAAABiEdh4AAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABf0lEQVR4nH1S6+vBYBh9/3S3Ym5p0j6Q5JJC+7R8Eh8UkSK22Epq%0AK3IJm1yy4Xc0zdvK7/m03p3zPOc55yEvqp7P5+VyGY/HzWaz0WgMBoPdbodHGkOc%0AL0Dr9XogEPBQ5fV6c7ncarVyaB/CZrNhWdbzo9BlOBx+CYfDIRKJ0IhisTidTpfL%0A5Ww2K5fL9ihJkj6EarVKo3mev9/vnU4HMyuVyvF4FAQB79Fo9Ha7EV3XfT6fgw6F%0AQlhmv9/jEe05juv1esCEw2H8HY1GBFbQ7TOZjGVZ5/MZzHw+HwwGYdr1ei0UCvZw%0AAvtoQiqVMk1zu936/X5RFDGh3W6fTqd0Om3vRlqtlsvHxWJhGEY8HscEhmEmkwm0%0A2bJLpRJRFMVlYiKR0DQNkXW7XVmW5/O54zjkEAiIxWIuDvRks1lYhNToKFVVfdva%0A7/dtMb+Cswt6kPeb8Hg8arXa/2johLnf0wAH+pxAXNOwPYxyHx9qvV4j1GQyiR1A%0Axr3gLuASfbB/ZA7y1rGsdFsAAAAASUVORK5CYII=)

[盲人按摩]

[小美][
KTV]

[绅士酒吧]

[黑八台球厅]

![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABUAAAAUCAIAAADtKeFkAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABn0lEQVR4nLXTS4/BYBQGYL9dXUtRl4SdxsKOWEjqthA2ItKwkRIq%0AQSLikhLRUMybnkk7LjOYyZyFtPTp955zwnb5W9n+y5/P53a7PRwOcYHP5XL5nt/v%0A96lUql6vDwaDWCwmiuLxeByPx5qmveRR6/X6cDhsNhtgRVEymYzP58vlcs/96XSa%0AzWa9Xm86neIa3+x2O47jPB7Pc49j8RAyh4xKJBLNZlPX9VarVa1W5/M53rtarR57%0ApBUEIRwO8zwPzBmFYyHx62KxCAQCDMMEg0HKdeUx6nw+DxyJRPAcumVZ1uv1wuMW%0Ak6tUKsB2u93hcGA0t77T6QDjWLze7/cDQ7rdbpfLBYDhNxoN8rVa7TY/2o7H44hN%0AmQmTZIyaTCYIiLmqqvpgfuVymbCZGSc7nU7C0WgUI7wf9qffbrfomTLfY0Todrv3%0A2PKSJNHACCM2Ych0Oo3OH2LLY+GsUWbPKLxxNBp9J698Mpmk5ObAkAXb/hlbPpvN%0AmnuinmVZfootj78HDqeesd5SqfQKvnzdH9ZbKBSKxWK/38ee3/a/qw9HMD5S7hNe%0AEgAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABAAAAAPCAIAAABiEdh4AAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABf0lEQVR4nH1S6+vBYBh9/3S3Ym5p0j6Q5JJC+7R8Eh8UkSK22Epq%0AK3IJm1yy4Xc0zdvK7/m03p3zPOc55yEvqp7P5+VyGY/HzWaz0WgMBoPdbodHGkOc%0AL0Dr9XogEPBQ5fV6c7ncarVyaB/CZrNhWdbzo9BlOBx+CYfDIRKJ0IhisTidTpfL%0A5Ww2K5fL9ihJkj6EarVKo3mev9/vnU4HMyuVyvF4FAQB79Fo9Ha7EV3XfT6fgw6F%0AQlhmv9/jEe05juv1esCEw2H8HY1GBFbQ7TOZjGVZ5/MZzHw+HwwGYdr1ei0UCvZw%0AAvtoQiqVMk1zu936/X5RFDGh3W6fTqd0Om3vRlqtlsvHxWJhGEY8HscEhmEmkwm0%0A2bJLpRJRFMVlYiKR0DQNkXW7XVmW5/O54zjkEAiIxWIuDvRks1lYhNToKFVVfdva%0A7/dtMb+Cswt6kPeb8Hg8arXa/2johLnf0wAH+pxAXNOwPYxyHx9qvV4j1GQyiR1A%0Axr3gLuASfbB/ZA7y1rGsdFsAAAAASUVORK5CYII=)

[盲人按摩]

[小美][
KTV]

[绅士酒吧]

[黑八台球厅]

![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABAAAAATCAIAAAAWBRqYAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABvElEQVR4nGNgwAaYGRmximMHQNU7LF0IKBLj4HQVl4awWRiZDtm4%0Aw6W8xWW4mFnQNYiwc2y3dAmUkkfTkCSnssLYhoOJGYsl0pzcO61c3MVlgGxDAWEg%0AGSGtsMrElo+FFafDZLl4zIVEBXk4FcX5gVw7YXEBVjYs6qyERD3EpIBIkJVNiJfz%0AaofX8+nBnvogqyxEJHyl5YFImpsHoaFAWatJwwCIVHn4bDUk3q7J+Li3ujVQGyiV%0ApardoWcKROYi4thdZacp+XY1SMO0WCNeTnacrkfW8GZ1OlDD836vy03OR8rtIi2V%0AgL7Cq2EVSMO7ddmvF8V93FfztNP5UosbTttAToJoWJ/9ekn85+Otzyf5Ah1poiSG%0ArpSbg21jrtXtVleghq/n+z4fb/t2ZfKPW7Peby54uybTVBlDg7ueNNAZH3aUAc37%0AeXfuzztzfsAQdg2Sgtz3Jgb+uDnz+43pIA135xLQAATpjqrvNuX/vIOiGqRhdYax%0AkigWDcxMTDuLbb9e6EfT8HxOlBAvF/ZQ0pMTfjYr4suZ7q9nur+A0YddFf2RBthV%0AQ4C3gUxvuEFfhEEvGJV5aXKwIvIDADTLxk8usulvAAAAAElFTkSuQmCC)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABIAAAAUCAIAAAAP9fodAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABpUlEQVR4nJWT2WrCUBCGff8ncBdt2rhWFEUbd1zBDSUXKiiCIIgS%0AENzwQvslR1PtRWv+i3DOyXyZmX9ObNe7LpfL9WXZzNV0Og0amkwmFrButyvLcigU%0AajQaFrDNZgPZbreXy6UF7HA4ACwWC03TLGCz2Ywi6W08HlvAOp2ObKharb6Kzedz%0AzJAk6d3QaDT6H1uv1+FwGObtLsg/jGHCOtZqtQiV7mLt8/kKhQKvdrtdNptNpVKJ%0AROLTUDwe3+/3OlYul0WSaDT6ZSiTydTrdV7hKqGcUw5uCc+2262OFYvFQCDg9/uT%0AyWSpVGKbz+ebzabATObDEOQNIwiGwgRGeSZGBJjIQ8NUBHk8HnUsl8vBeL1eGiAV%0AW0VRxBWje05giKZtvs725iRxMB6Ph3ZJBYMNtVpN+MbtYS0qpITT6XTDiINxuVyx%0AWIzyhCW/ht7r9QaDwdPc+Lbb7XY6nZFIRKRKp9OVSuURGw6Hqqo+YWRwOBx2ux2Y%0ApnGVJ7w5XArj6vX7fcz4wVarFV0Ju2gAx6iWX0FEMHHGjZ/cPs7P5/P18Spb0jfd%0ALqa0BRKC4AAAAABJRU5ErkJggg==)
![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACAAWABUDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD1ieZbeCSVvuopY1y2o6reR3RZ%0AJ2EbfNHt4BXtVzxJq4hjNnCQZGHzn0HpWPYwXd/B5YtnmgB4YEAofYn+Velh6KjD%0A2k1p5nVShaPNI6bRNTbUbRmkA8yM7Wx396KXRtKGl2zoX3u7bicY+gorhq8vO+TY%0A5525nbYgn8OW1zqLXUrsVY5MXYn61rxxrGgRFCqBgADAFFFKVSU0lJ7A5N6MdRRR%0AUEn/2Q==)

[list-wise]

[Feedback]

![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACABEAFQDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDyKiiigC9a6eskImuJGjRs7Aq5%0ALY79eBTLyx+zosscnmRMduSuCD6EVoW8bNpsDSoYVXKq8nCuMk8E8Z5P5Vbt7W2m%0AjX7UxeyWT99LG3CttwMeoG4ZI9RQBzNFdLfeEZPLFxpcy3UDDKqSN34EcN+n0qrp%0A/hXUL1/30bWsfcyqQ34L1pXRXJK9rFHS9Nk1S6MSMEVV3O552jp+PUVp33hZre1k%0Amt7nzzGNzIY9px3I5Na1rZWuiNI9vJLPalNlzN8pWNwRt6fU5Azjipbm/jezuBZZ%0AvJvLICwgttBGMkjpWcpT5lbY7aVOg6UnUfvf195wlFFFanAFFFFABWlo2lnUJ2d8%0AfZ4CpkAPzNnoo9zjHt1qO30i5uIVlzFEj/dMsgXd9B1/HpXU6dpcn9nfZbNkScAe%0AazZOxmHJyB8xxwuOMc5oGlfRGD4g1ATzC0iYGKE/OV+6z4xx7AcD8fWtDw7Y6h9l%0Al32xNq4LpvGCxxyAO4YcH8PSrKS6H4ZVwh+23ZGDwGII569F/DJrF1PxLf6luXf5%0AELfwRnr9T1NK5XKluzdgutM8N2XmIlyZrjP7ksex6HoBj1wTUmna3beIVfTriCWI%0Aygn5HJBA55I5H45FY0dyNY0mSCRGa7jAIdiAucgbixOBxwR3wKn0u4j8O6fczSxb%0A7uU7YnRgyEY6Ejpg8kd8Ck11LjUfw9DW1rTLtdCisdLgxEP9YpPzso5H1yeT34Fc%0Ato+oy6Jqe6RXVfuTRkYIGfT1FQ2ms31lO0sNw+Xbc6scq59xXQR69pWtRiHWbYRS%0A9BMOQPx6j6cii1gclJ3WhR8UaStpcLe2oU2tzz8vRWPPHseo/Gufr0JLG3tNDmtr%0Am6+0ac2BE6qWdcngDbkHnBB9a4+Tw/qcMLSvanaoycOpIH0BzQmKUHe6Rm0UUVRk%0AdJdRvcslxCDJDIqhCoztwANvHQjpip30++vbQW1lKFntlPnr5u3IY/KnpkYY8/3q%0A5mG5nt8+TNJHnrsYjP5V0VrPJf6eklnIYL23AUbOMnHQ46hv5/WgFa+pztxbTWkx%0AiuInikH8LDFRV1Vv4ntr2L7LrlqrL/z0Reh9x1H1H5Uy98L28kIu9Lvo2tj18xvu%0A/Qj+RAP1pX7l8t9Y6lDS0abT5Y4hl1k3lR1YY7euP61JcI8Gl3HmgqJQqop6khgc%0A49AAefen65Mlpa29hArKoAkBIwQOcH6nJJ/AUnhfUI4tQe2uwHjvF8os3PPYZ9Dn%0AH5elMlK7MGlVS7BVBLE4AA5JrqG8GSG+lBuEis1OVc8tj0I7Y6ZOKkOqaLoA26ZD%0A9qucYMrHj/vr+i4+tK/Yvka+LQg0fTr3TJobu/Qw2m7GJGxsYghWK9uTjnpmulH7%0Ak+bKwjjT5mkY4AHrmqVtdywaQ2r6yQ7N80MGMKAegA9W9TkgfjXCyzvMzFjhSxbY%0AOFH0Has5QU3dnXRxLw8XGKvcW5eOS6meFNkbOSif3RngUVFRWpwBVrT7w2N0smNy%0AH5XX+8p/r3HuKq0UAbOtWavNDc2uZDcgkqgzkjHzceueR65qeweDTNIaaRleeY8w%0Ak8kg4VSOuP4ifoKitmaDTbfycoJQzOynliGI5+gA496sDUGs44L+WNZ5YZdieZ1w%0AVPf2OD7ZoA3NPFxrViU1jTYwirlGPyk/QdV+owKlstPs9MtftOm2a3kobgiZSzDv%0Ah8ED0wBXG6lr99qmVml2Qn/llHwv4+v41WstRu9Ok8y0neInqAeG+o6Gpsb+0jta%0A/n1Ornv49a0i9tb7bZXkTF/LOVzjOOD19CPXBrG8PaObvVkS9jKQxoZisgK+YBjg%0Afnz7A1taNqZ1q5kuriOJbm0jCx7QejHlufTA/wC+jWlcRLfWs0Fxl4yhJzztwPvD%0A6Vm5qLUTohh5Vqbq32/GxynifWv7TvPJgb/RYCQmOjnu309PasOiitjhbbd2FFFF%0AAgooooAs21/NaoUTa0ZOSjjIz/Sm3N5NdlfNb5VGFVRhR+FFFAEFFFFAE9neT2Fw%0As9tIUkHGeoI9CO4q9e+Ir69tzA7JHG33hGu3d9aKKVkUpNKyehlUUUUyQooooA//2Q==)

[Pay
Net]

![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACAAYABUDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD1mWaOCMySuqIOrMcAVh/27MkJ%0AnzbXMMYYy/Z2yw5woA/LJrn/ABdrDXN81pG37mA4IH8Td/y6VR8Kx3U+vQtbhtiZ%0A8xwOAMdDTPdo5clh3WqPpe3l693/AJHo1nci7tIpwpTeoYo3Vc9jRWZofh/+x/tL%0APdPcPcMGLEYxjPv1560UHj1VBTag7o5248F31xqshMsYtXkLGTPzYJ9PWu2tLSCy%0At0gto1jjQYAAoooNq+Mq4iMYzeiJqKKKRyH/2Q==)
![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACAAWABUDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD1ieZbeCSVvuopY1y2o6reR3RZ%0AJ2EbfNHt4BXtVzxJq4hjNnCQZGHzn0HpWPYwXd/B5YtnmgB4YEAofYn+Velh6KjD%0A2k1p5nVShaPNI6bRNTbUbRmkA8yM7Wx396KXRtKGl2zoX3u7bicY+gorhq8vO+TY%0A5525nbYgn8OW1zqLXUrsVY5MXYn61rxxrGgRFCqBgADAFFFKVSU0lJ7A5N6MdRRR%0AUEn/2Q==)
![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACAAYABUDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD0vV5pIRB5V0LfLYOVzu9uhrRr%0AktclkuNWeEthYwAo9OAavnVbi00m2k+WR2JUs4Pbp3q3HRHQ6b5Y2N6is7RtQl1G%0ACSSVVXa20bQfSipatoYSTi7Mx9U02+fWJZ4bcyRsBggj0ArRSzk/sDyZ7ZnlIbCA%0AglSc4PWiim5N2NHUbSXY0LCNYrRFSEw4HKHGc/hRRRUmbd3c/9k=)
![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACAAUABUDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDuLq8lXUmuI5tu0bRGznb9SPWl%0At9ZunvYlkljMZbBRByaw9Z+TU7lf+mhP50aXZvcFZIXCTb8KxbAX3r5NVsQ6zUZP%0Afb5n0X1an7FTl27eR39Fcha+Irmxkntro/aTG20OD+fPeivoI4yk1d6HlSwNZOyV%0AzX1XQ7S9l8596OeCUIGf0qKz0G1toH2PMdxz8zDj9KKKHQpe0cuVXJVeqqajzOxd%0As9HsrVWaOBS0nLFhnP8AnNFFFdEYRSskc8qk5O7bP//Z)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAAB8AAAAkCAIAAAD+cXYAAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABDElEQVR4nLWVURLDIAhEtcn9z1wzThi6QWCx4aPTIDwQEVvLyUdJ%0A7z3pFUi/xdTvojVIcp/K4zjO85yfdXRoQwfIoKfIVjh63qFyBkkfMeMChFv+Axfa%0A5skdMjqHpptovVRJvC1qAvnCUiX3V+xZ+ri0lP0lVL/TXJZOxHj3rrJ9tjsJVv7F%0AQR9WcyxVBiQEMP3n67H1Qpl3Uoesz1792rXf8TCynm+3KFNhVicJegkJ+/NiwJDS%0Aps40Nh2XaFMvf6BJHPsU+lmocPwaZma9VgXJXAWPC+nrABDSoV+/GTqAnLqjY+Y6%0AdEsyLll6qNmiQ9Zhq9D01edu7u1xqhzdPDezHUUzB5nvksm7Ll8tbwNzyNXOBQAA%0AAABJRU5ErkJggg==)

[User Behavior]

[Sequence]

[15:00-17:00]

[17:30-18:30]

[19:00-21:00]

[21:00-22:00]

# Loss[NTP]**

[High-value]

[Session]

# RM[pCTR]**

# RM[pCVR]**

![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACABEAFQDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDyKiiigC9a6eskImuJGjRs7Aq5%0ALY79eBTLyx+zosscnmRMduSuCD6EVoW8bNpsDSoYVXKq8nCuMk8E8Z5P5VahtoJo%0AV+0HfZrJ++kRuFbbgYPcDcMkeooA5qiuhvfCz7BNpsouIWGVUkbj9COG/T6VWsfD%0AN/dv++ja2T1kU7vwXr/Kp5la4uZblLTtPfUbgxIwQKu5mPYf5NaF54ca3tnlguPO%0AKDJQx7Tj1HJrVgsrfRy8lu8k1uV23EuVIjYEY6fU5AzjipLi8R7ScWYN3LsI2wgt%0AtB4ycdKxlKfMuVaGMpT51yrQ4uiiiug3CiiigArS0bSzqE7O+Ps8BUyAH5mz0Ue5%0Axj261Hb6Rc3EKy5iiR/umWQLu+g6/j0rp7HTnTTfstoyJPgCVmBOxmHJyB8xxwuO%0AMc5o2E3Yw/EGoCeYWkTAxQn5yv3WfGOPYDgfj61e8PWl8LaUSW5+zONyBxgscYIA%0A7hhwfwqdJdH8Oq4Q/arojB4DEEc9ei8+mTWPqPiK9v8AcofyYm/gjPJ+p6mpu3sK%0A7extx3GneHrTeq3HmXGf3RYk8Hoewx64Jp1hrFvrivYTQyxmQEkK5IIHPUcj8QRW%0AZHcjWNJkgkRmu4wCHYgLnIG4sTgccEd8CpNOmTw/YXE0sW+5kO2N0YMpGOASOmDy%0AR3wKlwW/UTit+ppatp10uix2emwkRj76E4dlHIx65PJ79K5vSb+TR9RzKrKp+SVC%0AMEDPp6iorXVr2zmaSKdiXbc6scq59xW4mt6Zq6CLVbcRydBJ1A+jdR9ORQk0rPUS%0ATSs9Sl4j0xbadby22m2n5+XorHnj2PX86w67tbK3tdHmguLnzrFuI2ClmGTwBtyD%0Azgg+tcnJomoRRGR7c7VGThlJH4A5pqXcal3KFFFFWWdJdRvcslxCDJDIqhCoztwA%0ANvHQjpipZbK8u7Vbe0lCz26nzl8zbkMflQ9sjDHn+9XNw3M9vnyZpI89djEZ/Kui%0AtZ31DTke0fyb23AUbOMnHQ+ob+f1ofkI5y4t5rWUxXEbRuP4WGKjrprfxHb3kf2b%0AWbZWX++q8A+46j6j8qbd+GoJIxc6deIbc9fMbIH0I/kQD9anm7iv3Kelo02nyxxD%0ALrJvKjqwx29cf1qS4R4NLuPNBUShVRT1JDA5x6AA8+9P1ydLS0t7CBSqjEgJGCBz%0Ag/U5JP4Co/Dd8kd+9vcgOl2vlZbnnsPoc4/L0qm7IbMSlVS7BVBLE4AHU10h8JOb%0A2X9+sdoOVY8tj0x2x0yaedS0jRF26fF9ouMYMhb/ANm/oOPep5k9tRcyexBpen3m%0AnSRXF8pitt33Xb7rEEBivbk459a6D/VfvJGCInLOTgAfWq9tdyQaW2p6qQxPzRQg%0AYVQfugD1b1OSB+NcVLM8rMWOAzFto4UfQVjKn7R3fQylT9o7voFw6SXErxJsjZyV%0AX+6M8Cio6K6DcKtafeGxulkxuQ/K6/3lP9e49xVWigDZ1qzV5obm1zIbkElUGckY%0A+bj1zyPXNT2DQaZpDTSFXnmPMJPJIOFUjrj+In6CorZmg0238nKCUMzsp5YhiOfo%0AAOPepzfG1ihvpY1mlhl2Jv64Knv7HH50Aa9gJ9WsfL1XTkCgZQkFS30HVfqMCpLK%0AwtNOtvtFhaLdPngiYbmHcB8ED0wBXJ6hrd7qOVlk2RH/AJZpwp+vr+NVrO/urBy9%0ArM0eeoHRvqOhrLkfcz5WdPLeR6tpd5a3mLO6jJfyySOnTr19MfQ1k6DpButUVbuI%0ArFGhlKSAr5gGOP1yfYGtbStR/teaS4njjW4tYwse0HoTy3PTGB/30avzRLe28sE+%0AXjZSTnnbgfeH0qHUUHyWM3UUHyWOb8R6v/aF15MLZtoSQpHR27t9PT2+tYtFFbpW%0A0N0raBRRRTGFFFFAFm2v5rVCibWjJyUcZGf6U25vJrsr5rfKowqqMKPwoooAgooo%0AoAmtLuaynE1u5Rxx6gj0I71du9evLu3MDFI42+8I1xu+tFFKy3FZbmZRRRTGFFFF%0AAH//2Q==)
![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACABEAFQDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDyKiiigC9a6eskImuJGjRs7Aq5%0ALY79eBTLyx+zosscnmRMduSuCD6EVoW8bNpsDSoYVXKq8nCuMk8E8Z5P5VahtoJo%0AV+0HfZrJ++kRuFbbgc9SBuGSPUUAc1RXQXnhd9gm06UTxMMqrEZP0I4b9PpVey8N%0AX10486NrZP8Apop3H6L1/lVcrvYvkle1ilp9g+oTmNWCKq7mY9h0/HrV668PmG3e%0AWC4MrINxQx7SR7cmtaKxt9KDvA8ktuV23EpKlY2BGOn1OQM44pZbhWtpvsg+1S7D%0A8sI3Yzxk46fzraMYcr5tzpp06Xs25vU4+iiiuc4wooooAK0tG0s6hOzvj7PAVMgB%0A+Zs9FHucY9utR2+kXNxCsuYokf7plkC7voOv49K6ax09k037LasqT4AkYgkozDkj%0AA+Y44XHGOc0bjSbdkYniDUBPMLSJgYoT85X7rPjHHsBwPx9aveHrS9W2lElufszj%0AcgccscYIA6kMOD+HpUyS6R4dVwh+03JGDwrMO/XonPpk1j6h4hvb7cobyYm6pGeT%0A9T1NXZLcvljH4mbaT6foFnuVbjfcZ/dFiTweh6KMeuCaXT9WttaWSwlhljMqkkBy%0AQQOfvDkfiCKzo7kaxpMkEiM13GAQ7EBc5A3FicDjgjvgVJp8qaBYTyzR77mQ7Y2R%0AgyMMcAkdMHkjvgU4ze3QqNR/C9jR1XT7oaNHaadAVT+JCcOyjkY9cnk9+lc5pV/J%0Ao+o5lV1U/JKhGCBn09QaitdWvbSZpI52O9tzq/KufcVuJrem6tGItUgEb9BJyQPo%0A33l+nIobTd1oDkpO60ZS8R6YttMLy3Cm3n5O3orHnj2PUfjWHXdLZW9tpEsFxc+b%0AYnARtpZgCeANuQ2Dgg+ueK5R9Fv442kaD5VGTh1JA+gOaUovoKcHe6RQoooqDI6S%0A6je5ZLiEGSGRVCFRnbgAbeOhHTFSzWV5dWi29pMEnt1PnDzNuQx+VD2yMMef71c3%0ADcz2+fJmkjz12MRn8q6K1nfUNOR7R/JvLcBRs4ycdCO4b+f1przGrX1OduLaa1lM%0AVxE0bj+Fhioq6WDxFb3cX2bWLdSv99VyM+pHUH3X8qbd+G4JIxc6fdp5B6+Y2QPo%0Aw/kQD9afL2K5L6x1Kmlo02nyxxDLrJvKjqwx29cf1qS4R4NLuPNBUShVRT1JDA5x%0A6AA8+9P1ydLS0t7CBSqgCQEjBA5wfqckn8BUfhu+RL57a5wyXa+Xlueeyn2Ocfl6%0AUl2JSu7GJSqpdgqglicAAck10R8KM15JidY7UHKn7z49CO2OmTipTqOk6INunxfa%0AJ+hk3fzb+i4+tVytb6F+za+LQg0vT7zT3juL0GG2z913+4xBCsV7cnHPTNbSqYmD%0AyERohyXY4AHrmmW13Jb6WdS1TBJ+eOEDCqD90AerepyQPxri5ZnmZixwGYttHCj6%0ACtI1ORWR0QrexVktwuHSS5leJNkbOSq/3RngUVHRWBxhVrT7w2N0smNyH5XX+8p/%0Ar3HuKq0UAbOtWavNDc2uZDcgkqgzkjHzceueR65qewaDTNIaaRleeY8wk8kg4VSO%0AuP4ifoKitmaDTbfycoJQzOynliGI5+gA496nN8bWKG+lQTSwy7EL9cFT39jj6ZoA%0A1bET6pYmPVNPQADKA5Bb6Dqv1BAqWysbXTrcz2Votw+eCJl3OOcgPggemAK5PUNb%0AvNRBWSTZEf8AlmnAP19fxqvZ39zYOWtZmjz94DkN9R0Na867HR7SPa/n1Omku49V%0A0u8tboLZXMZL+USV6ZxwevoR64NZWg6Q11qardxMscaGUpICvmAY4+nOT7A1qaZq%0AR1V5LiWONLi1QLHtB6MeW56YwB/wI1cZPtkMlvPl4mUls87cD7w9xVqDmuY1VJ1I%0Aupfb8bdzn/EWrfb7ryIWzbQk4I6O3dvp2Ht9axqKKwbu7nG227sKKKKQgooooAs2%0A1/NaoUTa0ZOSjjIz/Sm3N5NdlfNb5VGFVRhR+FFFAEFFFFAE1pdzWU4mt32OOPUE%0AehHerl1rt3dW5hPlxo33hGuN31NFFO72GpNKyZm0UUUhBRRRQB//2Q==)
![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACABEAFQDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDyKiiigC9a6eskImuJGjRs7Aq5%0ALY79eBTLyx+zosscnmRMduSuCD6EVoW8bNpsDSoYVXKq8nCuMk8E8Z5P5Vbt7W2m%0AjX7UxeyWT99LG3CttwMeoG4ZI9RQBzNFdLfeEZPLFxpcy3UDDKqSN34EcN+n0qrp%0A/hXUL1/30bWsfcyqQ34L1pXRXJK9rFHS9Nk1S6MSMEVV3O552jp+PUVp33hZre1k%0Amt7nzzGNzIY9px3I5Na1rZWuiNI9vJLPalNlzN8pWNwRt6fU5Azjipbm/jezuBZZ%0AvJvLICwgttBGMkjpWcpT5lbY7aVOg6UnUfvf195wlFFFanAFFFFABWlo2lnUJ2d8%0AfZ4CpkAPzNnoo9zjHt1qO30i5uIVlzFEj/dMsgXd9B1/HpXU6dpcn9nfZbNkScAe%0AazZOxmHJyB8xxwuOMc5oGlfRGD4g1ATzC0iYGKE/OV+6z4xx7AcD8fWtDw7Y6h9l%0Al32xNq4LpvGCxxyAO4YcH8PSrKS6H4ZVwh+23ZGDwGII569F/DJrF1PxLf6luXf5%0AELfwRnr9T1NK5XKluzdgutM8N2XmIlyZrjP7ksex6HoBj1wTUmna3beIVfTriCWI%0Aygn5HJBA55I5H45FY0dyNY0mSCRGa7jAIdiAucgbixOBxwR3wKn0u4j8O6fczSxb%0A7uU7YnRgyEY6Ejpg8kd8Ck11LjUfw9DW1rTLtdCisdLgxEP9YpPzso5H1yeT34Fc%0Ato+oy6Jqe6RXVfuTRkYIGfT1FQ2ms31lO0sNw+Xbc6scq59xXQR69pWtRiHWbYRS%0A9BMOQPx6j6cii1gclJ3WhR8UaStpcLe2oU2tzz8vRWPPHseo/Gufr0JLG3tNDmtr%0Am6+0ac2BE6qWdcngDbkHnBB9a4+Tw/qcMLSvanaoycOpIH0BzQmKUHe6Rm0UUVRk%0AdJdRvcslxCDJDIqhCoztwANvHQjpip30++vbQW1lKFntlPnr5u3IY/KnpkYY8/3q%0A5mG5nt8+TNJHnrsYjP5V0VrPJf6eklnIYL23AUbOMnHQ46hv5/WgFa+pztxbTWkx%0AiuInikH8LDFRV1Vv4ntr2L7LrlqrL/z0Reh9x1H1H5Uy98L28kIu9Lvo2tj18xvu%0A/Qj+RAP1pX7l8t9Y6lDS0abT5Y4hl1k3lR1YY7euP61JcI8Gl3HmgqJQqop6khgc%0A49AAefen65Mlpa29hArKoAkBIwQOcH6nJJ/AUnhfUI4tQe2uwHjvF8os3PPYZ9Dn%0AH5elMlK7MGlVS7BVBLE4AA5JrqG8GSG+lBuEis1OVc8tj0I7Y6ZOKkOqaLoA26ZD%0A9qucYMrHj/vr+i4+tK/Yvka+LQg0fTr3TJobu/Qw2m7GJGxsYghWK9uTjnpmulH7%0Ak+bKwjjT5mkY4AHrmqVtdywaQ2r6yQ7N80MGMKAegA9W9TkgfjXCyzvMzFjhSxbY%0AOFH0Has5QU3dnXRxLw8XGKvcW5eOS6meFNkbOSif3RngUVFRWpwBVrT7w2N0smNy%0AH5XX+8p/r3HuKq0UAbOtWavNDc2uZDcgkqgzkjHzceueR65qeweDTNIaaRleeY8w%0Ak8kg4VSOuP4ifoKitmaDTbfycoJQzOynliGI5+gA496sDUGs44L+WNZ5YZdieZ1w%0AVPf2OD7ZoA3NPFxrViU1jTYwirlGPyk/QdV+owKlstPs9MtftOm2a3kobgiZSzDv%0Ah8ED0wBXG6lr99qmVml2Qn/llHwv4+v41WstRu9Ok8y0neInqAeG+o6Gpsb+0jta%0A/n1Ornv49a0i9tb7bZXkTF/LOVzjOOD19CPXBrG8PaObvVkS9jKQxoZisgK+YBjg%0Afnz7A1taNqZ1q5kuriOJbm0jCx7QejHlufTA/wC+jWlcRLfWs0Fxl4yhJzztwPvD%0A6Vm5qLUTohh5Vqbq32/GxynifWv7TvPJgb/RYCQmOjnu309PasOiitjhbbd2FFFF%0AAgooooAs21/NaoUTa0ZOSjjIz/Sm3N5NdlfNb5VGFVRhR+FFFAEFFFFAE9neT2Fw%0As9tIUkHGeoI9CO4q9e+Ir69tzA7JHG33hGu3d9aKKVkUpNKyehlUUUUyQooooA//2Q==)
![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACABEAFQDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDyKiiigC9a6eskImuJGjRs7Aq5%0ALY79eBTLyx+zosscnmRMduSuCD6EVoW8bNpsDSoYVXKq8nCuMk8E8Z5P5VahtoJo%0AV+0HfZrJ++kRuFbbgYPcDcMkeooA5qiuhvfCz7BNpsouIWGVUkbj9COG/T6VWsfD%0AN/dv++ja2T1kU7vwXr/Kp5la4uZblLTtPfUbgxIwQKu5mPYf5NaF54ca3tnlguPO%0AKDJQx7Tj1HJrVgsrfRy8lu8k1uV23EuVIjYEY6fU5AzjipLi8R7ScWYN3LsI2wgt%0AtB4ycdKxlKfMuVaGMpT51yrQ4uiiiug3CiiigArS0bSzqE7O+Ps8BUyAH5mz0Ue5%0Axj261Hb6Rc3EKy5iiR/umWQLu+g6/j0rp7HTnTTfstoyJPgCVmBOxmHJyB8xxwuO%0AMc5o2E3Yw/EGoCeYWkTAxQn5yv3WfGOPYDgfj61e8PWl8LaUSW5+zONyBxgscYIA%0A7hhwfwqdJdH8Oq4Q/arojB4DEEc9ei8+mTWPqPiK9v8AcofyYm/gjPJ+p6mpu3sK%0A7extx3GneHrTeq3HmXGf3RYk8Hoewx64Jp1hrFvrivYTQyxmQEkK5IIHPUcj8QRW%0AZHcjWNJkgkRmu4wCHYgLnIG4sTgccEd8CpNOmTw/YXE0sW+5kO2N0YMpGOASOmDy%0AR3wKlwW/UTit+ppatp10uix2emwkRj76E4dlHIx65PJ79K5vSb+TR9RzKrKp+SVC%0AMEDPp6iorXVr2zmaSKdiXbc6scq59xW4mt6Zq6CLVbcRydBJ1A+jdR9ORQk0rPUS%0ATSs9Sl4j0xbadby22m2n5+XorHnj2PX86w67tbK3tdHmguLnzrFuI2ClmGTwBtyD%0Azgg+tcnJomoRRGR7c7VGThlJH4A5pqXcal3KFFFFWWdJdRvcslxCDJDIqhCoztwA%0ANvHQjpipZbK8u7Vbe0lCz26nzl8zbkMflQ9sjDHn+9XNw3M9vnyZpI89djEZ/Kui%0AtZ31DTke0fyb23AUbOMnHQ+ob+f1ofkI5y4t5rWUxXEbRuP4WGKjrprfxHb3kf2b%0AWbZWX++q8A+46j6j8qbd+GoJIxc6deIbc9fMbIH0I/kQD9anm7iv3Kelo02nyxxD%0ALrJvKjqwx29cf1qS4R4NLuPNBUShVRT1JDA5x6AA8+9P1ydLS0t7CBSqjEgJGCBz%0Ag/U5JP4Co/Dd8kd+9vcgOl2vlZbnnsPoc4/L0qm7IbMSlVS7BVBLE4AHU10h8JOb%0A2X9+sdoOVY8tj0x2x0yaedS0jRF26fF9ouMYMhb/ANm/oOPep5k9tRcyexBpen3m%0AnSRXF8pitt33Xb7rEEBivbk459a6D/VfvJGCInLOTgAfWq9tdyQaW2p6qQxPzRQg%0AYVQfugD1b1OSB+NcVLM8rMWOAzFto4UfQVjKn7R3fQylT9o7voFw6SXErxJsjZyV%0AX+6M8Cio6K6DcKtafeGxulkxuQ/K6/3lP9e49xVWigDZ1qzV5obm1zIbkElUGckY%0A+bj1zyPXNT2DQaZpDTSFXnmPMJPJIOFUjrj+In6CorZmg0238nKCUMzsp5YhiOfo%0AAOPepzfG1ihvpY1mlhl2Jv64Knv7HH50Aa9gJ9WsfL1XTkCgZQkFS30HVfqMCpLK%0AwtNOtvtFhaLdPngiYbmHcB8ED0wBXJ6hrd7qOVlk2RH/AJZpwp+vr+NVrO/urBy9%0ArM0eeoHRvqOhrLkfcz5WdPLeR6tpd5a3mLO6jJfyySOnTr19MfQ1k6DpButUVbuI%0ArFGhlKSAr5gGOP1yfYGtbStR/teaS4njjW4tYwse0HoTy3PTGB/30avzRLe28sE+%0AXjZSTnnbgfeH0qHUUHyWM3UUHyWOb8R6v/aF15MLZtoSQpHR27t9PT2+tYtFFbpW%0A0N0raBRRRTGFFFFAFm2v5rVCibWjJyUcZGf6U25vJrsr5rfKowqqMKPwoooAgooo%0AoAmtLuaynE1u5Rxx6gj0I71du9evLu3MDFI42+8I1xu+tFFKy3FZbmZRRRTGFFFF%0AAH//2Q==)
![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACABEAFQDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDyKiiigC9a6eskImuJGjRs7Aq5%0ALY79eBTLyx+zosscnmRMduSuCD6EVoW8bNpsDSoYVXKq8nCuMk8E8Z5P5VahtoJo%0AV+0HfZrJ++kRuFbbgc9SBuGSPUUAc1RXQXnhd9gm06UTxMMqrEZP0I4b9PpVey8N%0AX10486NrZP8Apop3H6L1/lVcrvYvkle1ilp9g+oTmNWCKq7mY9h0/HrV668PmG3e%0AWC4MrINxQx7SR7cmtaKxt9KDvA8ktuV23EpKlY2BGOn1OQM44pZbhWtpvsg+1S7D%0A8sI3Yzxk46fzraMYcr5tzpp06Xs25vU4+iiiuc4wooooAK0tG0s6hOzvj7PAVMgB%0A+Zs9FHucY9utR2+kXNxCsuYokf7plkC7voOv49K6ax09k037LasqT4AkYgkozDkj%0AA+Y44XHGOc0bjSbdkYniDUBPMLSJgYoT85X7rPjHHsBwPx9aveHrS9W2lElufszj%0AcgccscYIA6kMOD+HpUyS6R4dVwh+03JGDwrMO/XonPpk1j6h4hvb7cobyYm6pGeT%0A9T1NXZLcvljH4mbaT6foFnuVbjfcZ/dFiTweh6KMeuCaXT9WttaWSwlhljMqkkBy%0AQQOfvDkfiCKzo7kaxpMkEiM13GAQ7EBc5A3FicDjgjvgVJp8qaBYTyzR77mQ7Y2R%0AgyMMcAkdMHkjvgU4ze3QqNR/C9jR1XT7oaNHaadAVT+JCcOyjkY9cnk9+lc5pV/J%0Ao+o5lV1U/JKhGCBn09QaitdWvbSZpI52O9tzq/KufcVuJrem6tGItUgEb9BJyQPo%0A33l+nIobTd1oDkpO60ZS8R6YttMLy3Cm3n5O3orHnj2PUfjWHXdLZW9tpEsFxc+b%0AYnARtpZgCeANuQ2Dgg+ueK5R9Fv442kaD5VGTh1JA+gOaUovoKcHe6RQoooqDI6S%0A6je5ZLiEGSGRVCFRnbgAbeOhHTFSzWV5dWi29pMEnt1PnDzNuQx+VD2yMMef71c3%0ADcz2+fJmkjz12MRn8q6K1nfUNOR7R/JvLcBRs4ycdCO4b+f1przGrX1OduLaa1lM%0AVxE0bj+Fhioq6WDxFb3cX2bWLdSv99VyM+pHUH3X8qbd+G4JIxc6fdp5B6+Y2QPo%0Aw/kQD9afL2K5L6x1Kmlo02nyxxDLrJvKjqwx29cf1qS4R4NLuPNBUShVRT1JDA5x%0A6AA8+9P1ydLS0t7CBSqgCQEjBA5wfqckn8BUfhu+RL57a5wyXa+Xlueeyn2Ocfl6%0AUl2JSu7GJSqpdgqglicAAck10R8KM15JidY7UHKn7z49CO2OmTipTqOk6INunxfa%0AJ+hk3fzb+i4+tVytb6F+za+LQg0vT7zT3juL0GG2z913+4xBCsV7cnHPTNbSqYmD%0AyERohyXY4AHrmmW13Jb6WdS1TBJ+eOEDCqD90AerepyQPxri5ZnmZixwGYttHCj6%0ACtI1ORWR0QrexVktwuHSS5leJNkbOSq/3RngUVHRWBxhVrT7w2N0smNyH5XX+8p/%0Ar3HuKq0UAbOtWavNDc2uZDcgkqgzkjHzceueR65qewaDTNIaaRleeY8wk8kg4VSO%0AuP4ifoKitmaDTbfycoJQzOynliGI5+gA496nN8bWKG+lQTSwy7EL9cFT39jj6ZoA%0A1bET6pYmPVNPQADKA5Bb6Dqv1BAqWysbXTrcz2Votw+eCJl3OOcgPggemAK5PUNb%0AvNRBWSTZEf8AlmnAP19fxqvZ39zYOWtZmjz94DkN9R0Na867HR7SPa/n1Omku49V%0A0u8tboLZXMZL+USV6ZxwevoR64NZWg6Q11qardxMscaGUpICvmAY4+nOT7A1qaZq%0AR1V5LiWONLi1QLHtB6MeW56YwB/wI1cZPtkMlvPl4mUls87cD7w9xVqDmuY1VJ1I%0Aupfb8bdzn/EWrfb7ryIWzbQk4I6O3dvp2Ht9axqKKwbu7nG227sKKKKQgooooAs2%0A1/NaoUTa0ZOSjjIz/Sm3N5NdlfNb5VGFVRhR+FFFAEFFFFAE1pdzWU4mt32OOPUE%0AehHerl1rt3dW5hPlxo33hGuN31NFFO72GpNKyZm0UUUhBRRRQB//2Q==)
![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACABeAHQDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDyKiiigAq1a6dPdo0ibEjU4LyM%0AFGfT3qrW9MNkNtGo2xiBGA7ZKgk/mTQBmXGm3FtEZTseMHBaNg2Pr3FVK6KwIMzo%0A5PlPGwk/3cHNYslhdxQRzyW0qwyDcshQ7SPr+FAFeiiigArT07w9qOqQGa1gBiB2%0A72cKCfQZ61Qjt5plZooncIMsVUnA969VsEjj0yzjhYtEIU2nPXjrW1Kn7SVjnxFb%0A2UbpHnOpeH9Q0mJZbuECJjtDo4YZ9OKzK9ZvoYrjTbuKdQ0JiYtk4AxyD+YFeTUV%0Aafs5WDD1vaxu0FFFFYnQFFFFABRRRQA+CGS5nSGFS8kjBVUdya6Ka1GkaahupIrm%0A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)
![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACACUAFIDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDyWGF55VjjGWY8VqjT7K2VTcy8%0AsMgyEhW+igFsdear6Qyi4kDA8pgkdQNwziqc8zXEzyv1Y/l6CgC/nTvSP85KM6d6%0AR/nJWZSqQHBZdwB5GetAGlnTvSP85KM6d6R/nJStqFqWO23QDPA8lKT+0Lb/AJ4J%0A/wB+EoAM6d6R/nJRnTvSP85KoXDpJcO8SbEJyF9KjoA2Fs7C6wIX+fHIiJJH/AWA%0Az+BrMubd7WYxuQeMhl6MPUVGrMjhlJDKcgjsa09YP7q14HzLv4HTcqtj6DJoAy6K%0AKKALul/69/8Ac/qKlmuDaxQKijDRqfTsKi0v/Xv/ALn9RSX/ANy2/wCuS/8AoIoA%0AX+03/uD8zR/ab/3B+ZqlRQBd/tN/7g/M0f2m/wDcH5mqVFAF3+03/uD8zR/ab/3B%0A+ZqlRQBqWd0bq5SKRF2see+aNY/1Vn/1xT/0WlV9K/4/4/r/AFqxrH+qs/8Arin/%0AAKLSgDLooooAu6X/AK9/9z+opL/7lt/1yX/0EUul/wCvf/c/qKS/+5bf9cl/9BFA%0AFOiiigAooooAKKKKALmlf8f8f1/rVjWP9VZ/9cU/9FpVfSv+P+P6/wBasax/qrP/%0AAK4p/wCi0oAy6KKKALul/wCvf/c/qKS/+5bf9cl/9BFLpf8Ar3/3P6ikv/uW3/XJ%0Af/QRQBTooooAKKKKACiiigC5pX/H/H9f61Y1j/VWf/XFP/RaVX0r/j/j+v8AWrGs%0Af6qz/wCuKf8AotKAMuiiigC7pf8Ar3/3P6ikv/uW3/XJf/QRT9IAa7IPQqAecfxC%0AmX5BW3HpEv8A6CKAKdFFFABRRRQAUUUUAXNLz9vjxjOR1+tWNX/1Vn/1xT/0WlV9%0ALGb6MAkcjkfUVNqrborTjpEg/wDIaUAZtFFFAF/SFDXRUsFBUDJ6D5hTL4ALb47x%0Arn/vkU/SDtuyxzgKDx/vCmX/ANy2/wCuS/8AoIoAp0UUUAFFFFABRRRQBc0sBr6M%0AH1H86sax/qrP/rin/otKr6V/x/x/X+tWNY/1Vn/1xT/0WlAGXRRRQBf0jH2s7sY2%0AjOTj+IUy/wDuW3/XJf8A0EU7SWK3LMOoXI/76FNv/uW3/XJf/QRQBTooooAKKKKA%0ACiiigC5pX/H/AB/X+tWNY/1Vn/1xT/0WlV9K/wCP+P6/1qxrH+qs/wDrin/otKAM%0AuiiigC7pf+vf/c/qKS/+5bf9cl/9BFLpf+vf/c/qKfdwvNHbmMZxGueR/dFAGfRU%0A/wBjn/ufqKPsc/8Ac/UUAQUVP9jn/ufqKPsc/wDc/UUAQUVP9jn/ALn6ij7HP/c/%0AUUAS6V/x/wAf1/rVjWP9VZ/9cU/9FpTNOt5Ir2NnXC5HOR60/WP9VZ/9cU/9FpQB%0Al0UUUAX9JRnuH2jPygfiWFVVuJ4xtSaRQOwYjFSWNyLacl8+W42Pgcgdcj6EA1oX%0AOmLdk3EMqoG5ZsFkY9yCAcH2I4/SgDM+2XP/AD8S/wDfZo+2XP8Az8S/99mrDaXI%0ADxNE3uN3+FJ/Zkn/AD0j/wDHv8KAJrS5dIHnlmlYr0G4n/63enTXbXFqZklkRx1A%0AYgfpx3p1pBJAjRSNG0bexOPwIwe3HtT7iFmh8mBo1Q9SQRn8APpz7UAZf2y5/wCf%0AiX/vs0fbLn/n4l/77NT/ANmSf89I/wDx7/Cj+zJP+ekf/j3+FAFc3VwwIaeQg9QX%0ANX9XIMVnjtEg/wDIaU+20gKyyzSLKi8lVyq/8CZgAB/n3qtqd2txIscbBkjJO7GN%0AzHGT9OAPwoAo0UUUAFOSR4m3RuyN6qcGiigCT7Zc/wDPxL/32aPtlz/z8S/99mii%0AgA+2XP8Az8S/99mj7Zc/8/Ev/fZoooAPtlz/AM/Ev/fZo+2XP/PxL/32aKKAGSSy%0ASkGR2cjgFjmmUUUAFFFFAH//2Q==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABUAAAAUCAIAAADtKeFkAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABn0lEQVR4nLXTS4/BYBQGYL9dXUtRl4SdxsKOWEjqthA2ItKwkRIq%0AQSLikhLRUMybnkk7LjOYyZyFtPTp955zwnb5W9n+y5/P53a7PRwOcYHP5XL5nt/v%0A96lUql6vDwaDWCwmiuLxeByPx5qmveRR6/X6cDhsNhtgRVEymYzP58vlcs/96XSa%0AzWa9Xm86neIa3+x2O47jPB7Pc49j8RAyh4xKJBLNZlPX9VarVa1W5/M53rtarR57%0ApBUEIRwO8zwPzBmFYyHx62KxCAQCDMMEg0HKdeUx6nw+DxyJRPAcumVZ1uv1wuMW%0Ak6tUKsB2u93hcGA0t77T6QDjWLze7/cDQ7rdbpfLBYDhNxoN8rVa7TY/2o7H44hN%0AmQmTZIyaTCYIiLmqqvpgfuVymbCZGSc7nU7C0WgUI7wf9qffbrfomTLfY0Todrv3%0A2PKSJNHACCM2Ych0Oo3OH2LLY+GsUWbPKLxxNBp9J698Mpmk5ObAkAXb/hlbPpvN%0AmnuinmVZfootj78HDqeesd5SqfQKvnzdH9ZbKBSKxWK/38ee3/a/qw9HMD5S7hNe%0AEgAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABIAAAAUCAIAAAAP9fodAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABpUlEQVR4nJWT2WrCUBCGff8ncBdt2rhWFEUbd1zBDSUXKiiCIIgS%0AENzwQvslR1PtRWv+i3DOyXyZmX9ObNe7LpfL9WXZzNV0Og0amkwmFrButyvLcigU%0AajQaFrDNZgPZbreXy6UF7HA4ACwWC03TLGCz2Ywi6W08HlvAOp2ObKharb6Kzedz%0AzJAk6d3QaDT6H1uv1+FwGObtLsg/jGHCOtZqtQiV7mLt8/kKhQKvdrtdNptNpVKJ%0AROLTUDwe3+/3OlYul0WSaDT6ZSiTydTrdV7hKqGcUw5uCc+2262OFYvFQCDg9/uT%0AyWSpVGKbz+ebzabATObDEOQNIwiGwgRGeSZGBJjIQ8NUBHk8HnUsl8vBeL1eGiAV%0AW0VRxBWje05giKZtvs725iRxMB6Ph3ZJBYMNtVpN+MbtYS0qpITT6XTDiINxuVyx%0AWIzyhCW/ht7r9QaDwdPc+Lbb7XY6nZFIRKRKp9OVSuURGw6Hqqo+YWRwOBx2ux2Y%0ApnGVJ7w5XArj6vX7fcz4wVarFV0Ju2gAx6iWX0FEMHHGjZ/cPs7P5/P18Spb0jfd%0ALqa0BRKC4AAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABAAAAAPCAIAAABiEdh4AAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABf0lEQVR4nH1S6+vBYBh9/3S3Ym5p0j6Q5JJC+7R8Eh8UkSK22Epq%0AK3IJm1yy4Xc0zdvK7/m03p3zPOc55yEvqp7P5+VyGY/HzWaz0WgMBoPdbodHGkOc%0AL0Dr9XogEPBQ5fV6c7ncarVyaB/CZrNhWdbzo9BlOBx+CYfDIRKJ0IhisTidTpfL%0A5Ww2K5fL9ihJkj6EarVKo3mev9/vnU4HMyuVyvF4FAQB79Fo9Ha7EV3XfT6fgw6F%0AQlhmv9/jEe05juv1esCEw2H8HY1GBFbQ7TOZjGVZ5/MZzHw+HwwGYdr1ei0UCvZw%0AAvtoQiqVMk1zu936/X5RFDGh3W6fTqd0Om3vRlqtlsvHxWJhGEY8HscEhmEmkwm0%0A2bJLpRJRFMVlYiKR0DQNkXW7XVmW5/O54zjkEAiIxWIuDvRks1lYhNToKFVVfdva%0A7/dtMb+Cswt6kPeb8Hg8arXa/2johLnf0wAH+pxAXNOwPYxyHx9qvV4j1GQyiR1A%0Axr3gLuASfbB/ZA7y1rGsdFsAAAAASUVORK5CYII=)

[massage]

[Mahjong]

[Kiki\'s Bar]

[Billiard Hall]

![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACAAPABADASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD0bUpJIrYSRBSTIQzNjIGT61Dc%0A6hNYXGmIsKFrs7JVHY8cj6ZNT6jYPdwiPDgo5dWQjnP4iqlpEbrVLRDbyRx6dGRu%0AlZSSxAA6E9smsHdMivKPsoxhpK/bz7+h/9k=)
![](data:image/jpeg;base64,%0A/9j/2wBDAAsICAoIBwsKCQoNDAsNERwSEQ8PESIZGhQcKSQrKigkJyctMkA3LTA9%0AMCcnOEw5PUNFSElIKzZPVU5GVEBHSEX/2wBDAQsNDREPESESEiFFLicuRUVFRUVF%0ARUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUVFRUX/wAAR%0ACABeAHQDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL%0A/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0Kx%0AwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNk%0AZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5%0AusLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEB%0AAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAEC%0AAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygp%0AKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImK%0AkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk%0A5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDyKiiigAq1a6dPdo0ibEjU4LyM%0AFGfT3qrW9MNkNtGo2xiBGA7ZKgk/mTQBmXGm3FtEZTseMHBaNg2Pr3FVK6KwIMzo%0A5PlPGwk/3cHNYslhdxQRzyW0qwyDcshQ7SPr+FAFeiiigArT07w9qOqQGa1gBiB2%0A72cKCfQZ61Qjt5plZooncIMsVUnA969VsEjj0yzjhYtEIU2nPXjrW1Kn7SVjnxFb%0A2UbpHnOpeH9Q0mJZbuECJjtDo4YZ9OKzK9ZvoYrjTbuKdQ0JiYtk4AxyD+YFeTUV%0Aafs5WDD1vaxu0FFFFYnQFFFFABRRRQA+CGS5nSGFS8kjBVUdya6Ka1GkaahupIrm%0A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)

# EGA**

![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABAAAAATCAIAAAAWBRqYAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABvElEQVR4nGNgwAaYGRmximMHQNU7LF0IKBLj4HQVl4awWRiZDtm4%0Aw6W8xWW4mFnQNYiwc2y3dAmUkkfTkCSnssLYhoOJGYsl0pzcO61c3MVlgGxDAWEg%0AGSGtsMrElo+FFafDZLl4zIVEBXk4FcX5gVw7YXEBVjYs6qyERD3EpIBIkJVNiJfz%0AaofX8+nBnvogqyxEJHyl5YFImpsHoaFAWatJwwCIVHn4bDUk3q7J+Li3ujVQGyiV%0ApardoWcKROYi4thdZacp+XY1SMO0WCNeTnacrkfW8GZ1OlDD836vy03OR8rtIi2V%0AgL7Cq2EVSMO7ddmvF8V93FfztNP5UosbTttAToJoWJ/9ekn85+Otzyf5Ah1poiSG%0ArpSbg21jrtXtVleghq/n+z4fb/t2ZfKPW7Peby54uybTVBlDg7ueNNAZH3aUAc37%0AeXfuzztzfsAQdg2Sgtz3Jgb+uDnz+43pIA135xLQAATpjqrvNuX/vIOiGqRhdYax%0AkigWDcxMTDuLbb9e6EfT8HxOlBAvF/ZQ0pMTfjYr4suZ7q9nur+A0YddFf2RBthV%0AQ4C3gUxvuEFfhEEvGJV5aXKwIvIDADTLxk8usulvAAAAAElFTkSuQmCC)

# Loss[MTP]**

# \...**

# Loss[PG]**

**[2.3 Payment
Network]**

![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABUAAAAUCAIAAADtKeFkAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABn0lEQVR4nLXTS4/BYBQGYL9dXUtRl4SdxsKOWEjqthA2ItKwkRIq%0AQSLikhLRUMybnkk7LjOYyZyFtPTp955zwnb5W9n+y5/P53a7PRwOcYHP5XL5nt/v%0A96lUql6vDwaDWCwmiuLxeByPx5qmveRR6/X6cDhsNhtgRVEymYzP58vlcs/96XSa%0AzWa9Xm86neIa3+x2O47jPB7Pc49j8RAyh4xKJBLNZlPX9VarVa1W5/M53rtarR57%0ApBUEIRwO8zwPzBmFYyHx62KxCAQCDMMEg0HKdeUx6nw+DxyJRPAcumVZ1uv1wuMW%0Ak6tUKsB2u93hcGA0t77T6QDjWLze7/cDQ7rdbpfLBYDhNxoN8rVa7TY/2o7H44hN%0AmQmTZIyaTCYIiLmqqvpgfuVymbCZGSc7nU7C0WgUI7wf9qffbrfomTLfY0Todrv3%0A2PKSJNHACCM2Ych0Oo3OH2LLY+GsUWbPKLxxNBp9J698Mpmk5ObAkAXb/hlbPpvN%0AmnuinmVZfootj78HDqeesd5SqfQKvnzdH9ZbKBSKxWK/38ee3/a/qw9HMD5S7hNe%0AEgAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABEAAAATCAIAAAD5x3GmAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABkElEQVR4nJWS6WrCYBBF8/5vImo1sS7RaF0DEhVBlIA7CuKOKCFx%0A7Wk+qzSoxfkRArln7p2ZSJf3S3r2wXGcxWIxGo3G4/F6vT6fz/8wk8mkXC6XSqVi%0AsVgoFLLZrGEYy+XyFdNsNpEC5PP5XC4H8+UWhg+Y3W5HKnxgAFBnMpl0Op1MJlVV%0AxZmQXqbdbne73ePxSBgcADRNA4jH459ukdDLIMVhtVptNhvCoE6lUjjEYjEARVH6%0A/b6X0XUdKeR+vx8MBqpbiUQiGo1GIpFwONzpdP4wTEIeGAZoNBqHw6HX65FKAKFQ%0AKBgMsv07czqdarUaCgagNy+VSsW27dlsRjyAQCBANvyvDNswTROpLMtEJwxPeuPJ%0ADiFbrRZdCHbfNcdGx5QkBqMfAE8xN+Ntt1uiir9BEjdhP4pbMB9uEUb4MFu1WmWT%0AtykkTsGl5N9CKhjU9H74l0hEvPVmSr/fz3JIz0oeAj8MOxVqATD6fD5/pr4yKPDx%0A+XwAHMeyrNfAdQfT6bRerw+Hwxd5vMy79Q177FfQwumNeAAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABIAAAAUCAIAAAAP9fodAAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABpUlEQVR4nJWT2WrCUBCGff8ncBdt2rhWFEUbd1zBDSUXKiiCIIgS%0AENzwQvslR1PtRWv+i3DOyXyZmX9ObNe7LpfL9WXZzNV0Og0amkwmFrButyvLcigU%0AajQaFrDNZgPZbreXy6UF7HA4ACwWC03TLGCz2Ywi6W08HlvAOp2ObKharb6Kzedz%0AzJAk6d3QaDT6H1uv1+FwGObtLsg/jGHCOtZqtQiV7mLt8/kKhQKvdrtdNptNpVKJ%0AROLTUDwe3+/3OlYul0WSaDT6ZSiTydTrdV7hKqGcUw5uCc+2262OFYvFQCDg9/uT%0AyWSpVGKbz+ebzabATObDEOQNIwiGwgRGeSZGBJjIQ8NUBHk8HnUsl8vBeL1eGiAV%0AW0VRxBWje05giKZtvs725iRxMB6Ph3ZJBYMNtVpN+MbtYS0qpITT6XTDiINxuVyx%0AWIzyhCW/ht7r9QaDwdPc+Lbb7XY6nZFIRKRKp9OVSuURGw6Hqqo+YWRwOBx2ux2Y%0ApnGVJ7w5XArj6vX7fcz4wVarFV0Ju2gAx6iWX0FEMHHGjZ/cPs7P5/P18Spb0jfd%0ALqa0BRKC4AAAAABJRU5ErkJggg==)
![](data:image/png;base64,%0AiVBORw0KGgoAAAANSUhEUgAAABAAAAAPCAIAAABiEdh4AAAACXBIWXMAAA7EAAAO%0AxAGVKw4bAAABf0lEQVR4nH1S6+vBYBh9/3S3Ym5p0j6Q5JJC+7R8Eh8UkSK22Epq%0AK3IJm1yy4Xc0zdvK7/m03p3zPOc55yEvqp7P5+VyGY/HzWaz0WgMBoPdbodHGkOc%0AL0Dr9XogEPBQ5fV6c7ncarVyaB/CZrNhWdbzo9BlOBx+CYfDIRKJ0IhisTidTpfL%0A5Ww2K5fL9ihJkj6EarVKo3mev9/vnU4HMyuVyvF4FAQB79Fo9Ha7EV3XfT6fgw6F%0AQlhmv9/jEe05juv1esCEw2H8HY1GBFbQ7TOZjGVZ5/MZzHw+HwwGYdr1ei0UCvZw%0AAvtoQiqVMk1zu936/X5RFDGh3W6fTqd0Om3vRlqtlsvHxWJhGEY8HscEhmEmkwm0%0A2bJLpRJRFMVlYiKR0DQNkXW7XVmW5/O54zjkEAiIxWIuDvRks1lYhNToKFVVfdva%0A7/dtMb+Cswt6kPeb8Hg8arXa/2johLnf0wAH+pxAXNOwPYxyHx9qvV4j1GQyiR1A%0Axr3gLuASfbB/ZA7y1rGsdFsAAAAASUVORK5CYII=)

# Loss[Pay]**

*[Ex-post]*

*[regret]*

# Loss[RM]**

[Pay
Net]

# \...**

**[Figure 4: Overview of the proposed optimization and
train-]**

**[ing pipeline. The training process consists of two steps:
1)]**

**[Interest-based Pre-training, where behavior sequences
are]**

**[used to pre-train EGA-V2 that predicts the next POI and
cre-]**

**[ative tokens; and 2) Auction-based Post-training, which
in-]**

**[cludes: 2.1) a permutation-aware reward model trained
with]**

**[list-wise feedback to predict reward signals, such as
pCTR]**

**[and pCVR; 2.2) generative allocation, where different
candi-]**

**[date sequences are scored by the reward model to
optimize]**

**[policy gradients; and 2.3) a payment network optimized
with]**

**[ex-post regret to ensure IC
approximately.]**

[The overall procedure of training is shown in Figure
4.]

*[4.4.1]*

*[Interest-based
Pre-training.]*[
In the pre-training phase, we
first]

[train the generative backbone to capture user interests from
histor-]

[ical interaction behaviors. Specifically, we optimize two
separate]

[cross-entropy losses for predicted target
sequence][
Y][: next-POI
pre-]

[diction loss][
L][NTP][
of main module, and next-creative
prediction]
:::

::: {#page0 style="width:612.0pt;height:792.0pt"}
[EGA-V2: An End-to-end Generative Framework for Industrial
Advertising]

[Woodstock '18, June 03--05, 2018, Woodstock,
NY]

[loss][
L][MTP][
of MTP module. Formally, according to Equation
(17),]

[L][NTP][
=][
−]^[1]^

*[𝐾]*

*[𝐾]*

[∑︁]

*[𝑖]*[=][1]

[log]*[
𝑃]*[(]*[𝒂]*^*[𝑝𝑜𝑖]*^

*[𝑖]*

[\|
Y][1:]*[𝑖]*[−][1]*[,]*[
S]^*[𝑒]*^[)]

[(27)]

[L][MTP][
=][
−]^[1]^

*[𝐾]*

*[𝐾]*[+][1]

[∑︁]

*[𝑖]*[=][2]

[log]*[
𝑃]*[(]*[𝒂]*^*[𝑖𝑚𝑔]*^

*[𝑖]*[−][1][
]^[\|\ Y][1:]*[𝑡]*[−][1]*[,]# \ 𝒂# 𝑝𝑜𝑖*^

*[𝑖]*

*[,]*[
S]^*[𝑒]*^[)]

[(28)]

[Then total pre-training loss is defined
as:]

[L][pre-train][
=][
L][NTP][
+
L][MTP]*[.]*

[(29)]

*[4.4.2]*

*[Auction-based
Post-training.]*[
Following the pre-training
phase,]

[we freeze its parameters and optimize the generative
advertising]

[model under auction constraints through an auction-based
post-]

[training stage. This phase aligns the generative outputs with
plat-]

[form revenue objectives and advertiser demands, consisting of
two]

[components: i) reward model training, ii) generative
allocation]

[training based on policy gradient, and iii) payment network
opti-]

[mization based on Lagrangian
method.]

**[Reward Model
Training.]**[
We train a separate reward
model]

[(RM) using users' real feedback signals (e.g., clicks and
conversions).]

[The RM is optimized by minimizing the binary cross-entropy
loss:]

[L]^[pcxr]^

[RM][
]^[=][−][1]^

[\|D\|]

[∑︁]

*[𝑑]*[∈D]

*[𝐾]*

[∑︁]

*[𝑖]*[=][1]

[ ]*[𝑦]*[pcxr][
log
ˆ]*[𝑟]*[pcxr][
+
(][1][
−]*[𝑦]*[pcxr][)][
log][(][1][
−][ˆ]*[𝑟]*[pcxr][)][]*[
,]*

[(30)]

[where]*[
𝑦]*^[pcxr][\ ]^[represents
ground-truth labels derived from real
user]

[interactions,][
ˆ]*[𝑟]*^[pcxr][\ ]^[is
the predicted probability from the
reward]

[model by Equation (20),
and][
D][ is the training
dataset.]

**[Generative Allocation
Training.]**[
After convergence of
the]

[reward model, we adopt a non-autoregressive policy gradient
based]

[method. Given a generated winning ad
sequence][
S]^[∗]^[=][
{]*[𝑦]*[1]*[,𝑦]*[2]*[,
\...,𝑦][𝐾]*[}][,]

[we define the marginal contribution of each
item]*[
𝑦][𝑖]*[to
platform]

[revenue as:]

*[𝑟][𝑦][𝑖]*[=]

[∑︁]

*[𝑦][𝑗]*[∈S]^[∗]^

*[𝑏][𝑗]*[ˆ]*[𝑟]*^[pctr]^

*[𝑗]*

[−]

[∑︁]

*[𝑦][𝑗]*[∈S]^[∗]^

[−]*[𝑖]*

*[𝑏][𝑗]*[ˆ]*[𝑟]*^[pctr]^

*[𝑗]*

*[,]*

[(31)]

[where][
S]^[∗]^

[−]*[𝑖]*^[denotes\ the\ best\ alternative\ ad\ sequence\ excluding]*[\ 𝑦]# 𝑖*[.]^

[We then apply a policy gradient objective to maximize the
expected]

[rewards:]

[L][PG][
=][
−]^[1]^

[\|D\|]

[∑︁]

*[𝑑]*[∈D]

[∑︁]

*[𝑦][𝑖]*[∈S]^[∗]^

*[𝑟][𝑦][𝑖]*[log]*[𝑧][𝑦][𝑖][,]*

[(32)]

[where]*[
𝑧][𝑦][𝑖]*[is
the allocation probability for
item]*[
𝑦][𝑖]*[by
Equation][
(22)][.]

[This design encourages the generator to produce sequences
that]

[yield higher overall revenue, using fixed reward model
parameters.]

**[Payment Network
Optimization]**[.
The payment network
opti-]

[mizes
Equation][
(8)][ to
balance revenue maximization and IC
con-]

[straint via Lagrangian dual formulation. The loss function
inte-]

[grates both total platform payment and ex-post regret
minimization.]

[Given the selected
sequence][
Y][, the payment
loss][
L][Pay][
is defined]

[as follows:]

[L][Pay][
=][−]^[1]^

[\|D\|]

[∑︁]

*[𝑑]*[∈D]

[©­]

[«]

[∑︁]

*[𝑦][𝑖]*[∈S]^[∗]^

*[𝑝][𝑖]*[ˆ]*[𝑟]*^[pctr]^

*[𝑖]*

[−]

[∑︁]

*[𝑦][𝑖]*[∈S]^[∗]^

*[𝜆][𝑖]*^[c]^

[rgt]*[𝑖]*[−]^*[𝜌]*^

[2]

[∑︁]

*[𝑦][𝑖]*[∈S]^[∗]^

[(]^[c]^

[rgt]*[𝑖]*[)]^[2][ª]^[®]

[¬]

*[,]*

[(33)]

[where][
]^[c]^

[rgt]*[𝑖]*[is
the ex-post regret of
ad]*[
𝑦][𝑖]*[,]*[
𝑝][𝑖]*[is
the predicted
payment]

[from the payment
network,]*[
𝜆][𝑖]*[is
a Lagrange multiplier,
and]*[
𝜌][\>]*

[0][ is the
hyperparameter for the IC penalty term. To solve
this]

[constrained optimization, we adopt an iterative
Lagrangian-based]

[approach to jointly optimize the payment network. Specifically,
we]

[alternate between two
steps:]

[•][ Payment Network
Update: Optimize the
parameters]*[
𝜽]*[Pay][
of]

[the payment network by minimizing the Lagrangian
objective]

[with fixed
multipliers:]

*[𝜽]*^[new]^

[Pay][
]^[=][\ arg\ min]^

*[𝜽]*[Pay]

[L][Pay][(]*[𝜽]*^[old]^

[Pay]^*[,]# \ 𝝀*[old][)]*[.]*^

[(34)]

[•][ Multiplier Update:
Adjust the Lagrange multipliers based
on]

[the observed empirical ex-post
regret:]

*[𝝀]*^[new][\ ]^[=]*[
𝝀]*^[old][\ ]^[+]*[
𝜌]*[·][
]^[c]^

[rgt][(]*[𝜽]*^[new]^

[Pay][
]^[)]*[.]*^

[(35)]

[Note that the overall objective is non-convex, and
convergence]

[to the global optimum is not theoretically guaranteed. However,
our]

[empirical results show that this optimization strategy
effectively]

[minimizes regret while maintaining near-optimal revenue in
real-]

[world
scenarios.]

# 5**

# Experiments**

[In this section, we evaluate our proposed model on industrial
dataset]

[and aim to answer the following research
questions]^[2]^[:]

[•]**[
RQ1]**[: How
does our EGA-V2 model perform, compared to
the]

[state-of-the-art advertising
models?]

[•]**[
RQ2]**[: What
is the impact of designs (e.g. MTP module,
token-]

[level bidding, payment network, and multi-phase training)
on]

[the performance of
EGA-V2?]

[•]**[
RQ3]**[: How
do hyperparameters affect model
performance?]

# 5.1**

**[Experiment
Setup]**

*[5.1.1]*

*[Dataset.]*[
The industrial dataset used in our experiments
con-]

[sists of real interaction logs collected from a large-scale
location-]

[based services (LBS) platform Meituan, spanning the period
from]

[September 2024 to April 2025. The dataset contains 200
million]

[requests from over 2 million users and nearly 10 million unique
ads]

[of low-traffic ad slot in Meituan. We use the first 200 days for
pre-]

[training and randomly sample 10% data for preference
alignment.]

[The last 14 days are used for
testing.]

*[5.1.2]*

*[Evaluation
Metrics.]*[ In
offline experiments, we employ
the]

[following
metrics]^[3][\ ]^[to
comprehensively evaluate platform
revenue,]

[user experience, and ex-post regret of advertisers,
respectively.]

[•]**[ Revenue Per
Mille:]**[
RPM][
=]

[Í][
click][×][payment]

[Í][
impression]

[×][
1000][.]

[•]**[ Click-Through
Rate:]**[
CTR][
=]

[Í][
click]

[Í][
impression][.]

[•]**[ IC
Metric:]**[
Ψ][
=]

[1]

[\| D\|]

[Í]

*[𝑑]*[∈D]

[Í]

*[𝑖]*[∈]*[𝑘]*

[c]

[rgt]^*[𝑑]*^

*[𝑖]*

*[𝑢][𝑖]*[(]*[𝑣]*^*[𝑑]*^

*[𝑖]*^[;]*[𝒃]# 𝑑*[)][\ ,\ where][\ c]^

[rgt]^*[𝑑]*^

*[𝑖]*^[de-]^

[notes the empirical ex-post regret for
advertiser]*[
𝑖]*[in
session]

[data]*[
𝑑]*[as
defined in
Equation][
(7)][,
and]*[
𝑢][𝑖]*[is
the realized
util-]

[ity. This metric evaluates incentive compatibility (IC),
rep-]

[resenting the relative utility gain an advertiser could
obtain]

[by manipulating its bid
\[][5][,][
14][,][
19][\].
Following
\[][25][,][
41][\], IC
is]

[empirically tested via counterfactual perturbation: for
each]

[2][Due to
computational complexity constraints, our current evaluation is limited
to]

[offline experiments, with online A/B testing reserved for future
work.]

[3][To protect
business confidentiality, the reported results on Meituan have been
trans-]

[formed in a way that preserves their statistical properties while
ensuring that
sensitive]

[business information cannot be inferred or reconstructed from the
published
data.]
:::

::: {#page0 style="width:612.0pt;height:792.0pt"}
[Woodstock '18, June 03--05, 2018, Woodstock,
NY]

[Zheng et al.]

[advertiser, the
bid]*[𝑏][𝑖]*[is
replaced
with]*[𝛾]*[×]*[𝑏][𝑖]*[,
where]*[𝛾]*[∈{][0]*[.]*[2][×]*[𝑗]*[\|]

*[𝑗]*[=][
1]*[,]*[
2]*[, . . .
,]*[
10][}][.]

[For offline experiments, evaluation metrics are computed using
the]

[predicted values from the reward
model.]

*[5.1.3]*

*[Baselines.]*[
We evaluate EGA-V2 against the following
two]

[widely adopted industrial
architectures.]

[•]**[
MCA:]**[ The
multi-stage cascading architecture (MCA) is a
stan-]

[dard paradigm in industrial online advertising. It consists
of]

[five key stages: recall, ranking, creative selection, auction,
and]

[ad allocation. For a strong baseline, we implement MCA
us-]

[ing representative methods: Tiger
\[][20][\]
for recall, HSTU
\[][34][\]]

[for ranking, Peri-CR
\[][32][\]
for creative selection, CGA
\[][41][\]
for]

[auction, and CrossDQN \[16\] for ad
allocation.]

[•]**[
GR:]**[
Generative recommendation (GR) formulates
recommen-]

[dation as a sequence generation task, where user-item
inter-]

[actions are modeled using transformer based
autoregressive]

[architectures. To apply GR in online advertising scenario,
we]

[construct this baseline by integrating OneRec
\[][4][\]
with Peri-]

[CR \[32\] for creative selection and GSP \[8\] for
payment.]

*[5.1.4]*

*[Implementation
Details.]*[ We
train EGA-V2 using the
Adam]

[optimizer with an initial learning rate of 0.0024. The batch
size]

[is set to 128. Model training and optimization are performed
on]

[NVIDIA A100 GPUs with 80G memory. For hyperparameters,
we]

[tried different hyperparameters using grid search. Due to space
con-]

[straints, we report only the most optimal hyperparameter
settings]

[in this paper. For interest-based pre-training, the block
number]*[
𝐿]*

[of encoder and decoder is 3, the number of codebook
layers]*[
𝐶]*[=][
3][,]

[and codebook
size]*[
𝑊]*[of each
layer is 1024. We
consider]*[
𝐾]*[=][
10]

[target session items and
use]*[
𝐵]*[=][
256][
historical behaviors as
context.]

[For auction-based preference alignment, the hyperparameters
in]

[token-level
bidding]*[
𝛼]*[=][
1]*[.]*[2][
and]*[
𝛽]*[=][
2][. We
generate]*[
𝑁][𝑆]*[=][
64][
differ-]

[ent sequences for each request by beam search. For reward
model]

[and payment network, the hidden layers of the MLP are 128,
32,]

[and 10.]

# 5.2**

**[Offline Performance
(RQ1)]**

**[Table 2: The experimental results of out model EGA-V2
and]**

**[competitors on industrial dataset. The bold value marks
the]**

**[best one in each column. Each result is presented in the
form]**

**[of mean (lift percentage). Lift percentage means the
improve-]**

**[ment of EGA-V2 over the best
baselines.]**

# Model**

# RPM**

# CTR-poi**

# CTR-img**

[Ψ]

[MCA]

[192.45
(-16.5%)]

[0.0558
(-8.8%)]

[0.0529
(-9.3%)]

[3.6%]

[GR]

[206.73
(-10.3%)]

[0.0582
(-4.9%)]

[0.0546
(-6.3%)]

[8.4%]

[EGA-V2]

# 230.41**

# 0.0612**

# 0.0583**

# 2.7%**

[As shown in Table 2, our key observations are 1) the
Multi-stage]

[Cascading Architecture (MCA) suffers from the well-known
early-]

[stage filtering problem: promising ads are often eliminated in
the]

[initial recall or ranking stages, leading to suboptimal overall
per-]

[formance. This limitation is reflected in its relatively lower
RPM]

[and CTR metrics compared to more unified approaches. 2) In
con-]

[trast, the generative recommendation baseline (GR) is designed
to]

[improve sequence modeling and personalization. However, GR
still]

[falls short in real advertising scenarios due to its limited ability
to]

[satisfy practical business constraints. Specifically, GR does not
guar-]

[antee incentive compatibility with GSP (as evidenced by a
much]

[higher IC regret of 8.4%), cannot flexibly implement dynamic
ad]

[allocation or control ad exposure rates, and is unable to
support]

[parallel creative selection for each ad. These limitations reduce
the]

[effectiveness of GR when applied to industrial
advertising.]

[Our proposed EGA-V2 overcomes these shortcomings by
inte-]

[grating end-to-end generation, permutation-aware reward
mod-]

[eling, token-level bidding, and a dedicated payment network.
As]

[a result, EGA-V2 achieves the best overall performance: it
signifi-]

[cantly improves revenue (RPM), enhances both POI and
creative]

[CTRs, and delivers superior economic robustness with the
lowest]

[IC regret among all baselines. This demonstrates that
addressing]

[both early-stage candidate loss and business constraints is
crucial]

[for practical deployment in industrial advertising
systems.]

# 5.3**

**[Ablation Study
(RQ2)]**

**[Table 3: Ablation Study of
EGA-V2.]**

# Model**

# RPM**

# CTR-poi**

# CTR-img**

[Ψ]

[EGA-V2]

# 230.41**

# 0.0612**

# 0.0583**

# 2.7%**

[EGA-mtp]

[225.45
(-2.1%)]

[0.0593
(-3.1%)]

[0.0562
(-3.6%)]

[2.7%]

[EGA-end]

[218.21
(-5.3%)]

[0.0600
(-2.0%)]

[0.0572
(-1.9%)]

[3.0%]

[EGA-bid]

[222.94
(-3.2%)]

[0.0603
(-1.5%)]

[0.0576
(-1.2%)]

[4.1%]

[EGA-gsp]

[226.17
(-1.8%)]

[0.0608
(-0.6%)]

[0.0580
(-0.5%)]

[8.2%]

[We conduct experiments to validate the effectiveness of
different]

[components. Correspondingly, we design a series of ablation
studies,]

[which considers four variants to simplify EGA-V2 in different
ways:]

[•]**[
EGA-mtp]**[
removes the Multi-Token Prediction (MTP)
mod-]

[ule, replacing it with a standard next-token prediction
(NTP)]

[approach that independently predicts POI and creative in
a]

[sequential manner. This setting examines the importance
of]

[joint modeling for POI and creative
generation.]

[•]**[
EGA-end]**[
disables the multi-phase training strategy and
in-]

[stead trains the entire model in a single end-to-end stage.
The]

[comparison assesses the benefit of our proposed
interest-based]

[pre-training and auction-based post-training
pipeline.]

[•]**[
EGA-bid]**[
simplifies the token-level bidding mechanism
by]

[replacing the maximum aggregation with an average
opera-]

[tion, i.e., each token's bid is computed as the average over
all]

[relevant items rather than the maximum.
Formally,]*[
𝑏]*[(]*[𝑎]*^*[𝑗]*^

*[𝑖]*^[)][\ =]^

[avg][(]*[𝑏]*[1]*[,𝑏]*[2]*[,
\...,𝑏][𝑁][𝑖]*[)][.
This tests the effect of aggregation
strategy]

[in the bidding
process.]

[•]**[
EGA-gsp]**[
removes the dedicated payment network and
re-]

[places it with standard GSP payment computation,
keeping]

[other modules unchanged. The comparison highlights the
role]

[of the learned payment network in optimizing revenue
and]

[enforcing incentive
compatibility.]
:::

::: {#page0 style="width:612.0pt;height:792.0pt"}
[EGA-V2: An End-to-end Generative Framework for Industrial
Advertising]

[Woodstock '18, June 03--05, 2018, Woodstock,
NY]

[The performance results are shown in Table 3, from which
we]

[observe the full EGA-V2 model consistently outperforms all
ablation]

[variants across all major metrics, validating the effectiveness of
our]

[design.
Specifically,]

[(1)][ EGA-mtp
leads to a noticeable drop in RPM, CTR-poi,
and]

[CTR-img (-2.1%, -3.1%, and -3.6% respectively), indicating
the]

[importance of jointly modeling POI and creative
selection.]

[(2)][ The
EGA-end variant, which disables multi-phase
training,]

[also results in a performance decrease, especially on RPM
(-]

[5.3%), highlighting the benefit of our two-stage
optimization]

[strategy for aligning user interests modeling and
business]

[objectives.]

[(3)][ The
EGA-bid variant, which replaces the max
aggregation]

[in token-level bidding with an average operation, causes
a]

[moderate decrease in all metrics, and notably increases the
IC]

[regret][
Ψ][ to 4.1%,
showing the importance of our
aggregation]

[choice for incentive compatibility and allocation
efficiency.]

[(4)][ EGA-gsp
results in the highest IC regret
(][Ψ][
rises from 2.7%
to]

[8.2%) due to the absence of payment network and
degrading]

[to GSP payment, although other metrics drop only
slightly.]

[This demonstrates that the payment network is crucial
for]

[achieving incentive compatibility approximately in
practice.]

[In summary, each component of EGA-V2, including
multi-token]

[prediction, multi-phase training, max aggregation in bidding,
and]

[a dedicated payment network, plays an important and
complemen-]

[tary role in improving revenue and user experience, and
ensuring]

[economic
robustness.]

# 5.4**

**[Hyperparameters
(RQ3)]**

[1]

[2]

[3]

[160]

[180]

[200]

[220]

[240]

[RPM]

[0.060]

[0.062]

[0.064]

[0.066]

[CTR-poi]

[RPM]

[CTR-poi]

[0]

[5]

[10]

[15]

[20]

[215]

[220]

[225]

[230]

[235]

[240]

[RPM]

[0.060]

[0.061]

[0.062]

[CTR-poi]

[RPM]

[CTR-poi]

**[Figure 5: Effect of hyperparameters of
EGA-V2.]**

[We conduct a comprehensive study to evaluate the sensitivity
of]

[EGA-V2 with respect to key hyperparameters. The results are
sum-]

[marized in Figure 5, where RPM and CTR-poi are used as the
evalu-]

[ation metrics. Figure 5 examines the impact of token-level
bidding]

[parameters]*[
𝛼]*[and]*[
𝛽]*[in
Equation][
(21)][.]*[
𝛼]*[measures
the importance]

[of bids during the allocation process.
Increasing]*[
𝛼]*[significantly]

[boosts RPM at first, as higher-bid ads become more likely to win
in]

[the generative allocation. However, this trend gradually
plateaus]

[as further
increasing]*[
𝛼]*[provides
diminishing marginal returns.
In]

[contrast, CTR-poi shows a consistent decline
as]*[
𝛼]*[rises,
reflecting]

[a shift in exposure from more organic contents to higher-bid
ads.]

[Besides,]*[
𝛽]*[acts as a
global balancing factor between ads and
organic]

[content.
Increasing]*[
𝛽]*[reduces
the probability of ads being
allocated.]

[As a result, RPM decreases steadily with
increasing]*[
𝛽]*[, and
eventu-]

[ally flattens out as ads rarely win positions in the list.
Conversely,]

[CTR-poi increases
with]*[
𝛽]*[, as the
display list becomes
dominated]

[by organic content. In practice, the optimal values
of]*[
𝛼]*[and]*[
𝛽]*[are]

[selected to achieve a trade-off between platform revenue and
user]

[experience.]

# 6**

# Conclusion**

[In this work, we presented End-to-end Generative
Advertising]

[(EGA-V2), a novel framework that unifies ranking, creative
selec-]

[tion, ad allocation, and payment computation into a single
genera-]

[tive model for industrial advertising systems. By leveraging
hier-]

[archical semantic tokenization, permutation-aware reward
mod-]

[eling, and token-level bidding and allocation, EGA-V2 bridges
the]

[gap between user interests modeling and business-critical
auction]

[constraints such as IC and IR. The proposed multi-phase
training]

[paradigm, including interest-based pre-training and
auction-based]

[post-training, ensures that EGA-V2 captures both user
interests]

[and advertiser utility under complex real-world conditions.
Exten-]

[sive offline experiments on an industrial dataset demonstrate
that]

[EGA-V2 achieves substantial improvements over both
multi-stage]

[cascading architecture and recent generative recommendation
base-]

[lines in platform revenue, user experience, and advertiser
return]

[on investment. Our ablation studies validate the effectiveness
of]

[each design component and highlight the flexibility of the
proposed]

[framework.]

[Looking forward, we believe EGA-V2 opens new directions
for]

[the integration of generative modeling and economic
mechanism]

[design in online advertising. Future work will further explore
scal-]

[ing laws and enhance business
interpretability.]

# References**

[\[1\]][ Yoram
Bachrach, Sofia Ceppi, Ian A Kash, Peter Key, and David Kurokawa.
2014.]

[Optimising trade-offs among stakeholders in ad auctions.
In]*[
Proceedings of
the]*

*[fifteenth ACM conference on Economics and
computation]*[.
75--92.]

[\[2\]][ Dagui
Chen, Qi Yan, Chunjie Chen, Zhenzhe Zheng, Yangsu Liu, Zhenjia
Ma,]

[Chuan Yu, Jian Xu, and Bo Zheng. 2022. Hierarchically constrained
adaptive]

[ad exposure in feeds.
In]*[
Proceedings of the 31st ACM International Conference
on]*

*[Information & Knowledge
Management]*[.
3003--3012.]

[\[3\]][ Nicola
De Cao, Gautier Izacard, Sebastian Riedel, and Fabio Petroni. 2020.
Au-]

[toregressive entity
retrieval.]*[
arXiv preprint
arXiv:2010.00904]*[
(2020).]

[\[4\]][ Jiaxin
Deng, Shiyao Wang, Kuo Cai, Lejian Ren, Qigen Hu, Weifeng
Ding,]

[Qiang Luo, and Guorui Zhou. 2025. OneRec: Unifying Retrieve and Rank
with]

[Generative Recommender and Iterative Preference
Alignment.]*[
arXiv
preprint]*

*[arXiv:2502.18965]*[
(2025).]

[\[5\]][ Yuan
Deng, Sébastien Lahaie, Vahab Mirrokni, and Song Zuo. 2020. A
data-]

[driven metric of incentive compatibility.
In]*[
Proceedings of The Web
Conference]*

*[2020]*[.
1796--1806.]

[\[6\]][ Paul
Duetting, Vahab Mirrokni, Renato Paes Leme, Haifeng Xu, and Song
Zuo.]

[2024. Mechanism design for large language models.
In]*[
Proceedings of the
ACM]*

*[Web Conference
2024]*[.
144--155.]

[\[7\]][ Paul
Dütting, Zhe Feng, Harikrishna Narasimhan, David Parkes, and Sai
Srivatsa]

[Ravindranath. 2019. Optimal auctions through deep learning.
In]*[
International]*

*[Conference on Machine
Learning]*[.
PMLR,
1706--1715.]

[\[8\]][
Benjamin Edelman, Michael Ostrovsky, and Michael Schwarz. 2007.
Internet]

[advertising and the generalized second-price auction: Selling billions
of dollars]

[worth of
keywords.]*[
American economic
review]*[ 97,
1 (2007),
242--259.]

[\[9\]][ Nicola
Gatti, Alessandro Lazaric, and Francesco Trovo. 2012. A truthful
learning]

[mechanism for contextual multi-slot sponsored search auctions with
externalities.]

[In]*[
Proceedings of the 13th ACM Conference on Electronic
Commerce]*[.
605--622.]

[\[10\]][
Fabian Gloeckle, Badr Youbi Idrissi, Baptiste Rozière, David Lopez-Paz,
and]

[Gabriel Synnaeve. 2024. Better & faster large language models via
multi-token]

[prediction.]*[
arXiv preprint
arXiv:2404.19737]*[
(2024).]

[\[11\]][ Siyu
Gu and Xiangrong Sheng. 2022. On Ranking Consistency of
Pre-ranking]

[Stage.]*[
arXiv preprint
arXiv:2205.01289]*[
(2022).]

[\[12\]][
Patrick Hummel and R Preston McAfee. 2014. Position auctions with
externalities.]

[In]*[ Web and
Internet Economics: 10th International Conference, WINE 2014,
Beijing,]*

*[China, December 14-17, 2014. Proceedings
10]*[.
Springer,
417--422.]

[\[13\]][
Xuejian Li, Ze Wang, Bingqi Zhu, Fei He, Yongkang Wang, and Xingxing
Wang.]

[2024. Deep automated mechanism design for integrating ad auction and
allocation]
:::

::: {#page0 style="width:612.0pt;height:792.0pt"}
[Woodstock '18, June 03--05, 2018, Woodstock,
NY]

[Zheng et al.]

[in feed. In]*[
Proceedings of the 47th International ACM SIGIR Conference on
Research]*

*[and Development in Information
Retrieval]*[.
1211--1220.]

[\[14\]][
Guogang Liao, Xuejian Li, Ze Wang, Fan Yang, Muzhi Guan, Bingqi
Zhu,]

[Yongkang Wang, Xingxing Wang, and Dong Wang. 2022. NMA: neural
multi-slot]

[auctions with externalities for online
advertising.]*[
arXiv preprint
arXiv:2205.10018]*

[(2022).]

[\[15\] Guogang Liao, Xiaowen Shi, Ze Wang, Xiaoxu Wu, Chuheng Zhang,
Yongkang]

[Wang, Xingxing Wang, and Dong Wang. 2022. Deep page-level interest
net-]

[work in reinforcement learning for ads allocation.
In]*[
Proceedings of the
45th]*

*[International ACM SIGIR Conference on Research and Development in
Information]*

*[Retrieval]*[.
2292--2296.]

[\[16\] Guogang Liao, Ze Wang, Xiaoxu Wu, Xiaowen Shi, Chuheng Zhang,
Yongkang]

[Wang, Xingxing Wang, and Dong Wang. 2022. Cross DQN: Cross deep Q
network]

[for ads allocation in feed.
In]*[
Proceedings of the ACM Web Conference
2022]*[.
401--]

[409.]

[\[17\]][ Aixin
Liu, Bei Feng, Bing Xue, Bingxuan Wang, Bochao Wu, Chengda Lu,
Cheng-]

[gang Zhao, Chengqi Deng, Chenyu Zhang, Chong Ruan, et
al][.][
2024.
Deepseek-v3]

[technical
report.]*[
arXiv preprint
arXiv:2412.19437]*[
(2024).]

[\[18\]][ Han
Liu, Yinwei Wei, Xuemeng Song, Weili Guan, Yuan-Fang Li, and
Liqiang]

[Nie. 2024. Mmgrec: Multimodal generative recommendation with
transformer]

[model.]*[
arXiv preprint
arXiv:2404.16555]*[
(2024).]

[\[19\]][
Xiangyu Liu, Chuan Yu, Zhilin Zhang, Zhenzhe Zheng, Yu Rong, Hongtao
Lv,]

[Da Huo, Yiqing Wang, Dagui Chen, Jian Xu, et
al][.][
2021. Neural auction:
End-to-]

[end learning of auction mechanisms for e-commerce advertising.
In]*[
Proceedings]*

*[of the 27th ACM SIGKDD Conference on Knowledge Discovery & Data
Mining]*[.]

[3354--3364.]

[\[20\] Shashank Rajput, Nikhil Mehta, Anima Singh, Raghunandan Hulikal
Keshavan,]

[Trung Vu, Lukasz Heldt, Lichan Hong, Yi Tay, Vinh Tran, Jonah Samost,
et
al][.]

[2023. Recommender systems with generative
retrieval.]*[
Advances in
Neural]*

*[Information Processing
Systems]*[ 36
(2023),
10299--10315.]

[\[21\]][ Zihua
Si, Zhongxiang Sun, Jiale Chen, Guozhang Chen, Xiaoxue Zang, Kai
Zheng,]

[Yang Song, Xiao Zhang, Jun Xu, and Kun Gai. 2024. Generative Retrieval
with]

[Semantic Tree-Structured Identifiers and Contrastive Learning.
In]*[
Proceedings
of]*

*[the 2024 Annual International ACM SIGIR Conference on Research and
Development]*

*[in Information Retrieval in the Asia Pacific
Region]*[.
154--163.]

[\[22\] Juntao Tan, Shuyuan Xu, Wenyue Hua, Yingqiang Ge, Zelong Li, and
Yongfeng]

[Zhang. 2024. Idgenrec: Llm-recsys alignment with textual id learning.
In]*[
Proceed-]*

*[ings of the 47th International ACM SIGIR Conference on Research and
Development]*

*[in Information
Retrieval]*[.
355--364.]

[\[23\]][ Yubao
Tang, Ruqing Zhang, Jiafeng Guo, and Maarten de Rijke. 2023.
Recent]

[advances in generative information retrieval.
In]*[
Proceedings of the Annual
In-]*

*[ternational ACM SIGIR Conference on Research and Development in
Information]*

*[Retrieval in the Asia Pacific
Region]*[.
294--297.]

[\[24\]][ Yi
Tay, Vinh Tran, Mostafa Dehghani, Jianmo Ni, Dara Bahri, Harsh
Mehta,]

[Zhen Qin, Kai Hui, Zhe Zhao, Jai Gupta, et
al][.][
2022. Transformer memory as
a]

[differentiable search
index.]*[
Advances in Neural Information Processing
Systems]*

[35 (2022),
21831--21843.]

[\[25\]][
Yiqing Wang, Xiangyu Liu, Zhenzhe Zheng, Zhilin Zhang, Miao Xu, Chuan
Yu,]

[and Fan Wu. 2022. On designing a two-stage auction for online
advertising.
In]

*[Proceedings of the ACM Web Conference
2022]*[.
90--99.]

[\[26\]][ Yidan
Wang, Zhaochun Ren, Weiwei Sun, Jiyuan Yang, Zhixiang Liang,
Xin]

[Chen, Ruobing Xie, Su Yan, Xu Zhang, Pengjie Ren, et
al][.][
2024.
Content-Based]

[Collaborative Generation for Recommender Systems.
In]*[
Proceedings of the
33rd]*

*[ACM International Conference on Information and Knowledge
Management]*[.
2420--]

[2430.]

[\[27\] Ze Wang, Guogang Liao, Xiaowen Shi, Xiaoxu Wu, Chuheng Zhang,
Yongkang]

[Wang, Xingxing Wang, and Dong Wang. 2022. Learning List-wise
Representation]

[in Reinforcement Learning for Ads Allocation with Multiple Auxiliary
Tasks. In]

*[Proceedings of the 31st ACM International Conference on Information &
Knowledge]*

*[Management]*[.
3555--3564.]

[\[28\]][
Ruobing Xie, Shaoliang Zhang, Rui Wang, Feng Xia, and Leyu Lin. 2021.
Hierar-]

[chical reinforcement learning for integrated recommendation.
In]*[
Proceedings
of]*

*[the AAAI conference on artificial
intelligence]*[,
Vol. 35.
4521--4528.]

[\[29\]][ Yue
Xu, Qijie Shen, Jianwen Yin, Zengde Deng, Dimin Wang, Hao Chen,
Lixi-]

[ang Lai, Tao Zhuang, and Junfeng Ge. 2023. Multi-channel Integrated
Recom-]

[mendation with Exposure Constraints.
In]*[
Proceedings of the 29th ACM
SIGKDD]*

*[Conference on Knowledge Discovery and Data
Mining]*[.
5338--5349.]

[\[30\]][
Jinyun Yan, Zhiyuan Xu, Birjodh Tiwana, and Shaunak Chatterjee. 2020.
Ads]

[allocation in feed via constrained optimization.
In]*[
Proceedings of the 26th
ACM]*

*[SIGKDD International Conference on Knowledge Discovery & Data
Mining]*[.
3386--]

[3394.]

[\[31\]][ Yuhao
Yang, Zhi Ji, Zhaopeng Li, Yi Li, Zhonglin Mo, Yue Ding, Kai Chen,
Zijian]

[Zhang, Jie Li, Shuanglong Li, et
al][.][
2025. Sparse Meets Dense: Unified
Generative]

[Recommendations with Cascaded Sparse-Dense
Representations.]*[
arXiv
preprint]*

*[arXiv:2503.02453]*[
(2025).]

[\[32\]][
Zhiguang Yang, Liufang Sang, Haoran Wang, Wenlong Chen, Lu Wang, Jie
He,]

[Changping Peng, Zhangang Lin, Chun Gan, and Jingping Shao. 2024.
Parallel]

[ranking of ads and creatives in real-time advertising systems.
In]*[
Proceedings
of]*

*[the AAAI Conference on Artificial
Intelligence]*[,
Vol. 38.
9278--9286.]

[\[33\]][ Neil
Zeghidour, Alejandro Luebs, Ahmed Omran, Jan Skoglund, and
Marco]

[Tagliasacchi. 2021. Soundstream: An end-to-end neural audio
codec.]*[
IEEE/ACM]*

*[Transactions on Audio, Speech, and Language
Processing]*[
30 (2021),
495--507.]

[\[34\]][ Jiaqi
Zhai, Lucy Liao, Xing Liu, Yueming Wang, Rui Li, Xuan Cao, Leon Gao,
Zhao-]

[jie Gong, Fangda Gu, Michael He, et
al][.][
2024. Actions speak louder than
words:]

[Trillion-parameter sequential transducers for generative
recommendations.]*[
arXiv]*

*[preprint
arXiv:2402.17152]*[
(2024).]

[\[35\]][
Zhanhao Zhang. 2021. A survey of online auction mechanism design using
deep]

[learning
approaches.]*[
arXiv preprint
arXiv:2110.06880]*[
(2021).]

[\[36\]][
Zhixuan Zhang, Yuheng Huang, Dan Ou, Sen Li, Longbin Li, Qingwen Liu,
and]

[Xiaoyi Zeng. 2023. Rethinking the role of pre-ranking in large-scale
e-commerce]

[searching
system.]*[
arXiv preprint
arXiv:2305.13647]*[
(2023).]

[\[37\]][
Zhilin Zhang, Xiangyu Liu, Zhenzhe Zheng, Chenrui Zhang, Miao Xu, Junwei
Pan,]

[Chuan Yu, Fan Wu, Jian Xu, and Kun Gai. 2021. Optimizing multiple
performance]

[metrics with deep GSP auctions for e-commerce advertising.
In]*[
Proceedings of
the]*

*[14th ACM International Conference on Web Search and Data
Mining]*[.
993--1001.]

[\[38\]][
Xiangyu Zhao, Changsheng Gu, Haoshenglun Zhang, Xiwang Yang,
Xiaobing]

[Liu, Jiliang Tang, and Hui Liu. 2021. Dear: Deep reinforcement learning
for]

[online advertising impression in recommender systems.
In]*[
Proceedings of
the]*

*[AAAI conference on artificial
intelligence]*[,
Vol. 35.
750--758.]

[\[39\]][
Zhishan Zhao, Jingyue Gao, Yu Zhang, Shuguang Han, Siyuan Lou,
Xiang-Rong]

[Sheng, Zhe Wang, Han Zhu, Yuning Jiang, Jian Xu, et
al][.][
2023. COPR:
Consistency-]

[Oriented Pre-Ranking for Online Advertising.
In]*[
Proceedings of the 32nd
ACM]*

*[International Conference on Information and Knowledge
Management]*[.
4974--4980.]

[\[40\]][ Bowen
Zheng, Yupeng Hou, Hongyu Lu, Yu Chen, Wayne Xin Zhao,
Ming]

[Chen, and Ji-Rong Wen. 2024. Adapting large language models by
integrating]

[collaborative semantics for recommendation.
In]*[ 2024 IEEE
40th
International]*

*[Conference on Data Engineering
(ICDE)]*[.
IEEE,
1435--1448.]

[\[41\]][
Ruitao Zhu, Yangsu Liu, Dagui Chen, Zhenjia Ma, Chufeng Shi, Zhenzhe
Zheng,]

[Jie Zhang, Jian Xu, Bo Zheng, and Fan Wu. 2024. Contextual Generative
Auction]

[with Permutation-level Externalities for Online
Advertising.]*[
arXiv
preprint]*

*[arXiv:2412.11544]*[
(2024).]
:::
