# Sentiment-Analysis-using-RNN
Sentiment Analysis using RNN
Application of RNN for customer review sentiment analysis

Downloading the data - 
Downloading the data from kaggle.	link-https://www.kaggle.com/datafiniti/consumer-reviews-of-amazon-products/version/2

About this data - This is a list of over 1,500 consumer reviews for Amazon products like the Kindle, Fire TV Stick, and more provided by Datafiniti's Product Database. The dataset includes basic product information, rating, review text, and more for each product.
Loading and preparing the data

As a starting point, I loaded a csv file containing 1,500 customer reviews in English with the corresponding rating on the scale from 1 to 5, where 1 is the lowest (negative) and 5 is the highest (positive) rating. Here is a quick glance at the data frame:
However, as our goal is to predict sentiment — whether review is positive or negative, we have to select appropriate data for this task.
To balance it out and to ensure a good representation of the sentiment classes, I decided to keep 5 star reviews for “positive”, while 1 and 2 star reviews for “negative” sentiment.
Prior to processing the reviews, the sentiment should be binary encoded with 1 for positive and 0 for negative sentiment using list comprehension.Prior to processing the reviews, the sentiment should be binary encoded with 1 for positive and 0 for negative sentiment using list comprehension.
 
Data preprocessing
RNN input requires array data type, therefore, we convert the “Reviews” into the X array and “Sentiment” into the y array accordingly.
Text data has to be integer encoded before feeding it into the RNN model. This can be easily achieved by using basic tools from the Keras library.
First, the text should be tokenized by fitting Tokenizer class on the data set. As you can see I use “lower = True” argument to convert the text into lowercase to ensure consistency of the data. Afterwards, we should map our list of words (tokens) to a list of unique integers for each unique word using texts_to_sequences class.
Next, we use pad_sequences class on the list of integers to ensure that all reviews have the same length, which is a very important step for preparing data for RNN model. Applying this class would either shorten the reviews to 100 integers, or pad them with 0’s in case they are shorter.

Now, we split the data set into training and testing using sklearn’s train_test_split and keeping 25% of original data as a hold out set.
Furthemore, the training set can be split into training and validation set.
The model is built and the training data is fitted using Keras.
As embedding requires the size of the vocabulary and the length of input sequences, we set vocabulary_size at the number of words in Tokenizer dictionary + 1 and input_length at 100 (max_words), where value of the latter parameter must be the same as for padding(!). Embedding size parameter specifies how many dimensions will be used to represent each word. Normally one uses the values 50, 100 and 300 as an input for this parameter, but while tuning the model value 32 delivered the best result in this case.Next, we add 1 hidden LSTM layer with 200 memory cells. Potentially, adding more layers and cells can lead to better results.Finally, we add the output layer with sigmoid activation function to predict a probability of a review being positive.
After training the model for 10 epochs, we achieve an accuracy of 68.75%  on validation set and 73.5% on test (hold out) set.

