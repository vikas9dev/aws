# Machine Learning on AWS - Handnote 📝

## 🤖 AWS ML SERVICES OVERVIEW

### Categories
- **Pre-built AI Services**: Ready-to-use ML services
- **ML Platform**: SageMaker for building custom models
- **AI Services**: Vision, speech, text, search, recommendations

---

## 🖼️ AMAZON REKOGNITION

### What is Rekognition?
- **Image & Video Analysis**: ML-powered object, people, text, scene detection
- **Use Cases**:
  - ✅ Facial analysis and search
  - ✅ People counting
  - ✅ Celebrity recognition
  - ✅ Content moderation
  - ✅ Text detection
  - ⚠️ Pathing (sports analysis) — People Pathing was discontinued on 31 Oct 2025

### Content Moderation
- **Purpose**: Detect inappropriate/offensive content
- **Confidence Threshold**: Set minimum confidence for flagging
- **Manual Review**: Optional with Amazon Augmented AI (A2I)
- **Use Cases**: Social networks, broadcast media, e-commerce

---

## 🗣️ AMAZON TRANSCRIBE

### What is Transcribe?
- **Speech to Text**: Automatic speech recognition (ASR)
- **Features**:
  - ✅ **PII Redaction**: Remove sensitive info (age, name, SSN)
  - ✅ **Multilingual**: Automatic language identification
  - ✅ **Real-time**: Streaming transcription

### Use Cases
- Customer service call transcription
- Closed captioning/subtitling
- Media asset metadata generation

---

## 🔊 AMAZON POLLY

### What is Polly?
- **Text to Speech**: Deep learning-based speech synthesis
- **Features**:
  - ✅ **Lexicons**: Custom pronunciation (acronyms, stylized words)
  - ✅ **SSML**: Speech Synthesis Markup Language (whisper, emphasis, breaks)
  - ✅ **Neural voices**: Most natural sound

### Customization
- **Lexicons**: Map words to pronunciations (e.g., AWS → "Amazon Web Services")
- **SSML**: Control speech style, pauses, phonetic pronunciation

---

## 🌍 AMAZON TRANSLATE

### What is Translate?
- **Language Translation**: Natural and accurate translation
- **Use Case**: Localize content for international users
- **Volume**: Efficiently translates large volumes of text

---

## 💬 AMAZON LEX & CONNECT

### Amazon Lex
- **Technology**: Same as Alexa
- **Features**:
  - ✅ **ASR**: Automatic Speech Recognition
  - ✅ **NLU**: Natural Language Understanding
- **Use Case**: Build chatbots, call center bots

### Amazon Connect
- **Contact Center**: Visual, cloud-based contact center
- **Benefits**:
  - ✅ No upfront payments
  - ✅ ~80% cheaper than traditional solutions
- **Integration**: CRM systems, AWS services
- **Flow**: Phone → Lex → Lambda → CRM

---

## 🧠 AMAZON COMPREHEND

### What is Comprehend?
- **NLP Service**: Natural Language Processing
- **Key Point**: Think Comprehend when you see "NLP" on exam
- **Features**:
  - ✅ Language detection
  - ✅ Key phrase extraction
  - ✅ Entity recognition (places, people, brands)
  - ✅ Sentiment analysis (positive, negative, neutral)
  - ✅ Topic modeling
  - ✅ Tokenization and parts of speech

### Use Cases
- Customer interaction analysis
- Article grouping by topic
- Extract insights from unstructured text

---

## 🏥 AMAZON COMPREHEND MEDICAL

### What is Comprehend Medical?
- **Medical NLP**: Extract medical information from text
- **Use Case**: Process medical records, clinical notes

---

## 🎓 AMAZON SAGEMAKER

### What is SageMaker?
- **ML Platform**: Build, train, deploy ML models
- **Full Lifecycle**: Data preparation → Training → Deployment
- **Use Case**: Custom ML models

---

## 🔍 AMAZON KENDRA

### What is Kendra?
- **Intelligent Search**: Enterprise search service
- **Features**: Natural language queries, understands context
- **Use Case**: Search internal documents, knowledge bases

---

## 🎯 AMAZON PERSONALIZE

### What is Personalize?
- **Recommendations**: Real-time personalized recommendations
- **ML-powered**: Learns from user behavior
- **Use Case**: Product recommendations, content personalization

---

## 📄 AMAZON TEXTRACT

### What is Textract?
- **Document Analysis**: Extract text and data from documents
- **Features**:
  - ✅ Text extraction
  - ✅ Form data extraction
  - ✅ Table extraction
- **Use Case**: Process invoices, forms, receipts

---

## ⚠️ CRITICAL EXAM POINTS

1. **Rekognition**: Image/video analysis, content moderation
2. **Transcribe**: Speech to text, PII redaction, multilingual
3. **Polly**: Text to speech, lexicons, SSML
4. **Translate**: Language translation
5. **Lex**: ASR + NLU, chatbots
6. **Connect**: Contact center, ~80% cheaper
7. **Comprehend**: NLP service (think Comprehend for NLP)
8. **SageMaker**: ML platform for custom models
9. **Kendra**: Intelligent document search
10. **Personalize**: Real-time recommendations
11. **Textract**: Document text/data extraction

---

## 📋 QUICK REFERENCE

### Service Selection
- **Image/Video Analysis**: Rekognition
- **Speech to Text**: Transcribe
- **Text to Speech**: Polly
- **Translation**: Translate
- **Chatbots**: Lex
- **Contact Center**: Connect
- **NLP**: Comprehend
- **Custom ML**: SageMaker
- **Document Search**: Kendra
- **Recommendations**: Personalize
- **Document Extraction**: Textract

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

