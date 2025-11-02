# TranslateBot

# Neural Machine Translation (NMT) with Keras LSTM (English-to-Arabic)
 ## Project Overview

This project implements a Neural Machine Translation (NMT) system using Keras and TensorFlow, designed to translate text from English to Arabic at the word level.
The goal is to build and understand a traditional Sequence-to-Sequence (Seq2Seq) model architecture based on LSTM networks — a foundational structure for modern translation models.

## Core Architecture: Encoder–Decoder

The model follows the classic Encoder–Decoder architecture:

## Encoder:

An Embedding layer converts English words into dense vector representations.

A stacked LSTM processes the input sequence and captures its semantic meaning in its final hidden and cell states.

## Decoder:

Initialized with the encoder’s final states to preserve the context using TeacherFocre.

Uses an Embedding layer for Arabic words, followed by an LSTM to generate the translated sequence step by step.

A Dense (softmax) layer predicts the next Arabic word in the sequence based on the decoder’s output state.

🧩 Data Preprocessing

Dataset Loading:

The dataset used is OPUS100 (ar–en), loaded via the Hugging Face datasets library.

Text Cleaning & Normalization:

Removal of diacritics, special characters, and unnecessary whitespace.

Normalization of Arabic letters

Filtering out very short or overly long sentence pairs.

Tokenization:

Separate Tokenizer objects are trained for English and Arabic.

Special tokens <start> and <end> are added to all Arabic sentences.

The vocabulary is limited to 20,000 words per language.

Sequence Padding:

All input and target sequences are converted into fixed-length integer arrays using pad_sequences.

## Model Training & Inference

Training:

The Seq2Seq model is built using the Functional API with:

Embedding → LSTM → Dense(softmax)

Compiled with:

optimizer = Adam

loss = sparse_categorical_crossentropy

Includes EarlyStopping to prevent overfitting and optimize training time.

Inference:

After training, the model is split into two components:

Encoder model: Processes the English sentence and outputs context vectors.

Decoder model: Generates the Arabic translation word by word until <end> is predicted.

 ## Technologies & Libraries Used

Python

TensorFlow / Keras

NumPy, Pandas

scikit-learn

Hugging Face Datasets (OPUS100)       
