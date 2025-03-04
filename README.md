# 📌 Boolean Text Search with Inverted Index

## 📖 Overview
This project implements **text preprocessing, inverted index construction, and Boolean search operations** using Python and **NLTK**. It allows users to:
- **Tokenize and preprocess text** (stopword removal, stemming, punctuation handling).
- **Build an inverted index** to store term-document mappings.
- **Perform Boolean search operations** (`AND`, `OR`) to find relevant documents.

---

## 📂 File Structure
```
|-- Boolean-Text-Search
    |-- HW1_stamps-and-no-stop_words.ipynb  # Jupyter Notebook with all the code
    |-- README.md  # Documentation (this file)
    |-- sample_documents.txt  # Sample dataset (if needed)
```

---

## 🛠️ Installation & Setup
### 🔹 Prerequisites
Make sure you have Python installed, along with the required libraries:
```sh
pip install nltk numpy pandas
```

### 🔹 Run the Notebook
1. Open the Jupyter Notebook:
   ```sh
   jupyter notebook HW1_stamps-and-no-stop_words.ipynb
   ```
2. Run the cells in sequence to process text and execute queries.

---

## 🔬 Code Explanation

### **1️⃣ Importing Required Libraries**
```python
import nltk
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from nltk.stem import SnowballStemmer
import numpy as np
import pandas as pd
```
✅ **Purpose:**
- Load **NLTK** for text processing (tokenization, stemming, stopword removal).
- Import **NumPy and Pandas** for data handling (not heavily used).

---
### **2️⃣ Defining Sample Documents**
```python
review_1 = "The Glider II II is a great soccer ball played , but mk2 is the greatness ."
review_2 = "What ball is a bad soccer ball ?"
review_3 = "I am happy play with The glider , but the happiness is with want mk2 "

docs = [review_1, review_2, review_3]
```
✅ **Purpose:**
- Defines a small collection of **3 sample documents**.

---
### **3️⃣ Preprocessing: Lowercasing, Stopword Removal, Stemming**
```python
sbs = SnowballStemmer("english")
stop_words = set(stopwords.words('english'))
no = ['?', ',', '.']  # Punctuation to remove

lower_docs = [doc.lower() for doc in docs]

doc = []
for r in lower_docs:
    review = []
    for term in r.split():
        if term not in stop_words:
            review.append(sbs.stem(term))
            if term in no:
                review.remove(term)
    doc.append(review)
```
✅ **Purpose:**
- **Converts text to lowercase**.
- **Removes stopwords** (e.g., "is", "the").
- **Applies stemming** (e.g., "happiness" → "happi").

---
### **4️⃣ Constructing an Inverted Index**
```python
inverted_index = {}
for term in sorted_terms:
    x, cc, ccc = [], [], []
    for j in range(len(doc)):
        if term in doc[j]:
            x.append(j)  # Store document index
        for n in doc[j]:
            if term == n:
                cc.append(term)
        if len(cc) != 0:
            ccc.append(len(cc))
            cc = []
    inverted_index[term] = x, ccc
```
✅ **Purpose:**
- Maps **each term** to the **documents where it appears**.
- Stores the **frequency of the term in each document**.

---
### **5️⃣ Retrieving Posting Lists**
```python
def post(term):
    return {term: inverted_index[term]}
```
✅ **Purpose:**
- Retrieves **posting lists** (documents where a word appears).

---
### **6️⃣ Boolean Search Operations**
#### **AND Query (Find Documents Containing Both Terms)**
```python
def and_postings(term1, term2):
    s1 = inverted_index[sbs.stem(term1)][0]  # Get document list for term1
    s2 = inverted_index[sbs.stem(term2)][0]  # Get document list for term2
    return list(set(s1) & set(s2))  # Intersection (AND)
```
✅ **Purpose:**
- Returns **documents containing both terms**.

#### **OR Query (Find Documents Containing Either Term)**
```python
def or_postings(term1, term2):
    s1 = inverted_index[sbs.stem(term1)][0]  # Get document list for term1
    s2 = inverted_index[sbs.stem(term2)][0]  # Get document list for term2
    return list(set(s1) | set(s2))  # Union (OR)
```
✅ **Purpose:**
- Returns **documents containing at least one of the terms**.

---
## 📌 Sample Outputs
### **1️⃣ Get Posting List for a Word**
```python
post('glider')
```
✅ **Example Output:**
```
{'glider': ([2, 0], [1, 1])}  # Appears in documents 2 and 0
```

### **2️⃣ Boolean AND Search**
```python
and_postings('play', 'happy')
```
✅ **Example Output:**
```
[2]  # Only document 2 contains both words
```

### **3️⃣ Boolean OR Search**
```python
or_postings('glider', 'soccer')
```
✅ **Example Output:**
```
[2, 0, 1]  # These documents contain either 'glider' or 'soccer'
```

---
## 🎯 **Key Takeaways**
| **Feature** | **Functionality** |
|------------|------------------|
| **Text Preprocessing** | Removes stopwords, applies stemming. |
| **Inverted Index** | Maps terms to documents. |
| **Boolean Search** | Supports `AND` and `OR` operations. |

---
## 🚀 Possible Improvements
- **Support phrase queries** (`"great soccer"`).
- **Implement ranking (TF-IDF)** for better search results.
- **Use a larger dataset** for more realistic testing.

---
## 📜 Conclusion
This project implements a **basic search engine** using **inverted indexing and Boolean retrieval**. It efficiently retrieves documents based on keyword searches and can be expanded for advanced **information retrieval**
