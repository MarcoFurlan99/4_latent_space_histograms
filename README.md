# some new datasets

I created new datasets with the same underlying masks. The masks are always generated with the same algorithm:

<img src="https://github.com/MarcoFurlan99/4_latent_space_histograms/blob/master/images/masks.png?raw=true">

## edges

Obtained convolving the mask with the filter $1$

<img src="https://github.com/MarcoFurlan99/4_latent_space_histograms/blob/master/images/edges.png?raw=true">

## gradient

Obtained creating two random perlin noises grayscales (shifted to avoid the issue of the constant zero-crossings) and replacing white with one of them and black with the second one.

<img src="https://github.com/MarcoFurlan99/4_latent_space_histograms/blob/master/images/gradient.png?raw=true">

## random

Simply randomize the parameters $(\mu_1,\sigma_1, \mu_2, \sigma_2)$, in the following way:
- $\mu_1$ random integer in $[10, 244]$
- $\sigma_1$ random integer in $[1, 100]$
- $\mu_2$ random integer in $[\mu_2, 244]$
- $\sigma_2$ random integer in $[1, 100]$

<img src="https://github.com/MarcoFurlan99/4_latent_space_histograms/blob/master/images/random.png?raw=true">

# the second experiment

