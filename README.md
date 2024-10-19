#### NAME: ROBINSON J

#### REG.NO: 212223040170

## Experiment-05: Information Retrieval Using Boolean Model in Python

## DATE: 19/10/24

### AIM: To implement Information Retrieval Using Boolean Model in Python.

### Description:

<div align = "justify">
The Boolean model in Information Retrieval (IR) is a fundamental model used for searching and retrieving information from a collection of documents. It operates on the principles of set theory and logic, where documents are represented as sets of terms or words, and queries are expressed as Boolean expressions using logical operators such as AND, OR, and NOT.
  
### Procedure:
1. ***Initialize the BooleanRetrieval class:*** The BooleanRetrieval class is defined to manage the indexing and searching of documents.
2. ***Constructor and Index Initialization:*** The class constructor initializes an empty index to store the inverted index mapping terms to documents.
3. ***Indexing Documents:***
    <p> a) The index_document method is responsible for indexing documents.
    <p> b) Tokenize the text content of documents, converting them into lowercase terms.
    <p> c) For each term in the document, it adds an entry in the index, associating the term with the document ID. </p>
4. ***Fetch Web Page Text:***
    <p>a) The fetch_webpage_text method uses the requests library to fetch content from a given URL.
    <p>b) Extract text content from the fetched HTML using BeautifulSoup.
    <p>c) The extracted text is returned for further processing.
5. ***Boolean Search:***
    <p>a) The boolean_search method performs Boolean searches on the indexed documents.
    <p>b) Tokenize the input query and iterates through its terms.
    <p>c) For each term in the query, it retrieves documents containing that term and performs Boolean operations (AND, OR, NOT) based on the query's structure.

### Program:

```python
import numpy as np
import pandas as pd

class BooleanRetrieval:
    def __init__(self):
        self.index = {}
        self.documents_matrix = None

    def index_document(self, doc_id, text):
        terms = text.lower().split()
        print("Document -", doc_id, terms)

        for term in terms:
            if term not in self.index:
                self.index[term] = set()
            self.index[term].add(doc_id)

    def create_documents_matrix(self, documents):
        terms = list(self.index.keys())
        num_docs = len(documents)
        num_terms = len(terms)

        self.documents_matrix = np.zeros((num_docs, num_terms), dtype=int)

        for i, (doc_id, text) in enumerate(documents.items()):
            doc_terms = text.lower().split()
            for term in doc_terms:
                if term in self.index:
                    term_id = terms.index(term)
                    self.documents_matrix[i, term_id] = 1

    def print_documents_matrix_table(self):
        df = pd.DataFrame(self.documents_matrix, columns=self.index.keys())
        print(df)

    def print_all_terms(self):
        print("All terms in the documents:")
        print(list(self.index.keys()))

    def boolean_search(self, query):
        query_terms = query.lower().split()
        results = None

        for term in query_terms:
            doc_ids = self.index.get(term, set())
            if results is None:
                results = doc_ids.copy()
            else:
                if term.startswith('not'):
                    results.difference_update(doc_ids)
                elif term == 'or':
                    results.update(doc_ids)
                elif term == 'and':
                    results.intersection_update(doc_ids)

        return list(results) if results else []

if __name__ == "__main__":
    indexer = BooleanRetrieval()


    documents = {
        1: "Python is a programming language",
        2: "Information retrieval deals with finding information",
        3: "Boolean models are used in information retrieval"
    }

    for doc_id, text in documents.items():
        indexer.index_document(doc_id, text)


    indexer.create_documents_matrix(documents)
    indexer.print_documents_matrix_table()


    indexer.print_all_terms()


    query1 = input("Enter your boolean query: ")
    results = indexer.boolean_search(query1)
    if results:
        print(f"Results for '{query1}': {results}")
    else:
        print("No results found for the query.")
```

### Output:
![Screenshot from 2024-10-19 09-10-06](https://github.com/user-attachments/assets/710a000f-883f-4d3a-ac8a-5046a9ddc55a)

### Result:

Successfully implemented Information Retrieval Using Boolean Model in Python.
