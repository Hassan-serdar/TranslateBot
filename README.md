# TranslateBot

# Neural Machine Translation (NMT) with Keras LSTM (English-to-Arabic)

## 📖 Project Overview

This project implements a foundational Neural Machine Translation (NMT) model from scratch using Keras and TensorFlow. The goal is to build and understand the classic **Sequence-to-Sequence (Seq2Seq)** architecture, which forms the basis of modern translation and text generation models.

The model is trained on a parallel corpus of **English-to-Arabic** sentences  to translate sentences from English to Arabic.

##  Core Architecture: Encoder-Decoder

This project is built around the **Encoder-Decoder** model, which consists of two main LSTM components:

1.  **The Encoder (The "Reader"):**
    * An `Embedding` layer followed by an `LSTM` layer.
    * Its sole purpose is to read the entire input **(English)** sentence, word by word (e.g., "Hello").
    * It doesn't produce any output. Instead, it compresses the "meaning" and "context" of the sentence into its final **hidden state** and **cell state** vectors (often called the "thought vector").

2.  **The Decoder (The "Writer"):**
    * Another `Embedding` and `LSTM` stack, followed by a `Dense(softmax)` layer.
    * Its `LSTM` layer is initialized using the Encoder's final state vectors as its `initial_state`. This "seeds" the Decoder with the context of what it needs to translate.
    * During training (using "Teacher Forcing"), it learns to map the input sequence (e.g., `\t مرحباً`) to the target sequence (e.g., `مرحباً \n`).
    * The final `Dense(softmax)` layer predicts the probability of each word in the *target* (**Arabic**) vocabulary being the next word.

## Data Preprocessing

1.  **Corpus Loading:** Loaded a parallel text file
2.  **Token Insertion:** Crucially, "Start of Sequence" (`\t`) and "End of Sequence" (`\n`) tokens were added to all *target* (**Arabic**) sentences. This teaches the Decoder when to start and stop generating.
3.  **Tokenization:** Two separate `tensorflow.keras.preprocessing.text.Tokenizer` instances were created:
    * One for the input (English) vocabulary.
    * One for the target (Arabic) vocabulary.
4.  **Padding:** Sequences were padded to ensure uniform length for batch processing.
5.  **Target Encoding:** The decoder target data was likely one-hot encoded (`to_categorical`) to be used with the `categorical_crossentropy` loss function.

## Prediction & Inference

To generate translations, a separate **Inference Model** is typically constructed (as seen in the notebook). This is necessary because, unlike training, we must generate the target sentence one word at a time:
1.  Feed the English sentence to the `encoder_model` to get the thought vector.
2.  Start the `decoder_model` with the thought vector and the `<start>` token (`\t`).
3.  Predict the first Arabic word.
4.  Feed that predicted word back into the `decoder_model` in a loop.
5.  Stop when the model predicts the `<end>` token (`\n`).

##  Technologies & Libraries Used

* **Python**
* **Deep Learning:** TensorFlow 2.x, Keras
* **Keras Layers:** `Sequential`, `Model`, `Input`, `Embedding`, `LSTM`, `Dense`
* **Preprocessing:** `Tokenizer`, `pad_sequences`
* **Utilities:** NumPy
* **Environment:** Jupyter Notebook
