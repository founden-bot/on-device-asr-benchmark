# On-device speech recognition benchmark: Parakeet vs Whisper vs Apple, 25 languages

Word error rates of three speech recognition engines that run **entirely on a Mac**, measured on the same 100 FLEURS recordings per language in **25 European languages**, with **every transcript published** so anyone can recompute every number.

| Engine | Maker | Median word error rate, all 25 languages | Median, the 6 languages all three support | Lowest error rate in | Median speed |
|---|---|---|---|---|---|
| Parakeet TDT 0.6B v3 | NVIDIA | 10.1% | 4.1% | 15 of 25 languages | 20× real time |
| Whisper large-v3-turbo | OpenAI | 11.3% | 3.6% | 10 of 25 languages | 13× real time |
| Apple SpeechTranscriber | Apple | · (supports 6) | 6.2% | 0 of 25 languages | 64× real time |

The six languages all three engines support are English, German, French, Spanish, Italian and Portuguese. Compare languages row by row below. Measured on an Apple M2 Pro with 16 GB of memory, macOS 26.6.2, 2026-10-19 (last updated 2026-10-21).

Interactive version with charts and every fact traced: **[yapapp.cc/data/on-device-asr-benchmark](https://yapapp.cc/data/on-device-asr-benchmark)**. Parakeet v3 against Whisper, language by language: **[yapapp.cc/parakeet-vs-whisper](https://yapapp.cc/parakeet-vs-whisper)**.

## Word error rate by language

Lower is better; the lowest rate in each row is in bold. `·` means the engine does not support the language.

| Language | Parakeet TDT v3 | Whisper large-v3-turbo | Apple SpeechTranscriber |
|---|---|---|---|
| English | 4.5% | **3.9%** | 6.7% |
| Bulgarian | **13.4%** | 15.9% | · |
| Croatian | 12.4% | **11.3%** | · |
| Czech | **10.4%** | 11.5% | · |
| Danish | 15.8% | **14.6%** | · |
| Dutch | **6.3%** | 6.6% | · |
| Estonian | **14.2%** | 19.9% | · |
| Finnish | 10.1% | **7.7%** | · |
| French | **5.3%** | 6.6% | 6.6% |
| German | **4.8%** | 5.1% | 6.9% |
| Greek | 31.7% | **12.0%** | · |
| Hungarian | **14.1%** | 17.2% | · |
| Italian | **2.5%** | 2.7% | 4.2% |
| Latvian | **17.3%** | 18.6% | · |
| Lithuanian | **17.8%** | 25.0% | · |
| Maltese | **22.0%** | 72.3% | · |
| Polish | 8.2% | **7.7%** | · |
| Portuguese | 3.6% | **3.3%** | 5.9% |
| Romanian | **10.0%** | 11.8% | · |
| Russian | 4.7% | **4.6%** | · |
| Slovak | **8.4%** | 11.5% | · |
| Slovenian | **19.8%** | 23.5% | · |
| Spanish | 2.8% | **2.5%** | 4.3% |
| Swedish | 12.0% | **9.9%** | · |
| Ukrainian | **5.7%** | 6.8% | · |

## Formatted error rate, the six languages all three engines support

Scores the text as a reader sees it: capital letters count, and each sentence mark (. , ! ? ; :) is a token of its own.

| Language | Parakeet TDT v3 | Whisper large-v3-turbo | Apple SpeechTranscriber |
|---|---|---|---|
| English | 8.2% | 7.7% | 11.1% |
| French | 8.0% | 9.3% | 10.3% |
| German | 7.1% | 7.1% | 10.6% |
| Italian | 6.4% | 7.7% | 10.7% |
| Portuguese | 7.7% | 7.5% | 12.3% |
| Spanish | 5.4% | 5.7% | 10.4% |

## The engines

- **Parakeet TDT 0.6B v3** (NVIDIA): NVIDIA Parakeet TDT 0.6B v3 converted to Core ML (Parakeet Ultra, FluidInference, model revision 95eaa59a39d4), run through FluidAudio 0.17.4 on the CPU and GPU, with the language given. This is the engine the Yap app uses on Mac.
- **Whisper large-v3-turbo** (OpenAI): OpenAI Whisper large-v3-turbo, 5-bit quantized (ggml-large-v3-turbo-q5_0.bin), run through whisper.cpp 1.9.4 with Metal on the GPU, with the language given.
- **Apple SpeechTranscriber** (Apple): Apple's on-device speech model in macOS 26 (SpeechAnalyzer with a SpeechTranscriber module, transcription preset), with the locale given (en-US, de-DE, fr-FR, es-MX, it-IT, pt-BR).

## Method

- **Recordings:** 100 rows per language at evenly spaced positions of the published test split (row i * rows // 100), fixed before any engine ran. Dataset `google/fleurs`, split `test`, revision `70bb2e84b976b7e960aa89f1c648e09c59f894dd` (CC BY 4.0). Each recording's SHA-256 is in the transcripts.
- **Word error rate:** Both texts: Unicode NFKC, lowercase, words = maximal runs of letters and digits; punctuation and symbols dropped. Word errors = edit distance between the word sequences.
- **Formatted error rate:** The formatted error rate scores the text as a reader sees it: words keep their capital letters, and each sentence mark (. , ! ? ; :) is a token of its own. Formatted errors = edit distance between the token sequences of FLEURS's written reference sentence and the engine's text, over the reference tokens.
- **Speed:** Time from handing the audio to the engine to receiving the text, on a warm engine (one uncounted warm-up recording per language). Recordings run one at a time. "Times real time" is audio seconds divided by processing seconds.

## Limits

- Read Wikipedia sentences in quiet rooms are easier than dictation, conversation or noise.
- Numbers and spelling variants are not normalized: a correct "25" against a reference "twenty-five" counts as an error for every engine alike.
- One Mac, one run, 100 recordings per language: small differences between engines are within noise.
- **We make [Yap](https://yapapp.cc), a dictation app that uses Parakeet on the Mac.** That is why every transcript, timing and recording hash is here: check us.

## Files

- [`results/summary.csv`](results/summary.csv): one row per engine and language (recordings, words, errors, rates, audio and processing time).
- [`results/transcripts/`](results/transcripts): one JSON Lines file per language. Each line is one recording: FLEURS ids, SHA-256, length, the reference sentence, and each engine's text, time and errors.

## More tests from the same lab

Accents (EdAcc), background noise, Bluetooth headset audio, Parkinson's speech and long dictation, each with its data: [yapapp.cc/data](https://yapapp.cc/data).

## Elsewhere

- Write-up with the results table: [DEV Community](https://dev.to/yapapp/parakeet-v3-vs-whisper-vs-apples-on-device-model-in-25-languages-every-transcript-published-3bjo)
- The same files as a Kaggle dataset: [kaggle.com/datasets/yapappcc/on-device-asr-parakeet-vs-whisper-vs-apple](https://www.kaggle.com/datasets/yapappcc/on-device-asr-parakeet-vs-whisper-vs-apple)

## License and citation

Results and transcripts: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). The reference sentences come from FLEURS (Google, 2022, CC BY 4.0).

> Yap (2026). On-device speech recognition benchmark: 25 languages [Data set]. https://yapapp.cc/data/on-device-asr-benchmark
