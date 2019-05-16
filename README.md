# WSD-system-implementation
WSD system implementation using - ontological model and supervised model

In this problem, you are going to implement a WSD system by using two different models: ontological model and supervised model. To start, read the English Lexical Sample Task written by Mihalcea, Chklovski and Kilgarriff in the following link http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.153.8426

It describes one lexical element: future (part-of-speech is noun) with its four different senses. Each sense has its own gloss (definition) and examples that are separated by j symbol. Each sense is also associated with the corresponding senses of WordNet 2.1. As our sense divisions and WordNet's are not identical, some of our senses could be mapped to multiple WordNet senses or possibly nothing (e.g., See the fourth sense item in the above example). Since the current NLTK is using WordNet 3, the sense mapping from 2.1 to 3.x could be useful https://stackoverflow.com/questions/46950379/how-to-fetch-a-specific-version-of-wordnet-when-doing-nltk-download

The training data specifies the correct sense of the target word providing its verbal context surrounding the target word. Each line of training data has the the following format: word.pos | sense-id | prev-context %% target %% next-context

 word is the original form of the target word for which we are to predict the sense. You will use it to lookup the XML dictionary.

 pos is the POS where `n', `v', and `a' stand for noun, verb, and adjective, respectively.

 sense-id is the integer number for the correct sense id defined in our dictionary.

 prev-context is the text given earlier than each of the target word occurrence.

 target is the actual occurrence of the target word. Note that the word \begin.v" could occur as \beginning" instead of \begin" to denote a participle at the given position.

 next-context is the text given later than each of the target word occurrence.

Note that sense-ids in the test data are all erased to 0 as those are what you should predict.

1 Ontological (dictionary-based) WSD
Dictionary-based approaches utilize definitions given in the dictionary. See the following example that tries to disambiguate "pine cone".

 pine (the context)
1. a kind of evergreen tree with needle-shaped leaves
2. to waste away through sorrow or illness

 cone (the target word)
1. A solid body which narrows to a point
2. Something of this shape, whether solid or hollow
3. Fruit of certain evergreen trees

As bold faced in the above, 3rd sense of the target word matches the most with the 1st sense of the context word among all possible combinations. This process shows the original Lesk algorithm to disambiguate senses based only on the cross-comparing the definitions. However, rich examples given in the dictionary can be utilized to extend this model for better matching.
1. Design a metric that rewards consecutive overlaps more. One overlap of two consecutive words must get higher scores than two distant overlaps of a single word. Note that there would be morphological variations in the definitions and examples. To increase the matching, stemming or lemmatizing could be useful.3
2. Implement a dictionary-based WSD system that disambiguates the sense by comparing the definitions of the target word to the definitions of relevant words in the context. Your design decision of choosing relevant words will determine the performance of the
dictionary-based system in combining with the metric you designed above.
3. Because we mainly use glosses and examples in the dictionary to figure out the correct senses, no training is necessary for the Simple Lesk WSD. If you want to try the Corpus Lesk WSD (extension), try to augment the dictionary by the trainig data. If you think that those are not enough to achieve competitive accuracy, feel free to use the WordNet dictionary to further improve the performance.4
4. If no training process is involved, you could verify the performance of your Simple Lesk WSD system on the entire training set. If you want to compare the performance of various WSD systems like your Corpus Lesk or supervised WSD in the next section, test on the same validation set that we provide. You should also submit prediction results on the test data for every model that you would try.

2 Supervised WSD

2. The performance of your WSD system would rely more on how to generate feature vectors from the context. Note that target words are always provided within sufficiently long sentence(s). As the above example shows, extracting informative words from surrounding context allows the model parameters to discriminate unlikely senses from the correct sense. In our model, this process of deciding model parameters becomes training. You have to train a separate model per each target word in the training data.

3. When initially training your model, make sure that you never use the validation/test data. Note that the correct sense-ids given in the test data are deliberately erased to 0, which means those are no longer true labels. Instead of marking the predicted senses directly on the testing file, you must generate a separate output file consisting only of the predicted sense-ids, one id per line in each test data.

4. To achieve quality performance, smoothing is necessary. Implement either add-1 or add-Lambda smooothing. If you want to compare the performance of multiple models (e.g., different Lambda's), feel free to use a validation set, which is randomly reserved from the original test data. Since the true senses are alive in the validation data, testing on the validation set will let you guess the true performance on the unseen data. Note that you must not train on the validation set if you want to validate the performance. However, you can add the validation set to the training data for predicting the best senses of the test data.
