<h1 class="tertiary-text-color"> The Disambiguator </h1>

## Summary

The Disambiguator is a bi-LSTM model designed to differentiate between the usage of `a` and `the` in a given text.
The primary objectives include practicing Named Entity Recognition (NER) and exploring the bi-LSTM architecture.

> NER is an NLP technique that involves identifying and categorizing named entities (such as persons, organizations, locations) in text.
> It plays a crucial role in extracting structured information from unstructured text, facilitating applications like information retrieval and question answering.

The model is evaluated using Charles Dickens' novel "A Tale of Two Cities."
For code and results, visit the project's repository: [Disambiguator Repository](https://github.com/JenMarks/disambiguation)


## Training Dataset 

To build the training dataset, we leveraged a Kaggle project containing Dickens' works in `.txt` format [Kaggle project](https://www.kaggle.com/fuzzyfroghunter/dickens).
The dataset is available in the `data` folder of our repository.
We replaced all instances of `a` and `the` with `XXX` and focused on disambiguating (classifying) `XXX`, mapping it to either `a` or `the` (ground truth labels). 
10 percent of the dataset was reserved for validation, and the model was trained on the remaining 90 percent.
The testing data is named `charles_dickens_test.txt` in the repository.

## Labels

After loading the dataset, we segmented it into sentences and tokenized each sentence. Tokens were labeled as follows:
* **Label 0**: Non-`XXX` tokens
* **Label 1**: `XXX` tokens corresponding to `a`
* **Label 2**: `XXX` tokens corresponding to `the`

## Model Description

The chosen model is a bidirectional LSTM with an embedding layer and softmax output.
The bidirectional LSTM considers information both before and after the token in question, which is crucial for effective classification.
For reproducing results, refer to the `main.py` file in the repository. Hyperparameters used for training:


| **Hyperparameter**     	 | **Value** 	  |
|:-------------------------|:------------:|
| Padded Sentence Length 	 | 200       	  |
| Embedding Dimension    	 | 50        	  |
| Optimizer              	 | Adam      	  |
| Learning Rate          	 | 0.001     	  |
| Batch Size             	 | 32        	  |
| Training Epochs        	 | 10        	  |


## Performance Report

The model demonstrated strong performance with a 99.98% tagging accuracy on the validation set and 96.86% on the test set.
Notably, when evaluating only the disambiguation of `XXX` (excluding class 0 predictions), the accuracy on the test set remained high at 91.30%.
It's essential to distinguish between tagging accuracy, which considers all classes, and disambiguation accuracy, which focuses specifically on the task of distinguishing between `a` and `the`.

## Final Notes

In future work, a more in-depth analysis of results would provide valuable insights into the model's performance nuances.
Experimentation with alternative model architectures, such as Transformers, should be pursued to enhance the understanding of the model's capabilities in disambiguating `a` and `the`.

Considering the sequential nature of language, Transformers, known for their attention mechanisms, could potentially outperform Bi-LSTM
in capturing long-range dependencies. Transformers excel at handling contextual information, which is crucial for disambiguating words like `a` and `the`
where context plays a pivotal role. Their parallel processing capability may offer advantages over the sequential processing inherent in Bi-LSTM, potentially leading to improved performance.

Exploring the use of pretrained models, such as [BERT](https://arxiv.org/abs/1810.04805), could be beneficial.
BERT, with its bidirectional contextual embeddings, has demonstrated exceptional performance on various NLP tasks.
Fine-tuning BERT on the specific disambiguation task could leverage its pre-learned language representations, potentially
yielding better results. However, careful consideration should be given to the trade-off between the model's complexity
and the size of the dataset, and complexity of the task, as using a pretrained model might be an overkill for smaller datasets.

In conclusion, these future directions aim to provide a more nuanced understanding of the model's behavior and improve its disambiguation capabilities.
Experimenting with advanced architectures and pretrained models could potentially elevate the model's performance, but
the choice should align with the specific characteristics and scale of the disambiguation task at hand.