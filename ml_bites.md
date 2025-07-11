# Foundation models
These are pre-trained models that can be fine-tuned for specific tasks. They are typically large and trained on diverse datasets. They are not generated to perform any specific task but rather are intended to be further fine tuned to specific tasks.

## Single cell foundation models
- scGPT
- geneFormer
- OmniPert
- GREmLN

## Downstream fine tuning
Some models like geneFormer are provided with specific code to fine-tune the model on specific tasks. This is typically done by providing a dataset and running a training script that adjusts the model weights based on the new data.

Fine-tuning seems to involve adjusting the weights of the model using a small training dataset that is specific to the task at hand. This process allows the model to adapt its pre-trained knowledge to perform well on the new task.

Another approach is Parameter-Efficient Fine-Tuning (PEFT) where only a small number of parameters are updated during fine-tuning. This is useful for adapting large models to specific tasks without the need for extensive computational resources. Huggingface has a library called `peft` that provides tools for this type of fine-tuning. This can sometimes involve inserting layers between layers internal to the model.

A third approach is to add a "head" to the model where the foundation model is not modified during training but rather new layers are added downstream of the model. This minimizes the number of parameters that need to be trained, avoids messing with the foundation model and avoids "catastrophic forgetting" where the model forgets what it learned during pre-training. To do this you basically are just using the model as-is, and then passing it's output to a new model to train it on the specific task. This is often referred to as "using the model as a feature extractor". This generally seems like it's just extracting embeddings generated from the foundation model and leaves me wondering what the benefit is over PCA or MDS or something. 

