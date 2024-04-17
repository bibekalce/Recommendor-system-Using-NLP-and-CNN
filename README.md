# Recommendor-system-Using-NLP-and-CNN
Dataset: (https://www.kaggle.com/datasets/airbnb/seattle)

The Seattle Airbnb Dataset, procured from Kaggle, was selected for its comprehensive
insights into homestay listings in Seattle, WA, USA. This dataset includes property descriptions, ratings, user reviews, and additional features, enabling a thorough understanding of user requirements and property features for personalized recommendations.


“Research_pipeline_0_Filtering_Out_Complete_Non_English_Reviews” is designed to preprocess a dataset of textual data, including both user reviews and property descriptions, with the aim of detecting and filtering out complete non-English reviews. Property descriptions, often containing both English and non-English segments, remain untouched as they typically include English content. However, this is not consistently the case with user reviews.
Leveraging the NLTK library for text processing and the langid library for language detection, the pipeline accurately identifies the language of each review. This enables effective filtering out of complete non-English reviews, followed by manual verification. The user IDs corresponding to these non-English reviews are then retained for further analysis. To ensure thoroughness, the process is iterated multiple times to handle cases where non-English reviews may contain partial English content beneficial for topic modeling. The identified user IDs are then utilized to filter out all non-English reviews from the dataset. The langid library achieved an accuracy rate of 89.6% in this process.
Subsequently, the dataset is prepared for topic modeling, ensuring that only English reviews are retained. Adhering to this organized pipeline guarantees that our dataset for topic modeling consists entirely of English-language content, thereby facilitating a more accurate and insightful analysis of the underlying topics within the reviews.



“Research_pipeline_1_Topic_Modelling(LDA/NMF)”  pipeline utilizes Latent Dirichlet Allocation (LDA) and Non-negative Matrix Factorization (NMF) on data extracted from a filtered dataset, ensuring the exclusion of non-English content. These algorithms perform topic modeling to discover underlying themes, aiming for better representation of both users and properties. Initially, LDA and NMF operate without seeding to independently identify latent topics. Subsequently, the discovered topic words act as seeds for further iterations of topic modeling. Through iterative seeding, topic word detection, and manual refinement, ambiguous or irrelevant topic words are identified and refined to enhance the interpretability and relevance of the discovered topics.
Furthermore, the pipeline conducts topic word prediction on test and validation datasets to gauge topic variation across subsets. However, these predictions do not influence the independent refinement of topic words, ensuring consistency and integrity.

“Research_pipeline_2:Data_tranformation” leverages previously identified topic words to transform user reviews into user profiles and property descriptions into property profiles. The pipeline consists of several components, each with specific tasks:

	Word2Vec Models: These models, trained on user reviews and property descriptions, enrich profiles by providing synonyms, antonyms, and corresponding similarity scores.

	Hybrid Sentiment Analysis: A hybrid approach combines sentiment polarity scores from Vader, scaled Afinn, and TextBlob. The hybrid sentiment score is computed as the sum of these individual scores.

	Transformation: User reviews are tokenized into sentences, and hybrid sentiment scores are computed. This involves identifying sentences containing topic words. 

The following assumptions are employed to create the profile:

•	If a topic word is detected, the hybrid sentiment score represents the user's stance towards that topic word.

•	If no topic words are found, similar words are sought, and the sentiment score for those words is computed. The profile is populated with this sentiment score multiplied by cosine similarity.

•	If neither the topic word nor similar words are detected, the profile is assigned a sentiment score of zero, indicating neutrality towards that feature.

Similar processes are applied to create property profiles from property descriptions. The sentiment score effectively represents the user's stance towards the topic word in the property context.


“Research_final_pipeline_benchmark/Proposed_model” pipeline comprises two main models: the Singular Value Decomposition (SVD) model and the Proposed Model, alongside a Baseline Model for comparison. Both the proposed and baseline models utilize transformed profiles.
	Singular Value Decomposition (SVD) Model:
•	Trained using user-property interactions with latent features set to 150.
•	Property popularity measured using property frequency within the training dataset and scaled to [0,1].
•	During the evaluation, the SVD model incorporates a scaled property popularity dictionary and testing/validation data to handle new users by recommending top properties from the item popularity dictionary.

	Baseline Model:
•	Utilizes only the user profile for training, along with the user-property interaction matrix as ground truth.
•	Employs a CNN architecture to capture intricate relationships between users and properties.
•	Output layer utilizes softmax activation to compute interaction probability for 3818 properties.
•	Training involves backpropagation to adjust feature weights, minimizing categorical cross-entropy loss to improve accuracy in user preference classification and recommendation generation.

	Proposed Model:

•	Shares the same layer architecture as the baseline model but integrates feature-engineered inputs.
•	Feature engineering technique employed is User-Property Profile Fusion, achieved through elementwise multiplication of user and property profiles within the model architecture.
•	During fusion, each batch combines reshaped user profiles with all reshaped property profiles through element-wise multiplication, effectively capturing interactions between user preferences and property features.
•	Utilizes backpropagation for optimization of feature weights, minimizing categorical cross-entropy loss to learn patterns and dependencies inherent in user and property profiles, enabling accurate prediction of user-property interactions.
