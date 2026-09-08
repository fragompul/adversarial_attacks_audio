# 🛡️ Adversarial Attacks in Audio Deep Learning Models

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![torchaudio](https://img.shields.io/badge/torchaudio-Speech_Commands-EE4C2C)](https://pytorch.org/audio/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> **The waveform analogue of [adversarial_attacks_vision](https://github.com/fragompul/adversarial_attacks_vision) and [adversarial_attacks_nlp](https://github.com/fragompul/adversarial_attacks_nlp): fooling speech-recognizing neural networks with perturbations a human ear barely notices.**

## 📖 Project Overview

Audio classifiers sit on a third point in the input-space spectrum this project family explores. Images are continuous and dense (perturb any pixel, by any tiny amount). Text is discrete (there is no in-between word). Audio is continuous like an image, a raw waveform is just a long vector of real-valued amplitudes, so gradient-based attacks translate almost directly from the vision repo, but the *perceptual* constraint is entirely different: what makes a perturbation "imperceptible" in a waveform is psychoacoustic masking and loudness, not an L2/L-infinity pixel bound alone.

**Origin & Scope:**
This is the second sibling repository to my Bachelor's Thesis project on adversarial vision, after `adversarial_attacks_nlp`, extending the same rigor (hand-derived math, from-scratch implementations, quantitative robustness evaluation) to the audio domain. The target task is **keyword spotting** (the "Hey Siri" / "Ok Google" style wake-word and short-command classification problem), trained on Google's Speech Commands dataset, small enough to train and attack entirely on CPU, the audio equivalent of TrafficNet/GTSRB in the vision repo: a compact, from-scratch-trained classifier rather than a giant pretrained model, so the full attack-defense loop (including retraining) is tractable.

---

## ✨ Key Features & Research Areas

1. **White-Box Attacks:** FGSM and PGD applied directly to the raw waveform, bounded in amplitude (L-infinity), the most direct translation of `attacks/whitebox/01_FGSM.ipynb`/`02_PGD.ipynb` from the vision repo, since audio, unlike text, is already a continuous signal.
2. **Black-Box Attack:** Query-only, gradient-free waveform perturbation search (`attacks/blackbox/01_WaveformSquareAttack.ipynb`), mirroring Square Attack's threat model, adapted to a 1-D signal instead of a 2-D image patch.
3. **Quantitative Robustness Analytics:** All three attacks compared at a matched perturbation budget over a real sample of test clips, reporting Attack Success Rate, Signal-to-Noise Ratio (the audio-specific imperceptibility metric, in place of the L2/L-infinity norms used for images), and query cost side by side.
4. **Defenses:** Adversarial training (PGD-AT) on the keyword-spotting classifier, with an explicit before/after comparison of clean accuracy and attack success rate.
5. **Latent Space Topology:** PCA of the classifier's penultimate-layer embeddings, tracking how PGD moves an utterance's representation across the decision boundary, the audio analogue of `latent_space/` in the vision and NLP repos.

---

## 📂 Repository Guide

Notebooks are numbered in the order they are meant to be read, and every folder has its own README.

1. **[`models/`](models/)**: trains `KeywordCNN` from scratch on Speech Commands, the audio equivalent of TrafficNet/GTSRB in the vision repo.
2. **[`attacks/`](attacks/)**, split into `whitebox/` (FGSM, PGD) and `blackbox/` (Waveform Square Attack).
3. **[`robustness_evaluation/`](robustness_evaluation/)**: all three attacks compared at a matched budget over a real sample of clips.
4. **[`defenses/`](defenses/)**: PGD-AT adversarial training, with an honest before/after reading.
5. **[`latent_space/`](latent_space/)**: PCA trajectory of an utterance's embedding as PGD attacks it.

Datasets are pulled directly through `torchaudio.datasets.SPEECHCOMMANDS` (auto-downloading) rather than kept as local copies, so there is no `data/` folder to track in git.

---

## 🚀 Getting Started

```bash
git clone https://github.com/fragompul/adversarial_attacks_audio.git
cd adversarial_attacks_audio
python -m venv .venv
.venv\Scripts\activate   # or: source .venv/bin/activate on Linux/macOS
pip install -r requirements-dev.txt
jupyter lab
```

---

## 🛠️ Technology Stack

* **Deep Learning Framework:** PyTorch
* **Audio Processing:** torchaudio + soundfile (Speech Commands dataset, mel-spectrogram/waveform transforms)
* **Data Science & Visualization:** NumPy, Pandas, Scikit-Learn, Matplotlib

---

## 🌐 Part of a Research Family

Adversarial vulnerability is not specific to any one input type; the same core idea, small deliberate input changes causing disproportionate output changes, shows up wherever a model makes a decision. This repo has sibling projects applying the same rigor elsewhere:

* **[adversarial_attacks_vision](https://github.com/fragompul/adversarial_attacks_vision)** — the flagship of the family: FGSM/PGD/C&W and more on image classifiers, plus defenses, explainability, and a live dashboard.
* **[adversarial_attacks_nlp](https://github.com/fragompul/adversarial_attacks_nlp)** — fooling a sentiment classifier with gradient-guided word substitution and black-box synonym swaps.
* **[adversarial_attacks_tabular](https://github.com/fragompul/adversarial_attacks_tabular)** — small, deliberate changes to a handful of numeric features swinging a regression model's prediction (house price appraisal), closer to fraud than to a picture or sound that looks/feels wrong.

---

## Author

**Francisco Javier Gómez Pulido**

*AI Lead @ AAPEX | Double Major in Mathematics & Computer Science* | Master's in Artificial Intelligence

📫 **Let's connect:**
* **LinkedIn:** [linkedin.com/in/frangomezpulido](https://www.linkedin.com/in/frangomezpulido)
* **GitHub:** [github.com/fragompul](https://github.com/fragompul)
* **Email:** [frangomezpulido2002@gmail.com](mailto:frangomezpulido2002@gmail.com)

---
*Sibling project to [adversarial_attacks_vision](https://github.com/fragompul/adversarial_attacks_vision) and [adversarial_attacks_nlp](https://github.com/fragompul/adversarial_attacks_nlp). If you find this interesting, feel free to ⭐ star it!*
