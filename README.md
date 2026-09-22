# Self-Attention as Burgers-type Transport

Notes de réflexion sur une piste de recherche : est-ce que la théorie des lois de conservation (Burgers, chocs, solutions entropiques) peut apporter un cadre théorique ou un biais inductif utile aux architectures Transformer ?

## L'idée

Depuis quelques années, plusieurs travaux modélisent les réseaux profonds comme des systèmes dynamiques continus : un ResNet ou un Transformer peut être vu comme la discrétisation d'une EDO/EDP appliquée à un ensemble de "particules" (les tokens). Dans cette optique :

- **Lu et al. (2019)** montrent qu'un bloc Transformer (attention + feed-forward) s'interprète comme un pas d'intégration d'une équation de **convection-diffusion** : l'attention joue le rôle du terme de transport/convection (non-linéaire, car le champ de vitesse dépend de la configuration des tokens), le feed-forward et les résidus jouant un rôle diffusif. C'est structurellement très proche de Burgers ($u_t + u u_x = \nu u_{xx}$), qui est le cas scalaire le plus simple de ce type d'équation.

- **Geshkovski, Letrouit, Polyanskiy, Rigollet (2023)** étudient la limite champ moyen du self-attention comme système de particules en interaction sur la sphère, et montrent l'émergence de **clusters** au cours du temps — un phénomène de concentration qui rappelle la formation de chocs (points où des caractéristiques distinctes convergent en un point singulier) dans les lois de conservation non-linéaires comme Burgers.

L'hypothèse à explorer : dans Burgers, la formation de chocs est *comprise et contrôlée* via les conditions d'entropie de Kruzhkov (elles sélectionnent la solution physique unique parmi les solutions faibles). Le "clustering" / la perte de rang (rank collapse) dans l'attention est un phénomène analogue mais aujourd'hui traité empiriquement (LayerNorm, dropout, résiduels agissent comme régularisation implicite). Une théorie des chocs pour l'attention pourrait potentiellement :

1. donner des garanties théoriques sur *quand* et *comment* les tokens se regroupent en fonction de la profondeur/temps ;
2. suggérer un terme de diffusion explicite et contrôlé (à la Burgers visqueux) plutôt que les mécanismes de régularisation actuels, avec un paramètre de viscosité interprétable et potentiellement appris ;
3. fournir des critères de stabilité numérique (type condition CFL) pour choisir profondeur/pas résiduel de façon plus principielle qu'empirique.

## Limites et prudence

- Le système d'attention est **vectoriel et non-scalaire**, en interaction sur la sphère (ou dans $\mathbb{R}^d$), alors que Burgers classique est un cas scalaire 1D. L'analogie est structurelle, pas une équivalence mathématique directe.
- L'attention est **doublement stochastique** (softmax), ce qui n'a pas d'équivalent direct dans Burgers — le lien est plus proche du transport optimal / Sinkhorn que d'une viscosité classique.
- À ce stade, c'est une piste de réflexion, pas un résultat établi : l'objectif de ce repo est de rassembler les références pertinentes avant d'éventuellement expérimenter.

## Références

- Lu, Y., Li, Z., He, D., Sun, Z., Dong, B., Qin, T., Wang, L., Liu, T.-Y. (2019). *Understanding and Improving Transformer From a Multi-Particle Dynamic System Point of View.* [arXiv:1906.02762](https://arxiv.org/abs/1906.02762)
- Geshkovski, B., Letrouit, C., Polyanskiy, Y., Rigollet, P. (2023). *The emergence of clusters in self-attention dynamics.* NeurIPS 2023. [arXiv:2305.05465](https://arxiv.org/abs/2305.05465)
- Geshkovski, B., Letrouit, C., Polyanskiy, Y., Rigollet, P. (2023). *A mathematical perspective on Transformers.* [arXiv:2312.10794](https://arxiv.org/abs/2312.10794)
- E, W. (2017). *A Proposal on Machine Learning via Dynamical Systems.* Communications in Mathematics and Statistics. [DOI:10.1007/s40304-017-0103-z](https://doi.org/10.1007/s40304-017-0103-z)
- Chen, R. T. Q., Rubanova, Y., Bettencourt, J., Duvenaud, D. (2018). *Neural Ordinary Differential Equations.* NeurIPS 2018. [arXiv:1806.07366](https://arxiv.org/abs/1806.07366)
- Kruzhkov, S. N. (1970). *First order quasilinear equations in several independent variables.* Mathematics of the USSR-Sbornik — référence classique sur les solutions entropiques des lois de conservation (dont Burgers).

Voir aussi le repo compagnon : [Burger's Equation with Stratonovich Noise](https://github.com/Alphaeo/Burger-s-Equation-with-Stratonovich-Noise), qui traite Burgers de façon numérique/stochastique.
