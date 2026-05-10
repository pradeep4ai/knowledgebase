---
id: tokenization
title: Tokenization
sources:
  - raw/ai/05_Karpathy_LLM_Wiki_Notes.pdf
related:
  - nanogpt-architecture
  - scaling-laws
tags: [llm, preprocessing, bpe]
manual: true
---

# Tokenization

Tokenizers split text into subword tokens before it enters the model. **GPT-4 uses ~100k BPE tokens; GPT-2 used 50,257.** The choice of tokenizer is upstream of every other decision in an LLM and is a major source of subtle bugs.

## Why tokenization matters

LLMs do not see characters or words — they see token IDs. Every quirk in how text is split shows up as a quirk in model behavior:

- **Numbers split oddly.** A four-digit year may become 1, 2, or 3 tokens depending on the value, which is why arithmetic on raw numbers is unreliable.
- **Leading spaces matter.** ` apple` and `apple` are different tokens. Prompts that omit the leading space can degrade output quality.
- **Non-English languages use more tokens per word.** That makes them effectively more expensive and reduces usable context.
- **Code indentation fragments poorly** with naive tokenizers, which is why GPT-4's tokenizer added dedicated whitespace runs.

Karpathy's framing: most "model bugs" turn out to be tokenizer bugs. Always look at the actual token stream before blaming the model.

## Byte-Pair Encoding (BPE)

GPT-2 and GPT-4 both use BPE. The algorithm:

1. Start with a vocabulary of single bytes (256 entries).
2. Find the most frequent adjacent pair of tokens in the corpus.
3. Merge that pair into a new token; add it to the vocabulary.
4. Repeat until the vocabulary reaches the target size (50,257 in GPT-2; ~100k in GPT-4).

Karpathy's `minbpe` implements the algorithm in a few hundred lines of Python.

## Practical advice

- When debugging unexpected model output, **check the tokenization first**.
- Always inspect the actual token stream, not the source string.
- For non-English content, budget context-window math against tokens, not characters.
- Vocabulary size is a hyperparameter with real cost: it sits in `nanoGPT`'s `vocab_size` and the embedding table grows linearly with it (see [nanoGPT architecture](nanogpt-architecture.md)).

## Sources

- [`raw/ai/05_Karpathy_LLM_Wiki_Notes.pdf`](../../raw/ai/05_Karpathy_LLM_Wiki_Notes.pdf) — section 2.1 ("Tokenization") and section 3 ("nanoGPT Architecture")
