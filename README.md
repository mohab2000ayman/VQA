### Visual Question Answering Report
## Mazen Ghanem: 6896, Mohab Ayman: 7127, Anas Emad: 7048

# VQA
Visual Question Answering (VQA) using transformers


In this work, we address the challenges associated with training multi-modality tasks, particularly visual question answering, which is known for its high complexity and resource-intensive
requirements. To overcome these challenges, we propose a novel architecture inspired by CLIP
that eliminates the need for fine-tuning feature extractors. Instead, we adopt a simple yet effective
approach of applying a linear classifier to the concatenated features of image and text encoders.
Furthermore, our training process incorporates an auxiliary loss that focuses on the prediction
of answer types. This classification outcome serves as an attention gate, allowing us to select
appropriate answer classes. Our experimental results are on the VizWiz 2022 Visual Question
Answering Dataset .
## 1 Introduction
CLIP (Contrasting Language-Image Pretraining) is an innovative neural network model developed by
OpenAI, designed to establish a strong connection between images and text. Trained on a massive
dataset of image-text pairs, CLIP exhibits exceptional capabilities in tasks such as image captioning,
image retrieval, and zero-shot learning. This powerful tool enables generating descriptive captions for
images, retrieving relevant images based on textual queries, and even classifying images into novel
categories without prior exposure. With its potential to enhance image search engines and create
interactive visual experiences, CLIP opens up exciting possibilities at the intersection of images and
text. In this project report, we delve into the architecture, training process, and applications of CLIP,
shedding light on its remarkable ability to bridge the gap between visual and textual domains.
## 2 Methodology
We were inspired from the paper Less Is More: Linear Layers on CLIP Features as Powerful VizWiz
Model.


• Data preprocessing:The VizWiz dataset was loaded using the pandas library to analyze its
structure and organization. Following the steps outlined in the paper, we proceeded to extract the
true answers from the ”answers” column by selecting the most common answer for each label.
In cases where multiple labels had the same number of votes, we employed the Levenshtein
algorithm as a tiebreaker. This process resulted in a vocabulary size of 5458.
To optimize processing time, we chose the ViT-B/32 model, which offers reduced encoded
feature sizes of 512. The images were read from their respective paths after stratified splitting.
We applied preprocessing techniques to the images, including encoding, and saved the processed
data in a torch files using torch save. This step allowed us to reduce the time required for reading
the data during subsequent sessions. Similar procedures were performed on the questions, and the
resulting encoded data was concatenated with the images. Overall, these steps ensured that the
VizWiz dataset was appropriately prepared for further analysis and modeling. The distribution
of our answer types is displayed in Figure 1


• Model Designing and Implementation:
The model’s data loader takes encoded image-question, true answer types, true answers, and
true answer-ability as inputs. The model consists of four gates. The Main Gate includes a
1
normalization layer with a dropout rate of 0.5 and a fully connected layer with 512 output
channels. The Main Gate’s output branches into three branches: Answer Gate, Answer Type
Gate, and Answer-ability Gate. The Answer Gate employs a fully connected layer with
normalization and dropout layers. The Answer Type Gate goes through a fully connected layer,
followed by projection onto a layer matching the vocabulary size, and then a sigmoid layer. The
Answer-ability Gate passes through a fully connected layer, sigmoid activation, and softmax to
determine answer-ability. This architecture achieves excellent results in visual question answering
tasks.


• Model Evaluation: We encoded our true answers using one hot encoding, and mapped the
answer types to a numeric list , and answerability was just inserted with no changes. in Trial 1
low number of epochs(30) and no hypertuning and no regulization
