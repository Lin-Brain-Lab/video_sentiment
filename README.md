# 🎬 Moving Sentiment Analysis on Video Clips

## 📌 Overview

This project explores **multi-modal sentiment analysis** on movie clips. We aim to understand and track **emotions as they evolve over time** in a video, using information from user comments, audio soundtrack, transcripts, and facial emotions.

The project is split into two major parts:
- **Model A**: Predicts the overall emotion of a video clip based on YouTube comments.
- **Model B**: Dynamically tracks changing emotions in a video using a combination of transcript, soundtrack, and image-based models.

---

## 🧠 Models

### 🧾 Model A: YouTube Comment-Based Emotion Classifier

This model predicts a video clip’s overall emotion using the top 50 English-language comments on the clip.

- **Model**: XGBoost + Random Forest
- **Tools** : Youtube API, Youtube clips
- **Input**: Top 50 most-liked comments per youtuve clip
- **Output**: One of four emotions — `Neutral`, `Funny`, `Fear`, `Sad`
- **Dataset**:
  - 7,500 hand-labeled YouTube comments by emotion. 
  - Non-English and irrelevant comments filtered out
- **Performance**:  
  - **Accuracy**: 87% on a 5-class classification task
- **Dataset** : https://docs.google.com/spreadsheets/d/1Ku0KQfNMllORcpadlNv-Ji9PYh5i9pGND68dlw5EFbk/edit?usp=sharing 

---

### 🎥 Model B: Moving Sentiment Analysis

A three-stream model that tracks sentiment **over time** in a video clip using three modalities:

#### 1️⃣ Transcript Model
- **Goal**: Predict emotion per line of dialogue
- **Model**: SBERT powered One-Vs-Rest sentiment classification
- **Dataset**: 
  - Primary: [DailyDialog](https://paperswithcode.com/dataset/dailydialog)
  - Exploring: [CMU-MOSEI](http://multicomp.cs.cmu.edu/resources/cmu-mosei-dataset/)
- **Status**: 
  - 79% accuracy on current dataset
- **Input**: Sentence-level dialogue
- **Output**: One of six emotion labels per sentence

#### 2️⃣ Soundtrack Model
- **Goal**: Predict **valence/arousal (V/A)** values every 0.5s across 30s clip segments
- **Model**: Random Forest Regression model 
- **Dataset**: [DEAM / EmoMusic Dataset](https://cvml.unige.ch/databases/DEAM/)
- **Input**: 15s–45s audio snippet
- **Output**: V/A pair every 0.5s
- **Performance**: 
  - **MSE Average**: 0.0280
  - **Status**: Fully trained and functioning well

#### 3️⃣ Image Model (Development Paused)
- **Components**:
  - **Bounding Box Detector**
    - Dataset: [WiderFace] (http://shuoyang1213.me/WIDERFACE/)
    - Output: 4 facial bounding box coordinates
    - Status: High accuracy
  - **Facial Emotion Recognition**
    - Datasets used: [FDDB] (https://paperswithcode.com/dataset/fddb)
    - Issues:
      - Emotion labels do not align with intended use
      - Labels are ambiguous and context-insensitive
      - No suitable replacement dataset available
    - Status: Development paused; focus shifted to audio and text

---

## 🧪 Dataset Summary

| Modality   | Dataset(s) Used                     | Notes |
|------------|-------------------------------------|-------|
| Comments   | Manually collected YouTube comments | 7,500 labeled, top-liked only, English-only |
| Transcript | DailyDialog, CMU-MOSEI              | Sentence-level emotion labels |
| Audio      | DEAM / EmoMusic                     | V/A labels every 0.5s |
| Image      | LFPW, WIDERFace, FDDB               | Face detection working; emotion labels unsuitable |

---

### 🌱 Next Steps

#### 🔄 Model Development

We are initiating another round of **manual data labeling** to support further training and testing:

- Labeling **YouTube movie clips** with overall emotions
- Extracting and labeling **soundtracks** from these clips
- Feeding labeled soundtracks into the existing **Soundtrack Model** for validation
- Using the **YouTube API** to collect **transcripts/dialogues** for those clips
- Testing the **Transcript Model** with new dialogue data
- Working toward a **combined Transcript + Soundtrack model** that tracks clip-level emotion progression using both modalities

#### 🌐 Website Development

We plan to build a two-page website with distinct purposes:

- **Page 1 – Research Portal**:  
  - Display a curated bank of pre-labeled movie clips  
  - Allow researchers to view or download model predictions and emotion trajectories  
  - Serve as a transparent hub for ongoing model refinement
  - Source code of final model included

- **Page 2 – Crowdsourced Survey**:  
  - Present selected video clips and predicted emotions  
  - Let users provide feedback or label their own interpretations  
  - Serve as a mechanism for continuous data collection and model improvement

  
