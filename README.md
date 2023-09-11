# Semiotics of the Kitchen

[Link to project video](https://youtu.be/2yFvlM5hdJo)

This project recreates Martha Rosler’s 1975 video performance Semiotics of the Kitchen from YouTube vlogs. As a parody performance of TV cooking demonstrations popularized by Julia Child in the 1960s, Semiotics of the Kitchen subverts the familiar symbols and meanings associated with everyday kitchen activities through the use of ironic sounds and gestures. Rosler assumes the role of an apron-clad housewife, progressing through the alphabet to name and gesture toward a kitchen utensil for each letter. In an explanation of the work, Rosler reflected that "when the woman speaks, she names her own oppression." 

The video is recreated in the style of popular YouTube vlogs of young, cosmopolitan women filming their daily lives and the meals they make in their kitchens and sharing them online with titles such as “What I Eat in A Day” or “Living Alone Diaries”. These videos amass thousands to millions of views, and the women engaged in mundane routines become aspirational figures for their subscribers and viewers. This project juxtaposes Rosler's staunchly feminist performance with contemporary YouTube vlogs created by women who have transformed their lives into curated aesthetics, serving as a critique of traditional women's roles in modern society.

### Technical Walkthrough

**Creating the Dataset**

The training dataset includes frames from Semiotics of the Kitchen and a collection of 6 downloaded YouTube vlogs. All the videos were broken down into frames with FFMPEG, a cross-platform framework for handling multimedia, and the frames were then cropped to 256x256 squares with dataset-tools, a Python library for normalizing image datasets. I organized the frames into the proper format for CUT model training by them into training and test sets with an 80/20 split. The details are as followed:
4417 frames from Semiotics of the Kitchen split into trainA (3533 frames) and testA (884 frames)
2943 frames from YouTube videos split into trainB (2354 frames) and testB (589 frames)

**Image generation: CUT Model Training & Testing**

I used a PyTorch implementation of the Contrastive Learning for Unpaired Image-to-Image Translation (CUT) model and fine-tuned it with a custom dataset. CUT builds on other image-to-image models such as pix2pix and CycleGan but offers faster training with patchwise contrastive learning and adversarial learning (Park et. al, 2020).

I did an initial round of training of 217 epochs but the results were unsatisfactory due to the quality of the dataset. Therefore, I recreated a new dataset taking care to only use frames from the YouTube vlogs where the vlogger was in the center of the frame with the camera directly in front of them, similar to the frame composition of Rosler’s video. The final results are from a second round of training of 85 epochs (~35 hours). I used the trained model to reproduce the input video frame by frame and stitched the images together with FFMPEG to produce the output image.

### Evaluation

In the current iteration of the work, I noticed that the model has learned to recognize two specific YouTubers from the training dataset. This is evident from the distinct figures present in the output video. To improve the performance in future iterations, I plan to optimize the training dataset by including YouTubers who share similar characteristics. Additionally, I will ensure that each video is represented equally in the dataset, avoiding any bias towards a particular individual.

While working on recreating human movements using an image-to-image approach, I was initially satisfied with the model's ability to capture significant movements from the input video. However, for future iterations, my goal is to enhance the realism of the output video by rendering the humans in a more photo-realistic manner. I also aim to refine the model's capability to replicate finer movements accurately by ensuring the correct representation of body points or landmarks.

By addressing these aspects in subsequent iterations, I anticipate achieving better results and advancing the quality of the output videos.

### Considerations & Ethical Implications

Although I extensively relied on YouTube content for this project, I want to emphasize that I did not obtain explicit consent from the YouTube creators to use their work and likeness in the development of the dataset or the training of the model. I recognize the ethical and privacy implications involved in downloading non-public domain material from the Internet, particularly when the videos encompass intimate personal video diaries that reveal sensitive information about the creators. Consequently, I have made the decision not to publish the dataset on any public platform to mitigate the risk of potential image misuse.

### Sources

FFmpeg. (n.d.). FFmpeg Documentation. Retrieved from https://ffmpeg.org/ffmpeg.html

Schultz, D. (2022). Dataset-tools [Source code]. GitHub. Retrieved from https://github.com/dvschultz/dataset-tools


**CUT Model**


Park, T., Efros, A. A., Zhang, R., & Zhu, J.-Y. (2020). Contrastive Learning for Conditional Image Synthesis. In ECCV.

Schultz, D. (n.d.) CUT [Notebook]. Retrieved from https://colab.research.google.com/github/dvschultz/Make-ML-Art-with-Google-Colab/blob/master/CUT_inference.ipynb


**Dataset Videos**


Everything has its first time. (2017, October 18). Martha Rosler - Semiotics of the Kitchen 1975 [Video file]. Retrieved from https://www.youtube.com/watch?v=ZuZympOIGC0

Michelle Choi. (2022, April 8). Living Alone Diaries | What I Eat in a Day (simple and easy meals I've been craving) [Video file]. Retrieved from https://youtu.be/4pEzJ9BFq1g

Michelle Choi. (2022, September 16). Living Alone Diaries | What I Eat in a Day (simple and easy meals)  [Video file]. Retrieved from https://www.youtube.com/watch?v=o0dtkcFLtvs

Michelle Choi. (2022, February 13). Living Alone Diaries | Simple week at home cooking, staying motivated after an emotional burnout) [Video file]. Retrieved from https://www.youtube.com/watch?v=o0dtkcFLtvs

Jellybean Celine. (2023, May 29). what i eat in a week living alone! home cooked meals, leftovers, matcha, takeout (PhD student) [Video file]. Retrieved from https://www.youtube.com/watch?v=Ds6x43mDcx4

Tâm Mai. (2023, May 5). living alone | grocery shopping, what I eat in 3 nights with 1 piece of salmon, adult money things [Video file]. Retrieved from https://www.youtube.com/watch?v=AQ9mYWS6CYw

Emily Mariko. (2021, August 2). WHAT I EAT IN A WEEK 🍴 cooking for two! [Video file]. Retrieved from https://www.youtube.com/watch?v=O0ioY7gKUvA&t=1s


**Code References**


OpenAI. (2021). ChatGPT [Computer software]. Retrieved from https://openai.com
Note: The specific lines of code from ChatGPT are credited in the notebook

Giantess Wiki. (n.d.). How to get screenshots from FFmpeg. In Miraheze. Retrieved from https://giantess.miraheze.org/wiki/How_to_get_screenshots_from_FFmpeg

Santilli, C. (2016, May 27). How to create a video from images with FFmpeg. In Stack Overflow. Retrieved from https://stackoverflow.com/questions/24961127/how-to-create-a-video-from-images-with-ffmpeg
