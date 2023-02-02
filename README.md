- Github: https://github.com/tzm-tora/Stroke-Based-Scene-Text-Erasing

# Paper Summary
- Paper: [Stroke-Based Scene Text Erasing Using Synthetic Data for Training](https://arxiv.org/pdf/2104.11493v3.pdf)
- *In this study, we propose a word-level two-stage scene text-erasing network that works on cropped text images.* First, it predicts the region of text stroke as a hole, then inpaints the hole according to the background.
- *The model can partially erase text instances in a scene image if text bounding boxes are provided.* The model can also work with existing scene-text detectors for automatic scene text erasing
- *We propose a practical text erasing method and a stroke-based text-erasing network using cropped text images instead of the entire image, which makes prediction of the pixel-level mask of text strokes more accurate and stable.* Benefiting from our erasing pipeline and network structure, our method can erase text instances while retaining and restoring more background details.
## Related Work
- Scene text erasing research has developed in two directions: onestep and two-step methods.
- One-step methods:
    - One-step methods do not require the input of any text location information because they combine text detection and inpainting functions into one network, making them lightweight and fast. ***The drawback is that the text localization mechanism of these networks is weak, and the text-erasing process is not controllable. To allow the network to learn the complicated distribution of scene texts, a considerable number of manually text-erased real-world images are required as training data because the text distribution of the Synth-text dataset, which is generated according to human rules, is significantly different from real-world cases.***
    - One-step methods use an end-to-end model to directly output a text-erased image without the aid of text location information.
    - Pix2Pix is a conditional generative adversarial network (cGAN) designed for general-purpose image-to-image translation tasks that can be applied to text removal.
    - ***The relatively weak text detection ability of one-step end-to-end methods can lead to excessive erasure of text-free areas and incomplete erasure of the text region. The detection ability of these networks can only be improved during end-to-end training, which would require expensive real-world training data.***
- Two-step methods:
    - The two-step approaches decompose the text-erasing task into two sub-problems: text detection and background inpainting. MTRNet inpaints the text region by manually providing a mask of the text regions. Zdenek et al. used a pretrained scene text detection model and a pretrained inpainting model to erase the text. ***This weak supervision method does not require paired training data. However, the inpainting model is trained on the Street View or ImageNet datasets, and thus, the pretrained models face domain shift problems, which cannot make a “perfect fit” in the context of scene text.***
## Dataset
- To allow the network to learn the complicated distribution of scene texts, a considerable number of manually text-erased realworld images are required as training data because *the text distribution of the Synth-text dataset, which is generated according to human rules, is significantly different from realworld cases.*
- *To compensate for the lack of pairwise real-world data, we made full use of synthetic texts.* The appearance of the synthetic texts was enhanced, and we trained our model only on the dataset generated by the improved synthetic text engine in an end-to-end fashion.
- we trained our model only on the dataset generated by the improved synthetic text engine in an end-to-end fashion.
- SCUT-Syn, ICDAR 2013 and SCUT-EnsText
- we generated one million synthetic text images and corresponding text mask images as training data from background images that did not contain text.
## Improved Text Synthesis
- *We enhance the Text Synthesis Engine to make the appearance of synthetic text instances share more similarity with real-world data.*
- Two-step methods remove the text with an awareness of text location, which can be provided by users or by a pretrained text detector.
- *We consider our proposed method to be a practical solution to scene text erasing tasks because the pretrained scene text detector and synthetic data obviate the need for paired real-world data.*
- *The advantage of our method over weak supervision methods is that using improved synthetic data for training can greatly reduce the domain shift problem during the inpainting process.*
## Methodology
- Given the source image containing the scene text and the corresponding text bounding boxes, text images are cropped from the source image. Subsequently, each cropped text image goes through the text-erasing network. The cropped text-erased images are then inserted into the source image to obtain an output image that does not contain text.
- Stroke Mask Prediction Module (SMPM):
    - An encoder–decoder FCN is adopted in this module. The input image is encoded by three down-sampling convolutional layers and four residual blocks, and the feature maps Fm are decoded by three up-sampling transposed convolutional layers to generate the text stroke mask.
    - The skip connections concatenate feature maps of the same shape between the encoder and decoder.
    - The feature maps 'F_m' output from the residual blocks are concatenated with the feature maps 'F_b' in the background inpainting module.