# 🧠 Transformer Interpretability

This project is a deep dive into Mechanistic Interpretability, the art and science of reverse-engineering neural networks to understand their internal "circuits." Instead of treating models like black boxes, we aim to understand how they work, piece by piece.

Our journey will follow a structured path, starting from the simplest possible language model and progressively adding complexity. At each stage, we will develop and apply visualization and analysis techniques to understand what the model has learned.

Our work is heavily inspired by the groundbreaking framework laid out by Anthropic's research team.



## 🔬 Experiments

### ✅ Experiment 1: The 0-Layer Transformer (Embedding-Only Model)

This is our starting point. A "0-layer Transformer" is the simplest possible language model, consisting only of an embedding layer and an unembedding layer.

Architecture: Token -> Embedding -> Unembedding -> Logits
What it Learns: It learns simple bigram statistics. The model's knowledge is stored in the interaction between the embedding matrix (W_E) and the unembedding matrix (W_U). For example, it learns that the embedding for "the" should be passed through W_U to produce a high score for "cat".
Interpretability Techniques

Direct Bigram Visualization: We can directly probe the learned bigram relationships.
 What follows this word? Feed a token's embedding into W_U and find the top-scoring next tokens.
 What words predict this one? Find which input embeddings most strongly activate the output neuron for a specific word.
 Embedding Space Visualization: Since a direct heatmap of the (50k, 64) embedding matrix is meaningless, we use dimensionality reduction (like PCA or t-SNE) to project the 64-dimensional vectors into 2D.

<hr/>

### ⌛ IN_PROGRESS: Experiment 2: The 1-Layer Transformer (Attention-Only)

Building on the 0-layer model, we introduce a single Causal Self-Attention head. No MLPs yet.

 Architecture: Token -> Embedding -> Positional Embedding -> Attention -> Unembedding -> Logits
 What it Learns: The model can now learn more complex trigram and longer-range dependencies. The attention head learns to "copy" relevant information from previous tokens to the current token's representation.
 Planned Interpretability Techniques

Attention Weight Visualization: We will create interactive heatmaps to see which tokens the model is "attending to" when making a prediction for any given token. This is the classic "text hovering" visualization.
Logit Attribution: We will trace the path from a specific prediction back through the attention mechanism to see which previous tokens contributed most to that prediction.

Additional refernces:
- Kobayashi et al., Attention is Not Only a Weight: Analyzing Transformers with Vector Norms (EMNLP 2020)
- Kobayashi et al., Incorporating Residual and Normalization Layers into Analysis of Masked Language Models (EMNLP 2021)
- Kobayashi et al., Analyzing Feed-Forward Blocks in Transformers through the Lens of Attention Maps (ICLR 2024 Spotlight)
<hr/>

### 🚧 TODO: Experiment 3: The 2-Layer Transformer (Attention-Only)

We add a second attention-only layer. This is where things get really interesting.

 Architecture: Embedding -> Pos. Emb. -> Attention(1) -> Attention(2) -> Unembedding -> Logits
 What it Learns: Models with two or more layers can learn compositional algorithms. A famous example is the Induction Head, where the second layer learns to recognize and copy patterns identified by the first layer (e.g., "A...B...A...B").
 Planned Interpretability Techniques

Circuit Analysis: We will attempt to isolate and understand the induction head circuit by analyzing the attention patterns of both layers and how they interact.
Activation Patching: We will run interventions where we replace the activations of one layer with those from a different input to see how it affects the final output.

<hr />

### 🚧 TODO: Experiment 4+: Scaling to "Big Models"

Finally, we will introduce the full suite of modern Transformer components: Multi-Head Attention, MLP blocks, Layer Normalization, and Residual Streams.

 Architecture: Standard Transformer Decoder.
 What it Learns: Highly complex algorithms for tasks like in-context learning, factual recall, and code generation. The model's "knowledge" is distributed across many components (attention heads, MLP neurons, etc.).
 Planned Interpretability Techniques

Head & Neuron Analysis: We will identify and catalog the function of individual attention heads (e.g., "name mover heads," "duplicate token heads") and MLP neurons.
Direct Logit Attribution (DLA): A more advanced technique to precisely quantify the contribution of each component in the residual stream to the final logits.


## 📚 Guiding Framework
This project is a practical implementation of the ideas presented in: 

```
Elhage, et al., "A Mathematical Framework for Transformer Circuits", Transformer Circuits Thread, 2021.
Olsson, et al., "In-context Learning and Induction Heads", Transformer Circuits Thread, 2022.
```


## 📜 License

MIT