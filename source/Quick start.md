# Quick Start

## Software requirements
CryoGrid is written in [Matlab](https://www.mathworks.com/products/matlab.html). Version 2018x or higher is required.

## Known Limitations
Multi-tile (3D) functionality not yet implemented.

## Quick start -model structure
CryoGrid is a simulation tool to calculate ground temperatures and volumetric water/ice contents (as well as salt concentration, etc. depending on the selected `SUBSURFACE` classes) in single-tile (1D) stratigraphies. Multi-tile (3D) models can be realized by coupling several 1D stratigraphies.

A stratigraphy is realized by stacking one or several `SUBSURFACE` classes, which each account for different physical processes. The different SUBSURFACE classes typically have specific state variables and model parameters and use different constitutive equations to calculate those variables.

And so on ...