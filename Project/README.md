# Water Quality Assessment using Visual Color

The project utilizes the **Arduino Nano 33 BLE Sense** that has Arm Cortex-M4 microcontroller and is powerful enough to run TensorFlow Lite Micro models and classify sensor data from the onboard sensors. The project utilizes the color sensor to obtain the RBG value of the water when the board is placed close to it.

<img src="https://user-images.githubusercontent.com/72914255/116243697-d5d59c80-a784-11eb-910c-17b9389514b8.png" width="400" height="350">

We utilize the **Arduino Create Web Editor** - a cloud-based tool used for programming Arduino boards.

<img src="https://user-images.githubusercontent.com/72914255/116244977-197cd600-a786-11eb-80cf-449639596bc4.jpg" width="500" height="350">

## Data Capture

The **object_color_capture.ino** is utilized that samples color data from objects when placed near it. The board sends the data from the color sensor as a CSV log over the USB cable.

![create_fruit_cap1](https://user-images.githubusercontent.com/72914255/116246008-2f3ecb00-a787-11eb-8258-9d30d614de63.gif)

This demonstrates the data capture setup. The data is captured from three differently colored water, i.e Clear, algae and muddy water.
![dc](https://user-images.githubusercontent.com/72914255/116246340-880e6380-a787-11eb-8367-b2b904c36d0e.JPG)

## Model Training

We utilize the Google Colaboratory for model training. Once the CSV logs for each class of water is obtained, it is uploaded to the notebook. Then,the data is parsed and prepared.

<img src="https://user-images.githubusercontent.com/72914255/116247693-bc365400-a788-11eb-9c9b-c1a47ce118d3.JPG" width="300" height="300">                     <img src="https://user-images.githubusercontent.com/72914255/116248014-0e777500-a789-11eb-8fe5-977e524d8d17.JPG" width="500" height="400"> 

## DL Model

We train a sequential TF Keras model with Dense layers as **8,8,5,3** ( 3 because of number of classes) for 500 epochs. The model is converted to TensorFlow Lite model using the TF lite Model converter. 

![dv3](https://user-images.githubusercontent.com/72914255/116248808-c86ee100-a789-11eb-8536-07132e080caa.JPG)

## TensorFLow Lite Micro model on the Arduino Nano

The TFLite model (i.e **model.h**) is imported to the Arduino Board. The **Classify_Object_Color.ino** is the script responsible for classify the water. Once, the sketch is compiled, it is uploaded to the Arduino Nano.

<img src="https://user-images.githubusercontent.com/72914255/116250101-0e787480-a78b-11eb-8bcb-7de0801f2eb0.JPG" width="700" height="350">

## Testing the DL model

We test the model on both sythetic and real-world data

![results_syn](https://user-images.githubusercontent.com/72914255/116250805-b2622000-a78b-11eb-8884-f3e731520a9d.JPG)

![Real_results](https://user-images.githubusercontent.com/72914255/116250860-c3129600-a78b-11eb-850b-8802f3a8ece7.JPG)







