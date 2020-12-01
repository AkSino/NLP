# Scrapping

We have 55 questions/answers on NLP. They come from these 2 sites:

-https://www.onlineinterviewquestions.com/nlp-interview-questions/
-https://www.i2tutorials.com/nlp-interview-question-answers/nlp-interview-questions-part-1/

# Data Preparation

To use BERT, we had to make the answers more beautiful, ie for example replacing "it" by the subject of the question. Indeed in our dataset, we only put the answers to the questions,"it" did not correspond to anything because the subject was not in the dataset.

Since BERT is forced to use a maximum of 512 tokens (after the tokenization of the dataset + question), we have to split the dataset into several datasets that BERT will analyze to find the most suitable answer to the question asked.

# First mode: Question/Answering system with BERT:

BERT is a method of pre-training language representations, meaning that we train a general-purpose "language understanding" model on a large text corpus (like Wikipedia), and then use that model for downstream NLP tasks that we care about (like question answering). BERT outperforms previous methods because it is the first unsupervised, deeply bidirectional system for pre-training NLP.

Using BERT has two stages: Pre-training and fine-tuning.

Pre-training is fairly expensive (four days on 4 to 16 Cloud TPUs), but is a one-time procedure for each language (current models are English-only, but multilingual models will be released in the near future). We are releasing a number of pre-trained models from the paper which were pre-trained at Google. Most NLP researchers will never need to pre-train their own model from scratch.

Fine-tuning is inexpensive. All of the results in the paper can be replicated in at most 1 hour on a single Cloud TPU, or a few hours on a GPU, starting from the exact same pre-trained model. SQuAD, for example, can be trained in around 30 minutes on a single Cloud TPU to achieve a Dev F1 score of 91.0%, which is the single system state-of-the-art.

We use a pretrained model: BERT-Large, Uncased: 24-layer, 1024-hidden, 16-heads, 340M parameters

Tokenization:

For sentence-level tasks (or sentence-pair) tasks, tokenization is very simple. Just follow the example code in run_classifier.py and extract_features.py. The basic procedure for sentence-level tasks is:

Instantiate an instance of tokenizer = tokenization.FullTokenizer

Tokenize the raw text with tokens = tokenizer.tokenize(raw_text).

Truncate to the maximum sequence length. (You can use up to 512, but you probably want to use shorter if possible for memory and speed reasons.)

Add the [CLS] and [SEP] tokens in the right place.

Word-level and span-level tasks (e.g., SQuAD and NER) are more complex, since you need to maintain alignment between your input text and output text so that you can project your training labels. SQuAD is a particularly complex example because the input labels are character-based, and SQuAD paragraphs are often longer than our maximum sequence length. See the code in run_squad.py to show how we handle this.

Before we describe the general recipe for handling word-level tasks, it's important to understand what exactly our tokenizer is doing. It has three main steps:

Text normalization: Convert all whitespace characters to spaces, and (for the Uncased model) lowercase the input and strip out accent markers. E.g., John Johanson's, → john johanson's,.

Punctuation splitting: Split all punctuation characters on both sides (i.e., add whitespace around all punctuation characters). Punctuation characters are defined as (a) Anything with a P* Unicode class, (b) any non-letter/number/space ASCII character (e.g., characters like $ which are technically not punctuation). E.g., john johanson's, → john johanson ' s ,

WordPiece tokenization: Apply whitespace tokenization to the output of the above procedure, and apply WordPiece tokenization to each token separately. (Our implementation is directly based on the one from tensor2tensor, which is linked). E.g., john johanson ' s , → john johan ##son ' s ,

## Running:

You have to just ask your question and it is going to look for the best answer in dataset. If it doesnt find a correct answer, it responds: "Sorry i dont understand the question"
If you dont have an idea of question, algorithm propose to you prebuild questions.
You can enter "q" or "question" to ask standard question and you can enter e or exit to end the discussion

# Second mode: Question/Answering system with DIALOGFLOW:

We put the web scrapping data in a csv file (question / answer) then in dialogflow we create a knowledge base with that. It will then look among the questions for the one that most closely resembles yours and then send you the answer.

## Running

Really easy to use: Open index.html in your browzer and then ask you question.


