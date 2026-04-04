import nltk
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize, sent_tokenize
import heapq

nltk.download('punkt')
nltk.download('stopwords')

def summarize_text(text, num_sentences=2):
    stop_words = set(stopwords.words('english'))
    words = word_tokenize(text.lower())

    freq = {}
    for word in words:
        if word not in stop_words and word.isalnum():
            freq[word] = freq.get(word, 0) + 1

    sentences = sent_tokenize(text)
    sentence_scores = {}

    for sent in sentences:
        for word in word_tokenize(sent.lower()):
            if word in freq:
                sentence_scores[sent] = sentence_scores.get(sent, 0) + freq[word]

    summary = heapq.nlargest(num_sentences, sentence_scores, key=sentence_scores.get)
    return " ".join(summary)

text = """Artificial Intelligence is transforming industries. It improves efficiency and decision making."""
print(summarize_text(text))
