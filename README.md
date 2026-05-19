# 🤗 Natural Language Processing with Hugging Face Transformers

> **Generative AI Guided Project on Cognitive Class by IBM**

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![HuggingFace](https://img.shields.io/badge/HuggingFace-Transformers-yellow?logo=huggingface&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-Framework-red?logo=pytorch&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

---

## 👤 Identitas

| Field | Detail |
|---|---|
| **Nama** | Zulkifli |
| **Program** | Studi Independen — Artificial Intelligence |
| **Institusi** | Infinite Learning |

---

## 📋 Daftar Tugas

1. [Analisis Sentimen](#1-analisis-sentimen)
2. [Klasifikasi Topik (Zero-Shot)](#2-klasifikasi-topik-zero-shot)
3. [Generator Teks](#3-generator-teks)
4. [Pengenalan Entitas Bernama (NER)](#4-pengenalan-entitas-bernama-ner)
5. [Menjawab Pertanyaan (QA)](#5-menjawab-pertanyaan-qa)
6. [Ringkasan Teks](#6-ringkasan-teks-summarization)
7. [Terjemahan](#7-terjemahan-translation)

---

## 1. Analisis Sentimen

**Model:** `cardiffnlp/twitter-roberta-base-sentiment`

**Kode:**
```python
sentiment_model = pipeline(
    "sentiment-analysis",
    model="cardiffnlp/twitter-roberta-base-sentiment"
)
sentiment_model("I am so relieved and happy that the pipeline error is finally fixed! 🚀")
```

**Hasil:**
```json
[{"label": "LABEL_2", "score": 0.9805663824081421}]
```

**Analisis:**

Model klasifikasi sentimen berbasis Twitter ini secara akurat mendeteksi nada positif (`LABEL_2`) dari kalimat yang mengekspresikan kelegaan dan kebahagiaan. Confidence score **98%** menunjukkan keandalan model dalam menangkap emosi manusia dalam teks tertulis, bahkan dengan adanya emoji.

---

## 2. Klasifikasi Topik (Zero-Shot)

**Model:** `facebook/bart-large-mnli`

**Kode:**
```python
topic_classifier = pipeline("zero-shot-classification", model="facebook/bart-large-mnli")

kalimat = "Implementing artificial intelligence to optimize warehouse inventory and material handling."
pilihan_topik = ["logistics and supply chain", "game development", "healthcare"]

topic_classifier(kalimat, candidate_labels=pilihan_topik)
```

**Hasil:**
```json
{
  "sequence": "Implementing artificial intelligence to optimize warehouse inventory and material handling.",
  "labels": ["logistics and supply chain", "game development", "healthcare"],
  "scores": [0.9854783415794373, 0.008162161335349083, 0.006359483115375042]
}
```

**Analisis:**

Klasifikasi zero-shot terbukti sangat efektif. Model langsung mengidentifikasi `logistics and supply chain` sebagai label paling relevan dengan skor **98.5%** — tanpa training khusus. Model mampu mengaitkan kata kunci seperti *"warehouse"* dan *"material handling"* ke kategori logistik yang tepat.

---

## 3. Generator Teks

**Model:** `gpt2`

**Kode:**
```python
generator = pipeline("text-generation", model="gpt2")
generator(
    "As an Information Systems student at ITEBA, my main focus is",
    max_length=30,
    num_return_sequences=2
)
```

**Hasil:**
```json
[
  {"generated_text": "As an Information Systems student at ITEBA, my main focus is on software and hardware architecture. My main focuses are on development and implementation of high"},
  {"generated_text": "As an Information Systems student at ITEBA, my main focus is the ability to learn things in front of a monitor. In fact, having my"}
]
```

**Analisis:**

GPT-2 menghasilkan dua kelanjutan teks yang koheren dari prompt yang sama. Output pertama berfokus pada sisi teknis (arsitektur hardware/software), sedangkan output kedua lebih bernada personal. Ini menunjukkan fleksibilitas kreativitas bahasa dari model generatif dalam melengkapi struktur kalimat.

---

## 4. Pengenalan Entitas Bernama (NER)

**Model:** `Jean-Baptiste/camembert-ner`

**Kode:**
```python
ner_model = pipeline(
    "ner",
    model="Jean-Baptiste/camembert-ner",
    aggregation_strategy="simple"
)
ner_model("I am learning Machine Learning with my mentor Arifian at Infinite Learning in Batam.")
```

**Hasil:**
```json
[
  {"entity_group": "MISC", "score": 0.987819,   "word": "Machine Learning",  "start": 13, "end": 30},
  {"entity_group": "ORG",  "score": 0.6696281,  "word": "Arifian",           "start": 45, "end": 53},
  {"entity_group": "ORG",  "score": 0.9817379,  "word": "Infinite Learning", "start": 56, "end": 74},
  {"entity_group": "LOC",  "score": 0.99267715, "word": "Batam",             "start": 77, "end": 83}
]
```

**Analisis:**

Pipeline NER berhasil mengekstrak entitas penting berkat parameter `aggregation_strategy="simple"`. Model mengenali `Machine Learning` (MISC), `Infinite Learning` (ORG), dan `Batam` (LOC) dengan akurasi tinggi. Meski nama mentor `Arifian` salah dikategorikan sebagai ORG, kemampuan ekstraksi informasi secara keseluruhan tetap sangat berguna.

---

## 5. Menjawab Pertanyaan (QA)

**Model:** `distilbert-base-cased-distilled-squad`

**Kode:**
```python
qa_model = pipeline("question-answering", model="distilbert-base-cased-distilled-squad")

konteks = "My name is Zulkifli and I am developing an interactive educational game called Riddle.Cyz."
pertanyaan = "What is the name of the interactive game?"

qa_model(question=pertanyaan, context=konteks)
```

**Hasil:**
```json
{"score": 0.9674302339553833, "start": 79, "end": 89, "answer": "Riddle.Cyz"}
```

**Analisis:**

Model QA berhasil mengekstrak jawaban pasti **"Riddle.Cyz"** dari konteks dengan confidence **96.7%**. Kemampuan *machine reading comprehension* ini merupakan fondasi kuat untuk membangun chatbot otomatis atau asisten pintar berbasis dokumen.

---

## 6. Ringkasan Teks (Summarization)

**Model:** `sshleifer/distilbart-cnn-12-6`

**Kode:**
```python
summarizer = pipeline("summarization", model="sshleifer/distilbart-cnn-12-6")

dokumen = """Natural Language Processing (NLP) is a subfield of linguistics, computer science,
and artificial intelligence concerned with the interactions between computers and human language,
in particular how to program computers to process and analyze large amounts of natural language data.
The goal is a computer capable of understanding the contents of documents, including the contextual
nuances of the language within them."""

summarizer(dokumen, max_length=50, min_length=15, do_sample=False)
```

**Hasil:**
```json
[{
  "summary_text": " NLP is a subfield of linguistics, computer science, and artificial intelligence concerned with the interactions between computers and human language. The goal is a computer capable of understanding the contents of documents, including the contextual nuances of the language within them."
}]
```

**Analisis:**

Model perangkum berhasil memadatkan paragraf konseptual menjadi intisari yang padat dan informatif. Konsep kunci definisi NLP dan tujuannya tetap terjaga tanpa kehilangan konteks yang signifikan. Ini membuktikan kekuatan model dalam kompresi data tekstual.

---

## 7. Terjemahan (Translation)

**Model:** `Helsinki-NLP/opus-mt-en-de` *(default `translation_en_to_de`)*

**Kode:**
```python
translator = pipeline("translation_en_to_de")
translator("I have successfully completed my guided project on Hugging Face Transformers.")
```

**Hasil:**
```json
[{"translation_text": "Ich habe mein geführtes Projekt über Hugging Face Transformers erfolgreich abgeschlossen."}]
```

**Analisis:**

Model menerjemahkan kalimat Bahasa Inggris ke Bahasa Jerman dengan struktur sintaksis yang benar dan akurat. Ini membuktikan betapa mudahnya mengimplementasikan kemampuan alih bahasa hanya dengan satu model pra-latih tanpa arsitektur tambahan.

---

## 📝 Kesimpulan

Proyek terbimbing ini memberikan wawasan teknis yang berharga mengenai kemudahan ekosistem **Hugging Face Transformers**. Melalui fungsi abstrak `pipeline()`, mengimplementasikan model Machine Learning yang kompleks untuk berbagai tugas NLP menjadi jauh lebih mudah dan efisien.

Pengalaman langsung ini mencakup ekstraksi entitas, meringkas dokumen, hingga menjawab pertanyaan secara otomatis — sekaligus melatih *problem-solving* saat menghadapi kendala teknis seperti *library versioning*. Pengalaman ini menjadi fondasi yang kokoh untuk mengembangkan aplikasi berbasis AI di masa depan.

---

## 🛠️ Tasks yang Dikerjakan

| No | Task | Model | Confidence |
|:--:|------|-------|:----------:|
| 1 | Sentiment Analysis | `cardiffnlp/twitter-roberta-base-sentiment` | 98.1% |
| 2 | Zero-Shot Classification | `facebook/bart-large-mnli` | 98.5% |
| 3 | Text Generation | `gpt2` | — |
| 4 | Named Entity Recognition | `Jean-Baptiste/camembert-ner` | ~99% |
| 5 | Question Answering | `distilbert-base-cased-distilled-squad` | 96.7% |
| 6 | Summarization | `sshleifer/distilbart-cnn-12-6` | — |
| 7 | Translation (EN→DE) | `Helsinki-NLP/opus-mt-en-de` | — |
