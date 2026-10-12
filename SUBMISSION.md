# HW1 submission

**Name:** Kyzyrgeldi Kyran
**Student ID:** S23068969
**Group:** CSS4007-ENG-8
**Repository:** https://github.com/kKyran/hw1-kyran

## AI tool disclosure

> I used ChatGPT/Codex to understand the assignment, debug Python errors, and interpret API/tokenizer outputs. I ran the code myself using my own repository and API keys.

---

## Sublab Easy — the registration bot and its bill

**How I laid the catalogue out inside the system prompt, and why:**

> I laid the catalogue out as formatted JSON inside the system prompt, because JSON keeps courses, prerequisites, schedules, seats, completed courses, and rules structured.

**My turn 5 (Kazakh or Russian):**

> Мен үшінші курс студентімін. Қай курстарға әлі тіркеле аламын?

### Run 1 — OpenAI, `gpt-5.6-luna`

| Turn | Input tokens | Output tokens | Cost $ |
|---|---|---|---|
| 1 | 1156 | 474 | 0.000800 |
| 2 | 1395 | 184 | 0.000500 |
| 3 | 1526 | 137 | 0.000470 |
| 4 | 1617 | 175 | 0.000533 |
| 5 | 1732 | 393 | 0.000818 |
| **total** | | | 0.003121 |

### Run 2 — OpenRouter, `google/gemma-4-26b-a4b-it:free`

| Turn | Input tokens | Output tokens | Cost $ |
|---|---|---|---|
| 1 | failed | failed | 0.000000 |
| 2 | failed | failed | 0.000000 |
| 3 | failed | failed | 0.000000 |
| 4 | failed | failed | 0.000000 |
| 5 | failed | failed | 0.000000 |
| **total** | | | 0.000000 |

### Turn 4, verbatim

The turn where you asked for CSS-4090, which does not exist. Paste both replies
exactly as they came back — do not tidy them.

**OpenAI:**

```
I can’t add **CSS-4090 — Quantum Machine Learning** because it is not listed in the available **2026-FALL** course catalogue. Its credits, prerequisites, seats, and schedule cannot be verified.

Also, **CSS-4007** and **CSS-4102** already conflict on Tuesday from **09:00–10:50**, so they cannot both be part of the same registration.
```

**OpenRouter:**

```
OpenRouter did not return a turn-4 reply because google/gemma-4-26b-a4b-it:free was temporarily rate-limited upstream with HTTP 429.
```

### Written answers

**1. The two providers used almost identical code. What actually changed, and
what did not?**

> Only the client base URL and API key/provider changed: OpenAI used the default OpenAI client, while OpenRouter used the same OpenAI client class with base_url="https://openrouter.ai/api/v1". The message format, chat call, parsing of choices, and token usage reading stayed the same.

**2. Why did the input token count climb on every turn when your questions
stayed roughly the same length? Use the numbers from your own table. What
happens to the bill at fifty turns?**

> The input tokens climbed because every turn resends the whole growing history. My OpenAI input tokens rose from 1156 to 1395, 1526, 1617, and 1732. At fifty turns the bill would keep growing because each new request includes many previous user and assistant messages, so later turns are much more expensive than early turns.

**3. Turn 4: did the bot refuse, or did it invent CSS-4090?** If it refused, what
in your system prompt held the line? If it invented, what did it make up —
credits, a room, an instructor?

> The OpenAI bot refused CSS-4090. It said the course was not in the 2026-FALL catalogue and that its credits, prerequisites, seats, and schedule could not be verified. The catalogue JSON and the instruction to refuse courses not in the catalogue held the line.

**4. Where else was either bot wrong?** Turn 2 asks for two courses that meet at
the same hour; two courses in the catalogue are full. Did the bots notice?

> The OpenAI bot noticed the Tuesday 09:00-10:50 collision between CSS-4007 and CSS-4102 and refused to register both. It also noticed the full/already-completed courses. The OpenRouter Gemma run could not be evaluated because it was rate-limited.

---

## Sublab Medium — one task, six models

Paste the per-model summary printed by `correct_kazakh.py`:

| Model | Exact | Failed | Tokens | Cost $ |
|---|---|---|---|---|
| google/gemma-4-26b-a4b-it:free | 0 | 8 | 0 | 0.00000 |
| qwen/qwen3.8-27b | 5 | 2 | 11710 | 0.03546 |
| deepseek/deepseek-v4-flash-0731 | 6 | 0 | 11896 | 0.00319 |
| gpt-5.6-luna | 5 | 0 | 2388 | 0.00232 |
| gpt-5.6-terra | 4 | 0 | 1486 | 0.01238 |
| gpt-5.6-sol | 4 | 0 | 1560 | 0.03318 |

### Which error types did each model repair?

Rows are error labels, columns are models. Write "yes", "no" or "partial".

| Error type | gemma | qwen | deepseek | luna | terra | sol |
|---|---|---|---|---|---|---|
| kaz_to_rus | failed | partial | yes | yes | yes | yes |
| latin_homoglyph | failed | partial | yes | partial | partial | partial |
| drop_hyphen | failed | yes | yes | yes | yes | yes |
| join_words | failed | partial | yes | partial | partial | partial |
| double_letter | failed | partial | yes | yes | partial | partial |

**The `latin_homoglyph` row: what happened?** Describe what you observed. The
explanation is Sublab Harder's job, not this one's.

> Gemma could not be tested because OpenRouter returned upstream 429 errors. Qwen sometimes repaired the latin homoglyph rows, but also sometimes returned blank/no JSON. DeepSeek handled them more consistently in my successful run.

**Where a model returned good Kazakh that was not identical to the original,
say so here.** Exact match is not correctness.

> Some outputs were good Kazakh but not exact matches, for example models sometimes changed plural/case forms or wording while preserving a grammatical sentence. I treated exact match as a signal, not the whole correctness judgment.

**Cheapest model that was good enough, and why:**

> The cheapest good-enough model was gpt-5.6-luna in my run: it had no failed rows, 5 exact matches, and cost only $0.00232. DeepSeek was also cheap and strong, but Luna was cheaper in this table and more reliable than the OpenRouter models.

---

## Sublab Harder — open the tokenizer

### A. What a language costs

**`cl100k_base`:**

| Language | Tokens | Chars | Tok/char | × English | $ per 1,000 sentences |
|---|---|---|---|---|---|
| kk | 200 | 263 | 0.760 | 3.75 | 0.167 |
| ru | 129 | 277 | 0.466 | 2.30 | 0.108 |
| en | 59 | 291 | 0.203 | 1.00 | 0.049 |

**`o200k_base`:**

| Language | Tokens | Chars | Tok/char | × English | $ per 1,000 sentences |
|---|---|---|---|---|---|
| kk | 84 | 263 | 0.319 | 1.58 | 0.070 |
| ru | 74 | 277 | 0.267 | 1.32 | 0.062 |
| en | 59 | 291 | 0.203 | 1.00 | 0.049 |

### B. What a homoglyph does

One row per `latin_homoglyph` sentence in the dataset. Paste the actual decoded
token strings around the divergence point, not a description of them.

| Sentence id | Foreign char (index, name) | Tokens correct | Tokens corrupted | Δ | Diverges at |
|---|---|---|---|---|---|
| KZ-03 | 0 LATIN CAPITAL LETTER A; 2 LATIN SMALL LETTER A; 5 LATIN SMALL LETTER T | 16 | 20 | +4 | 0 |
| KZ-08 | 1 LATIN SMALL LETTER O; 3 LATIN SMALL LETTER A; 9 LATIN CAPITAL LETTER T | 21 | 24 | +3 | 1 |

**Token pieces around the divergence:**

```
KZ-03
correct  : ['А', 'лая', 'қ', 'тарға', ' ақша']
corrupted: ['A', 'л', 'a', 'я', 'қ']

KZ-08
correct  : ['Д', 'он', 'аль', 'д', ' Т', 'рамп']
corrupted: ['Д', 'o', 'н', 'a', 'л', 'ль']
```

### C. Did it get better?

| Language | cl100k_base | o200k_base | Change |
|---|---|---|---|
| kk | 0.760 | 0.319 | -0.441 |
| ru | 0.466 | 0.267 | -0.199 |
| en | 0.203 | 0.203 | 0.000 |

### Written answers

**1. What is the Kazakh tax?** The ratio against English in both encodings, the
dollar figure from A, and how much it changed between the two tokenizers.

> In cl100k_base, Kazakh cost 3.75x English: about $0.167 per 1,000 average sentences versus English at about $0.049. In o200k_base, Kazakh cost 1.58x English: about $0.070 versus $0.049. The newer tokenizer reduced the Kazakh ratio by 2.17x English and the dollar estimate by about $0.097 per 1,000 sentences.

**2. Why did the models repair `kaz_to_rus` but struggle with
`latin_homoglyph`?** Both are single-letter substitutions and both look almost
identical on screen. Use your token streams from B as the evidence. Say what the
model actually received in each case.

> kaz_to_rus substitutions are Cyrillic-to-Cyrillic, so the model still receives Cyrillic-looking tokens in the same script. Latin homoglyphs look similar to us, but the tokenizer receives different code points. In KZ-03 the correct stream starts ['А', 'лая', 'қ', ...], while the corrupted stream starts ['A', 'л', 'a', ...]. In KZ-08, correct has ['Д', 'он', ...], corrupted has ['Д', 'o', 'н', 'a', ...]. The model is not just seeing a normal Kazakh typo; it receives mixed Latin/Cyrillic fragments.

**3. Name one thing this measurement does not explain about your Sublab Medium
results.** You measured OpenAI's tokenizers; three of your six models were not
OpenAI's. What follows, and what would you have to do to close the gap?

> This measurement does not fully explain the OpenRouter models because I measured OpenAI tokenizers, not Gemma, Qwen, or DeepSeek tokenizers. To close the gap, I would need to inspect each model family’s own tokenizer and compare the same corrupted/correct strings under those tokenizers.
