# Spam Classification

## Introduction
We may often receive some texts or emails that may seem like spam. The aim of this project is to classify some text as spam or not spam.

## Contents
1. [Technologies, Launching & Running the Project](#technologies--launching---running-the-project)
2. [Data](#data)
3. [Preprocessing](#preprocessing)
4. [Models](#models)
5. [Results](#results)
6. [Limitations](#limitations)

## Technologies, Launching & Running the Project
This project uses Python 3.6 on Jupyter Notebook. It makes use of sklearn and keras libraries.

Follow the instructions [here](https://jupyter.readthedocs.io/en/latest/install.html) to install Jupyter Notebook and Python3 if you do not have that on your machine. Please ensure you have the libraries sklearn and keras. If not, you will not be able to run the notebook. You can use `pip install package-name` or `conda install package-name` in your command prompt/terminal, depending on how you installed Jupyter Notebook.

To run the project, clone this repo into your local machine. On your terminal/command prompt, run: `git clone link-to-repo`.
After the repo has been cloned, run: `jupyter notebook`.
Open the notebook in the AI & Advanced Analytics folder and run all cells.

## Data
The dataset used is obtained from [here](https://archive.ics.uci.edu/ml/datasets/Spambase). It contains a total of 4601 instances, of which 1813 are classified as spam. It has a total of 57 features which are continuous values, and 1 binary class label. 1 represents spam, and 0 represents non-spam.
Of the 57 features, 48 features are `word_freq_WORD`, which is the percentage of words in the e-mail that match WORD (100 * (number of times the WORD appears in the email) / total number of words in email).  A "word" in this case is any string of alphanumeric characters bounded by non-alphanumeric characters or end-of-string.
6 features are `char_freq_CHAR`, representing the percentage of characters in the e-mail that match CHAR, which is 100 * (number of CHAR occurrences) / total characters in email.
`capital_run_length_average` represents the average length of uninterrupted sequences of capital letters, `capital_run_length_longest` represents the length of longest uninterrupted sequence of capital letters, and `capital_run_length_total` represents the total number of capital letters in the email.

## Preprocessing
The data does not have any missing values. We do not have to handle any missing values.

However, some columns are correlated. This introduces redundant features, and gives more weights to such features, which is something we do not want.
We find the features that have correlation greater than 0.85 and remove these features. The only feature found to have high correlation with other features is 'word_freq_415'.

We also note that the scale of the features are not similar. Some features have a range of values up to 15,841, while some features have a maximum value less than 10. This can affect models with algorithms that use distance or similarity measures, such as k-nearest neighbours (kNN) and support vector machines (SVMs). These models will give higher weights to features with larger magnitudes. However, even if we are not using such models, it is still good practice to scale the data.
To do this, we shall first split our data into train and test set. We then fit a MinMaxScaler to our train set, and transform it. Using the same fitted scaler, we transform our test set. We do this so our scaler is only based on the train set, minimising any leakage of information from the train set into the test set. This is also commonly done in real-world practices, where we get a new instance, and would like to predict the target value of that. We do not rescale the entire dataset when we get new data, but use the fitted scaler to transform the new instance.

## Models
Two models adopted in this project are Random Forests and Neural Networks.

Random Forest is an ensemble learning method. It constructs multiple decision trees during training and outputs the majority vote of the trees. We can also get the probability output. Decision trees have a tendency to overfit, and Random Forests introduce more variability in the learners and more randomness, reducing overfitting. In Random Forests, each decision tree uses a subset of the features, which is drawn randomly without replacement. Each tree is built fully without any pruning.

Neural Networks are modeled loosely after a human brain, and are designed to recognise patterns. They are able to detect complex patterns and can be used when the underlying structure of the data is unknown. They are modeled by adding layers of neurons, each taking in an input and giving out an output through an activation function.

## Results
The Random Forest Classifier produced an accuracy of 96.6%, with an ROC score of 99.2%.
The Neural Network has an average accuracy of 93.9%, and an average F1 score of 93.9%.
Both models performed well, as seen by the high accuracies obtained. There was also little overfitting and the results were not affected by the skew in the number of instances with each class.

## Limitations
However, there are also some limitations to the project. As in all Machine Learning problems that have low homogeneity, it would be better to have more data for the model to capture patterns that are representative of the population data. This dataset's collection of non-spam emails came from filed work and personal emails, and hence 'george' and the area code '650' are indicators of spam. This would be useful when constructing a personal spam filter but will not be applicable in a general purpose spam filter. A very wide collection of non-spam text will have to be available for such purposes.

Furthermore, the nature of the data that was used is not text data. It only takes into account the percentage of specific words and characters in the email. It does not include other words or characters and there is no understanding of the context of the text.







