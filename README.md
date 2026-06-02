# stopwords removal
import nltk
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
nltk.download('punkt')
nltk.download('stopwords')
text = '''NLTK, the Natural Language Toolkit, is a suite of open source program
modules, tutorials and problem sets, providing ready-to-use computational
linguistics courseware. NLTK covers symbolic and statistical natural language
processing, and is interfaced to annotated corpora. Students augment
and replace existing components, learn structured programming by example,
and manipulate sophisticated models from the outset.'''
words = word_tokenize(text)
stop_words = set(stopwords.words('english'))
filtered_words = []
for word in words:
    if word.lower() not in stop_words:
        filtered_words.append(word)
print("Text after removing stop words:")
print(filtered_words)


# most frequently occuring Non-stopwords
from nltk.book import text1
from nltk.probability import FreqDist
from nltk.corpus import stopwords
stop_words = set(stopwords.words('english'))

def frequent_words(text, limit=100):
    filtered_words = [
        word.lower() for word in text
        if word.isalpha() and word.lower() not in stop_words
    ]
    freq = FreqDist(filtered_words)
    return freq.most_common(limit)
result = frequent_words(text1)


# Words occuring atleast 3 times
from nltk.corpus import brown
from nltk.probability import FreqDist
words = brown.words()
filtered_words = [word.lower() for word in words if word.isalpha()]
freq_dist = FreqDist(filtered_words)
result = []
for word, count in freq_dist.items():
    if count >= 3:
        result.append(word)
print("Words occurring at least 3 times:\n")
print(result)


# frequent bigrams
from nltk.book import text3
from nltk.corpus import stopwords
from nltk import bigrams
from collections import Counter
stop_words = set(stopwords.words('english'))
words = [word.lower() for word in text3 if word.isalpha() and word.lower() not in stop_words]
bigram_freq = Counter(bigrams(words))
most_common_bigrams = bigram_freq.most_common(50)
print("50 Most Frequent Bigrams:\n")
for bigram, frequency in most_common_bigrams:
    print(bigram, ":", frequency)
    

    
# Word frequency 
from nltk.corpus import brown
def word_freq(word, section):
    words = brown.words(categories=section)
    count = words.count(word)
    print("Frequency of", word, "in", section, "section is:", count)
word_freq("government", "news")


# Sorted words
words = ["people", "America", "cars", "bikes","boat","bus"]
words.sort()
print("Using sort():", words)
new_words = sorted(words)
print("Using sorted():", new_words)


#  porter and lancaster stemmer 
import nltk
from nltk.stem import PorterStemmer, LancasterStemmer
from nltk.tokenize import word_tokenize
text = "Japanese people always works hard for their life and wellbeing. they treat tourist people with a warm heart."
porter = PorterStemmer()
lancaster = LancasterStemmer()
porter_stems = [porter.stem(word) for word in words]
lancaster_stems = [lancaster.stem(word) for word in words]
print("Original words:", words)
print("Porter Stemmed:", porter_stems)
print("Lancaster Stemmed:", lancaster_stems)

# noun,path and ich similarity
import nltk
import nltk
from nltk.corpus import wordnet
noun1 = wordnet.synset("dog.n.01")
noun2 = wordnet.synset("cat.n.01")
print("Noun Similarity (dog, cat):")
print("Path Similarity:", noun1.path_similarity(noun2))
print("Wup Similarity:", noun1.wup_similarity(noun2))
print("--------------------------------")
verb1 = wordnet.synset("run.v.01")
verb2 = wordnet.synset("walk.v.01")
print("Verb Similarity (run, walk):")
print("Path Similarity:", verb1.path_similarity(verb2))
print("lch Similarity:", verb1.lch_similarity(verb2))


# Word definition and example 
import nltk
from nltk.corpus import wordnet
words = ["education", "fight", "travel"]
for word in words:
    synset = wordnet.synsets(word)[0]
    print("Definition:", synset.definition())
    print("Example:", synset.examples())
print("100 Most Frequently Occurring Non-Stopwords:\n")
for word, count in result:
    print(word, ":", count)

# synonyms and Antonyms
from nltk.corpus import wordnet
import nltk
words = ["Artful", "Ballot", "Chorus", "Deceptive", "Enormous"]
def get_syn_ant(word):
    synonyms = set()
    antonyms = set()
    for syn in wordnet.synsets(word):
        for lemma in syn.lemmas():
            synonyms.add(lemma.name())
            if lemma.antonyms():
                antonyms.add(lemma.antonyms()[0].name())
    return synonyms, antonyms
for word in words:
    syns, ants = get_syn_ant(word)
    print("\nWord :", word)
    print("Synonyms :")
    if syns:
        print(", ".join(syns))
    else:
        print("No synonyms found")
    print("Antonyms :")
    if ants:
        print(", ".join(ants))
    else:
        print("No antonyms found")    
