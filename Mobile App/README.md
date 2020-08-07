# AR-IOT Home Automation

---
**Introduction:**

The aim of the project is to develope a android application to detect electronic/electrical devices and to provide a means to turn them on/off. The devices considered in this project are tube-light, bulb, desktop, fan. Datasets for bulb and desktop were taken from image-net. The app uses realtime database provided by Firebase.

---
**Tools Used:**

python3

Tensorflow 1.12.0

Android Studios 3.3

---
**Setup**

#### 1. Create a virtual environment(optional)
```
	mkvirtualenv android
	workon android
```

#### 2. Install tensorflow and PILLOW

```
pip install --upgrade "tensorflow==1.12.*"
pip install PILLOW
```

#### 3. Clone git repository

```git clone https://github.com/amitBalki/AR-IOT-Home-Automation.git```

#### 4. Download Dataset(Or make your own)

You can download flower dataset provided by tensorflow or you can create your own. If you are creating your own dataset make sure to properly label each folder with appropriate class name and each file must have correct extension. Copy the foder containing your dataset folders to ```AR-IOT-Home-Automation/tf_files```

```
curl http://download.tensorflow.org/example_images/flower_photos.tgz \
    	| tar xz -C tf_files
```

#### 5. Set shell Variable
	
 ```
  IMAGE_SIZE=224
  ARCHITECTURE="mobilenet_0.50_${IMAGE_SIZE}"
  set | grep IMAGE_SIZE
  set | grep ARCHITECTURE
 ```
 
#### 6. Start TensorBoard

Start tensorboard in another terminal.
```
tensorboard --logdir tf_files/training_summaries &
``` 

#### 7. Training 

Change flower_photos to name of your dataset directory. Default value number of iterations is 4000.

```
	python -m scripts.retrain \
	  --bottleneck_dir=tf_files/bottlenecks \
	  --how_many_training_steps=500 \
	  --model_dir=tf_files/models/ \
  	  --summaries_dir=tf_files/training_summaries/"${ARCHITECTURE}" \
  	  --output_graph=tf_files/retrained_graph.pb \
  	  --output_labels=tf_files/retrained_labels.txt \
  	  --architecture="${ARCHITECTURE}" \
  	  --image_dir=tf_files/flower_photos
```

#### 8. Conver model to TFlite format:- 
	
  ```
  	IMAGE_SIZE=224
	tflite_convert \
	  --graph_def_file=tf_files/retrained_graph.pb \
	  --output_file=tf_files/optimized_graph.lite \
	  --input_format=TENSORFLOW_GRAPHDEF \
	  --output_format=TFLITE \
	  --input_shape=1,${IMAGE_SIZE},${IMAGE_SIZE},3 \
	  --input_array=input \
	  --output_array=final_result \
	  --inference_type=FLOAT \
	  --input_data_type=FLOAT
```

####  9. Open Android Studios:- 

Open existing project in location: AR-IOT-Home-Automation/android/tflite

---
**Reference**

1. [TensorFlow for Poets](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/index.html#0)
2. [TensorFlow for Poets 2](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets-2-tflite/#2)
