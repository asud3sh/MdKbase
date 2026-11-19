
# 18 · DATA SCIENCE

## 18.1 AI Foundations

### AI Basics

**Definition:** AI is the science of making machines that think or act intelligently.

**Agents:** Perceive environment; take actions; maximize reward.

**Rationality:** Choose action maximizing expected utility.

**Symbolic vs connectionist:**
- **Symbolic:** Logic, rules, knowledge representation
- **Connectionist:** Neural networks, statistical learning

**Recall:** *Machines that think/act.*

### Search Algorithms

**Uninformed:**
- **BFS:** Level-by-level; O(b^d)
- **DFS:** Depth-first; O(b^m)
- **UCS:** Uniform-cost; Dijkstra for graphs
- **IDDFS:** Iterative deepening; combines BFS + DFS

**Informed:**
- **Greedy:** h(n)
- **A*:** f(n) = g(n) + h(n)

**Recall:** *Explore state space.*

### Heuristics

**Admissible:** Never overestimates (h(n) ≤ h*(n)).
**Consistent:** h(n) ≤ c(n, n') + h(n').

**Examples:** Manhattan, Euclidean, Chebyshev.

**Recall:** *Estimate to goal.*

### A*

**Algorithm:**
```
open = {start}
g[start] = 0
f[start] = h(start)
while open:
    n = min f in open
    if n == goal: return path
    for each neighbor m:
        g_new = g[n] + cost(n, m)
        if g_new < g[m]:
            g[m] = g_new
            f[m] = g_new + h(m)
            parent[m] = n
```

**Optimal:** With admissible heuristic.
**Complete:** With finite branching.

**Recall:** *Best-first with heuristic.*

### Game Playing

**Minimax:** Maximize own, minimize opponent.
**Alpha-beta pruning:** Skip branches that can't affect result.
**MCTS:** Monte Carlo Tree Search; random simulations.

**Recall:** *Adversarial search.*

### Minimax

**Algorithm:**
```
minimax(state, depth, maximizing):
    if terminal or depth == 0: return eval(state)
    if maximizing:
        return max(minimax(child, depth-1, false))
    else:
        return min(minimax(child, depth-1, true))
```

**Alpha-beta:** Prune when α ≥ β.

**Recall:** *Assume optimal opponent.*

### Knowledge Representation

**Logic:** Propositional, predicate.
**Frames:** Objects with slots.
**Semantic nets:** Graph of concepts.
**Ontologies:** Formal specifications (RDF, OWL).

**Recall:** *Encode facts + rules.*

### Logic Programming

**Prolog:** Facts + rules + queries.
**Datalog:** Declarative; recursive queries.

**Example:**
```prolog
parent(alice, bob).
ancestor(X, Y) :- parent(X, Y).
ancestor(X, Y) :- parent(X, Z), ancestor(Z, Y).
```

**Recall:** *Declarative reasoning.*

### Planning

**STRIPS:** States, actions, preconditions, effects.
**PDDL:** Planning Domain Definition Language.
**Hierarchical:** HTN (Hierarchical Task Network).

**Recall:** *Sequence actions to goal.*

### Expert Systems

**Rule-based:** IF-THEN rules.
**Inference engine:** Forward/backward chaining.
**Examples:** MYCIN, XCON.

**Recall:** *Encode expert rules.*

---

## 18.2 Machine Learning

### Machine Learning

**Definition:** Learn patterns from data.

**Types:**
- **Supervised:** Labeled data
- **Unsupervised:** Unlabeled data
- **Reinforcement:** Reward signal
- **Self-supervised:** Labels from data

**Recall:** *Improve with experience.*

### Supervised Learning

**Classification:** Predict discrete label.
**Regression:** Predict continuous value.

**Algorithms:** Linear regression, logistic regression, decision trees, SVM, k-NN, random forests, gradient boosting.

**Recall:** *Learn mapping X → Y.*

### Unsupervised Learning

**Clustering:** Group similar points.
- K-means, hierarchical, DBSCAN

**Dimensionality reduction:** Reduce features.
- PCA, t-SNE, UMAP, autoencoders

**Recall:** *Find structure.*

### Reinforcement Learning

**Components:** Agent, environment, state, action, reward.

**MDP:** Markov Decision Process; (S, A, P, R, γ).

**Algorithms:**
- **Q-learning:** Learn Q(s, a)
- **SARSA:** On-policy
- **Policy gradients:** Learn policy directly
- **Actor-critic:** Combine value + policy

**Recall:** *Learn by trial.*

### Self-Supervised Learning

**Definition:** Generate labels from data itself.

**Techniques:**
- **Contrastive:** SimCLR, MoCo
- **Masked prediction:** BERT, MAE
- **Next-token prediction:** GPT

**Recall:** *Data labels itself.*

### Feature Engineering

**Manual:** Domain knowledge.
**Automated:** Featuretools, auto-sklearn.
**Normalization:** Min-max, z-score.
**Encoding:** One-hot, label, target.

**Recall:** *Craft inputs.*

### Model Evaluation

**Classification metrics:**
| Metric    | Formula               |
| --------- | --------------------- |
| Accuracy  | (TP+TN)/(TP+TN+FP+FN) |
| Precision | TP/(TP+FP)            |
| Recall    | TP/(TP+FN)            |
| F1        | 2 × (P×R)/(P+R)       |
| ROC-AUC   | Area under ROC curve  |

**Regression metrics:** MSE, RMSE, MAE, R².

**Cross-validation:** K-fold, stratified, leave-one-out.

**Recall:** *Measure model quality.*

### Bias & Variance

**Bias:** Error from wrong assumptions (underfit).
**Variance:** Error from sensitivity to data (overfit).

**Trade-off:** Total error = bias² + variance + noise.

**Recall:** *Underfit vs overfit.*

### Regularization

**L1 (Lasso):** Add Σ|w|; sparse weights.
**L2 (Ridge):** Add Σw²; small weights.
**Dropout:** Randomly zero activations.
**Early stopping:** Stop when validation error rises.

**Recall:** *Penalize complexity.*

---

## 18.3 Deep Learning

### Deep Learning

**Definition:** Neural networks with many layers.

**Representation learning:** Learn features automatically.

**Recall:** *Stacked layers learn features.*

### Neural Networks

**Neuron:** Weighted sum + activation.
**Layers:** Input, hidden, output.
**Activation:** ReLU, sigmoid, tanh, softmax.

**Forward pass:** Compute output.
**Backward pass:** Compute gradients.

**Recall:** *Layered function approximator.*

### Backpropagation

**Chain rule:** ∂L/∂w = ∂L/∂a × ∂a/∂w.

**Gradient descent:** w ← w − η ∂L/∂w.

**Recall:** *Credit assignment via chain rule.*

### CNNs (Convolutional Neural Networks)

**Layers:** Convolution, pooling, fully connected.

**Convolution:** Sliding filter; weight sharing.
**Pooling:** Max/average; downsampling.

**Architectures:** LeNet, AlexNet, VGG, ResNet, EfficientNet.

**Use:** Images, video.

**Recall:** *Local patterns.*

### RNNs (Recurrent Neural Networks)

**Definition:** Process sequences; hidden state.

**Vanishing gradient:** Gradients shrink over time.
**Exploding gradient:** Gradients grow; clip.

**Variants:** LSTM, GRU.

**Recall:** *Memory over time.*

### LSTMs (Long Short-Term Memory)

**Gates:**
- **Forget:** What to forget
- **Input:** What to store
- **Output:** What to output

**Cell state:** Long-term memory.

**Recall:** *Gated RNN.*

### Transformers

**Definition:** Attention-based architecture; parallelizable.

**Components:**
- **Self-attention:** Q, K, V
- **Multi-head:** Multiple attention heads
- **Positional encoding:** Inject position
- **Feed-forward:** MLP

**Architectures:** BERT (encoder), GPT (decoder), T5 (encoder-decoder), ViT (vision).

**Recall:** *Attention is all you need.*

### Attention

**Scaled dot-product:** Attention(Q, K, V) = softmax(QKᵀ/√d_k)V.

**Multi-head:** Multiple attention operations; concatenate.

**Recall:** *Weighted focus.*

### Diffusion Models

**Forward:** Add noise progressively.
**Reverse:** Denoise step-by-step.

**Architectures:** DDPM, Stable Diffusion, DALL·E 2.

**Recall:** *Learn to denoise.*

### GNNs (Graph Neural Networks)

**Message passing:** Nodes exchange messages.
**Aggregation:** Combine neighbor messages.

**Tasks:** Node classification, link prediction, graph classification.

**Recall:** *Neural nets on graphs.*

---

## 18.4 Foundation Models & LLMs

### Foundation Models

**Definition:** Large pretrained models adaptable to many tasks.

**Examples:** GPT, CLIP, Llama, Gemini.

**Recall:** *Big pretrained base.*

### LLMs (Large Language Models)

**Definition:** Transformer-based; billions of parameters.

**Training:** Pretrain → fine-tune → RLHF.

**Capabilities:** Text generation, translation, summarization, code, reasoning.

**Recall:** *Large language models.*

### Prompt Engineering

**Techniques:**
- **Zero-shot:** No examples
- **Few-shot:** Examples in prompt
- **Chain-of-thought:** "Let's think step by step"
- **ReAct:** Reason + act
- **Self-consistency:** Multiple samples; majority vote

**Recall:** *Craft input to steer output.*

### Fine-Tuning

**Full:** Update all parameters.
**LoRA:** Low-rank adaptation; efficient.
**Adapters:** Small modules inserted.
**Instruction tuning:** Fine-tune on instructions.

**Recall:** *Adapt pretrained model.*

### RLHF (Reinforcement Learning from Human Feedback)

**Steps:**
1. Collect human preferences
2. Train reward model
3. Optimize policy with PPO

**Use:** Align LLMs with human values.

**Recall:** *Learn from human feedback.*

### Multimodal Models

**Definition:** Process multiple modalities (text, image, audio).

**Examples:** CLIP, GPT-4V, Gemini, Flamingo.

**Recall:** *Many modalities.*

### AI Agents

**Definition:** LLM + tools + memory + planning.

**Frameworks:** ReAct, AutoGPT, LangChain, CrewAI.

**Recall:** *LLM that acts.*

---

## 18.5 Responsible AI

### AI Safety

**Alignment:** Ensure AI goals match human values.
**Robustness:** Perform under distribution shift.
**Misuse:** Prevent harmful applications.

**Recall:** *Ensure AI does good.*

### AI Ethics

**Fairness:** Avoid discrimination.
**Accountability:** Clear responsibility.
**Transparency:** Explainable decisions.

**Recall:** *Responsible AI.*

### Interpretability

**Local:** SHAP, LIME.
**Global:** Feature importance, surrogate models.
**Mechanistic:** Understand internal circuits.

**Recall:** *Explain model decisions.*

---

## 18.6 ML Systems

### MLOps

**Definition:** DevOps for ML.

**Components:** Pipelines, feature stores, model registry, serving, monitoring.

**Tools:** MLflow, Kubeflow, TFX.

**Recall:** *DevOps for ML.*

### Feature Stores

**Definition:** Central repository for features.

**Benefits:** Consistency, reuse, freshness.

**Tools:** Feast, Tecton, Hopsworks.

**Recall:** *Centralize features.*

### Model Serving

**Modes:** Batch, real-time, streaming.

**Tools:** TorchServe, Triton, KServe, TensorFlow Serving.

**Recall:** *Deploy models.*

### Vector Search

**Definition:** Search by similarity in embedding space.

**ANN algorithms:** HNSW, IVF, PQ.

**Tools:** Pinecone, Milvus, FAISS, Weaviate.

**Recall:** *Similarity search.*

### RAG (Retrieval-Augmented Generation)

**Definition:** Retrieve relevant docs; generate answer.

**Components:** Retriever, ranker, generator.

**Recall:** *Retrieve then generate.*

### Inference Optimization

**Techniques:**
- **Quantization:** Reduce precision
- **Pruning:** Remove weights
- **Distillation:** Train smaller model
- **ONNX:** Portable format
- **TensorRT:** NVIDIA optimization
- **vLLM:** Fast LLM serving

**Recall:** *Make inference fast.*

### Distributed Training

**Parallelism:**
- **Data:** Split data
- **Model:** Split model
- **Pipeline:** Split layers

**Tools:** DeepSpeed, Megatron-LM, Horovod.

**Recall:** *Train across GPUs.*

### Experiment Tracking

**Tools:** MLflow, Weights & Biases, Neptune.

**Track:** Parameters, metrics, artifacts, code.

**Recall:** *Track experiments.*

---

## 18.7 NLP

### NLP: Text Processing

**Normalization:** Lowercase, punctuation, Unicode.
**Tokenization:** Split into tokens.
**Stemming:** Reduce to stem (running → run).
**Lemmatization:** Reduce to lemma (better → good).
**Stop words:** Remove common words.
**N-grams:** Sequences of n words.

**Recall:** *Clean + split text.*

### NLP: Tokenization

**Word:** Split on whitespace.
**Subword:** BPE, WordPiece, SentencePiece.

**Recall:** *Text → tokens.*

### NLP: Word Embeddings

**Word2Vec:** Skip-gram, CBOW.
**GloVe:** Global vectors.
**FastText:** Subword information.

**Properties:** Similar words close in vector space.

**Recall:** *Words as vectors.*

### NLP: Sequence Models

**RNN, LSTM, GRU, Transformer.**

**Recall:** *Model sequences.*

### NLP: Language Models

**N-gram:** Markov assumption.
**Neural:** RNN, Transformer.
**Pretrain + fine-tune:** BERT, GPT.

**Recall:** *Predict next token.*

### NLP: Machine Translation

**Seq2seq:** Encoder-decoder.
**Attention:** Align source/target.
**Transformer:** State-of-art.
**BLEU:** Evaluation metric.

**Recall:** *Translate languages.*

### NLP: Sentiment Analysis

**Task:** Classify opinion polarity.
**Approaches:** Lexicon, ML, deep learning.

**Recall:** *Positive/negative.*

### NLP: Named Entity Recognition

**Task:** Identify persons, places, orgs.
**Tagging:** BIO (Begin, Inside, Outside).

**Recall:** *Extract entities.*

### NLP: Question Answering

**Extractive:** Span from passage.
**Generative:** Generate answer.
**Retrieval-augmented:** Retrieve + generate.

**Recall:** *Answer questions.*

### NLP: Summarization

**Extractive:** Select sentences.
**Abstractive:** Generate summary.
**ROUGE:** Evaluation metric.

**Recall:** *Shorten text.*

### NLP: Speech Recognition

**ASR:** Automatic Speech Recognition.
**Components:** Acoustic model, language model.
**Models:** Whisper, Wav2Vec.

**Recall:** *Audio → text.*

### NLP: TTS

**TTS:** Text-to-Speech.
**Models:** Tacotron, VITS.

**Recall:** *Text → audio.*

---

## 18.8 Computer Vision

### CV: Image Processing

**Filters:** Convolution, blur, sharpen.
**Edges:** Sobel, Canny.
**Histograms:** Distribution of intensities.

**Recall:** *Manipulate pixels.*

### CV: Feature Detection

**SIFT:** Scale-invariant.
**SURF:** Speeded-up.
**ORB:** Oriented FAST + Rotated BRIEF.
**Harris:** Corner detection.

**Recall:** *Find keypoints.*

### CV: Convolutional Networks

**Architectures:** LeNet → AlexNet → VGG → ResNet → EfficientNet.

**Recall:** *Learn visual features.*

### CV: Object Detection

**Two-stage:** R-CNN, Fast R-CNN, Faster R-CNN.
**One-stage:** YOLO, SSD, RetinaNet.
**Transformer:** DETR.

**Output:** Bounding boxes + classes.

**Recall:** *Where + what.*

### CV: Image Segmentation

**Semantic:** Class per pixel.
**Instance:** Object instances.
**Panoptic:** Both.

**Models:** U-Net, Mask R-CNN, SAM.

**Recall:** *Pixel-level labels.*

### CV: Face Recognition

**Embeddings:** FaceNet, ArcFace.
**Similarity:** Cosine distance.

**Recall:** *Identify faces.*

### CV: Pose Estimation

**Keypoints:** Joints on body.
**Models:** OpenPose, MediaPipe.

**Recall:** *Body joint locations.*

### CV: 3D Vision

**Depth:** Distance from camera.
**Point clouds:** Set of 3D points.
**Meshes:** Vertices + faces.

**Recall:** *3D from images.*

### CV: Stereo Vision

**Two cameras:** Triangulate depth.
**Disparity:** Difference in position.

**Recall:** *Triangulate depth.*

### CV: SLAM

**SLAM:** Simultaneous Localization and Mapping.
**Use:** Robotics, AR.

**Recall:** *Map + locate at once.*

### CV: Neural Rendering

**NeRF:** Neural Radiance Fields.
**Gaussian Splatting:** 3D Gaussians.

**Recall:** *Render via neural nets.*

### CV: NeRF

**Definition:** MLP represents 3D scene.
**View synthesis:** Render from novel views.

**Recall:** *MLP represents 3D scene.*

### CV: Gaussian Splatting

**Definition:** 3D Gaussians; fast rendering.
**Alternative to NeRF.**

**Recall:** *Fast NeRF alternative.*

---

## 18.9 Generative AI

### Generative AI: Music Generation

**Models:** MuseNet, MusicLM, AudioCraft.

**Recall:** *Generate music.*

### Generative AI: Art Generation

**Models:** DALL·E, Stable Diffusion, Midjourney.

**Recall:** *Generate art.*

### Generative AI: Design

**Generative design:** Explore designs.
**CAD:** Computer-aided design.

**Recall:** *Generate designs.*

### Generative AI: Code Generation

**Models:** Copilot, Codex, AlphaCode.

**Recall:** *Generate code.*

### Generative AI: Human-AI Collaboration

**Co-creation:** Human + AI.
**Prompt engineering:** Craft inputs.

**Recall:** *Collaborate with AI.*

---

## 18.10 Information Retrieval

### Information Retrieval

**Inverted index:** Term → documents.
**TF-IDF:** Term frequency × inverse document frequency.
**BM25:** Probabilistic ranking.
**PageRank:** Link analysis.

**Recall:** *Find relevant documents.*

### Recommender Systems

**Collaborative filtering:** User-item interactions.
**Content-based:** Item features.
**Matrix factorization:** Latent factors.
**Metrics:** Precision@k, recall@k, NDCG.

**Recall:** *Recommend items.*

**SOTA:** Mixture-of-Experts, state-space models (Mamba), reasoning models, agentic AI (AutoGPT, BabyAGI, LangChain, CrewAI), test-time compute, vLLM, speculative decoding, multimodal generation, world models, Agent-Native models (NeoHorse-1 trained on real agent execution traces), AI self-improvement systems breaking scaling law discovery records, Gemini 3.5 Flash for agentic coding.

---
