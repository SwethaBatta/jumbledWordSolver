# CodeChallenge-PythonSpark

### Data pre-processing
-	Read freq_dict.json file as json and puzzle data as csv
-	Created an anagram dictionary anagram_dict from freq_dict.json
-	Created  a dataframe from puzzleData.csv file after some data pre-processing steps to convert it to acceptable format for script
-	Computed a list of relevant words using question and comment from the comic illustration which serves as the basis for comparing contextual similarity for the possible word combinations.  
  Filtered these query words based on their :-
    -	Frequency from freq_dict json file
    -	Eliminated stop-words, punctuations, words with irrelevant parts of speech

### Algorithm
- Created an anagram dictionary which holds the word in its sorted alphabetic order as the key and all the matching anagrams as values. The jumbled word is sorted and looked up in this dictionary to return results if any.  
  Sample dictionary record: ‘aegil’ : [‘agile’, ‘galei’, ‘agiel’] 

-	Created all the available combinations of hint letters when there is more than one solution for a jumbled word.
  Example: If we have two jumbled words with the following possible answers where 2 and 5 are the circled hint letters for the final result,  
  <pre>    
  <b>Jumbled words</b>        <b>Possible solutions</b>
  KNIDY                 DINKY
  LEGIA                 AGILE, GALEI, AGIEL
  </pre>
  We have the below cases here while using hint letters for the final result:
  <pre>
  DINKY and AGILE ----- [I, K], [A, I]
  DINKY and GALEI ----- [I, K], [G, L]
  DINKY and AGIEL ----- [I, K] [A, I]
  </pre>
  Since 1 and 3 above result in the same combination, we have the below possible letter combinations to consider
  [I, K] [A, I] and [I, K] [G, L]
-	Choosing the largest word length from the required words to generate all the possible words to create an initalList.  
  Example: If two words are to be generated for the final solution, say an 8 letter word and a 6 letter word, we target to fill out the 8 letter word first. 
-	Generating all the N length permutations of words.  
  Filtered based on their:-
    -	Frequency from freq_dict json file   
    - Eliminated stop-words, punctuations, words with irrelevant parts of speech
    - Sorted in the order of frequency (most frequent to least)
    - Computed similarity index with the query (comment+question) words
    - Sorted the final list based on this context similarity index (most similar to least)
- Using this initialList to generate the permutations for remaining required word lengths for the solution. 
- Choosing a word say token from initialList
- Removing the letters of token from the available hint list to generate valid words of remaining required lengths using the above generate N length words algorithm.
- If choosing token allows us to generate the remaining required word lengths, we add token and the resultant words to our final list.


### Further steps
-	Choose the most probable jumbled letter solutions to improve computational efficiency reducing the number of evaluations
-	The final list returns a list of possible answers for the end query. Implementing an algorithm to choose one solution based on further context similarity comparisons
-	Currently using the pre-trained statistical model from spacy - en_core_web_lg to compute context similarity between query and possible solution. 
-	Using a corpus close to comics such as ‘brown’ corpus from nltk which has ‘humor’ category. 
-	Using data which provides information about homophones which are a common occurrence in these illustrations. Found these datasources which can be useful to achieve this 
  - CMU pronouncing dictionary  - https://github.com/Alexir/CMUdict/blob/master/cmudict-0.7b
  - Python module – pronouncing
-	Using spark to leverage its parallel processing feature to improve performance.






