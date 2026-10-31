# 23 Machine Learning

Sections:-
- [1. Amazon Rekognition](#1-amazon-rekognition)
- [2. Amazon Transcribe](#2-amazon-transcribe)
- [3. Amazon Polly: Text-to-Speech with Deep Learning 🗣️](#3-amazon-polly-text-to-speech-with-deep-learning-️)
- [4. Amazon Translate](#4-amazon-translate)
- [5. Amazon Lex and Connect](#5-amazon-lex-and-connect)
- [6. Amazon Comprehend](#6-amazon-comprehend)
- [7. Amazon Comprehend Medical](#7-amazon-comprehend-medical)
- [8. Amazon SageMaker](#8-amazon-sagemaker)
- [9. Amazon Kendra: Intelligent Document Search](#9-amazon-kendra-intelligent-document-search)
- [10. Amazon Personalize: Real-Time Personalized Recommendations](#10-amazon-personalize-real-time-personalized-recommendations)
- [11. Amazon Textract](#11-amazon-textract)
- [12. Machine Learning Services on AWS](#12-machine-learning-services-on-aws)
- [Q & A](#q--a)

---

## 1. Amazon Rekognition

[Amazon Rekognition](https://aws.amazon.com/rekognition/) is a service that uses machine learning to identify objects, people, text, and scenes in images and videos. 🖼️ 🎬

![Amazon Rekognition](https://d1.awsstatic.com/onedam/marketing-channels/website/aws/en_US/product-categories/ai-ml/artificial-intelligence/approved/images/f39063be-8cc6-42f0-a741-a828b63bd886.e56aacfd5f2d2d2f337f18dabed282cbac9ee4a1.png)

Here's what it can do:

*   Facial analysis and search for user verification. ✅
*   Count the number of people in an image. 🔢
*   Create a database of familiar faces. 🧑‍🤝‍🧑
*   Compare images against a database of celebrities. 🌟

Rekognition's use cases include:

*   Labeling images and videos. 🏷️
*   Content moderation. 👮‍♀️
*   Text detection. 🔤
*   Face detection and analysis (e.g., gender, age range, emotions). 😃
*   Face search and verification. 🔍
*   Celebrity recognition. 🌟
*   Pathing (e.g., sports game analysis). ⚽ ⚠️ AWS discontinued Rekognition People Pathing on 31 Oct 2025 (closed to new customers since Oct 2024). Older exam questions may still list it.

📌 **Example:** Identifying elements in an image such as a person, rock, mountain bike, crest, and outdoors.

📌 **Example:** Labeling images with objects like golden retrievers or dogs. 🐕

📌 **Example:** Detecting text in images, such as runner numbers in a race. 🏃‍♀️

📌 **Example:** Analyzing faces to determine if a person is happy, smiling, female, and has open eyes. 😊

📌 **Example:** Recognizing the CTO of AWS in a picture. 🧑‍💼

📌 **Example:** Monitoring a soccer game to track player movements for real-time analytics. ⚽

### Content Moderation

![Amazon Rekognition Content Moderation](./img/Amazon-Rekognition-Content-Moderation.png)

One key feature to understand for the exam is content moderation. ⚠️

*   It's used to detect inappropriate, unwanted, or offensive content in images and videos. 🚫
*   Use cases include social networks, broadcast media, advertising, and e-commerce. 🛍️
*   The goal is to create a safe user experience. 👍

How it works:

1.  Amazon Rekognition analyzes the image. 🤖
2.  You set a **Minimum Confidence Threshold** for items to be flagged. ⚙️
    *   The lower the percentage, the more matches you'll get. 📈
    *   The confidence percentage indicates how confident Rekognition is that the flagged image is inappropriate. 💯
3.  Optional manual review using Amazon Augmented AI (A2I). 🧑‍💻

This process helps you:

*   Automatically flag sensitive images. 🚩
*   Use manual review to decide whether to keep or delete them. 🗑️
*   Comply with regulations. ⚖️

---

## 2. Amazon Transcribe

Amazon Transcribe automatically converts speech into text. 🗣️➡️📝 You provide an audio input, and it transcribes it into text. 📌 **Example:** Saying "Hello, my name is Stephane, and I hope you're enjoying the course" will result in that text being generated.

How does it work? It uses a deep learning process called **ASR** (Automatic Speech Recognition) to convert speech to text quickly and accurately. 🚀

Here are some key features:

*   **PII Redaction:** Automatically remove Personally Identifiable Information (PII) using redaction. 🛡️ This includes things like:
    *   Age
    *   Name
    *   Social Security Number
*   **Automatic Language Identification:** Supports multilingual audio. 🌐 Transcribe can recognize and transcribe audio in multiple languages (e.g., French, English, Spanish).

Use Cases:

*   Transcribing customer service calls. 📞
*   Automating closed captioning and subtitling. 📺
*   Generating metadata for media assets to create a fully searchable archive. 🗄️

Let's look at a demo:

1.  You can create a transcript and select the language (e.g., English US).
2.  Click "Start Streaming."
3.  Speak, and the audio is directly transcribed into text. 🎤➡️📝

You can also configure content removal, specifically PII identification and redaction.

📌 **Example:**

If you say, "Hello, my name is Stephane, I am 31 years old, and my phone number is 910-747-280," Transcribe can be configured to hide or redact the name and phone number:- Hello, my name is [NAME]. I am [AGE]  old, and my phone number is [PHONE]

Automatic language identification is another powerful feature. You can select multiple languages (e.g., English and French) and Transcribe will attempt to identify and transcribe in the correct language.

📌 **Example:**

Speaking in English and then switching to French will result in the transcription reflecting both languages.

```text
English: Hello, this is some recognition happening in English.
French: (Transcription of the French speech)
```

If you want to explore further, check out all the options available in the Amazon Transcribe console. 💻

---

## 3. Amazon Polly: Text-to-Speech with Deep Learning 🗣️

Amazon Polly is a service that turns text into lifelike speech using deep learning. It's essentially the opposite of transcription services. This allows you to create applications that can "talk."

📌 **Example:** Polly can take the text: "Hello, my name is Stephane and this is a demo of Amazon Polly" and generate an audio file of that text being spoken.

### Customizing Speech with Lexicons and SSML ⚙️

Amazon Polly offers powerful customization options using Lexicons and SSML (Speech Synthesis Markup Language).

#### Pronunciation Lexicons 📚

Pronunciation lexicons allow you to customize the pronunciation of specific words.

*   Useful for stylized words or acronyms.
*   📌 **Example:** If you have a stylized name like "Stephane" with numbers instead of letters (e.g., "St3ph4ne"), Polly might mispronounce it. You can create a lexicon to ensure it's pronounced correctly.
*   📌 **Example:** You can configure Polly to say "Amazon Web Services" instead of "A-W-S" whenever it encounters the acronym AWS.

- St3ph4ne => Stephane
- AWS => Amazon Web Services

To use lexicons:

1.  Upload your lexicon file.
2.  Use the lexicon in the `SynthesizeSpeech` operation.

### Speech Synthesis Markup Language (SSML) ✍️

SSML provides even more granular control over how speech is generated.

*   You can emphasize specific words or phrases.
*   You can use phonetic pronunciation.
*   You can include breathing sounds or whispering effects.
*   You can even use different speaking styles, such as a Newscaster style.

Instead of plain text, you use SSML tags to control the speech.

📌 **Example:** You can insert a `<whisper>` tag to make Polly whisper a section of the text.

📝 **Note:** For pronunciation of stylized words or acronyms, use Pronunciation lexicons. For more customization like whispering or phonetic pronunciation, use SSML.

### Using Amazon Polly in the Console 💻

You can experiment with Amazon Polly directly in the AWS console.

1.  Choose a voice (including neural network voices for the most natural sound).
2.  Enter the text you want Polly to speak.

📌 **Example:**

```text
Hey, my name is Stephane and I love AWS.
```

You can also use SSML (enable it) in the console.

📌 **Example:** Adding a break:

```xml
<speak>Hey, my name is Joanna. <break time="3s"/> I will read any text you type here<speak/>.
```

This code snippet will cause Polly to pause for three seconds after saying "Hey, my name is Joanna."

To customize the pronunciation of "AWS" to "Amazon Web Services" within the console:

1.  Go to additional settings.
2.  Customize pronunciation.
3.  Apply a lexicon that maps "AWS" to "Amazon Web Services." You'll need to create and upload this lexicon file first.

💡 **Tip:** Creating a lexicon file and uploading it will automatically replace instances of "AWS" with "Amazon Web Services" in your generated speech.

---

## 4. Amazon Translate

Amazon Translate is a natural and accurate language translation service. 🌍 It allows you to localize content, such as websites and applications, for your international users. This service efficiently translates large volumes of text.

📌 **Example:** Let's see how a simple sentence translates into different languages:

*   English: "Hello, my name is Stephane and I'm excited to be speaking with you today."

*   French: "Bonjour, je m'appelle Stephane et je suis ravi de vous parler aujourd'hui."

*   Portuguese: "Ol , meu nome  Stephane e estou animado em falar com voc  hoje."

*   Hindi: "नमस्त , मेरा नाम स्टफन ह  और मैं आज आपके साथ बात करने के लिए उत्साहित ह ."

That's how easy it is to use Amazon Translate! 🚀

---

## 5. Amazon Lex and Connect

### Amazon Lex

Amazon Lex uses the same technology that powers Alexa devices. Think of Alexa as a device in your home that responds to voice commands. For example, you can ask, "Alexa, what's the weather like tomorrow?" and it will provide a weather forecast.

Amazon Lex provides:

*   Automatic Speech Recognition (ASR): Converts speech into text.
*   Natural Language Understanding (NLU): Understands the intent of text and sentences.

Amazon Lex helps you build chatbots or call center bots. 🤖

### Amazon Connect

Speaking of call centers, Amazon Connect is a visual contact center service that allows you to:

*   Receive calls. 📞
*   Create contact flows.
*   It's entirely cloud-based. ☁️
*   Integrates with Customer Relationship Management (CRM) systems and other AWS services.

A key advantage of Amazon Connect is that there are no upfront payments. It's approximately 80% cheaper than traditional contact center solutions. 💰

Here's the typical flow for building a smart contact center:

1.  A phone call is made to a number defined by Amazon Connect to, for example, schedule an appointment.
2.  Lex streams information from the call and understands the intent.
3.  Lex invokes the appropriate Lambda function.
4.  The Lambda function can then interact with a CRM to schedule the meeting by writing code. ⚙️

📌 **Example:**

Someone says, "Schedule a meeting tomorrow with Tom at 3:00 PM." The Lambda function will update the CRM accordingly.

![Amazon Lex and Amazon Connect](./img/Amazon_Lex_and_Amazon_Connect.png)

In summary:

*   **Lex is for ASR (automatic speech recognition) and NLU (natural language understanding).**
*   **Connect is for building contact centers.**

---

## 6. Amazon Comprehend

Amazon Comprehend is a straightforward service, especially from an exam perspective. Its primary function is to "comprehend" text, making it a tool for **Natural Language Processing (NLP)**. 🧠

Anytime you encounter **NLP** on the exam, immediately think of Amazon Comprehend.

It's a fully managed and serverless service that leverages machine learning to extract insights and relationships from your text data. 🚀

Here's what Comprehend can do:

*   🌐 Language Detection: Identify the language of the text.
*   🔑 Key Phrase Extraction: Extract important phrases from the text.
*   📍 Entity Recognition: Identify places, people, brands, and events.
*   😊 Sentiment Analysis: Determine the sentiment (positive, negative, neutral) of the text.
*   🧮 Tokenization and Parts of Speech Analysis: Analyze the text's structure.
*   🎤 Audio: Comprehend accepts text only, not audio. To analyze speech, first convert it to text with Amazon Transcribe.
*   📂 Topic Modeling: Organize a collection of text files by topic.

Comprehend's core purpose is to ingest large amounts of data and automatically understand its meaning. It transforms unstructured text data into structured insights using the features mentioned above. ✨

Here are some use cases for **NLP** using Amazon Comprehend:

*   Customer Interaction Analysis:
    *   Analyze customer emails to understand what leads to positive or negative experiences.
    *   Extract key features from customer interactions.
    *   Gain business insights to improve your services.
*   Article Grouping by Topic:
    *   Automatically group articles by topics uncovered by Comprehend.
    *   Feed articles into Comprehend to get topic suggestions.

In essence, Comprehend helps you take text or unstructured data and structure it around these features.

📌 **Example:** Imagine you have thousands of customer support tickets. You can use Comprehend to automatically identify the most common issues, the sentiment of customers regarding those issues, and the key phrases used to describe them. This allows you to prioritize improvements and address the root causes of customer dissatisfaction.

📝 **Note:** From an exam perspective, remember that Comprehend is primarily for **Natural Language Processing (NLP)**.

---

## 7. Amazon Comprehend Medical

Amazon Comprehend Medical is designed to detect and return useful information from unstructured clinical text. This is particularly helpful for:

*   📝 Doctor's notes
*   📝 Discharge summaries
*   📝 Test results
*   📝 Case notes

It leverages **NLP** (Natural Language Processing) to identify **protected health information (PHI)** within documents using the `DetectPHI` API.

### Architecture Options

Here's how you can integrate Comprehend Medical into your workflows:

1.  Store documents in Amazon S3 and invoke the Comprehend Medical API.
2.  Use Kinesis Data Firehose to analyze data in real-time.
3.  Utilize Amazon Transcribe to convert voice to text, then pass the text to Comprehend Medical.

    ```
    Voice -> Amazon Transcribe -> Text -> Amazon Comprehend Medical
    ```

### Real-Time Analysis 🚀

You can perform real-time analysis by inputting text directly into the Amazon Comprehend Medical service.

📌 **Example:** Imagine a doctor's note containing various observations.

```text
Pt is 87 yo woman, highschool teacher with past medical history that includes
   - status post cardiac catheterization in April 2019.
She presents today with palpitations and chest pressure.
HPI : Sleeping trouble on present dosage of Clonidine. Severe Rash  on face and leg, slightly itchy.
Meds : Vyvanse 50 mgs po at breakfast daily, 
            Clonidine 0.2 mgs -- 1 and 1 / 2 tabs po qhs 
HEENT : Boggy inferior turbinates, No oropharyngeal lesion.
Lungs : clear.
Heart : Regular rhythm.
Skin :  Mild erythematous eruption to hairline.

Follow-up as scheduled
```

More sample notes:- https://chatgpt.com/s/t_68c83441cd748191a17bef8a1bdf86ca

By analyzing this text, Comprehend Medical can identify:

*   Entities (e.g., age, profession)
*   Relationships between entities (e.g., procedure name and date)
*   Medication details (e.g., generic name, strength, dosage, route, frequency)

This process helps structure unstructured health data using machine learning.

### Benefits ✨

Amazon Comprehend Medical enables you to extract valuable insights from text-based medical information, even if you don't have extensive medical expertise. It helps in organizing and understanding complex medical data more efficiently.

---

## 8. Amazon SageMaker

Amazon SageMaker is a fully managed service designed for developers and data scientists to build, train, and deploy machine learning models. 🚀 Unlike other managed machine learning services with specific purposes (e.g., translating text or transcribing audio), SageMaker is a higher-level service that empowers your organization's developers and data scientists to create and manage their own custom models.

Building machine learning models involves several complex steps that can be challenging to execute in a unified environment. Provisioning servers for the necessary computations can also be cumbersome. SageMaker aims to streamline this entire process.

Let's illustrate this with a simplified example:

📌 **Example:** Imagine you want to build a model to predict a student's score on a certification exam.

Here's how you might approach it:

1.  **Gather Data:** 📊 Collect data from a large number of students (e.g., 10,000) regarding their experience (years in IT, years with AWS), time spent on the course, number of practice exams taken, etc.
2.  **Label Data:** 🏷️ Assign meaning to each column of data and provide the actual exam score obtained by each student. For instance:
    *   Student A: Didn't pass (score: 670) - Didn't complete the course.
    *   Student B: Passed with a high grade (score: 990).
    *   Student C: Passed with an even higher grade (score: 934).
    The goal is to predict the score based on the collected data.
3.  **Build a Machine Learning Model:** ⚙️ Develop a model that can predict scores based on the historical data.
4.  **Train and Tune the Model:** 🎯 Refine the model over time to improve its accuracy and fit the data and desired outputs. This is often a difficult and iterative process.

SageMaker assists with all these steps: labeling, building, training, and tuning. But its capabilities extend beyond these core functions.

Once a machine learning model is created and fully operational, it needs to be deployed to make predictions on new data.

📌 **Example:** Consider a new student who provides their experience details (years in IT, AWS experience, time spent on the course). The trained machine learning model can then be applied to this new data to predict the student's potential exam score (e.g., "Based on the data, this student is predicted to pass with a score of 906").

This entire process—labeling, building, training, tuning, and deploying—can be managed within SageMaker. 🚀

---

## 9. Amazon Kendra: Intelligent Document Search

Amazon Kendra is a fully-managed **document search service** powered by machine learning. It allows you to extract answers directly from within your documents. 📚

Kendra supports various document formats, including:

*   Text files 📝
*   PDFs 📄
*   HTML 🌐
*   PowerPoint presentations 📊
*   Microsoft Word documents ✍️
*   FAQs ❓
*   And many more!

These documents can reside in a multitude of data sources, some of which are shown below. Amazon Kendra indexes these documents to build a knowledge index using machine learning. 🧠

From an end-user perspective, Kendra provides natural language search capabilities, similar to using Google. 🗣️

📌 **Example:** If a user asks, "Where is the IT support desk?" Kendra can reply, "1st floor." This is possible because Kendra has learned this information from the indexed resources. 🏢

Kendra also supports normal search queries and learns from user interactions and feedback to improve search results through incremental learning. 📈

![Amazon Kendra](./img/AWS_Kendra.png)

You can fine-tune search results based on factors like:

*   Importance of data ✅
*   Freshness of data ⏳
*   Custom filters ⚙️

💡 **Tip:** For the exam, remember that Amazon Kendra is the document search service on AWS. 🎯

---

## 10. Amazon Personalize: Real-Time Personalized Recommendations

Amazon Personalize is a fully managed machine learning service designed to help you build applications with real-time personalized recommendations. 🛍️

What kind of recommendations can you create? Think:

*   Personalized product recommendations.
*   Re-ranking of search results.
*   Customized direct marketing campaigns.

📌 **Example:** Imagine a user who frequently purchases gardening tools. Amazon Personalize can analyze their purchase history and provide recommendations for related items they might be interested in buying next.

This service leverages the same technology that powers Amazon.com's recommendation engine. 🤖 You know, the one that suggests products based on your browsing and purchasing behavior?

Here's how it works:

1.  **Data Input:** Read your input data (e.g., user interactions) from Amazon S3. 🗄️
2.  **Real-Time Integration:** Use the Amazon Personalize API for real-time data integration. 🔄
3.  **Personalized API:** Expose a customized, personalized API for your websites, applications, and mobile apps. 📱
4.  **Personalized Communication:** Send personalized SMS or emails. 📧

With Amazon Personalize, you can build a recommendation model in days, not months. 🚀 You don't need to build, train, and deploy ML solutions from scratch. It's all bundled and ready to use.

![Amazon Personalize](./img/AWS_Personalize.png)

Use cases include:

*   Retail stores 🏪
*   Media outlets 📰
*   Entertainment platforms 🎬

💡 **Tip:** For the exam, if you encounter a question about a machine learning service for building recommendations and personalized experiences, the answer is likely Amazon Personalize. ✅

---

## 11. Amazon Textract

Amazon Textract is a service used to extract text, handwriting, and data from scanned documents. 📄 It leverages AI and machine learning behind the scenes to analyze and process these documents.

![Amazon Textract](./img/AWS_Textract.png)

Here's how it works:

1.  Upload a document (e.g., a driver's license) to Amazon Textract. ⬆️
2.  Textract automatically analyzes the document. 🤖
3.  The results are provided to you as a data file. 💾
4.  You can then extract specific information, such as date of birth or document ID. 🎂🆔

Textract can extract data from:

*   Forms
*   Tables
*   PDFs
*   Images

The use cases for Textract are varied and span multiple industries:

*   Financial Services: Processing invoices and financial reports. 🧾
*   Healthcare: Managing medical records and insurance claims. 🏥
*   Public Sector: Handling tax forms, ID documents, and passports. 🏛️

---

## 12. Machine Learning Services on AWS

Here's a rundown of the key Machine Learning services offered by AWS. Remember these for the exam! 🧠

*   **Rekognition**: Enables face detection, labeling, and celebrity recognition. 🌟
*   **Transcribe**: Converts audio to text, useful for generating subtitles. 🗣️➡️📝
*   **Polly**: Converts text to audio. 📝➡️🗣️
*   **Translate**: Provides language translation services. 🌐
*   **Lex**: Used to build conversational bots or chatbots. 🤖
    *   When bundled with **Connect**, you can create a cloud contact center. 📞
*   **Comprehend**: Facilitates natural language processing (NLP). 💬
*   **SageMaker**: A fully-featured machine learning service for developers and data scientists. 🧑‍💻
*   **Kendra**: An ML-powered document search engine. 🔍
*   **Personalize**: Provides real-time personalized recommendations for customers. 🎁
*   **Textract**: Detects and extracts text and data from various documents. 📄➡️📊

Hopefully, this list is helpful! 📝 This knowledge can definitely earn you some points on the exam. 👍

---

## Q & A

### ❓ Question

A company would like to implement a chatbot that will:

* Convert **speech-to-text**
* Recognize the **customers' intentions**

👉 What AWS service should it use?

Options:

* Transcribe
* Rekognition
* Connect
* Lex

<details>

<summary>Explanation</summary>

Amazon Lex is the correct service for this scenario because it:

* ✅ Provides **automatic speech recognition (ASR)** → converts speech into text.
* ✅ Provides **natural language understanding (NLU)** → understands the **intent** of what users say.
* ✅ Enables building **chatbots and voice assistants** that can interact naturally with users.
* ✅ Integrates with other AWS services like **Connect** (for contact centers) and **Lambda** (for custom logic).

#### 📌 Why not the other options?

* **Amazon Transcribe** → Only converts **speech-to-text**, but doesn't understand intent (no NLU).
* **Amazon Rekognition** → Used for **image and video analysis**, not speech/chatbots.
* **Amazon Connect** → A cloud contact center service, can integrate with Lex, but doesn't handle NLU itself.

✅ Answer: **Amazon Lex**

✅ **Final Takeaway:** If the goal is to build a chatbot that can **understand and respond to customer speech with intent recognition**, the best choice is **Amazon Lex**.

</details>

### ❓ Question

A research team would like to group articles by topics using **Natural Language Processing (NLP)**. Which service should they use?

Options:

* Translate
* Comprehend
* Lex
* Rekognition

<details>

<summary>Explanation</summary>

Amazon Comprehend is the right service because it is AWS's **natural language processing (NLP)** service. It uses **machine learning** to extract insights from text, such as:

* ✅ **Topic modeling** – automatically grouping documents/articles by subject.
* ✅ **Entity recognition** – identifying names, places, dates, etc.
* ✅ **Sentiment analysis** – detecting emotions (positive, negative, neutral, mixed).
* ✅ **Key phrase extraction** – highlighting important terms.
* ✅ **Language detection** – figuring out the text's language.

✅ Answer: **Amazon Comprehend**

#### 📌 Why not the other options?

* **Translate** → Used for **language translation**, not grouping articles by topics.
* **Lex** → Builds **chatbots** with speech-to-text and intent recognition, not NLP text analysis.
* **Rekognition** → Analyzes **images and videos**, not text.

✅ **Final Takeaway:** For analyzing and grouping articles using **NLP techniques**, the correct AWS service is **Amazon Comprehend**.

</details>

### ❓ Question

An online medical company that allows you to book an appointment with doctors using a phone call is using AWS to host their infrastructure. They are using **Amazon Connect** and **Amazon Lex** to receive calls, create workflows, book appointments, and process payments.

According to the company’s policy, all calls must be recorded for review. However, there is a requirement to **remove any Personally Identifiable Information (PII)** from the call before it is saved.

👉 What do you recommend to use, which helps in removing PII from calls?

Options:

* Amazon Polly
* Amazon Transcribe
* Amazon Rekognition
* Amazon Translate

<details>

<summary>Explanation</summary>

Amazon **Transcribe** is an **automatic speech recognition (ASR)** service that converts speech into text. It also includes a feature called **PII redaction**, which can automatically detect and remove sensitive data such as:

* 📌 Names
* 📌 Credit card numbers
* 📌 Social security numbers
* 📌 Addresses
* 📌 Other PII elements

This ensures that the recorded calls are **compliant with security and privacy requirements** by masking or removing PII before storage.

✅ Answer: **Amazon Transcribe**

#### 📌 Why not the other options?

* **Amazon Polly** → Converts text into lifelike speech (text-to-speech), not for transcription or PII removal.
* **Amazon Rekognition** → Analyzes images and videos (facial recognition, object detection), not audio or speech.
* **Amazon Translate** → Provides real-time text translation between languages, not transcription or PII redaction.

✅ **Final Takeaway:** For removing **PII from recorded calls**, the correct AWS service is **Amazon Transcribe** with its **PII redaction feature**.

</details>

---