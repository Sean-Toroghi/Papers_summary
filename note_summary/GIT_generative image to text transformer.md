# Overall impression
This paper covers deep-learning vision-language filed. Paper provides a simplyfied architecture for generative image-to-text transformer (GIT), consists of one image encoder and one text decoder. Also authors propose a new scheme for generative-based image classification and scene text recognition. The model was tested on TextCap dataset with 
# Key ideas
Create a generative iamge-to-text transformer that consists of onve image encoder and one text decoder. Pre-training tast maps the input image to the entire associated text description with language modeling objective. 

Image decoder pre-trtained on image-text pairs based on contrastive task (eliminate dependency on the object detector). Text decoder is a transformer network to predict the associated text. Model was trained with language modeling task. 




# Technical details

- Image encoder architecture: it is based on the contrastive pre-trained model (Yuan et al. 2021; Dou et al 2021, Alayrac et al 2022). 
- The text decoder is a transformer module to predict the text description. The transformer module consists of multiple transformer blocks, each of which is composed of one self-attention layer and one feed-forward layer. The text is tokenized and embedded into D dimensions, followed by an addition of the positional encoding and a layernorm layer.


# Notes
Questions and notes on how to improve/revise the current work
