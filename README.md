# dl-generalization-study
---
**Improving Generalization in MLPs: Effects of Regularization and Optimization Strategies**
---
## 1. Introduction

In this project, we investigate how different training strategies affect the performance and generalization ability of a Multi-Layer Perceptron (MLP).

Rather than designing complex architectures, our goal is to analyze how specific methodological choices such as learning rate, regularization, and optimization impact training dynamics and generalization behavior.

We conduct controlled experiments where all components are kept constant except for one variable at a time. This allows us to isolate the effect of each technique and draw meaningful conclusions.

---

## 2. Model and Dataset

We use the Fashion-MNIST dataset, which consists of grayscale images of clothing items across 10 classes.

### Model Architecture
- Input Layer: 784 (flattened 28×28 images)
- Hidden Layer 1: 128 neurons (ReLU)
- Hidden Layer 2: 64 neurons (ReLU)
- Output Layer: 10 classes

---

## 3. Fixed Training Setup

To ensure fair comparison across experiments:

- Optimizer: Adam  
- Learning Rate: 0.001  
- Batch Size: 64  
- Epochs: 15  

### Data Split
- Training: 54,000  
- Validation: 6,000  
- Test: 10,000  

All experiments follow this identical setup.

---

# 4. Experiments

---

## 4.0 Base Experimental Setup

Before conducting individual experiments, a common reference configuration was established.

The purpose of this setup is not to define a separate model, but to create a controlled experimental pipeline where only one variable is modified at a time.

### Rationale
- Ensures fair comparison  
- Eliminates confounding variables  
- Improves reproducibility  

This approach aligns with best practices in experimental deep learning.

---

## **4.1 Learning Rate Analysis**

We analyze the effect of different learning rates:

* 0.1
* 0.01
* 0.001

### **Findings**

* High LR (0.1) → unstable training
* Medium LR (0.01) → improved but still inconsistent
* Low LR (0.001) → best convergence and stability

Thus, **0.001 is selected as the optimal learning rate**.

---

**Figure 1 — Performance comparison across different learning rates**

<img width="501" height="114" alt="image" src="https://github.com/user-attachments/assets/2085b003-eb95-4ed4-a637-83151c4b7a34" />


---


## **4.2 L2 Regularization (Weight Decay)**

We evaluate the effect of L2 regularization using:

* 0.0 (no regularization)
* 1e-4 (moderate)
* 1e-3 (strong)

---

**Figure 2 — Validation Accuracy (L2)**

<img width="496" height="480" alt="image" src="https://github.com/user-attachments/assets/f1253b57-dc55-4f8f-b577-49f23b60d9bf" />


---

**Figure 3 — Generalization Gap**

<img width="522" height="495" alt="image" src="https://github.com/user-attachments/assets/46ca2dd6-cd1c-432b-8070-95c719e26135" />


---

**Figure 4 — Validation Loss (L2)**

<img width="518" height="539" alt="image" src="https://github.com/user-attachments/assets/d86084f7-0cf3-4287-8732-4b8d2cfe1337" />


---

### **Results Summary**

| Weight Decay | Test Accuracy | Test Loss | Generalization Gap |
| ------------ | ------------- | --------- | ------------------ |
| 0.0          | 0.8856        | 0.3519    | 0.0338             |
| 1e-4         | 0.8815        | 0.3524    | 0.0295             |
| 1e-3         | 0.8748        | 0.3493    | 0.0126             |

---

### **Analysis**

* No regularization → highest accuracy but large gap (overfitting)
* Moderate regularization → balanced performance
* Strong regularization → lowest gap but reduced accuracy


---

## 4.3 Dropout Regularization

### Objective

This experiment evaluates the effect of Dropout on generalization.

Unlike L2 regularization, Dropout randomly deactivates neurons during training, forcing the network to learn more distributed representations.

---

### Methodology

- Dropout rate: 0.5  
- Applied to hidden layers  
- All other parameters unchanged  

---

### **Figure 5 — Performance metrics across different dropout rates**
<img width="420" height="85" alt="image" src="https://github.com/user-attachments/assets/b05e9a9e-8c3b-4210-9eba-ce69b58ca739" />


---

### Analysis

Dropout reduces overfitting by preventing co-adaptation between neurons.

While training accuracy slightly decreases, validation performance becomes more stable.

Compared to L2:
- L2 → constrains weights  
- Dropout → improves feature robustness  

---

## **4.4 Optimizer Comparison Analysis**

### **Objective**

This experiment evaluates how different optimization algorithms affect convergence speed, stability, and final performance.

---

### **Methodology**

Optimizers tested:

* SGD
* SGD + Momentum (0.9)
* Adam

All other parameters remain fixed.

---

**Figure 6 — Optimizer Comparison (Accuracy & Loss)**

<img width="469" height="334" alt="image" src="https://github.com/user-attachments/assets/c866f38f-68e6-4970-94e8-df7c69684df0" />


---

### **Results Summary**

| Optimizer      | Test Accuracy | Test Loss | Behavior     |
| -------------- | ------------- | --------- | ------------ |
| SGD            | 69.88%        | 0.7827    | Underfitting |
| SGD + Momentum | 87.82%        | ~0.38     | Stable       |
| Adam           | 88.61%        | 0.3442    | Best         |

---

### **Analysis**

* SGD → slow convergence → underfitting
* Momentum → faster convergence
* Adam → adaptive learning → best performance

Optimizer choice is **critical for training efficiency**

---

# **4.5. Robustness Analysis**

## **Objective**

We evaluate how models behave under noisy input conditions to measure real-world reliability.

---

## **Methodology**

Gaussian Noise added:

* σ = 0.0 → clean
* σ = 0.1 – 0.4 → increasing noise

Models:

* Baseline
* Dropout
* L2 Regularization
* Adam-based model

---

 **Figure 7 — Robustness Comparison**

<img width="465" height="279" alt="image" src="https://github.com/user-attachments/assets/9cf723ec-a255-4aa7-95d1-f85f2e13e0e2" />


---

### **Results Table**

| Noise | Baseline | Dropout | L2     | Adam   |
| ----- | -------- | ------- | ------ | ------ |
| 0.0   | 0.8815   | 0.8593  | 0.8593 | 0.8863 |
| 0.1   | 0.8787   | 0.8608  | 0.8578 | 0.8658 |
| 0.2   | 0.8773   | 0.8528  | 0.8538 | 0.8240 |
| 0.3   | 0.8646   | 0.8500  | 0.8515 | 0.7438 |
| 0.4   | 0.8478   | 0.8488  | 0.8425 | 0.6595 |

---

### **Analysis**

* Adam → best on clean data but fragile
* Dropout → most stable
* L2 → moderate robustness

Regularization improves **robustness under noise**

---

# **6. Final Conclusion**

This project demonstrates that generalization in neural networks is not only determined by model architecture, but heavily influenced by training strategies.

### Key Insights:

* Learning rate controls convergence stability
* Regularization reduces overfitting
* Optimizers determine training efficiency
* Dropout improves robustness

### Final Takeaway:

> There is a fundamental trade-off between **accuracy, generalization, and robustness**.
> The best-performing model in clean conditions is not always the most reliable in real-world scenarios.



