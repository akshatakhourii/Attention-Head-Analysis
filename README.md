# Attention Head Analysis of a Math-Trained Transformer

## Author
Akshat Akhouri

## Research Question
What patterns do attention heads recognize after being trained on competition mathematics data, and how do these patterns change throughout layers?

## Dataset
This project uses the MATH dataset (EleutherAI/hendrycks_math) . The dataset has 6,757,945 characters and includes problems formatted in LaTeX notation with  solutions. Problems emulate AMC and AIME difficulty. 

## Model Architecture
The model is a character-level GPT built from scratch following Andrej Karpathy's "Neural Networks: Zero to Hero" series. Every component was coded and understood.

| Component | Specification |
|-----------|--------------|
| Type | Decoder-only transformer (GPT) |
| Parameters | 10.81M |
| Embedding dimension | 384 |
| Attention heads | 6 per layer |
| Layers | 6 |
| Context length | 256 characters |
| Vocabulary | 98 characters (ASCII + LaTeX symbols) |
| Tokenization | Character-level |
| Activation | ReLU (feedforward), Softmax (attention) |
| Normalization | LayerNorm (pre-norm configuration) |
| Dropout | 0.2 |
| Optimizer | AdamW (lr = 3e-4) |

## Training Results
The model was trained for 5,000 iterations on a T4 GPU with a batch size of 32.

| Step | Train Loss | Val Loss |
|------|-----------|----------|
| 0 | 4.6105 | 4.5982 |
| 500 | 1.6001 | 1.5786 |
| 1000 | 1.1711 | 1.1449 |
| 2000 | 0.9418 | 0.9180 |
| 3000 | 0.8478 | 0.8376 |
| 5000 | 0.7591 | 0.7645 |

Training and validation loss stayed similar, and the loss was relatively low. This demonstrates the models ability to replicate competition problems, without relying on overfitting.

### Generated Sample
```
$(9,9)$ is slome intersect able, so each other twith two different points.
What possible sum that Melance to there worry of $99^2$ are discalars,
or $13^2=17$, if the form \[9(99 \alpha) = 129 = 13^2.\]
```

The model was able to use LaTex notation and follow the structure of typical competition problems. Conversely, the math was incoherent.

## Interpretability Findings

Attention weights were visualized from all heads. Heads that demonstrated a specific pattern were analyzed in detail.

### Head Specialization Rankings

**Most specialized (highest concentration):**
| Head | Concentration |
|------|--------------|
| Layer 5, Head 4 | 0.5073 |
| Layer 4, Head 2 | 0.4897 |
| Layer 0, Head 1 | 0.4643 |

**Least specialized (lowest concentration):**
| Head | Concentration |
|------|--------------|
| Layer 1, Head 3 | 0.2212 |
| Layer 1, Head 2 | 0.2635 |
| Layer 1, Head 1 | 0.2690 |

### Layer 0, Head 1 — Local Context Head
The model demonstrated a diagonal heatmap pattern. This demonstrates that the model failed to gain any real understanding at this stage. Instead, it simply used the token before it to inference its output.

### Layer 4, Head 2 — Mathematical Expression Head
The model began to understand the LaTeX notation that was used within the dataset. This allowed the model to recognize that mathematical operations, digits, and variables are contained within the corresponding $'s.

### Layer 5, Head 4 — Problem Structure Head
The model began to analyze the connection between the mathematical output and the text that was asking the question. Specifically, it identified the correlation between the mathematics and the text that determined the mathematics.

## Key Finding
Initially, the attention heads of the model begin to pick up on basic patterns, such as the formatting and how the actual math problems tend to follow text like "Problem". As the attention heads deeper in the network analyze the inputs, they begin to learn the actual structure of the problems. Specifically, in its deepest layer the model began to recognize the actual mathematical portion tends to be based on the initial prompt.

## Limitations
Although the model was able to recreate the structure of these math competition problems, it was unable to understand the math. In fact, it demonstrated that it learned to memorize the format of these math problems, and failed to learn any mathematical reasoning. Therefore, the output resembles a mathematical olympiad problem but fails to make the problems coherent mathematically.

## Future Work
- What model size would allow the model to comprehend mathematical reasoning?
- Could focusing on simpler math computations allow the model to learn the actual mathematics faster due to the increased similarity?

## References
- Karpathy, A. "Neural Networks: Zero to Hero." YouTube lecture series, 2023. https://github.com/karpathy/nn-zero-to-hero
- Vaswani, A., et al. "Attention Is All You Need." NeurIPS, 2017.
- Elhage, N., et al. "A Mathematical Framework for Transformer Circuits." Anthropic, 2021.
- Hendrycks, D., et al. "Measuring Mathematical Problem Solving With the MATH Dataset." NeurIPS, 2021.
