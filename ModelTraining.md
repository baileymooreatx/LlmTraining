# Model Training

LLM training is a **multi-stage process** that transforms a neural network from
random parameters into a capable language model. The core pipeline has three
main phases:

| **Phase**                                         | **What happens**                                                                                                                                                                                     | **Data used**                                                         |
|:--------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------|
| Pre-training                                      | The model learns to **predict the next token** in a sequence via self-supervised learning. It acquires grammar, facts, reasoning patterns, and world knowledge.                                      | Trillions of tokens from books, web pages, code repos, articles, etc. |
| Supervised Fine-Tuning (SFT)                      | The pre-trained model is trained on curated prompt–response pairs so it learns to **follow instructions** and act as an assistant rather than just continuing text.                                  | Tens of thousands to hundreds of thousands of high-quality examples   |
| Reinforcement Learning from Human Feedback (RLHF) | A **reward model** is trained on human rankings of responses, then used to fine-tune the LLM (often via PPO) to produce outputs that are more **helpful, safe, and aligned** with human preferences. | Human preference rankings / verifiable rewards                        |

**Key technical details:**

- **Architecture:** Most LLMs use the **transformer** architecture, with
  self-attention mechanisms that let the model weigh relationships between all
  tokens in a sequence.
- **Tokenization:** Raw text is split into subword units (e.g., via Byte Pair
  Encoding) and converted into numerical vectors (embeddings) before training.
- **Optimization:** The model repeatedly predicts the next token, compares its
  prediction to the actual token (computing a loss), and updates its billions of
  parameters via **backpropagation** and gradient descent.
- **Scale:** Training requires **hundreds to thousands of GPUs/TPUs** working in
  coordinated clusters, with techniques like mixed-precision arithmetic,
  gradient checkpointing, and data/model parallelism to manage memory and
  throughput.
- **Compute cost:** Training a frontier LLM can take weeks or months and consume
  millions of GPU-hours, making it one of the most expensive ML tasks in
  existence.

The overall flow is: **Raw text → Clean & tokenize → Pre-train (next-token
prediction) → Fine-tune (instruction following) → RLHF (human alignment) →
Deploy.**
