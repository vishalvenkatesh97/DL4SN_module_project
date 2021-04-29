
# Water Quality Assessment using Visual Color
***Vishal Venkatesh*** 

***Github repository:*** https://github.com/vishalvenkatesh97/DL4SN_module_project.git

***Youtube presentation:*** https://youtu.be/XhSMjPh4lFg

## Introduction

We are living in an era where qualitative assessment of natural resources like air, water, soil etc plays an important role in the health and well-being of living organisms including humans. In this project, we focus on the topic of Water Quality assessment. Water quality refers to the chemical, physical and biological characteristics of water based on the standards of its usage. Different water quality indicators are Alkalinity, color, pH, odour, hardness, dissolved oxygen content etc. Visual color serves as a physical indicator for quality of water. Different colors indicate different characteristics of water, i.e brownish for muddy water, greenish tinge for water with algae etc.

In recent times, with rise in the variety of sensors and internet of things there is a growing interest to develop systems that collect real-time data and assess the water quality. These real-time monitoring systems are deployed to measure characteristics of water using a range of sensors. Hence, this project aims to classify the quality of water based on the physical indicator, i.e visual color using a Deep learning classification model on an edge device. This project removes the hassle of manual monitoring of water quality and hence is inspired from applications of AI at edge scenarios. Also, with growing concerns of water pollution and degradation of water bodies, such an AI application would be highly beneficial. It is based on the examples where trained professionals collect water samples to assess the quality but in this case with help of an edge AI device for remote monitoring.

![dv7](https://user-images.githubusercontent.com/72914255/116253662-47feaf00-a78e-11eb-84a6-4409bccfaa4b.JPG)

## Research Question

**Automatic Classification of the quality of water based on the physical indicator, i.e visual color using a Deep learning model on an edge device**

There have been several works on IOT-based water monitoring [1], [2], [3] and the use of AI to assess water quality on different parameters [4], [5], [6]. Hence, there has been active research and the advent of cheaper, lower compute embedded devices and efficient AI algorithms is going to drive more research in such practical, monitoring applications.

## Application Overview

The project provides a data-driven approach and follows the Deep Learning workflow. The Application of classifying the visual color makes use of the Color sensor on the Arduino Nano BLE. The three essential building blocks of the project are:
-	**Data Acquisition and Labelling**: On the basis of different classes of data we aim to classify, the first step is to acquire the required data and label the classes respectively. This building block is the curation of the dataset.
-	**Training**: The Labelled dataset is utilized to train a Deep Learning model. This block also includes steps like model assessment, parameters and architectures search to produce an optimal trained model. The model is converted to TensorFlow Lite model to make it compatible to run on edge devices.
-	**Inference**: The final block deals with the inference on the Edge Device.The edge device performs processing on the edge with tasks like pre-processing of input data, execution of the model on the TF lite Interpreter, post-processing of the predictions and finally outputs the results.


![dl_f](https://user-images.githubusercontent.com/72914255/116254149-bb082580-a78e-11eb-85ef-b816eecdaae2.JPG)
![dv8](https://user-images.githubusercontent.com/72914255/116254357-e7bc3d00-a78e-11eb-90d5-3955452703f0.JPG)

## DATA

The steps involved with respect to the data for the project are as follows:

- The Arduino Nano board sends color data as a CSV log to the arduino create monitor over the USB cable using the object_color_capture.ino script.
- The three classes of water quality : Clean (clear), Muddy (Brownish), Algae (Greenish) from synthetically-colored water which has close resemblance to real water. We adopted this procedure as the acquisition of synthetic data is much easier and reduces the time spent on data collection, thereby fastening the entire Deep learning workflow.
- We acquire 500 sample data (RGB) values for each class that amounts to 1500 samples. There was no pre-processing steps like cleaning, data clipping etc applied on the dataset as we expected to create a diverse dataset that could potentially resemble real-world data.
- The individual CSV data files are uploaded to the Colab notebook. The data is parsed and prepared such that the input data and the corresponding labels are one-hot encoded and randomized. 
- Exploratory data analysis was performed on the different class data. The red, green and blue pixel intensity values for all the samples (i.e 500) for each class was visualized by means of an intensity graph. This analysis provides the estimate of the RGB pixel intensities range for different classes. Further, the color estimate of the each class is also depicted.
- The Data Split is performed to obtain a split such as 80% training data, 10% validation data, 10% test data. The data split amounts to 1200 samples utilized for training while 150 samples for validation and the remaining 150 samples for testing.

![dc](https://user-images.githubusercontent.com/72914255/116255107-919bc980-a78f-11eb-97b7-c632f1ca2418.JPG)
![data_plot](https://user-images.githubusercontent.com/72914255/116255167-a0827c00-a78f-11eb-9f31-6640dbce17f7.JPG)
![dv9](https://user-images.githubusercontent.com/72914255/116255359-cad43980-a78f-11eb-864a-2723cbac1b2c.JPG)

## MODEL

We develop a sequential model built using the Keras Framework. We design three models with different architectural parameters (i.e different dense layers) .All the models consists of Batch Normalization layer followed by the Dense layer that is helpful in reducing the problem of overfitting. The models designed are:

- Sequential model with Dense layers : **4,8,4,3**
- Sequential model with Dense layers : **8,8,5,3**
- Sequential model with Dense layers : **8,16,8,3**


The table below provides details such the parameters of the models, accuracies obtained on test set (150 samples), confusion matrices and the respective TF Lite model size. We are interested in obtaining the best accuracy on the validation set and also smaller TF Lite model size. It can be observed that **Sequential_2** ( dense layers **8,8,5,3**) obtains the best accuracy score of **98.6%** and also has a relatively small model size of **4576 bytes**. Hence, sequential model 2 is chosen as the architecture for the project as it meets the desired accuracy and size criterias.

![Model_final_table](https://user-images.githubusercontent.com/72914255/116255854-42a26400-a790-11eb-9862-c7e7ac6db272.JPG)

## EXPERIMENTS

With model architecture chosen as a sequential model with set of set Dense (8,8,5,3)and Batch Normalization layers along with Relu Activation for all layers except the last one, which is the softmax activation, that outputs the probabilities for each class. We further perform experiments to obtain the best hyper-parameters to train the network. The hyper-parameters we are interested are the Learning rate, optimizer and the batch size of data.

- **Optimizer**: We first experiment with different optimizers with the learning rate and batch size fixed as 0.001 and 4 respectively. We evaluate the optimizers on the basis of the accuracy score on the validation set. The table below depicts the accuracy scores of different optimizers and it is observed that the **RMSprop** optimizer obtains the best accuracy score.
![dv10](https://user-images.githubusercontent.com/72914255/116256161-901ed100-a790-11eb-8cb1-6de28c0cf7e0.JPG)

- **Learning Rate**: We utilize qualitative assessment to evaluate the learning rate as aspects of convergence, oscillations of the loss etc depend on the learning rate. A good learning provides optimal convergence of the network with minimal oscillations/deviations in the loss curve. We evaluate three learning rates : 0.01, 0.001 and 0.0001. The figure depicts the loss and accuracy curves for the train and validation sets. It is observed that a learning rate of 0.01 results in alot of oscillations with respect to the validation set, while learning rate of 0.0001 results in lesser oscillations but the loss values are still high at certain peaks. The learning of **0.001** has the least number of oscillations and also depicts good convergence as the loss values and accuracy is best in comparison. Hence, we select a learning rate of 0.001.
![LR_1](https://user-images.githubusercontent.com/72914255/116256634-fe639380-a790-11eb-91eb-4b3d20bd66ee.JPG)
![LR_2](https://user-images.githubusercontent.com/72914255/116256711-10453680-a791-11eb-910b-71641b97a32d.JPG)
![LR_3](https://user-images.githubusercontent.com/72914255/116256741-16d3ae00-a791-11eb-9ba3-c23657e5b2eb.JPG)

- **Batch Size**: The selection of appropriate batch size influences the update of weights and bias in the network during the back-propagation step. A smaller batch size would result update due to lesser samples and vice versa for a bigger batch size. We quantitatively evaluate different batch sizes based on the the accuracy scores. From this experimentation, a batch size of **4** depicts the highest accuracy score.
![dv11](https://user-images.githubusercontent.com/72914255/116258611-bb0a2480-a792-11eb-9c5b-339f0f5593a9.JPG)

After the experimentation step, the hyper-parameters chosen are:
- Optimizer: RMSprop
- Learning rate: 0.001
- Batch Size: 4


## RESULTS AND OBSERVATION

We test the capabilities of the of the proposed Edge AI project on synthetic and real-world examples. The results and observations were as follows:

- **Synthetic Test set**: The Deep learning classification model was accurate when evaluated on the Test set and resulted in **96%** accuracy. The model when deployed on the Arduino Nano rarely had any inaccurate predications except in the conditions when reflection of light from water resulted in occasional misclassifications. The Arduino’s color sensor was robust even when input was data was captured at different angles while testing.
![results_syn](https://user-images.githubusercontent.com/72914255/116256994-4be00080-a791-11eb-9642-4c0e3bdb1798.JPG)

- **Real-world deployment**: The Edge AI project was applied to some real world examples and it’s performance was impressive. This was done to test the ability of the network that was trained only on easily-curated synthetic dataset to how well it could generalize to the real-world setting. For each class we selected a real world example. For the clean water class, we selected an aquarium, for the algae class, the pond was considered and the muddy class, a sample of water from a nearby shallow lake was chosen. In all real world examples we tested the classification model, the predictions were accurate. The reflection of sunlight at times resulted in abnormal results but overall, it was consistent at producing the right classification outputs.
![Real_results](https://user-images.githubusercontent.com/72914255/116257106-5ef2d080-a791-11eb-94cc-55a184e0844d.JPG)

## FEEDBACK RESPONSE AND FUTURE WORK

Overall, the project was well-appreciated by the peers and the staff as it depicted practical use-case. The process of addressing the feedbacks and comments has enriched the report. Few of the main feedback received and the response to them are as follows:
- An exploratory data analysis of the RGB values of the samples is provided in order to offer more clarity with respect to the data acquired (i.e 1 pixel RGB data). One limitation of such a data could be in scenarios where the pixel intensities overlap/similar range for different classes which could lead to misclassification and in conditions in which debris or the pollutants like plastic etc could deteriorate the performance as such cases are not present in the training data acquired.
- Ideas for system integration could potential utilize a battery-powered embedded board that could transmit data either through Bluetooth/Wifi. This makes the Edge device independent of power source and also wireless making it easy to deploy without wires. A simple Web interface/mobile app could be used as an UI to display to results.
- Feedback on the model architecture and other parameters was addressed by demonstrating a detailed, systematic analysis for choice of architecture and parameters in the report.

The potential future work for the project are:
- The addition of the camera sensor along with the color sensor to identify different items like plastic, cans etc. This sensor fusion could lead to more accurate results.
- Explore Bigger models and quantization strategies in order to make the model compatible for most embedded devices.
- Convert the classification problem to a regression problem that provides quantification of water quality on a scale of 1-10.

## Bibliography

1.	Moparthi, Nageswara Rao, Ch Mukesh, and P. Vidya Sagar. "Water quality monitoring system using IoT." 2018 Fourth International Conference on Advances in Electrical, Electronics, Information, Communication and Bio-Informatics (AEEICB). IEEE, 2018.

2.	Pappu, Soundarya, et al. "Intelligent IoT based water quality monitoring system." International Journal of Applied Engineering Research 12.16 (2017): 5447-5454.

3.	Daigavane, Vaishnavi V., and M. A. Gaikwad. "Water quality monitoring system based on IoT." Department Electronics & Telecommunication Engineering, Advances in Wireless and Mobile Communications 10.5 (2017): 1107-1116.

4.	Sengorur, Bulent, Rabia Koklu, and Asude Ates. "Water quality assessment using artificial intelligence techniques: SOM and ANN—a case study of Melen River Turkey." Water Quality, Exposure and Health 7.4 (2015): 469-490.

5.	Ahmed, Ali Najah, et al. "Machine learning methods for better water quality prediction." Journal of Hydrology 578 (2019): 124084.

6.	Khan, Yafra, and Chai Soo See. "Predicting and analyzing water quality using Machine Learning: A comprehensive model." 2016 IEEE Long Island Systems, Applications and Technology Conference (LISAT). IEEE, 2016.

## Declaration of Authorship

I, Vishal Venkatesh, confirm that the work presented in this assessment is my own. Where information has been derived from other sources, I confirm that this has been indicated in the work.

***Vishal Venkatesh***

29/04/2021



