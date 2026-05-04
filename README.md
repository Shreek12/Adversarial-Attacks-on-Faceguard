🛡️ FaceGuard Security Assessment — Red Team & Blue Team 

This project extends the FaceGuard facial recognition system developed by performing a full AI security assessment using a structured Red Team / Blue Team methodology. The goal is to evaluate how an adversary could compromise the system through model stealing and adversarial attacks, and how defensive strategies such as adversarial training can improve robustness.

##**Project Overview**
FaceGuard is a CNN‑based facial recognition system that controls access to a secure server room. Only three employees are authorised:

CEO — ID 0
CTO — ID 5
System Administrator — ID 10

In this coursework, we investigate whether an attacker with black‑box access (query‑only) can:
-Steal the model
-Generate adversarial examples
-Transfer attacks to the real system
-Bypass access control (targeted impersonation)
-Be defended against using adversarial training

**Red Team Tasks**
**Task 1 — Victim Model Setup (Prerequisite)**
The original FaceGuard CNN from CW1 is loaded and evaluated on the Augmented Olivetti Faces dataset.

Architecture: 2‑block CNN (Conv → BN → ReLU → Pool → FC)

Dataset: 2000 images, 40 classes

Clean Test Accuracy: 98.75% (meets ≥85% requirement)

This model acts as the victim/target model for all attacks.

**Task 2 — Model Stealing Attack**
An attacker with query‑only access steals the model by:
Black‑box query wrapper
Only returns predicted labels — no gradients or internal details.
Surrogate dataset creation
All training images are queried to obtain stolen labels.
Proxy model training
A lighter CNN (ProxyCNN) is trained on the stolen dataset.

Results
Proxy Test Accuracy: 94.00%
Victim Accuracy: 98.75%
Fidelity Gap: 4.75%

The proxy successfully mimics the victim model and is strong enough for adversarial crafting.

**Task 3 — Adversarial Attacks on Proxy Model**
Implemented Attacks
FGSM (untargeted)
PGD (untargeted, iterative)
Targeted PGD (non‑authorised → CEO impersonation)

Robustness Evaluation
Accuracy vs. epsilon curves show:

FGSM collapses accuracy at ε ≥ 0.05
PGD is even stronger — accuracy drops to 0% at small epsilons
Proxy model is highly vulnerable
Targeted Attack
A non‑authorised user (e.g., ID 39) is modified to impersonate the CEO (ID 0).

Result:

Proxy model predicts 0
Victim model also predicts 0
Targeted impersonation attack succeeds and transfers
This demonstrates a severe security risk.

**Task 4 — Transferability Attack**
Adversarial examples crafted on the proxy model are tested on the victim model.

Transferability Findings
FGSM and PGD adversarial examples successfully fool the victim model
Transfer rate increases with epsilon
PGD‑crafted examples transfer more reliably
Targeted impersonation (→ CEO) also transfers

Implication
Even without access to the victim model’s architecture or gradients, an attacker can still bypass FaceGuard using a stolen proxy.

##**Blue Team Task**
**Task 5 — Adversarial Training Defence**
A new model, Robust_FaceGuard, is trained using PGD adversarial training.
Defence Steps
Generate PGD adversarial examples during training
Train model on both clean + adversarial samples
Evaluate robustness against FGSM and PGD

Results
Clean accuracy decreases (expected tradeoff)
Robust accuracy significantly improves
Robust_FaceGuard withstands higher epsilons compared to original FaceGuard

Accuracy–Robustness Tradeoff
Original model: high clean accuracy, poor robustness
Robust model: slightly lower clean accuracy, much higher adversarial resilience
In real‑world access control, robustness is more important than marginal clean accuracy gains

Key Visualisations Included
Victim model training curves
Proxy model training curves
FGSM & PGD accuracy vs. epsilon

Transferability comparison (proxy vs. victim)
Targeted attack visualisation (original, adversarial, perturbation)
Robust vs. non‑robust model comparison plots

## Technologies Used
Python
PyTorch
NumPy
Matplotlib
Scikit‑Learn
Seaborn

##**Project Structure**
Code

├── augmented_faces.npy
├── augmented_labels.npy
├── best_faceguard_model.pth
├── CW2_ShreeyaKangutkar.ipynb
├── README.md
└── images/ (optional visualisations)

## Academic Context
This project was completed as part of:

ELE8100 – CyberAI  
MSc Applied Cyber Security
Queen’s University Belfast
Academic Year 2025/26

## AI Assistance Declaration
This coursework includes AI‑assisted support for:

Structuring explanations
Improving clarity of markdown analysis
All code implementation, attack logic, and analysis were authored by the me.
