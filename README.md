# FaceGuard AI Security Assessment

This repository contains the full Red Team / Blue Team security evaluation of **FaceGuard**, the facial recognition access‑control system developed in CW1. The project investigates how an attacker can steal the model, generate adversarial examples, transfer attacks to the real system, and how adversarial training can defend against these threats.

---

## 1. Overview

FaceGuard is a CNN‑based facial recognition system trained on the Augmented Olivetti Faces dataset (40 classes). Only three employees are authorised for server‑room access:

- CEO (ID 0)  
- CTO (ID 5)  
- System Administrator (ID 10)

This coursework evaluates the security of FaceGuard using:

- Model Stealing  
- FGSM and PGD Adversarial Attacks  
- Targeted Impersonation Attacks  
- Transferability Analysis  
- Adversarial Training Defence  

---

## 2. Victim Model (FaceGuard)

- Architecture: 2‑block CNN (Conv → BN → ReLU → MaxPool → FC)  
- Dataset: Augmented Olivetti Faces (2000 images, 40 classes)  
- Clean Test Accuracy: **98.75%**

This model acts as the **victim/target** for all attacks.

---

## 3. Task 2 — Model Stealing (Proxy Model)

An attacker with black‑box access queries the victim model to build a surrogate dataset.

### Steps:
1. Query victim model for labels  
2. Train a smaller CNN (ProxyCNN) on stolen labels  
3. Compare proxy vs victim performance  

### Results:
- Proxy Model Accuracy: **94.00%**  
- Victim Model Accuracy: 98.75%  
- Fidelity Gap: 4.75%

The proxy successfully mimics the victim and is suitable for adversarial crafting.

---

## 4. Task 3 — Adversarial Attacks on Proxy Model

### Implemented Attacks:
- FGSM (untargeted)  
- PGD (untargeted, iterative)  
- Targeted PGD (non‑authorised → CEO impersonation)  

### Key Findings:
- FGSM accuracy collapses at epsilon ≥ 0.05  
- PGD reduces accuracy to **0%** at small epsilons  
- Targeted PGD successfully forces a non‑authorised user to be classified as **CEO (ID 0)**  
- The targeted attack also fools the victim model  

This demonstrates a serious impersonation risk.

---

## 5. Task 4 — Transferability Attack

Adversarial examples crafted on the proxy model are tested on the victim model.

### Results:
- FGSM and PGD adversarial examples transfer successfully  
- Transfer rate increases with epsilon  
- Targeted impersonation attack also transfers  

This shows that even black‑box attackers can bypass FaceGuard.

---

## 6. Task 5 — Adversarial Training Defence

A new model, **Robust_FaceGuard**, is trained using PGD adversarial training.

### Results:
- Clean accuracy decreases (expected tradeoff)  
- Robust accuracy improves significantly  
- Robust_FaceGuard withstands higher epsilons compared to original FaceGuard  

### Conclusion:
Adversarial training improves robustness but reduces clean accuracy.  
For real‑world access control, robustness is more important than small accuracy gains.

---

## 7. Technologies Used

- Python  
- PyTorch  
- NumPy  
- Matplotlib  
- Scikit‑Learn  
- Seaborn  

---

## 8. Project Structure

├── augmented_faces.npy

├── augmented_labels.npy

├── best_faceguard_model.pth

├── CW2_ShreeyaKangutkar.ipynb

└── README.md

---

## 9. Academic Context

ELE8100 – CyberAI  
MSc Applied Cyber Security  
Queen’s University Belfast  
Academic Year 2025/26

---

## 10. AI Assistance Declaration

AI assistance was used for structuring explanations and improving clarity of markdown documentation.  
All code, attack implementations, and analysis were authored by the student.

