# Tokenization: A Comprehensive Guide

## 1. What is Tokenization?
Tokenization is the process of converting raw text into smaller units called **tokens** that a Large Language Model (LLM) can understand.

- **Humans** read words and sentences.
- **LLMs** read tokens.

> **Example:**
> - **Text:** `Hello world!`
> - **Possible tokens:** `["Hello", " world", "!"]`
> 
> *Note: The model never sees the original text directly. It only sees token IDs.*

---

## 2. The Tokenization Pipeline
Computers cannot directly understand human language. Before training or inference, text must go through a strict pipeline:

```text
Raw Text 
   ↓
Tokenizer 
   ↓
Tokens 
   ↓
Token IDs 
   ↓
Model
```

> **Example:**
> `Hello world!` → `[9906, 1917, 0]`  
> *(The exact IDs depend entirely on the specific tokenizer used.)*

---

## 3. Token vs. Word
A token is **not always** a whole word. Depending on the algorithm, a token can be:
- A whole word
- Part of a word (subword)
- Punctuation
- Whitespace
- A single character

> **Examples:**
> - `unbelievable` → `["un", "believ", "able"]`
> - `ChatGPT is amazing` → `["Chat", "G", "PT", " is", " amazing"]`

---

## 4. Why Not Use Words Directly?
Using whole words as tokens creates significant challenges for LLMs:

### ⚠️ Huge Vocabulary
- English contains hundreds of thousands of words.
- New words appear every day (e.g., `ChatGPT`, `OpenAI`, `OpenCode`).
- These may not exist in a predefined, static dictionary.

### ⚠️ Out-of-Vocabulary (OOV) Problem
- If the model encounters a word it has never seen (e.g., `SuperMegaUltraWidget`), it fails to process it.
- **Solution:** Subword tokenization breaks it down into known parts (e.g., `["Super", "Mega", "Ultra", "Widget"]`), allowing the model to still infer meaning.

---

## 5. Types of Tokenization

| Type | Example (`hello world`) | Advantages | Disadvantages |
| :--- | :--- | :--- | :--- |
| **Character-Level** | `["h", "e", "l", "l", "o", " ", "w", ...]` | Simple; handles *any* text. | Sequences become excessively long. |
| **Word-Level** | `["hello", "world"]` | Intuitive; matches human reading. | Huge vocabulary; severe OOV problem. |
| **Subword-Level** | `["un", "believ", "able"]` | Manageable vocabulary; handles unseen words; highly efficient. | Slightly less intuitive than word-level. |

*Modern LLMs predominantly use **Subword-Level** tokenization.*

---

## 6. Popular Tokenization Algorithms

### 🔹 BPE (Byte Pair Encoding)
- **Used by:** GPT-2, GPT-3, GPT-4 family (variants).
- **Basic Idea:** 
  1. Start with individual characters (`h`, `e`, `l`, `l`, `o`).
  2. Find frequently occurring pairs (`he`, `el`, `ll`, `lo`).
  3. Merge common pairs repeatedly until `hello` becomes a single token (if it appears frequently enough).

### 🔹 WordPiece
- **Used by:** BERT.
- **Example:** `playing` → `["play", "##ing"]`
- **Note:** The `##` prefix indicates that this token is a continuation of the previous token.

### 🔹 SentencePiece
- **Used by:** T5, LLaMA, and many modern multilingual models.
- **Key Feature:** Treats text as a sequence of bytes or Unicode symbols. It does *not* require whitespace splitting beforehand, making it exceptionally robust for languages without spaces (e.g., Chinese, Japanese).

---

## 7. Tokenization in LLM Training
The end-to-end training pipeline looks like this:

```text
Raw Text 
    ↓ (Tokenization)
Token IDs  (e.g., [9906, 1917])
    ↓ (Lookup)
Embedding Vectors
    ↓ (Processing)
Transformer Layers
```

---

## 8. Context Window & Token Count
LLMs have strict limits based on **tokens**, not words.

- An **8K context** means **8,192 tokens**, *not* 8,192 words.
- **General Approximation:** 
  - `1 token` ≈ `0.75 English words`
  - `100 tokens` ≈ `75 words`
  - *(Note: This ratio varies significantly by language and tokenizer).*

### Special Case: Chinese Tokenization
Chinese has no spaces between words, making tokenization highly dependent on the algorithm.
- **Text:** `今天天气很好`
- **Possible Tokenization A:** `["今天", "天气", "很好"]`
- **Possible Tokenization B:** `["今", "天天", "气很", "好"]`

*This is a primary reason why token counts differ so significantly between models for the same non-English text.*

---

## 9. Why Token Count Matters
Token count directly dictates the resource requirements of an LLM. 

**More tokens =**
- ➕ More Computation
- ➕ More Memory Consumption
- ➕ Higher API/Inference Cost
- ➕ Higher Latency

It directly impacts:
1. Inference cost
2. API pricing
3. Response latency
4. Available context length

---

## 10. Tokenization and Prompt Engineering
Two prompts with the exact same meaning can produce vastly different token counts.

> **Prompt A:** `Please summarize this article.` 
> **Prompt B:** `Summarize this article.`

*Prompt B typically uses fewer tokens. Efficient, concise prompting is a proven way to reduce both cost and latency.*

---

## 11. Key Takeaways
- ✅ Tokenization converts raw text into numerical tokens.
- ✅ LLMs understand token IDs, not human words.
- ✅ Modern LLMs rely on **subword tokenization** (BPE, WordPiece, SentencePiece).
- ✅ Token count directly determines context size, inference speed, and API cost.
- ✅ Transformer models operate entirely on token IDs and their corresponding embedding vectors.
