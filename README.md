# SUMMARY
This page contains the ipynb file for knowledge extraction focusing on Named Entity Recognition (NER). Readme is a short summary of the assignment project.

The dataset mainly uses three entity type; PER (person), ORG (organisation), and LOC (location).

Before using NER extractor, token reconstruction was performed by manually setting the punctuation rules to create a full sentence. 
Then used the Hugging Face token-classification pipeline `dslim/bert-base-NER` to get the predicted span. 
Manually constructed a function for calculating the confusion matrix by calculating the true positive, false positive and false negative by comparing the gold span and the predicted span. 

One main issue that was found was that the model didn't consider consecutive tokens that are within the same span. For example, when LOC is consecutive, gold span considers them as one, whereas our baseline model splits them. Since our LOC evaluation scores are the lowest, we can improve by conducting a postprocessing to have one LOC span when it's consecutive.

Also manually set entity 'MISC' to be 'O (Other)' since the dataset doesn't use MISC. Used `AutoTokenizer` to modify the NER extractor to get the predicted tags that contains B- and I- (e.g. B-LOC, I-LOC), also converted the gold NER tags that are numeric to texts so we can match up with the predicted tags.
Realised that comma between the consecutive LOC tag made the LOC tag right after the comma to be a new starting LOC, whereas the gold dataset treats it as an inside tag. Manually set a function for fixing this issue on the predicted tags. 

As a result, the post-processing step raised the precision (37.5% → 79.1%), recall (45.6% → 74.2%) and F1-score (41.2% → 76.6%).

Remaining challenge: Album titles are considered as ORG on gold dataset, whereas our model doesn't. Contextual understanding could be the next challenge.
