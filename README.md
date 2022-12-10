
- Manas Angalakuduti
- Soham Kar
- Joshua Reid
- Wendy Sun

# Designing a Personified Chatbot to Advise College Students

## Introduction
Natural language processing (NLP) is a branch of computer science situated at the intersection between artificial intelligence and linguistics, encompassing computational methods that allow machines to process and generate representations of human language (Cambria & White, 2014). One of the most recent developments in practical applications of NLP is allowing conversations between a human and a machine to take place in the form of chatbots, which are conversational agents that generate outputs resembling human language in response to inputs made in human language (Lalwani et al., 2018).
One of the most significant issues encountered during the implementation of modern chatbots is their frequent inability to generate multiple responses that are consistent in personality due to the chatbot being trained on extremely large datasets containing many different speakers and responses. In this paper, we will detail our work of designing and implementing a chatbot model that strives to alleviate this issue within the context of conversing with college students. 
Our chatbot concept builds off of previous work done by Lalwani et al. (2018) on implementing a chatbot system capable of addressing college-related inquiries of website users. The design of our chatbot differs by placing a greater emphasis on responding to concerns specific to college students that arise within the collegiate environment as opposed to responding to inquiries by users from all backgrounds about the collegiate institution itself. With college students being at higher risk of suffering from loneliness and anxiety, especially during periods of intense stress, the goal of our chatbot is to act as a resource that engages in thoughtful conversations with college students as needed. With this goal in mind, we hope to contribute to an unexplored application of NLP chatbot systems by designing a chatbot model that acts as an important resource for a specific, relevant target demographic.

## Background
Prior to designing a chatbot system capable of accomplishing our goals, we first conducted a literature review of research related to our project. Specifically, we reviewed related work on the implementation of chatbots and their limitations in order to cultivate a strong foundation for our own project.
Within the broad scope of NLP research, chatbot models reside within what Cambria and White (2014) defined as Statistical NLP–a range of language models that utilize statistical methods and machine learning algorithms to analyze and generate human language through repeated training on large text corpus datasets. Specifically, Lalwani et al. (2018) defined the chatbot model as “an artificially intelligent creature [that] can converse with humans”(p. 1). In the theoretical chatbot model, Lalwani et al. stated that the machine is tasked with processing a statement made by the human and generating a logical response by drawing from “a predefined knowledge base” (p. 1). The conversation is prolonged through the repeated back-and-forth exchange between the human and the machine, with the end goal of simulating human-to-human communication by having the machine mimic human speech.
While research on chatbot models is relatively new and carries exciting implications for the future of NLP research, chatbot models face certain implementation challenges that limit its applicable potential. For instance, Li et al. (2016) detailed the frequent inability of modern chatbot models to embody a consistent personality over the course of a conversation because they are trained on large datasets consisting of many different speakers, causing them to generate consensus responses that may not apply to the current conversation. In addition, Li et al. (2015) found that modern chatbot models also suffer from a tendency to generate generic responses such as “I don’t know,” once again as a result of these models being trained on large datasets that cause a standardization in responses by ranking nonspecific, insignificant responses higher than specific, nuanced responses. These issues weaken the machine’s mimicry of thoughtful human speech, leading to a less engaging conversation with the human.
Zhang et al. (2018) alleviated these problems in their work by “endowing [a chatbot model] with a configurable, but persistent persona” in the form of a profile that enables the chatbot to produce consistent responses aligned with a set personality (p. 1). Each chatbot model’s profile was refined by Zhang et al. through repeated training using the PERSONA-CHAT dataset–a text corpus consisting of 162,064 remarks exchanged between crowdworkers who were each randomly assigned a personality (persona) and tasked with getting to know each other by acting in accordance with their assigned persona. After the profile had been refined through many training iterations, Zhang et al. stated that the profile “[could] be stored in a memory-augmented neural network,” which allowed the chatbot using the profile to generate responses consistent with the profile’s trained personality, better resembling conventional human interaction (p. 1).       

## Methods
The design of our chatbot model drew inspiration from the works of both Lalwani et al. (2018) and Zhang et al. (2018). More specifically, we wanted to design our chatbot model to expand on Lalwani et al.’s concept and incorporate Zhang et al.’s work on imbuing chatbots with consistent personalities. Overall, we designed our chatbot model with the goal of eventually enabling it to interact with college students on a variety of different topics.

In order to infuse our chatbot model with a personality, we first needed to find a dataset for our model that was similar in nature to the PERSONA-CHAT dataset used by Zhang et al. (2018). We settled on using the “3 turns per line without topic words” version of the Reddit Conversation Corpus (RCC), which contained data from 95 subreddits (Dziri & Ehsan, 2020). Afterward, we converted the dataset from 3 turns per line to 1 turn per line in order to fit our specific implementation. The final size of our dataset was 9.2M for the training set and 812K for the evaluation set. 

For the implementation of our chatbot model, we first loaded our version of the RCC dataset, which consisted of 812,246 training conversation pairs and 100 evaluation conversation pairs. We then generated a vocabulary consisting of 140,706 words and tokenized all of the sentences in the vocabulary. Afterward, we implemented a Seq2Seq baseline model consisting of a bidirectional GRU encoder and a GRU decoder, with an embedding dimension of 300, a hidden dimension of 300, 2 layers, and a dropout rate of 0.1. In addition, we also implemented a Seq2Seq model with attention with the same parameter values. Once the two models were implemented, we created a data loader with a batch size of 64 and 4 epochs. We proceeded to train and evaluate both of the models using our modified version of the RCC dataset.  

In order to interface with our chatbot, we also implemented code that enabled the chatbot to listen to messages sent from other users in a GroupMe chat and then share its generated responses with the chat. The basic functionality of the code allowed our chatbot to fetch the most recent message sent to a GroupMe chat, analyze its content, generate a response, and then share that response to the GroupMe chat. As such, we were able to communicate back and forth with our chatbot through a shared GroupMe chat containing the bot and our group members. 

## Results
Training and evaluation of the Seq2Seq baseline model on the RCC dataset completed with a mean loss of 4.82, 4.41, 4.27, and 4.18 for the four epochs respectively. Training and evaluation of the Seq2Seq attention model on the RCC dataset completed with a mean loss of 4.81, 4.36, 4.17, and 4.04 for the four epochs respectively.
In order to further evaluate our chatbot implementation’s performance on our modified version of the RCC dataset, we also trained and evaluated our implementation on the Cornell Movie-Dialogs Corpus (CMDC) dataset provided by Danescu-Niculescu-Mizil and Lee (2011), which consisted of 53,065 training conversation pairs, 100 evaluation conversation pairs, and 7,727 total words in its vocabulary. Training and evaluation of the Seq2Seq baseline model on the CMDC dataset completed with a mean loss of 3.54, 3.02, 2.84, and 2.7 for the four epochs respectively. Training and evaluation of the Seq2Seq attention model on the CMDC dataset completed with a mean loss of 3.48, 2.96, 2.73, and 2.53 for the four epochs respectively. Figure 1 below illustrates the differences in mean loss between the RCC dataset and the CMDC dataset for both the baseline model and the attention model across the four epochs.

<img align="center" alt="graph" width="500px" src="https://github.com/joshuarreid/University-Neural-ChatBot/blob/main/image2.png?raw=true" />

### Figure 1: Line graphs illustrating differences in the trends in mean loss for the baseline and attention models between the RCC dataset and the CMDC dataset.   
 
In order to examine the differences that occur in our chatbot implementation when it is trained on different datasets, we asked our chatbot the same set of questions when it was trained on the RCC dataset and when it was trained on the CMDC dataset. Figure 2 below illustrates the responses generated by our chatbot for both datasets.


<img align="center" alt="graph" width="500px" src="https://github.com/joshuarreid/University-Neural-ChatBot/blob/main/image1.png?raw=true" />

<img align="center" alt="graph" width="500px" src="https://github.com/joshuarreid/University-Neural-ChatBot/blob/main/image3.png?raw=true" />

### Figure 2: The responses generated by our chatbot implementation for the same set of questions, when trained on the RCC dataset (left) and when trained on the CMDC dataset (right).

## Discussion
The results of the training and evaluation of our chatbot implementation reveal that the modified version of the RCC dataset that we used was not very effective at generating sensible, personality-driven responses in our chatbot. While our chatbot’s responses were at the very least comprehensible when trained on the RCC dataset, the responses were often unusual and reminiscent of the consensus responses detailed in the work of Li et al. (2016). However, when trained on the CMDC dataset, our chatbot exhibited more personality by producing specific responses that answered the questions being asked instead of “dodging the question” by producing an adjacent but nonspecific response like with the RCC dataset. This disparity between the datasets is likely the result of the CMDC dataset being much smaller in size than the RCC dataset, which ties back to Li et al.’s findings that using bigger datasets for chatbot models is more conducive to the chatbot learning general responses due to the large number of different speakers and potential responses found in bigger datasets.

There are a couple of changes we would make to our implementation if we had more time to work on it. First, we would spend more time curating our datasets to best fit our implementation goals; we would utilize datasets that are not very large and contain less diversity in their speakers to avoid our model learning consensus responses. In addition, we would train our model on a larger number of datasets in order to get more points of comparison for the evaluation of our model, which would help us develop a better understanding of the types of datasets that work best with our implementation. For example, we would explore datasets such as r/collegeadvice, r/gatech/, etc. that contain topics more similar to the context we want our chatbot to operate in. Lastly, we would also conduct hyperparameter tuning in order to increase the accuracy of our model such that our chatbot produces more specific responses when it is asked questions. These adjustments to the implementation of our chatbot model would help us get closer to our initial goals for the chatbot by having it generate responses that are more specific, more personalized, and more engaging for the human to read and respond to as the conversation unfolds.
References

Cambria, E., & White, B. (2014). Jumping NLP curves: A review of natural language processing 
research. IEEE Computational intelligence magazine, 9(2), 48-57. Retrieved from https://ieeexplore.ieee.org/abstract/document/6786458

Danescu-Niculescu-Mizil, C., & Lee, L. (2011). Chameleons in imagined conversations: A new 
approach to understanding coordination of linguistic style in dialogs. Retrieved from https://convokit.cornell.edu/documentation/movie.html

Dziri, N., & Ehsan. (2020). Topical hierarchical recurrent encoder decoder [GitHub Repository]. 
https://github.com/nouhadziri/THRED

Lalwani, T., Bhalotia, S., Pal, A., Rathod, V., & Bisen, S. (2018). Implementation of a Chatbot 
System using AI and NLP. International Journal of Innovative Research in Computer Science & Technology (IJIRCST) Volume-6, Issue-3. Retrieved from https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3531782

Li, J., Galley, M., Brockett, C., Gao, J., & Dolan, B. (2015). A diversity-promoting objective 
function for neural conversation models. arXiv preprint arXiv:1510.03055. Retrieved from https://arxiv.org/abs/1510.03055

Li, J., Galley, M., Brockett, C., Spithourakis, G. P., Gao, J., & Dolan, B. (2016). A persona-based 
neural conversation model. arXiv preprint arXiv:1603.06155. Retrieved from https://arxiv.org/abs/1603.06155

Zhang, S., Dinan, E., Urbanek, J., Szlam, A., Kiela, D., & Weston, J. (2018). Personalizing 
dialogue agents: I have a dog, do you have pets too?. arXiv preprint arXiv:1801.07243. Retrieved from https://paperswithcode.com/paper/personalizing-dialogue-agents-i-have-a-dog-do
