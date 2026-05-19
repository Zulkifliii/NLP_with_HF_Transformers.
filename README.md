<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Natural Language Processing with Hugging Face Transformers</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700&family=Sora:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0d1117;
    --surface: #161b22;
    --surface2: #21262d;
    --border: #30363d;
    --text: #c9d1d9;
    --text-dim: #8b949e;
    --green: #3fb950;
    --blue: #58a6ff;
    --orange: #f0883e;
    --purple: #bc8cff;
    --red: #ff7b72;
    --yellow: #e3b341;
    --teal: #39d353;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Sora', sans-serif;
    font-size: 15px;
    line-height: 1.7;
    max-width: 900px;
    margin: 0 auto;
    padding: 40px 24px 80px;
  }

  /* HEADER */
  .header {
    text-align: center;
    padding: 48px 0 40px;
    border-bottom: 1px solid var(--border);
    margin-bottom: 40px;
  }

  .header h1 {
    font-family: 'Sora', sans-serif;
    font-size: 2rem;
    font-weight: 700;
    color: #e6edf3;
    line-height: 1.3;
    margin-bottom: 12px;
  }

  .header .subtitle {
    color: var(--text-dim);
    font-size: 0.9rem;
    margin-bottom: 20px;
  }

  .badges {
    display: flex;
    justify-content: center;
    gap: 10px;
    flex-wrap: wrap;
    margin-bottom: 24px;
  }

  .badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 5px 14px;
    border-radius: 6px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.78rem;
    font-weight: 700;
    letter-spacing: 0.05em;
  }

  .badge-python { background: #1f4068; color: #4fc3f7; border: 1px solid #1e88e5; }
  .badge-torch  { background: #3e1a1a; color: #ff7043; border: 1px solid #e64a19; }
  .badge-hf     { background: #2e2413; color: #ffd54f; border: 1px solid #ff8f00; }

  .author-info {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 16px 24px;
    display: inline-block;
    text-align: left;
    margin-top: 8px;
  }
  .author-info p { color: var(--text-dim); font-size: 0.88rem; }
  .author-info span { color: var(--blue); font-weight: 600; }

  /* TOC */
  .toc {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 24px 28px;
    margin-bottom: 40px;
  }
  .toc h2 {
    font-size: 1rem;
    font-weight: 700;
    color: var(--yellow);
    margin-bottom: 14px;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    font-size: 0.82rem;
  }
  .toc ol { padding-left: 20px; }
  .toc li { margin: 6px 0; }
  .toc a {
    color: var(--blue);
    text-decoration: none;
    font-size: 0.92rem;
    transition: color 0.15s;
  }
  .toc a:hover { color: #79c0ff; text-decoration: underline; }

  /* SECTION */
  .section {
    margin-bottom: 48px;
    scroll-margin-top: 24px;
  }

  .section-title {
    font-size: 1.25rem;
    font-weight: 700;
    color: #e6edf3;
    margin-bottom: 20px;
    padding-bottom: 10px;
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .section-number {
    background: var(--blue);
    color: #0d1117;
    font-size: 0.75rem;
    font-weight: 700;
    padding: 2px 8px;
    border-radius: 4px;
  }

  /* CODE BLOCK */
  .code-block {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    overflow: hidden;
    margin: 16px 0;
  }

  .code-header {
    background: var(--surface2);
    padding: 8px 16px;
    display: flex;
    align-items: center;
    gap: 8px;
    border-bottom: 1px solid var(--border);
  }

  .code-dot {
    width: 10px; height: 10px;
    border-radius: 50%;
  }
  .dot-red { background: #ff5f57; }
  .dot-yellow { background: #febc2e; }
  .dot-green { background: #28c840; }

  .code-label {
    margin-left: auto;
    font-size: 0.72rem;
    font-family: 'JetBrains Mono', monospace;
    color: var(--text-dim);
    text-transform: uppercase;
    letter-spacing: 0.08em;
  }

  pre {
    padding: 18px 20px;
    overflow-x: auto;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.82rem;
    line-height: 1.6;
    color: var(--text);
  }

  .kw { color: var(--purple); }
  .str { color: var(--orange); }
  .fn { color: var(--blue); }
  .cm { color: var(--text-dim); font-style: italic; }
  .num { color: var(--green); }
  .key { color: var(--red); }

  /* RESULT BLOCK */
  .result-label {
    font-size: 0.8rem;
    color: var(--text-dim);
    font-weight: 600;
    margin: 14px 0 6px;
    text-transform: uppercase;
    letter-spacing: 0.07em;
  }

  .result-block {
    background: #0c1521;
    border: 1px solid #1e4060;
    border-left: 3px solid var(--blue);
    border-radius: 8px;
    padding: 14px 18px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.8rem;
    color: #79c0ff;
    overflow-x: auto;
    line-height: 1.6;
  }

  /* ANALYSIS */
  .analysis {
    background: var(--surface);
    border: 1px solid var(--border);
    border-left: 3px solid var(--green);
    border-radius: 8px;
    padding: 16px 20px;
    margin-top: 16px;
    font-size: 0.9rem;
    color: var(--text);
    line-height: 1.8;
  }

  .analysis-title {
    font-size: 0.78rem;
    font-weight: 700;
    color: var(--green);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-bottom: 8px;
  }

  /* FINAL ANALYSIS */
  .final-analysis {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 28px 32px;
    margin-top: 48px;
  }

  .final-analysis h2 {
    font-size: 1.2rem;
    font-weight: 700;
    color: #e6edf3;
    margin-bottom: 16px;
    padding-bottom: 12px;
    border-bottom: 1px solid var(--border);
  }

  .final-analysis p {
    color: var(--text);
    font-size: 0.92rem;
    line-height: 1.85;
  }

  /* FOOTER */
  .footer {
    text-align: center;
    margin-top: 60px;
    padding-top: 24px;
    border-top: 1px solid var(--border);
    color: var(--text-dim);
    font-size: 0.82rem;
  }
</style>
</head>
<body>

<!-- HEADER -->
<div class="header">
  <h1>Pemrosesan Bahasa Alami dengan Hugging Face Transformers</h1>
  <p class="subtitle">Proyek Terbimbing AI Generatif pada Kelas Kognitif oleh IBM</p>
  <div class="badges">
    <span class="badge badge-python">🐍 Python</span>
    <span class="badge badge-torch">🔥 PyTorch</span>
    <span class="badge badge-hf">🤗 Hugging Face</span>
  </div>
  <div class="author-info">
    <p>Nama: <span>Zulkifli</span></p>
    <p>Program: <span>Studi Independen - Artificial Intelligence (Infinite Learning)</span></p>
  </div>
</div>

<!-- TOC -->
<div class="toc">
  <h2>📋 Daftar Tugas Saya</h2>
  <ol>
    <li><a href="#s1">Analisis Sentimen</a></li>
    <li><a href="#s2">Klasifikasi Topik</a></li>
    <li><a href="#s3">Generator Teks</a></li>
    <li><a href="#s4">Pengenalan Entitas Bernama (NER)</a></li>
    <li><a href="#s5">Menjawab Pertanyaan (Question Answering)</a></li>
    <li><a href="#s6">Ringkasan Teks (Summarization)</a></li>
    <li><a href="#s7">Terjemahan (Translation)</a></li>
  </ol>
</div>

<!-- SECTION 1 -->
<div class="section" id="s1">
  <div class="section-title">
    <span class="section-number">1</span>
    Contoh 1 - Analisis Sentimen
  </div>

  <div class="code-block">
    <div class="code-header">
      <span class="code-dot dot-red"></span>
      <span class="code-dot dot-yellow"></span>
      <span class="code-dot dot-green"></span>
      <span class="code-label">Python</span>
    </div>
    <pre><span class="cm"># TODO :</span>
<span class="fn">sentiment_model</span> = <span class="fn">pipeline</span>(<span class="str">"sentiment-analysis"</span>, model=<span class="str">"cardiffnlp/twitter-roberta-base-sentiment"</span>)
<span class="fn">sentiment_model</span>(<span class="str">"I am so relieved and happy that the pipeline error is finally fixed! 🚀"</span>)</pre>
  </div>

  <div class="result-label">Hasil :</div>
  <div class="result-block">[{'label': 'LABEL_2', 'score': 0.9805663824081421}]</div>

  <div class="analysis">
    <div class="analysis-title">Analisis pada contoh 1:</div>
    Model klasifikasi sentimen berbasis Twitter ini secara akurat mendeteksi nada positif (yang direpresentasikan dengan <code>LABEL_2</code>) dari kalimat yang mengekspresikan kelegaan dan kebahagiaan. Skor kepercayaan (<em>confidence score</em>) yang sangat tinggi yaitu <strong>98%</strong> menunjukkan bahwa model ini sangat andal dalam menangkap emosi manusia dalam teks tertulis, bahkan dengan adanya emoji.
  </div>
</div>

<!-- SECTION 2 -->
<div class="section" id="s2">
  <div class="section-title">
    <span class="section-number">2</span>
    Contoh 2 - Klasifikasi Topik
  </div>

  <div class="code-block">
    <div class="code-header">
      <span class="code-dot dot-red"></span>
      <span class="code-dot dot-yellow"></span>
      <span class="code-dot dot-green"></span>
      <span class="code-label">Python</span>
    </div>
    <pre><span class="cm"># TODO :</span>
<span class="fn">topic_classifier</span> = <span class="fn">pipeline</span>(<span class="str">"zero-shot-classification"</span>, model=<span class="str">"facebook/bart-large-mnli"</span>)
<span class="fn">kalimat</span> = <span class="str">"Implementing artificial intelligence to optimize warehouse inventory and material handling."</span>
<span class="fn">pilihan_topik</span> = [<span class="str">"logistics and supply chain"</span>, <span class="str">"game development"</span>, <span class="str">"healthcare"</span>]
<span class="fn">topic_classifier</span>(kalimat, candidate_labels=pilihan_topik)</pre>
  </div>

  <div class="result-label">Hasil :</div>
  <div class="result-block">{
  'sequence': 'Implementing artificial intelligence to optimize warehouse inventory and material handling.',
  'labels': ['logistics and supply chain', 'game development', 'healthcare'],
  'scores': [0.9854783415794373, 0.008162161335349083, 0.006359483115375042]
}</div>

  <div class="analysis">
    <div class="analysis-title">Analisis pada contoh 2:</div>
    Klasifikasi zero-shot terbukti sangat efektif. Model langsung mampu mengidentifikasi <strong>"logistics and supply chain"</strong> sebagai label yang paling relevan (skor <strong>98.5%</strong>). Model ini dapat mengaitkan konteks kata seperti <em>"warehouse"</em> dan <em>"material handling"</em> dengan kategori logistik tanpa perlu dilatih atau disesuaikan secara khusus untuk kumpulan data klasifikasi tersebut.
  </div>
</div>

<!-- SECTION 3 -->
<div class="section" id="s3">
  <div class="section-title">
    <span class="section-number">3</span>
    Contoh 3 - Generator Teks
  </div>

  <div class="code-block">
    <div class="code-header">
      <span class="code-dot dot-red"></span>
      <span class="code-dot dot-yellow"></span>
      <span class="code-dot dot-green"></span>
      <span class="code-label">Python</span>
    </div>
    <pre><span class="cm"># TODO :</span>
<span class="fn">generator</span> = <span class="fn">pipeline</span>(<span class="str">"text-generation"</span>, model=<span class="str">"gpt2"</span>)
<span class="fn">generator</span>(
  <span class="str">"As an Information Systems student at ITEBA, my main focus is"</span>,
  max_length=<span class="num">30</span>,
  num_return_sequences=<span class="num">2</span>
)</pre>
  </div>

  <div class="result-label">Hasil :</div>
  <div class="result-block">[
  {'generated_text': 'As an Information Systems student at ITEBA, my main focus is on software and hardware architecture. My main focuses are on development and implementation of high'},
  {'generated_text': 'As an Information Systems student at ITEBA, my main focus is the ability to learn things in front of a monitor. In fact, having my'}
]</div>

  <div class="analysis">
    <div class="analysis-title">Analisis pada contoh 3:</div>
    Model GPT-2 berhasil menghasilkan dua kelanjutan teks yang koheren dari prompt yang diberikan. Output pertama berfokus pada sisi teknis (arsitektur perangkat keras/lunak), sedangkan output kedua lebih bernada personal. Ini menunjukkan <strong>fleksibilitas kreativitas bahasa</strong> dari model generatif dalam menyelesaikan struktur kalimat.
  </div>
</div>

<!-- SECTION 4 -->
<div class="section" id="s4">
  <div class="section-title">
    <span class="section-number">4</span>
    Contoh 4 - Pengenalan Entitas Bernama (NER)
  </div>

  <div class="code-block">
    <div class="code-header">
      <span class="code-dot dot-red"></span>
      <span class="code-dot dot-yellow"></span>
      <span class="code-dot dot-green"></span>
      <span class="code-label">Python</span>
    </div>
    <pre><span class="cm"># TODO :</span>
<span class="fn">ner_model</span> = <span class="fn">pipeline</span>(<span class="str">"ner"</span>, model=<span class="str">"Jean-Baptiste/camembert-ner"</span>, aggregation_strategy=<span class="str">"simple"</span>)
<span class="fn">ner_model</span>(<span class="str">"I am learning Machine Learning with my mentor Arifian at Infinite Learning in Batam."</span>)</pre>
  </div>

  <div class="result-label">Hasil :</div>
  <div class="result-block">[
  {'entity_group': 'MISC', 'score': 0.987819,    'word': 'Machine Learning',  'start': 13, 'end': 30},
  {'entity_group': 'ORG',  'score': 0.6696281,   'word': 'Arifian',           'start': 45, 'end': 53},
  {'entity_group': 'ORG',  'score': 0.9817379,   'word': 'Infinite Learning', 'start': 56, 'end': 74},
  {'entity_group': 'LOC',  'score': 0.99267715,  'word': 'Batam',             'start': 77, 'end': 83}
]</div>

  <div class="analysis">
    <div class="analysis-title">Analisis pada contoh 4:</div>
    Pipeline NER berhasil mengekstrak entitas penting dari kalimat tanpa memecah sub-kata berkat parameter <code>aggregation_strategy</code>. Model mengenali <strong>"Machine Learning"</strong>, institusi <strong>"Infinite Learning"</strong>, dan lokasi <strong>"Batam"</strong> dengan akurasi sangat tinggi. Meskipun model mengklasifikasikan nama mentor (<em>"Arifian"</em>) sebagai organisasi (ORG), secara keseluruhan model ini sangat berguna untuk menandai dan mengekstrak informasi spesifik dari sebuah dokumen.
  </div>
</div>

<!-- SECTION 5 -->
<div class="section" id="s5">
  <div class="section-title">
    <span class="section-number">5</span>
    Contoh 5 - Menjawab Pertanyaan (Question Answering)
  </div>

  <div class="code-block">
    <div class="code-header">
      <span class="code-dot dot-red"></span>
      <span class="code-dot dot-yellow"></span>
      <span class="code-dot dot-green"></span>
      <span class="code-label">Python</span>
    </div>
    <pre><span class="cm"># TODO :</span>
<span class="fn">qa_model</span> = <span class="fn">pipeline</span>(<span class="str">"question-answering"</span>, model=<span class="str">"distilbert-base-cased-distilled-squad"</span>)
<span class="fn">konteks</span>   = <span class="str">"My name is Zulkifli and I am developing an interactive educational game called Riddle.Cyz."</span>
<span class="fn">pertanyaan</span> = <span class="str">"What is the name of the interactive game?"</span>
<span class="fn">qa_model</span>(question=pertanyaan, context=konteks)</pre>
  </div>

  <div class="result-label">Hasil :</div>
  <div class="result-block">{'score': 0.9674302339553833, 'start': 79, 'end': 89, 'answer': 'Riddle.Cyz'}</div>

  <div class="analysis">
    <div class="analysis-title">Analisis pada contoh 5:</div>
    Model QA bekerja dengan sangat luar biasa. Ia mengekstrak jawaban pasti yaitu <strong>"Riddle.Cyz"</strong> langsung dari konteks teks dengan tingkat kepercayaan <strong>96.7%</strong>. Kemampuan pemahaman bacaan mesin ini merupakan dasar yang sangat kuat untuk membangun sistem chatbot otomatis atau asisten pintar.
  </div>
</div>

<!-- SECTION 6 -->
<div class="section" id="s6">
  <div class="section-title">
    <span class="section-number">6</span>
    Contoh 6 - Ringkasan Teks (Summarization)
  </div>

  <div class="code-block">
    <div class="code-header">
      <span class="code-dot dot-red"></span>
      <span class="code-dot dot-yellow"></span>
      <span class="code-dot dot-green"></span>
      <span class="code-label">Python</span>
    </div>
    <pre><span class="cm"># TODO :</span>
<span class="fn">summarizer</span> = <span class="fn">pipeline</span>(<span class="str">"summarization"</span>, model=<span class="str">"sshleifer/distilbart-cnn-12-6"</span>)
<span class="fn">dokumen</span> = <span class="str">"""Natural Language Processing (NLP) is a subfield of linguistics,
computer science, and artificial intelligence concerned with the interactions
between computers and human language, in particular how to program computers
to process and analyze large amounts of natural language data. The goal is a
computer capable of understanding the contents of documents, including the
contextual nuances of the language within them."""</span>
<span class="fn">summarizer</span>(dokumen, max_length=<span class="num">50</span>, min_length=<span class="num">15</span>, do_sample=<span class="kw">False</span>)</pre>
  </div>

  <div class="result-label">Hasil :</div>
  <div class="result-block">[{'summary_text': ' NLP is a subfield of linguistics, computer science, and artificial intelligence concerned with the interactions between computers and human language . The goal is a computer capable of understanding the contents of documents, including the contextual nuances of the language within them .'}]</div>

  <div class="analysis">
    <div class="analysis-title">Analisis pada contoh 6:</div>
    Model perangkum berhasil memadatkan paragraf konseptual menjadi intisari kalimat yang padat dan informatif. Konsep kunci mengenai definisi NLP dan tujuannya tetap dipertahankan tanpa ada kehilangan konteks yang signifikan. Ini membuktikan kekuatan model dalam melakukan <strong>kompresi data tekstual</strong>.
  </div>
</div>

<!-- SECTION 7 -->
<div class="section" id="s7">
  <div class="section-title">
    <span class="section-number">7</span>
    Contoh 7 - Terjemahan (Translation)
  </div>

  <div class="code-block">
    <div class="code-header">
      <span class="code-dot dot-red"></span>
      <span class="code-dot dot-yellow"></span>
      <span class="code-dot dot-green"></span>
      <span class="code-label">Python</span>
    </div>
    <pre><span class="cm"># TODO :</span>
<span class="fn">translator</span> = <span class="fn">pipeline</span>(<span class="str">"translation_en_to_de"</span>)
<span class="fn">translator</span>(<span class="str">"I have successfully completed my guided project on Hugging Face Transformers."</span>)</pre>
  </div>

  <div class="result-label">Hasil :</div>
  <div class="result-block">[{'translation_text': 'Ich habe mein geführtes Projekt über Hugging Face Transformers erfolgreich abgeschlossen.'}]</div>

  <div class="analysis">
    <div class="analysis-title">Analisis pada contoh 7:</div>
    Model dengan mulus menerjemahkan kalimat Bahasa Inggris ke dalam Bahasa Jerman dengan struktur sintaksis yang benar. Keberhasilan tugas ini menunjukkan betapa mudahnya kita dapat mengimplementasikan kemampuan alih bahasa lintas negara hanya dengan menggunakan <strong>satu model pra-latih bawaan</strong> tanpa perlu arsitektur tambahan.
  </div>
</div>

<!-- FINAL ANALYSIS -->
<div class="final-analysis">
  <h2>Analisis Terhadap Proyek Ini</h2>
  <p>
    Proyek terbimbing ini memberikan wawasan teknis yang sangat berharga mengenai kemudahan dan kepraktisan ekosistem Hugging Face Transformers. Melalui penggunaan fungsi abstrak <code>pipeline()</code>, mengimplementasikan model Machine Learning yang kompleks untuk tugas pemrosesan bahasa alami (NLP) menjadi jauh lebih mudah, efisien, dan cepat.
  </p>
  <br>
  <p>
    Saya mendapatkan pengalaman langsung terkait bagaimana teknologi AI generatif beroperasi di balik layar untuk berbagai kasus penggunaan dunia nyata—mulai dari ekstraksi entitas, meringkas dokumen, hingga menebak dan menjawab pertanyaan. Selain itu, proyek ini juga melatih <em>problem-solving</em> saya ketika menghadapi kendala teknis library versioning (seperti saat harus meng-<em>upgrade</em> dan membersihkan instalasi library transformers agar model summarization berjalan lancar). Pengalaman ini menjadi fondasi yang kokoh bagi saya untuk mengembangkan aplikasi berbasis AI di masa depan.
  </p>
</div>

<div class="footer">
  <p>© 2026 · Zulkifli · Studi Independen AI — Infinite Learning × IBM Cognitive Class</p>
</div>

</body>
</html>
