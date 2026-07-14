# NLP Basics: Tokenization, Preprocessing, and Classical Word Embeddings

## TL;DR
This session introduced natural language processing as the bridge between human language and numerical models, with hands-on NLTK demos for tokenization, stemming, lemmatization, stop-word removal, and POS tagging. The instructor then walked through classical vectorization—Bag of Words, TF-IDF, and pre-trained Word2Vec (Google News via Gensim)—and contrasted them with modern high-dimensional embeddings (OpenAI, Hugging Face). These topics are framed as conceptual building blocks for generative and agentic AI, not as production defaults in 2026.

## Topics Covered
- Recap: prior ML/DL content and doubt-clearing session for missed material
- What NLP is and why it matters (computers need numerical/binary input)
- NLP as a prerequisite for generative AI and agentic AI
- Tokenization (word-level and sentence-level)
- Stemming vs. lemmatization
- Part-of-speech (POS) tagging and grammatical roles
- Stopword removal
- Vectorization / word embeddings (overview of techniques)
- Practical lab: NLTK setup and basic NLP pipeline
- Bag of Words (theory and worked example)
- TF-IDF (formulas and worked example on sample sentences)
- Word2Vec with Google News pre-trained model (Gensim, Colab, Google Drive)
- Why classical embeddings are largely obsolete vs. modern API embeddings
- Preview of next session: custom Word2Vec, transformers, BERT, encoder–decoder

## Key Concepts Explained

### Natural Language Processing (NLP)
- A field of study (alongside ML) that leads toward AI; focuses on processing human language so machines can work with it.
- Why it matters: users speak/write in natural language; models need numbers—NLP is the “middleman.”
- The instructor emphasized that even if you rarely implement these steps manually today, understanding them helps with GenAI/agentic systems.

### Tokenization
- Breaking text into the smallest useful units (tokens)—often words, sometimes subwords (e.g. `go` + `ing`).
- Why it matters: larger inputs mean more tokens and harder processing; smaller, focused inputs are preferable in classic pipelines.
- Two main types: **word tokenization** and **sentence tokenization** (split paragraphs on sentence boundaries).
- Caveat: there is no single “correct” tokenization—choices depend on use case; modern LLMs use different tokenization than these basics.

### Stemming
- Reducing words to a **rough root** (e.g. *studies* → *studi*, *glorifies* → *glorifi*)—outputs may not be valid dictionary words.
- Why it matters: cheap normalization for search/IR-style tasks.
- Drawback: irregular or over-stemmed forms; instructor showed PorterStemmer quirks (*easily*, *fairly* not cleaned well).

### Lemmatization
- Reducing words to a **dictionary base form** using context/POS (e.g. *running* → *run*, *studies* → *study*).
- Why it matters: more linguistically correct than stemming when you need real words.
- Caveats: not always “better” than stemming—has weaknesses too; WordNet lemmatizer often needs POS hints; batch lemmatization in the demo notebook behaved differently than single-word calls.

### Part-of-Speech (POS) Tagging
- Labeling each token with its grammatical role (noun, verb, determiner, proper noun, etc.).
- Why it matters: when converting text to vectors, preserving grammatical structure helps models capture meaning; POS tags help NLP systems understand sentence structure.
- Example discussed: *“Rahul is going to the market.”* — *Rahul* = proper noun (NNP), *is*/*going* = verbs, *to* = preposition, *the* = determiner (DT), *market* = noun.
- Instructor noted pronoun vs. proper noun distinction (*he/him* vs. names).

### Stopword Removal
- Removing very common words (*a*, *the*, *is*, *and*, pronouns, etc.) that add little semantic value.
- Why it matters: first crucial cleaning step—reduces noise and dimensionality (e.g. five tokens → two meaningful ones).
- Caveat: whether a word is a stopword can be context-dependent (*both* was questioned in the Bag of Words example).

### Vectorization / Word Embeddings
- Converting textual data into numerical vectors so ML models can consume language.
- Multiple historical techniques; session focused on Bag of Words, TF-IDF, and Word2Vec.

### Bag of Words (BoW)
- Represents each document as a vector of word **presence/absence** (or counts) over a vocabulary built from the corpus—order is ignored.
- Worked example: after lowercasing and stopword removal, vocabulary `{boy, girl, good}` → S1=`101`, S2=`011`, S3=`111`.
- Why it matters: simplest introduction to text-as-numbers; instructor compared it loosely to one-hot-style encoding.
- Caveat: “not used nowadays”—poor quality; conceptual only.

### TF-IDF (Term Frequency–Inverse Document Frequency)
- **TF(word, sentence)** = (number of times word appears in sentence) / (total words in sentence).
- **IDF(word)** = log(total number of sentences / number of sentences containing that word).
- **TF-IDF** = TF × IDF.
- Why it matters: down-weights words that appear in almost every document; instructor said it had real production success historically.
- Key insight from example: *good* appears in all three sentences → IDF term drives TF-IDF toward zero → word loses discriminative importance.
- Caveat: dated for 2026 production; still worth knowing the math.

### Word2Vec (Google News pre-trained model)
- Dense embeddings (Google News model: **300 dimensions** per word); features are learned, not human-labeled—Google has not published what the 300 dimensions mean.
- Supports **similarity** (`most_similar`), **analogies** (king − man + woman ≈ queen), and vector arithmetic.
- Why it matters: step up from sparse BoW/TF-IDF; illustrates semantic relationships in vector space.
- Caveat: 300-D vs. modern 1,536–3,000+ API embeddings; use `.gz` directly in Gensim—no need to unzip.

### Modern Embeddings (OpenAI, Hugging Face, etc.)
- Production today uses API/provider embeddings (e.g. OpenAI `text-embedding-3-small` at **1,536** dimensions; larger models up to ~**3,000**).
- Instructor: do **not** use BoW, TF-IDF, or Word2Vec in real projects tomorrow—use latest embeddings (OpenAI, Hugging Face, Titan, etc.).

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| NLTK | Primary library for tokenization, stemming, lemmatization, stopwords, POS tagging |
| `pip install nltk` | Required before imports; many students hit errors without this |
| `nltk.download('punkt_tab')` | Punkt tokenizer data (transcript also mentions `punkt` for stopwords section) |
| `nltk.download('stopwords')` | English stopword list |
| `nltk.word_tokenize()` | Word-level tokens (punctuation often separate tokens) |
| `nltk.sent_tokenize()` | Sentence-level splits; periods matter, commas do not split sentences |
| PorterStemmer | NLTK stemming algorithm; demo on *running*, *runs*, *better*, etc. |
| WordNet Lemmatizer | `nltk.stem.WordNetLemmatizer`; needs WordNet data downloaded |
| `nltk.pos_tag()` | POS tagging; uses averaged perceptron tagger (`nltk.download` for tagger model) |
| Gensim | `pip install gensim`; load pre-trained Word2Vec (`KeyedVectors.load_word2vec_format`) |
| Google News Word2Vec | ~1.53 GB `.gz` model; store on Google Drive, mount in Colab |
| Google Colab | Upload week 5 `.ipynb` notebooks via File → Upload Notebook |
| Week 5 course materials | Bookmark shared link; download “NLP Basics” and “Word2Vec pre-trained model” notebooks |
| Bag of Words | Classical sparse representation |
| TF-IDF | Term frequency × inverse document frequency |
| CBOW / Skip-gram | Mentioned as advanced Word2Vec variants; deferred to doubt-clearing if needed |
| OpenAI embeddings | e.g. `text-embedding-3-small` (1,536-D); dimensionality varies by model |
| Hugging Face embeddings | Modern alternative cited |
| Titan embeddings | AWS embedding family mentioned in passing |
| Discord | Contact instructor “Sat” privately for coupon codes if watching recording later |

## Code & Examples

### NLTK setup
```python
# pip install nltk
import nltk
nltk.download('punkt_tab')
```

### Word and sentence tokenization
```python
text = "Hello, world! This is the first class on tokenization."
nltk.word_tokenize(text)
# -> ['Hello', ',', 'world', '!', 'This', 'is', ...]

nltk.sent_tokenize(text)
# -> ['Hello, world!', 'This is the first class on tokenization.']
```

### Stemming (PorterStemmer)
```python
from nltk.stem import PorterStemmer
stemmer = PorterStemmer()
words = ["running", "run", "runs", "runner", "easily", "fairly", "better", "best"]
stemmed = [stemmer.stem(w) for w in words]
```

### Lemmatization (WordNet)
```python
from nltk.stem import WordNetLemmatizer
lemmatizer = WordNetLemmatizer()
# Single word works in demo: lemmatizer.lemmatize("running") -> "run"
# Batch loop in notebook had issues without POS — instructor to verify
```

### Stopword removal
```python
from nltk.corpus import stopwords
nltk.download('stopwords')
nltk.download('punkt')

stop_words = set(stopwords.words('english'))
word_tokens = nltk.word_tokenize(example_text)
filtered = [w for w in word_tokens if w.lower() not in stop_words]
```

### POS tagging
```python
text = "Hello, how are you? I am fine."
word_tokens = nltk.word_tokenize(text)
nltk.pos_tag(word_tokens)
# Tags include NNP, WRB, VBP, PRP, JJ, etc.
```

### Bag of Words (manual workflow described in class)
1. Lowercase all sentences (`.lower()` — NLP treats `Good` and `good` as different otherwise).
2. Remove stopwords.
3. Build vocabulary from remaining tokens.
4. Binary presence vector per sentence over vocabulary.

Example outcome after cleaning three sentences:
- S1: `101`, S2: `011`, S3: `111` for vocabulary `{boy, girl, good}`.

### TF-IDF (formulas from whiteboard)
```
TF(word, sentence) = count(word in sentence) / total_words(sentence)
IDF(word) = log(N / df(word))   # N = total sentences, df = sentences containing word
TF-IDF(word, sentence) = TF(word, sentence) × IDF(word)
```

Worked numbers (approximate from transcript): TF(good, S1)=0.5, TF(good, S3)≈0.33; IDF(good)=log(3/3)=0 → TF-IDF(good,*)=0 when word appears in all documents.

### Word2Vec with Gensim (Google News)
```python
# pip install gensim
import gensim.downloader as api
from gensim.models import KeyedVectors
from google.colab import drive
drive.mount('/content/drive')

# Load pre-trained model from Drive (.gz, no unzip)
model = KeyedVectors.load_word2vec_format(
    '/content/drive/MyDrive/.../GoogleNews-vectors-negative300.bin.gz',
    binary=True
)

model['man'].shape          # (300,)
model.most_similar('man')   # e.g. woman ~0.766
model.similarity('man', 'woman')

# Analogy: king - man + woman ≈ queen
result = model.most_similar(
    positive=['king', 'woman'],
    negative=['man']
)
# Ignore 'king' in results when evaluating analogy
```

### Dimensionality reduction visualization (demo)
- Subset first 10 of 300 dimensions for words like *king*, *queen*, *man*, *woman* → heatmap plot.
- Instructor: axis labels 0–9 are indices, not interpretable feature names.

## Q&A / Discussion Points
- **Computers and English:** Computers do not natively understand English; NLP converts language for numeric models.
- **“Rahul is going to the market” POS exercise:** Students identified noun/verb/preposition; instructor clarified proper noun (NNP) vs. pronoun.
- **NNP, WRB, VBP, PRP, JJ:** Instructor partially recalled tag meanings live (VBP = verb non-3rd person singular; PRP = personal pronoun; WRB = wh-adverb; NNP discussed as proper noun).
- **Sentence tokenizer vs. commas:** Libraries split on periods, not commas—by design of Punkt/human rules.
- **How can one word have 300 dimensions?** Compared to BoW with 3 unique tokens → 3-D sparse vectors; Word2Vec uses a fixed 300 learned features per word.
- **Why `most_similar` after king−man+woman still shows king?** King is in the query set—exclude it; next closest is queen.
- **Google News file size:** ~1.53 GB (instructor initially said 300 GB, then corrected); use Google Drive, not local machine.
- **Direct upload to Drive:** Download locally then upload, or use Drive workflow student asked about.
- **Unzip model?** No—use `.gz` directly with `load_word2vec_format`.
- **Recording availability:** Should be posted within a few hours.
- **Coupon codes for ML course:** Time series and deep learning shared (code `100`); full ML course access promised in 1–2 days.

## Action Items & Assignments
- [ ] Watch doubt-clearing recording if you missed weekend ML/DL completion
- [ ] Bookmark the week 5 materials link (not re-shared every class)
- [ ] Download and open **NLP Basics** notebook in Google Colab (week 5)
- [ ] Run full NLTK practical: install NLTK, download `punkt_tab`, tokenization, stemming, lemmatization, stopwords, POS
- [ ] Download **Word2Vec / Google News** notebook; `pip install gensim`
- [ ] Download Google News model (~1.53 GB) to Google Drive; mount Drive in Colab
- [ ] Complete Word2Vec exercises (similarity, analogies, optional heatmap) or defer if download incomplete
- [ ] Enroll in instructor’s time series and deep learning courses via shared coupon links (`code 100`)
- [ ] Fill session poll
- [ ] Leave a five-star rating after studying the gifted courses
- [ ] **Next class:** build custom Word2Vec with explicit features; then transformers, BERT, encoder–decoder architectures
- [ ] **Real projects:** use modern embeddings (OpenAI, Hugging Face, etc.)—not BoW, TF-IDF, or Word2Vec

## Open Questions / Unclear Spots
- Lemmatization loop in the week 5 notebook returned unchanged words for the list (*running* stayed *running*) while single-word `lemmatize("running")` returned *run*—instructor said they would check the code (likely missing POS argument in batch call).
- Instructor was unsure of some POS tag mnemonics during live demo (exact expansion of NNP vs. student guess “pronoun”).
- CBOW and Skip-gram skipped; available on request in doubt-clearing session.
- [unclear: exact week 5 URL] — transcript refers to bookmarking link and “freq five” folder but does not spell out the URL in text.
- Coupon / course distribution policy with Codecademy mentioned informally—not a technical topic.
