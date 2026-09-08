# Attacks

A waveform is continuous like an image, just a long vector of real-valued amplitudes, so the gradient-based attacks below translate almost directly from the vision repo; only the perceptual constraint changes, from an L2/L-infinity pixel bound to signal-to-noise ratio and, eventually, psychoacoustic masking. `whitebox/` needs the model's gradient with respect to the raw waveform; `blackbox/` needs only its output probabilities.

## whitebox/

`01_FGSM` applies a single signed-gradient step directly to the raw waveform, bounded in L-infinity amplitude, then clips back to a valid [-1, 1] range. Swept across six epsilon values, attack success rate climbs from 50.0% to 100.0% while SNR falls from 36.3dB to -3.7dB, crossing the 0dB point (noise as loud as the signal) somewhere around epsilon 0.07-0.1.

`02_PGD` repeats the same sweep with the iterative, epsilon-ball-projected version of the attack (Madry et al., 2018), 10 steps instead of one. PGD saturates at 100% ASR already at the smallest epsilon tested (0.001), where FGSM only reaches 50%; FGSM needs the largest epsilon in the sweep (0.1) to catch up. The PGD-over-FGSM gap is larger here than in either the vision or the text repo.

## blackbox/

`01_WaveformSquareAttack` adapts Square Attack to a 1D signal: instead of a square patch of pixels, each iteration flips the sign of a fixed-amplitude perturbation over a random contiguous segment (5-10% of the clip's length), keeping the change only if it strictly lowers the true class's probability. At a deliberately low, fixed epsilon (0.005, chosen because FGSM only reaches 50% ASR there), attack success rate climbs from 12.5% at a 5-query budget to 87.5% at 100 queries, a real ramp even without any gradient access, though the small sample size (8 attacked clips) makes the middle of that curve noisy.
