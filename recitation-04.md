# CMPS 2200  Recitation 04

**Name (Team Member 1):**Zev Gaslin  
**Name (Team Member 2):**Josh Haddad


In this lab you will practice using the `map` and `reduce` functions. These functions are commonly used together in a `map-reduce` framework, used by Google and others to parallelize and scale common computations.


## Part 1: Counting Words

In the first part, we will use map-reduce to count how often each word appears in a sequence of documents. E.g. if the input is two documents:

```python
['i am sam i am', 'sam is ham']
```

then the output should be

```python
[('am', 2), ('ham', 1), ('i', 2), ('is', 1), ('sam', 2)]
```

We have given you the implementation of the main map-reduce logic
```python
def run_map_reduce(map_f, reduce_f, docs)
```

To use this function to count words, you'll need to implement your own `map_f` and `reduce_f` functions, described below.

1. Complete `word_count_map` and test it with `test_word_count_map`. Please use doc.split() to split a string. 

2. Complete `word_count_reduce` and test it with `test_word_count_reduce`.

3. If the above are correct, then you should now be able to test it the full solution `test_word_count`

4. Assume that a word `w` appears `n` times. What is the **work** and **span** of `word_count_reduce` for this word, assuming a parallel implementation of the `reduce` function?

**Enter answer here**
Work:
W(N)=N
The work of this function is based on the number of operations preformed. Because the first step of this function splits each word into its own touple which are then grouped independently and combined in the last step. Because of this an operation is preformed at each of the 3 steps for each word giving a total work of 3N or N


Span:
S(N)=log(N)
The span of this function is based of the critical path length. In this intsance the function is parralized because the first step splits each word into its own touple which is then recombined in the final steps of the problem. This first step has a span of 1. The next step combines all the same words into one touple and has a span of log the number of words because the words are still parralized. The final step has to condense each touple which occurs n times for a given word given a runtime of log(n). Because the number of words is larger then the amount of repeated words the total span of this function is just log(the number of words) or log(N).

5. Why are we going through all this trouble? Couldn't I just use this function to count words?

```python
docs = ['i am sam i am', 'sam is ham']
counts = {}
for doc in docs:
    for term in doc.split():
        counts[term] = counts.get(term, 0) + 1
# counts = {'i': 2, 'am': 2, 'sam': 2, 'is': 1, 'ham': 1}
```

What is the problem that prevents us from easily parallelizing this solution?

**Enter answer here**
Counts is updated after each word and therefore if the whole list were to be checked in parrallel multiple entries could be created for the same word which would could cause errors correctly counting the words. Additionally, unforseen issues could occur if two identical words are trying to be added to the dictionary at the exact same time.

## Part 2: Sentiment analysis

Finally, we'll adapt our approach above to perform a simple type of sentiment analysis. Given a document, rather than counting words, we will instead count the number of positive and negative terms in the document, given a predefined list of terms. E.g., if the input sentence is `it was a terrible waste of time` and the terms `terrible` and `waste` are in our list of negative terms, then the output is

`[('negative', 1), ('negative', 1)]`

6. Complete the `sentiment_map` function to implement the above idea and test it with `test_sentiment_map`.

7. Since the output here is similar to the word count problem, we will reuse `word_count_reduce` to compute the total number of positive and negative terms in a sequence of documents. Confirm your results work by running `test_sentiment`.
