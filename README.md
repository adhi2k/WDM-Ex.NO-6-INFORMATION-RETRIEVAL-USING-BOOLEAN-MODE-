# WDM-Ex.NO-6-INFORMATION-RETRIEVAL-USING-BOOLEAN-MODE

## Aim

To find the ranking of web documents using the Boolean Model.

## Program

### 1. Importing the Libraries

```python
import pandas
from contextlib import redirect_stdout

terms = []
keys = []
vec_Dic = {}
dicti = {}
dummy_List = []
```

### 2. Filtering Documents

```python
def filter(documents, rows, cols):
    """Extract document names and terms from the dataset."""

    for i in range(rows):
        for j in range(cols):
            if j == 0:
                keys.append(documents.loc[i].iat[j])
            else:
                dummy_List.append(documents.loc[i].iat[j])

                if documents.loc[i].iat[j] not in terms:
                    terms.append(documents.loc[i].iat[j])

        copy = dummy_List.copy()
        dicti.update({documents.loc[i].iat[0]: copy})
        dummy_List.clear()
```

### 3. Boolean Representation of Documents

```python
def bool_Representation(dicti, rows, cols):
    """Create Boolean vectors for each document."""

    terms.sort()

    for i in dicti:
        for j in terms:
            if j in dicti[i]:
                dummy_List.append(1)
            else:
                dummy_List.append(0)

        copy = dummy_List.copy()
        vec_Dic.update({i: copy})
        dummy_List.clear()
```

### 4. Query Vector

```python
def query_Vector(query):
    """Represent the user query as a Boolean vector."""

    qvect = []

    for i in terms:
        if i in query:
            qvect.append(1)
        else:
            qvect.append(0)

    return qvect
```

### 5. Ranking the Documents

```python
def prediction(q_Vect):
    """Rank documents according to Boolean vector similarity."""

    dictionary = {}
    listi = []
    count = 0

    term_Len = len(terms)

    for i in vec_Dic:
        for t in range(term_Len):
            if q_Vect[t] == vec_Dic[i][t]:
                count += 1

        dictionary.update({i: count})
        count = 0

    for i in dictionary:
        listi.append(dictionary[i])

    listi = sorted(listi, reverse=True)

    ans = ""

    with open("output.txt", "w") as f:
        with redirect_stdout(f):
            print("Ranking of the documents")

            for count, i in enumerate(listi):
                key = check(dictionary, i)

                if count == 0:
                    ans = key

                print(key, "rank is", count + 1)
                dictionary.pop(key)

            print(ans, "is the most relevant document for the given query")
```

### 6. Checking Document Rankings

```python
def check(dictionary, val):
    """Return the document name for a given ranking value."""

    for key, value in dictionary.items():
        if val == value:
            return key
```

### 7. Main Function

```python
def main():
    documents = pandas.read_csv(
        "/content/sample_data/documents.csv",
        encoding="unicode_escape"
    )

    rows = len(documents)
    cols = len(documents.columns)

    filter(documents, rows, cols)

    bool_Representation(dicti, rows, cols)

    print("Enter query")
    query = input()

    query = query.split(" ")

    q_Vect = query_Vector(query)

    prediction(q_Vect)


if __name__ == "__main__":
    main()
```

## Dataset

Create a file named `documents.csv` with the following content:

```csv
documents,term1,term2,term3
document1,ice cream,mango,litchi
document2,hockey,cricket,sport
document3,litchi,mango,chocolate
document4,nice,good,cute
```

## Output

Enter a query when prompted by the program.

Example query:

```text
hockey is a national sport
```

The program writes the document ranking to `output.txt`.

Insert the output screenshot below.

![Output](https://github.com/user-attachments/assets/1fb25b95-70f0-44fe-9e40-240d130247e5)

## Result

Thus, the Python program for information retrieval using the Boolean Model was implemented successfully.
