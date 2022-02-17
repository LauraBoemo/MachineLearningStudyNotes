## AWS STP: Machine Learning (ML) on AWS for ML Practitioners (Technical)
<br>

---
## The modules...
1. Introduction to Machine Learning
2. Artificial Intelligence Services on AWS
3. Machine Learning Process
4. Data Collection, Integration, Preparation and Visualization, and Analysis
5. Deep Learning Amazon Machine Images
6. Amazon SageMaker Concepts
7. Amazon SageMaker Notebooks
8. Amazon SageMaker Built-In Algorithms
9. Amazon SageMaker - Debugging and Monitoring
10. Introduction to MLOps
11. Next Steps and Additional Learning

---

<br>

## Module 1: Introduction to Machine Learning
---
### Machine Learning, Artificial Intelligence, and Deep Learning

Artificial Intelligence is a collection of various different technologies, different disciplines that comes together. The key work for AI is "mimic human intelligence".

Machine Learning is a subset of AI, it is about using machines to search for patterns in data, building logic models from a very large dataset. Apply statistics to solve business problems (can't solve using only "if" and "then" type of rules). 

Deep Learning is a subset of ML (a subset of AI), is an expansion of ML, solve problems that ML cant solve.

---
### Solving problem: Traditional programming or Machine Learning

In Traditional Programming you write the business rules, with Machine Learning, the data writes the business rules.

Use ML when...
1. You can't code it: complex tasks where deterministic solutions don't suffice (Eg., Recognizing speech/images);
2. When you can't scale it: replace repetitive tasks needing human-like expertise (E.g., recommendations, spam, fraud detection, machine translation);
3. When you have to adapt/personalize: E.g., recommendation  and personalization;
4. When you can't track it: E.g., automated driving;

ML timeline...<br>
    - 1950 to 1998 > Intense academic activity {
        <br>1986 > Backpropagation algoritm
        <br>1998 > Long short-term memory (LSTM) and convolutional neural networks<br>
    }
    <br>- 2998 to 2007 > Neural winter {
        <br>2007 > GPU training
    <br>}
    <br>- 2017 > The GPU era 

--- 
### Types of Machine Learning

There are tree types...
1. Supervised learning
    <br>Similar how humans learn: Human intervention and validation required; <br>e.g., photo classification (input. e.g., photo of a dog) + tagging (label. e.g., "Labrador" label) = training data --> Machine Learning Algorithm --> Prediction ("Cat"). In this case, we compare our prediction with the label, cat = dog? So, cat is not equal as dog, and then we go back to our ML algorithm and adjust its model.<br><br>Two types of algorithms
    <br>1.1. Regression;<br>- E.g., estimating tag price. A regression algorithm is applicable for predicting outcomes when the input falls within a numeric range.  Common use cases include house price prediction and demand forecasting. For example, you could use a regression algorithm to forecast product demand during Christmas.<br>1.2. Classification;<br>- E.g., Disting in categories, bynary  and multyclass clasifications for ex.<br><br>
2. Unsupervised learning
    <br>When we don't have any label data. The ML Algorithm looks to the parameter (usually, it is a graphic) and then do predictions, which is placing things into specific groups. It is not comparable..<br><br>One type of algorithms
    <br>2.1. Grouping/Clustering
3. Reinforcement learning
    <br>Where you train an agent. The agent take actions inside and environment, it returns to the agent its states and rewards. Train robots to walk etc<br><br>One type of algorithms
    <br>3.1. Correct actions rewarded

---
### Key Machine Learning terminology and concepts
<br>Label/Target is a dependent variable that youre trying to predict
<br>Feature is a independent variable that helps you make predictions
<br>Feature Engineering is a data transformation, the process of reshaping data to get more value out of it
<br>Feature Selection is a variable/subset selection process of using the most valuable data 

<br>Common use cases for AI
- Marketing
- Finance
- Transportation
- Medical
- Energy
- Manufacturing
- Government
- Legal
- Finance

<br>Example of use cases: <br>
Image Recognition > Amazon Rekognition<br>
Translation and transcription > Amazon Translate and Amazon 
- Obs.: You don't need any API. AWS already provide everything for you...<br>

Text analysis > Amazon Comprehen (understand the key words - there is a location? name? type?)

---
### Challenges and limitations of AI
DATA: If you put bad data, you will get bad predictions. It fuels ML, so garbage-in/garbage-out still applies<br>
BIAS: Poor predictions and decisions can be traced back to biased data<br>
EXPLAINABILITY: How and why the model makes certain predictions<br>
NARROW AI: Lack of generalized AI, where the current AI can only perform specific tasks within a narrow domain<br>
ALGORITHM TRANSPARENCY: Ability of ML Models to take learning from one arena and apply to another<br>
EMOTIONAL INTELLIGENCE: Basic human trait is still a challenge in ML
<br>
<br>
Biases in ML that we need to be aware of and deal with, such as...
- Sample bias (balance your data, dont exclude no one), Prejudicial bias (data might not be valid/corret in regards to gender, community, income), Exclusion bias (feature selections taking away some features, dont take away important bias), Measurement bias (data collecet from training is different than the data collected from production), Algorithmic bias (th wrong choice of algorithmic)
- data preparation is the keuy to avoid bias, AmazonSageMakerClarify design to help developers to solve bias in AI
- Responsible AI: harnessing the power of AI in an ethical, fair, and responsible manner with full transparency, accountability and freedom from bias.
- Data lineage tracks: the origin of the data, what happens to it, where it moves over time during data anaytics processes
- Reproducibility/Auditability: Any results should be documented by making all data and code available in such a way that computations can be run again with identical results
- Data lineage, reproducibility, and auditability are important to investigate potentia bias in data and to ensur transparency
---
### Case study: Insurance Fraud use case