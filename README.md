# SDPH: Signed Distance Persistent Homology

``SDPH`` allows you to quantify the texture or patterns of 3D shapes using persistent homology. You may apply it on 3D shapes representing vascular networks, porous materials, tubular shapes, branching membranes, etc. 

The framework is described in the article (.bib sample below)
> [Generalized Morse Theory of Distance Functions to Surfaces for Persistent Homology
](http://arxiv.org/abs/2306.14716)
> 
> First submitted 26 June 2023, by Anna Song (1,2), Ka Man Yim (3), Anthea Monod (1)

(1) Department of Mathematics, Imperial College London, London, UK \
(2) Haematopoietic Stem Cell Laboratory, The Francis Crick Institute, London, UK \
(3) School of Mathematics, Cardiff University, Cardiff, UK

# Usage

Computing SDPH diagrams on a shape `S` is easy and holds in just a few lines. 
We first convert `S` to its signed distance field `distmap`, then compute the sublevel-set persistent homology of `distmap` based on a cubical representation.

Suppose that you have a shape `S` separating an "inside" region `W-` from an "outside" region `W+`. Instead of manipulating the shape `S` directly, we represent `S` implicitly, with a scalar field `u : R^3 -> R`, such that to `S = {u = 0}`, `W- = {u < 0}`, and `W+ = {u > 0}`. Then the SDPH diagrams `PH0, PH1, PH2` can be computed with:

```
import skfmm
from gtda.homology import CubicalPersistence

distmap = -skfmm.distance(u >= 0, dx = 1) + skfmm.distance(u < 0, dx = 1)
CP = CubicalPersistence(homology_dimensions=[0, 1, 2], reduced_homology = False) 
diagrams = CP.fit_transform(distmap[None])[0]
```

# Structure of the directory

This repository simply presents ways to use, explore, and visualize the results, by wrapping around these few lines.

- `SDPHG_git.ipynb` : jupyter notebook to reproduce the results of the paper
- `figures/` : some figures used in the paper

and for each category of shapes, generated with different models (Gaussian Random Fields, curvatubes, segmented vascular data), there are two associated folders named with the pattern

- `cvtub_shapes/` : contains shapes in .nii.gz files 
- `cvtub_shapes_PH/`: contains corresponding SDPH diagrams

One interesting way to test SDPH is to use it on random porous morphologies generated with the [curvatubes model](https://github.com/annasongmaths/curvatubes). To be able to use it, please refer to the indications in the notebook `SDPHG_git.ipynb`.

# Vascular data

The vascular data used in this repository is based on confocal images of bone marrow vessels produced by [Antoniana Batsivari](https://www.researchgate.net/profile/Antoniana-Batsivari) and [Dominique Bonnet](https://www.crick.ac.uk/research/find-a-researcher/dominique-bonnet), to whom we are very grateful.

# Academic use

If you use this code, or ideas from the associated paper, please cite us inside your work. Thank you !

```tex
@misc{song2023generalized,
      title={Generalized Morse Theory of Distance Functions to Surfaces for Persistent Homology}, 
      author={Anna Song and Ka Man Yim and Anthea Monod},
      year={2023},
      eprint={2306.14716},
      archivePrefix={arXiv},
      primaryClass={math.AT}
}
```

# License

This code is licensed under the permissive [MIT License](https://en.wikipedia.org/wiki/MIT_License).

# Bugs and Feedback

If you have remarks or find a bug in the code, please contact me directly at `[first name].[last name].maths@gmail.com`.
