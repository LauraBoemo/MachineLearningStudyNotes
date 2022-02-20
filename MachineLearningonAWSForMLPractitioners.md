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

<br>
<br>
<br>


## Module 2: Artificial Intelligence Services on AWS
---

<br>

---
### Amazon Machine Learning Stack
- Our misison at AWS<br><br>
Put machine learning in the hands of every developer. Make it easy to all developers.<br><br>

- Why AWS for ML?<br><br>
    Broad and deep set of ML and AI services
    - 200+ new features and services launched within the last year
    - Solutions for everyone from ML scientists to application developers
    - Support for all three major frameworks
    <br>

    Machine learning with Amazon SageMaker
    - Single integrated development environment (IDE) for the entire ML workflow
    - At least 54% lower total cost of owneership (TCO) 
    - In the last 2 years, 10.000 customers have adopted Amazon SageMaker
    <br>

    Comprehensive Cloud Offerings
    - Highly secure, reliable, fully featured data store
    - Strong set of compute, storage, security, and analytics capabilites to build on
    - 85% of TensorFlow in the cloud runs in the AWS Cloud

- Amazon ML Stack<br><br>
    More deeper you go in this stack, more flexibility you get to your modules. More up, more acessible.


    AI Services: addressed to business problems, developers only point their input in a API for each services and recieve in return an specific output, which is the prediction (AI services dont require any expertise) - modules pre-trained
    - Vision
    - Speech
    - Text
    - Search
    - Chatbots
    - Personalize
    - Forecast
    - Fraud
    - Development
    - Contact Centers
    <br>

    ML Services: for data scientis, to work directaly on the frameworks - train your modules
    - Amazon SageMaker
    - Ground Truth 
    - AWS Marketplace for ML
    - SageMaker Studio IDE
    - Neo
    - Augmented AI
    <br>

    ML Frameworks and infrastructure
    - Tensorflow, MXNext, PyTorch
    - Gluon, Keras, DeepGraphLibrary
    - Deep Learning, AMIs and Containers
    - GPUs and CPUs
    - Elastic Inference
    - Inferencia
    - FPGA

    <br><br>
- Amazon ML Stack - AI Services<br>
Pré-trained modules adressed to business problems.<br><br>
    - Amazon Rekognition
        - Function: Automate image and video analysis with ML
        - Use Cases: Media analysis, Identity verification, Content moderation
        - Key Features
            - Labels: Custom Labels (used on Tinder, like you put and image biking, theu suggest put "biking" on your bio)
            - Content moderation: Text detection (avoid violence images on social media, for example)
            - Facial detection: Face search and verification (someone is happy or unhappy, or looking for specific persons)
            - Celebrity recognition: Pathing
    - Amazon Textract
        - Function: Extract any text data from any document using machine learning and without manual effort
        - Use Cases: Create smart search indexes, Build automated document processing workflows, Maintain compliance documents archives
        - Benefits
            - Extract structured and unstructured data
            - Go beyond simple optical character recognition (OCR)
            - Security and compliance
            - Implement human reviews 
        - Amazon Textract uses machine learning to instantly read and process any type of document, accurately extracting printed text, handwriting, forms, tables and, other data without the need for any manual effort or custom code.
    - Amazon Comprehend
        - Function: Discover insights and relationships in text
        - Use Cases: Call center analytics, Index and search product reviews, Personalize content on a website
        - Benefits: 
            - Get answers from text 
            - Organize documents by topics
            - Train modles on your own data
            - Support general and industry-specific text
        - Amazon Comprehend is a natural language processing (NLP) service that uses machine learning to find insights and relationships in text. It uses machine learning to help your customers uncover the insights and relationships in their unstructured data.
    - Amaozon Personalize (fantastic ecommerce!!)
        - Function: Create real-time, personalized user experiences fast, at scale
        - Use cases: Retail - Help customers discover products, Media end entretainment - Recommend new content, based on preference
        - Benefits
            - Deliver recommendations in real time
            - Implement personalized recommendations, in days
            - Personalize touchpoints alog the costumer journey
    - Amazon Kendra
        - Function: Enterprise search service powered by machine learning
        - Use Cases: Improve access to internal knowledge, Enhace sales and customer support services, Help customers find information efficiently
        - Benefits
            - Ask natural-language questions, get immediate answers
            - Bring dat together with a few clicks
            - Constantly improve search results
    - Amazon Forecast
        - Function: Time-series forecasting service
        - Use Cases: product demand planning, Financial planning, Resource planning
        - Benefits
            - Reduce forecasting time from months to hours
            - Create any time series forecast
            - Secure business data
    - Amazon Fraud Detector
        - Function: Detect more inline fraud faster
        - Use cases: detect common types of fraud (new account, online payment, guest checkout, online service and loyalty program abuse)
        - Benefits
            - Prevent and detect online fraud
            - Fraud detection in minutes
            - Customized for your unique business needs
<br><br><br>

- How you can help customers
    - AWS AI services solve specific business problems
    - Customers can have little to no Amazon ML experience
    - Customers can use APIs to interact with AWS IA services
---

<br>
<br>
<br>


## Module 3: Machine Learning Process
---

### Phases of ML & ML Pipeline
Business problem >> ML problem framing
- This is the MOST IMPORTANT part: you can solve this business with traditional code? If no, go to ML. "Can the business problem be framed as a ML problem?"

Business problem >> ML problem framing >> Data Collection >> Data Integration >> Data Preparation
- Time to look for the content to our ML
- Set up Data Pipeline to collect data and store data
- Cleanse, analyze, and prepare data
- Use different tools for these tasks

Business problem >> ML problem framing >> Data Collection >> Data Integration >> Data Preparation >> Data visualization and analysis
- Time to check if this content is usefull for a ML model, in good condition to go to trainig
- Set up and manage notebook environments
- Use statistical toolls to visualize and analyse data

Business problem >> ML problem framing >> Data Collection >> Data Integration >> Data Preparation >> Data visualization and analysis >> Feature Engineering >> Model training and parameter tuning >> Model evaluation
- Time to test and select an algorithm for our purpose
- Perform feature engineering
- Set up and manage inferenfe clusters
- Manage and scale model inference APIs
- Train, monitor and debug model predictions
- Model versioning and performance tracking

Business problem >> ML problem framing >> Data Collection >> Data Integration >> Data Preparation >> Data visualization and analysis >> Feature Engineering >> Model training and parameter tuning >> Model evaluation >> Are business goals met?
- If no, we go back to the data argumentation and the feature argumentation and improve it.

- If yes...

do >> Model deployment >> Will generate predictions >> Monitoring and debugging over time (add to the Data argumentation - data collection)
- Evaluate if model performance meets business goals
- Will feature augmentations or data augmentations leads to improve model performance
- Monitor for model performance and model drift
- To correct model drift, will need to go back to data collectio and training phase

### ML and the cloud journey
Data Flywheel
1. Migrate data and workloads to the cloud
2. Manage data and workloads in the cloud
3. Build data-driven applications 4. Analyze with data lake architectures
5. Innovate with machine learning

### How can you help customers
- Estimating where they are in the ML journey 
- Help them with the ML Process
- Avoind unecessary costs     s