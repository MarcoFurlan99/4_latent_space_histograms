# New datasets

I created new datasets with the same underlying masks. The masks are always generated with the same algorithm:

<img src="https://github.com/MarcoFurlan99/4_latent_space_histograms/blob/master/images/masks.png?raw=true">

## edges

Obtained convolving the mask with the filter

$$\begin{bmatrix}
-1 & -1 & -1\\
-1 &  8 & -1\\
-1 & -1 & -1\\
\end{bmatrix}$$

<img src="https://github.com/MarcoFurlan99/4_latent_space_histograms/blob/master/images/edges.png?raw=true">

## gradient

Obtained creating two random perlin noises grayscales (shifted to avoid the issue of the constant zero-crossings) and replacing white with one of them and black with the second one.

<img src="https://github.com/MarcoFurlan99/4_latent_space_histograms/blob/master/images/gradient.png?raw=true">

## random

Usual white noise but we randomize the parameters $(\mu_1,\sigma_1, \mu_2, \sigma_2)$, in the following way:
- $\mu_1$ random integer in $[10, 244]$
- $\sigma_1$ random integer in $[1, 100]$
- $\mu_2$ random integer in $[\mu_1, 244]$
- $\sigma_2$ random integer in $[1, 100]$

<img src="https://github.com/MarcoFurlan99/4_latent_space_histograms/blob/master/images/random.png?raw=true">

# The experiment

These new datasets allow a bigger variety of casistics than the usual white noise datasets with fixed parameters $(\mu_1,\sigma_1, \mu_2, \sigma_2)$. What I did is I trained the model on 5 source datasets, and got the results on 10 (distinct) target datasets. The division was done in the following way:

<img src="https://github.com/MarcoFurlan99/4_latent_space_histograms/blob/master/images/samples.png?raw=true">

The first 5 are generated twice, one dataset is used as source (training) and the other dataset as target (testing); the last 5 are used only as target. Each dataset has 5000 samples (both source and target). Here's a quick explanation for each:

- (118, 05, 138, 50) - very hard
- (118, 05, 138, 50) - very easy
- random
- gradient
- edges
- (78, 50, 178, 50) - very large variance
- (20, 50, 60, 50) - dark
- (180,50,220,50) - bright
- (33, 10, 43, 100) - $\mu_1 \approx \mu_2, \sigma_1 << \sigma_2$
- (56, 100, 66, 10) - $\mu_1 \approx \mu_2, \sigma_1 >> \sigma_2$




Here is the usual graph with the results:

<img src="https://github.com/MarcoFurlan99/4_latent_space_histograms/blob/master/images/results.png?raw=true">

Some observations:
- generally, UNet without BN does not do well with new datasets, with 2 remarkable exceptions: *edges-(28,50,228,50)*, having 0.92 IoU, and *gradient-edges* having 0.95 IoU.

- some guesses: the *gradient*-trained UNet essentially learned edge detection. This would explain the low scores on noisy datasets but high scores on *gradient* and *edges*. The *edges*-trained one I have no clue but it does something around the 128-value. The *random*-trained one essentially learns a threshold around 128 which explains the high score in *(28, 50, 228, 50)* and low score everywhere else.

- BN adaptation works well in the white noise targets (the ones represented by 4-tuples), but not as well in the other 3 (*random*, *gradient*, *edges*). Plus, the effective increase is hard to explain sometimes (a prime example is the last two target datasets, *(33, 10, 43, 100)* and *(56, 100, 66, 10)*, which are notably hard to predict given our source datasets, but in some cases BN adaptation does miracles).

And here are the latent space histograms relative to the 0th dimension

<img src="https://github.com/MarcoFurlan99/4_latent_space_histograms/blob/master/images/dim0.png?raw=true">

<img src="https://github.com/MarcoFurlan99/4_latent_space_histograms/blob/master/images/dim0_adapted.png?raw=true">

And the training history:

<img src="https://github.com/MarcoFurlan99/4_latent_space_histograms/blob/master/images/training_history.png?raw=true">
