# Once Upon A.I.
by Anton Haugen

### Sources 
-Pulled from Gutenberg.org using a Gutenberg API developed by Clemens Wolf

## Introduction
Inspired by the Brothers Grimm work in philology, a primarily 19th century field of study dedicated to creating language genealogies and finding language families and ur-languages, which eventually led to modern linguistics and thus natural language processing, I aimed to use a data science tool kit to understand story structures. However, also like the Brothers Grimm, I also wanted this body of research to have a strong commercial component. Thus, I decided to create a recommendation engine for public domain short stories. Not only would this service provide busy parents a way of finding the perfect bedtime story when they can only remember the plot of Emily in Paris, the resulting consumer insights would be valuable for narratology researchers and of course, streaming services like DisneyPlus and Netflix who could use the data to develop new original programs. The webapp urstory can be found at https://urstoryproject.appspot.com/

## Data

Initially 1,500 books pulled from Gutenberg.org using a Gutenberg API developed by Clemens Wolf, after data cleaning this turned into over 20,000 stories spanning the late 18th to early 20th century. Most texts that would be problematic by today's standards were not pulled. Chunking methods and cleaning functions were employed as well as LDA topic modeling to highlight keywords in problem texts.

## EDA

Once an initial round of data-cleaning was completed, I preprocessed the text using spaCy to find word frequencies across the text. Surprisingly, 'come' and 'little' were more frequent than any noun.
![Image](Images/word_frequency.png?raw=true)

I also looked at noun frequencies as well as verb frequencies. The noun frequencies helped give an idea of the subject matter across the texts while the verb frequencies helped give me an idea of how I could eventually model and visual plot structure down the road. 
![Image](Images/noun_frequency.png?raw=true)
![Image](Images/verb_frequency.png?raw=true)

It was no surprise that male nouns were employed more than female nouns. Of course, this does not give an idea of subject, but does show how frequently an anonymous third-person male figure is used in the text. It's worth noting that "mother" appears prior to "woman" and "father" does not appear. In a future iteration of this project, TF-IDF could be performed on verbs to highlight plot elements. The frequency does give an idea of how these children's stories are stylized and structured.

## Feature Engineering/Preprocessing
spaCy was used for tokenization and lemmatization, alongside GenSim's doc2Vvec to topic model the data and produce keywords to pursue further data cleaning.
The baseline recommendation model used Scikit learn's TF-IDF Vectorizer to weight the terms within the corpus.
Primarily the recommendations for "The Ant and the Grasshopper" story by Aesop were used to evaluate the recommendations, among other stories. 
The Aesop story follows this structure: It’s almost winter. Ants are carrying food down the road when they encounter a grasshopper. Because he had spent the summer playing the fiddle and singing, he asks them for food. They reply, “For winter, dance!”

## Modeling
TF-IDF based recommender measure cosine similarity between n-grams ranging from 1 to 3 tokens. Although the model was light weight and did not consume much memory, the recommendations were often disappointing and predictable. For example, many just featured a wise talking grasshopper who advised a sad child named Billy through his various pitfalls.  The best recommendations feature the juxtaposition of productivity and sloth or in the case of one story featuring a trapped priest who plays the fiddle to summon help, provided a nice counterpoint to the original tale.

![Image](Images/tfidf_word_cloud.png?raw=true)

The spaCy model allowed for more nuance in plot and even included the best examples from TF-IDF Model. It allowed for a more diverse and surprising body of recommendations. In addition, it allowed for a more query function that could handle words outside of the corpus. Below is a word cloud generated from word frequency of the top 100 recommendations for the "Ant and Grasshopper" story, which resembles 

![Image](Images/spacy_word_cloud.png?raw=true)


### Querying Recommendations
The recommendation avoids the cold start problem by immediately involving the user. Queries also help avoid the cold start problem and add more surprise to the dataset,  though the functionality needs improvement.


I compared the performance of two elements from "the Ant and Grasshopper" story, the grasshopper begging for food and the ants dismissing the grasshopper. Both returned the Ant and Grasshopper story, though the performed better than the latter. 
![Image](Images/grasshopper_query.png?raw=true)
![Image](Images/antsquery.png?raw=true)

For now the queries need to be plot elements. I tried to query "Aladdin" and received primarily bibliographic material that need to be removed from the dataset. When I queried "Aladdin has three wishes granted by a genie" it returned the Aladdin story, among some recommendations for "Cinderella," which I did not initially think as related to "Aladdin," but now the relationship seems evident.

![Image](Images/aladdinquery.png?raw=true)
![Image](Images/aladdinsummaryquery.png?raw=true)

## urStory WebApp
Built on Flask and hosted on Google Cloud, for every query the app gives 12 choices from a randomized selection of the top 36 results. Strong choices are shuffled with weaker choices to give more diversity.Test users enjoyed using it and said it was fun. One user tried “girl falls in love with another girl” and got a story on maternal love.  


## Conclusions
The final recommendation engine performs well, but there are still a number of issues that will be addressed as this project continues. In the future, I would like to customize the spaCy pipeline more and introduce custom attributes and labels like 'conflict', 'hero', and 'villain' to improve query functionality. I would also like to improve the design of the web app and improve formatting of textsWith more user data, I can eventually use a more concrete metric for improving the recommendation engine. I also plan to use this corpus to eventually train a text generation neural network.

