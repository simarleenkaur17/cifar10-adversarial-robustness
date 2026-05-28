
CIFAR-10 Adversarial Robustness Study
Investigating how easily a trained convolutional neural network can be fooled by adversarial examples, using the CIFAR-10 image classification dataset.
Overview
This project trains a custom CNN on CIFAR-10 and then attacks it using the Fast Gradient Sign Method (FGSM) — a gradient-based adversarial attack. The goal is to understand how small, imperceptible perturbations to input images can cause a model to make completely wrong predictions with high confidence.
The results are benchmarked against a naive random perturbation baseline to highlight why gradient information is so powerful for constructing adversarial examples.
Results
AttackAccuracy at ε = 0.03NotesNo attack~68%Baseline test accuracyFGSM~0% (10/10 fooled)Perturbations invisible to humansNaive randomNo changeFailed in 2,000 attempts per image

At ε = 0.01 (barely visible), FGSM drops model accuracy from 68% → 15.8%
At ε = 0.05, accuracy collapses to ~1.6% — below random guessing for 10 classes

Model Architecture
Custom CNN trained on 60,000 CIFAR-10 images (32×32 RGB, 10 classes):

Conv Block 1: 6 filters (5×5), Batch Normalisation, ReLU, 2×2 Max Pooling
Conv Block 2: 16 filters (5×5), Batch Normalisation, ReLU, 2×2 Max Pooling
Fully Connected: 400 → 120 → 84 → 10

Training: Adam optimiser (lr=0.001), Cross-Entropy loss, 30 epochs, batch size 128, GPU (Google Colab)
Augmentation: Random horizontal flip + random crop (4px padding)
Final test accuracy: ~68%
Key Concepts

FGSM: Computes gradient of the loss w.r.t. the input image (not the weights), then nudges each pixel in the direction that maximises the loss: x_adv = x + ε · sign(∇ₓL(θ, x, y))
Why it works: The same backpropagation that makes neural networks trainable also makes them exploitable
Why random fails: CIFAR-10 images have 3,072 input dimensions — finding an effective perturbation blindly is nearly impossible

Tech Stack

Python 3
PyTorch
NumPy
Matplotlib
Google Colab (GPU)

Files
FileDescriptioncifar10_adversarial.ipynbFull notebook: training, FGSM attack, benchmarking, visualisations
Background
Adversarial examples were first highlighted by Goodfellow et al. (2014). This project explores their practical impact on a moderately-sized CNN and has direct relevance to safety-critical ML applications such as autonomous driving and medical imaging.

Final year project — BSc Mathematics & Statistics, Queen Mary University of London (2026)
