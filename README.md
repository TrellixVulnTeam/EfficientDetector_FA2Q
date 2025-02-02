<html>
<body>

<h1>EfficientDetector (Updated: 2021/11/26)</h1>
<font size=3>
This is a simple python class EfficientDetector based on <a href="https://github.com/google/automl">Brain AutoML efficientdet</a>.<br>
We have added the following python classes to train custom dataset, and detect objects in an images by using a custom-trained model.<br>
<br>
- <a href="./CategorizedAPWriter.py">CategorizedAPWriter </a><br>
- <a href="./CategorizedCOCOEvaluator.py">CategorizedCOCOEvaluator(NEW)</a><br>
- <a href="./COCOMetricsWriter.py">COCOMetricsWriter</a><br>
- <a href="./DetectConfig.py">DetectConfig</a><br>
- <a href="./DetectConfigParser.py">DetectConfigParser</a><br>
- <a href="./DownloadCkpt.py">DownloadCkpt</a><br>
- <a href="./DownloadImage.py">DownloadImage</a><br>
- <a href="./EfficientDetFinetuningModel.py">EfficientDetFinetuningModel</a><br>
- <a href="./EfficientDetModelInspector.py">EfficientDetModelInspector</a><br>
- <a href="./EfficientDetSavedModelCreator.py">EfficientDetSavedModelCreator</a><br>
- <a href="./EfficientDetObjectDetector.py">EfficientDetObjectDetector</a><br>
- <a href="./EpochChangeNotifier.py">EpochChangeNotifier</a><br>
- <a href="./EvaluationResultsWriter.py">EvaluationResultsWriter</a><br>
- <a href="./LabelMapReader.py">LabelMapReader</a><br>
- <a href="./EarlyStopping.py">EarlyStopping</a><br>
- <a href="./FvalueEarlyStopping.py">FvalueEarlyStopping</a><br>
- <a href="./mAPEarlyStopping.py">mAPEarlyStopping</a><br>
- <a href="./TrainConfig.py">TrainConfig</a><br>
- <a href="./TrainConfigParser.py">TrainConfigParser</a><br>
- <a href="./TrainingLossesWriter.py">TrainingLossesWriter</a><br>

<br>
We have changed the orginal efficientdet source code to apply <b>filters</b> which enables to select some objects only specified by the filters, which is a list of category(or class) something like this <b>[car,person]</b>.<br>
<br>
- <a href="./inference2.py">inference2.py</a><br>
- <a href="./vis_utils2.py">vis_utils2.py</a><br>
<br>

We also have changed the orginal efficientdet and cocoeval source code to enable evaluation of AP, AR and F for each category(class).
<br>
- <a href="./coco_metric2.py">coco_metric2.py</a><br>
- <a href="./det_model_fn2.py">det_model_fn2.py</a><br>
- <a href="./CategorizedCOCOEvaluator.py">CategorizedCOCOEvaluator.py</a><br>
<br>
We have defined <b>EfficientDetFinetuningModel</b> class to train a cutom dataset model.<br>
- <a href="./EfficientDetFinetuningModel.py">EfficientDetFinetuningModel</a><br>
<br>
In the latest version, we also have defined the following classes to use a <b>saved_model</b> in an object detection(inference) task.<br>
The <b>EfficientDetModelInspector</b> class  is based on <b>ModelInspector</b> class in the original 
<a href="./model_inspect.py">model_inspect.py</a>.
<pre>
 <a href="./EfficientDetModelInspector.py">EfficientDetModelInspetor</a>
     |
     +- <a href="./EfficientDetSavedModelCreator.py">EfficientDetSavedModelCreator</a>
     |
     +- <a href="./EfficientDetObjectDetector.py">EfficientDetObjectDetector</a>
</pre> 
</font>
<br>

<h2>
Documentation
</h2>
<font size="4">
<a href="#1">1 EfficientDetector Inference</a><br>
<a href="#1.1">1.1 Installing EfficientDetector</a><br>
<a href="#1.2">1.2 How about darknet YOLOv4?</a><br>
<a href="#1.3">1.3 Some inference examples by EfficientDetObjectDetector</a><br>
<a href="#2">2 Customizing a visualization process in EfficientDetObjectDetector</a><br>
<a href="#2.2">2.2 How to apply filters to detected objects?</a><br>
<a href="#3">3 Training a custom dataset by EfficientDetFinetuninigModel</a><br>
<a href="#3.1">3.1 Deploy BloodCells dataset</a><br>
<a href="#3.2">3.2 Write a train.config and a default.yaml files</a><br>
<a href="#3.3">3.3 Train BloodCells dataset model by EfficientDetFinetuningModel</a><br>
<a href="#3.4">3.4 Create a saved_model from BloodCells ckpt model by EfficientDetSavedModelCreator</a><br>
<a href="#4">4 Inference by a custom-trained model</a><br>
<a href="#4.1">4.1 Write a detect.config file</a><br>
<a href="#4.2">4.2 Inference by EfficientDetObjectDetector</a><br>
</font>

<br>

<h2><a name="1">1 EfficientDetector Inference</a></h2>
<h3>
<a name="1.1">1.1 Installing EfficientDetector</a>
</h3>
<font size=2>
We have merged our previous EfficientDetector repository with the efficientdet in <a href="https://github.com/google/automl/">Google Brain AutoML</a>,  
which is a repository contains a list of AutoML related models and libraries,
and built an inference-environment to detect objects in an image by using a EfficientDet model "efficientdet-d0".<br><br>
We have been using Python 3.8.10 to run tensorflow 2.4.0 on Windows10.
To build an <b>efficientdet</b> environment on Window10, at first, please install <b>Microsoft Visual Studio 2019 Community Edition</b> for Windows10. Because the <b>efficientdet</b> needs the Python <a href="https://github.com/cocodataset/cocoapi"><b>cocoapi</b></a>  to run it , and a  C++ compiler is required to  build and intall the <b>cocoapi</b>.  <br>

<br>
Please clone <a href="https://github.com/atlan-antillia/EfficientDetector.git">EfficientDetector.git</a> in a working folder, for example <b>c:/work</b>.

<pre>
>mkdir c:/work
>cd c:/work
>git clone  https://github.com/atlan-antillia/EfficientDetector.git
>cd EfficientDetector
>pip install -r requirements.txt
>git clone https://github.com/cocodataset/cocoapi<br>
>cd cocoapi/PythonAPI<br>
<br> 
# Probably you have to modify extra_compiler_args in setup.py in the following way:<br>
# setup.py<br>
        #extra_compile_args=['-Wno-cpp', '-Wno-unused-function', '-std=c99'],<br>
        extra_compile_args=[],<br><br>
>python setup.py build_ext install<br>
>pip install pyyaml<br>
</pre>

You have to run the following command to download an <b>efficientdet-d0</b> pretrained model ckpt file:<br>
<pre>
>python <a href="./DownloadCkpt.py">DownloadCkpt.py</a>
</pre>
, which will download <i>efficientand-d0.tar.gz</i> file and expand it on the currnet directory <b>c:/work/EfficientDetector</b>.

And run the following command to download a sample image file.<br>

<pre>
>python <a href="./DownloadImage.py">DownloadImage.py</a>
</pre>
,which will download <i>img.png</i> file and save it to <b>images</b> folder under the current folder.  

<b>How to build and run a detection task</b>

<b>Step1:</b><br>
Run <i>EfficientDetSavedModelCreator.py</i> to create a saved model from the <b>efficientdet-d0</b> ckpt model.<br><br>

<pre>
>python <a href="./EfficientDetSavedModelCreator.py">EfficientDetSavedModelCreator.py</a> ./projects/coco/configs/saved_model.config
</pre>

 This command will generate a <b>saved_model</b> and save it under the <b>./projects/coco/</b> folder.<br>
<b>./projects/coco/config/saved_model.config</b><br>
<pre>
;saved_model.config

[configuration]
runmode            = saved_model
name               = efficientdet-d0
model_name         = efficientdet-d0

log_dir            = ./projects/coco/
tensorrt           = None
threads            = 0
trace_filname      = None
use_xla            = False
freeze             = False
export_ckpt        = None
delete_logdir      = True
ckpt_dir           = ./efficientdet-d0
saved_model_dir    = ./projects/coco/saved_model
hparams            = ./projects/coco/configs/default.yaml
output_image_dir   = ./projects/coco/outputs
</pre>

<b>Step2:</b><br>
Run <i>EfficientDetObjectDetector.py</i> to detect the objects in an image in the following way.<br>

<pre>
>python <a href="./EfficientDetObjectDetector.py">EfficientDetObjectDetector.py</a> ./projects/coco/configs/detect.config
</pre>
 This command will generate a detected image, on which a lot of bounding boxes ,category, names and scores will be drawn as shown below.
<br> 

<b>Detected image:</b><br> 
<img src="./projects/coco/outputs/img.png">
<br>

<br>

<b>./projects/coco/configs/detect.config</b><br>
<pre>
;detect.config
[configuration]
runmode            = saved_model_infer
model_name         = efficientdet-d0
ckpt_dir           = ./efficientdet-d0
saved_model_dir    = ./projects/coco/saved_model
output_dir         = ./projects/coco/outputs
hparams            = None
filters            = None
log_dir            = ./projects/coco/
label_map_pbtxt    = ./projects/coco/mscoco_label_map.pbtxt
tensorrt           = None
threads            = 0
trace_filname      = None
use_xla            = False
freeze             = False
export_ckpt        = None
delete_logdir      = True
batch_size         = 1
output_image_dir   = ./projects/coco/outputs
detect_results_dir = ./projects/coco/results
input_image        = ./images/*.png
input_video        = None
output_video       = None
trace_filename     = ./projects/coco/trace.log
line_thickness     = 2
max_boxes_to_draw  = 100
min_score_thresh   = 0.2
nms_method         = hard
</pre>

<br>
Unfortunately, the detected image will contain a lot of labels (classname, score) to the detected objects, 
and the original input image wll be covered by them, and it is difficult to discrimate those objects respectively.  <br>
If possible, the labels should be shown in somewhere like a listbox or written to a file something like a csv format,
 not only to be drawn on the input image as shown below <br><br>
<img src="./ref-images/html_generator.png" width="80%"><br>

See also: <a href="http://www.antillia.com/sol4py/samples/keras-yolo3/DetectedObjectHTMLFileGenerator.html">DetectedObjectHTMLFileGenerator</a>
<br>
<br>
To show the labels (id, classname, score) for the detected objects on the console window, 
we have created a new <a href="./vis_utils2.py">'./vis_utils2.py</a>
 from the original <i>automl/efficientdet/visualize/vis_utils.py.</i><br>
By using the updated visualizing function <i>draw_bounding_box_on_image_with_filters</i> in vis_utils2.py, 
you can see detailed information on the detected objects as shown below:<br><br>

<img src="./ref-images/id_classname_score_list.png" width="80%">

<br>
<h3>
<a name="1.2">1.2 How about <a href="https://github.com/AlexeyAB/darknet">darknet YOLOv4</a>?</a>
</h3>
The following is a detected image by <a href="https://github.com/atlan-antillia/YOLOv4ObjectDetector">YOLOv4ObjectDetector</a>.
<br>
>python YOLOv4ObjectDetector.py images/img.png coco_detect.config
<br>
<br>
<img src="https://github.com/atlan-antillia/YOLOv4ObjectDetector/blob/main/dataset/coco/outputs/img.png" >
<br><br>
<br>

<h3>
<a name="1.3">1.3 Some inference examples by EfficientDetObjectDetector</a>
</h3>
<pre>
>python EfficientDetObjectDetector.py ./projects/coco/configs/detect.config
</pre>

<img src="./projects/coco/outputs/ShinJuku.jpg" >
<br>
<pre>
>python EfficientDetector.py ./projects/coco/configs/detect.config
</pre>
<img src="./projects/coco/outputs/ShinJuku2.jpg" >
<br>
<pre>
>python EfficientDetector.py ./projects/coco/configs/detect.config
</pre><br>
<img src="./projects/coco/outputs/Takashimaya2.jpg">
<br><br>

You can specify <b>input_image</b> and <b>output_image_dir</b> in the <i>detect.config</i> file in the following way.
<pre>
input_image        = ./images/*.*
output_image_dir   = ./projects/coco/outputs
...
</pre>


<pre>
>python EfficientDetObjectDetector.py ./projects/coco/configs/detect.config
</pre>
This command reads all image files in the <i>input_image</i>, and
detects the objects in those images and saves detected images in the <i>output_image_dir</i> 
in <i>detect.config</i> file</i>.
<h2>
<a name="2">2 Customizing a visualization process in EfficientDetector</a>
</h2>
<h3>
2.1 How to save the detected objects information?
</h3>
 We would like to save the detected objects information as a csv file in the following format.<br>
<pre>
id, class,      score, x,   y, width, height
--------------------------------------------
1,  car,         90%, 1305, 612, 326, 281
2,  car,         87%, 1245, 896, 336, 183
3,  car,         85%, 1569, 643, 328, 261
4,  car,         84%, 211,  631, 261, 203
5,  car,         84%, 1024, 376,  91,  93
6,  car,         83%, 1173, 481, 140, 127
7,  motorcycle,  81%, 1230, 747,  89, 122
8,  car,         81%, 486,  871, 293, 205
9,  car,         81%, 1107, 327,  69,  54
10,  person,     79%, 1094, 472,  40,  72
11,  motorcycle, 78%, 1082, 717,  73, 125
12,  person,     77%, 813,  593,  54, 108
</pre>
Furthermore, the number of objects in each class (objects_stats) on the detected objects as csv below.<br>
<pre>
id class     count
-------------------
1, car,        24
2, motorcycle, 16
3, person,     52
4, bus,         2
</pre>
We have updated <i>inference</i> method of <a href="./inference2.py">ServingDriver</a> class which was derived from 
the original <i>ServingDriver</i>class of <i>automl/efficientdet/model_inspect.py</i>
to save detected objects information as a text file.<br>

Furthermore, we have created a new file <i>vis_utils2.py</i> to define some visualization functions such as a <a href="./vis_utils2.py">visualize_boxes_and_labels_on_image_array_with_filters</a>.
<br>

<h2>
<a name="2.2">2.2 How to apply filters to detected objects?</a>
</h2>

Imagine to select some specific objects only by specifying object-classes from the detected objects.<br>
To specify classes to select, we use the list format like this.
<pre>
  [class1, class2,.. classN]
</pre>
For example, you can run the following command to select objects of <i>person</i> and <i>car</i> from <i>images/img.png.</i><br>
<br>
Example 1: filters=[car,person]<br>
<b>./projects/coco/configs/detec.config</b><br>

<pre>
filters            = [car,person]
input_image        = ./images/img.png
...
</pre>

<pre>
>python EfficientDetObjectDetector.py ./projects/coco/configs/detect.config
</pre>
In this case, the detected image, objects, objects_stats filenames will become as shown below, with filters name. 
<pre>
[car,person]img.png
[car,person]img.png_objects.csv
[car,person]img.png_stats.csv

</pre>

You can see the detected image and objects information detected by above command as shown below.<br>
<br>
<br>
[car,person]img.png<br>
<img src="./projects/coco/outputs/[car,person]img.png">
<br><br>

[car,person]img.png_objects.csv<br>
<img src="./projects/coco/outputs/[car,person]img.png_objects.png" width="50%">
<br>
<br>
[car,person]img.png_stats.csv<br>

<img src="./projects/coco/outputs/[car,person]img.png_stats.png" width="40%">
<br>
<br>
Example 2: filters=[person]<br>
<b>./projects/coco/configs/detec.config</b><br>
<pre>
filters            = [person]
input_image        = ./images/ShinJuku2.jpg
...

</pre>

<pre>
>python EfficientDetObjectDetector.py  ./projects/coco/configs/detect.config [person]
</pre>
<br>

[person]ShinJuku2.jpg<br>
<img src="./projects/coco/outputs/[person]ShinJuku2.jpg">
<br>
<br>
[person]ShinJuku2.jpg.objects.csv<br>

<img src="./projects/coco/outputs/[person]ShinJuku2.jpg_objects.png" width="50%">
<br>
<br>
[person]ShinJuku2.jpg_stats.csv<br>

<img src="./projects/coco/outputs/[person]ShinJuku2.jpg_stats.png" width="40%">
<br>

<br>

<h2>
<a name="3">3 Training a custom dataset by EfficientDetFinetuninigModel</a>
</h2>
<h3>
<a name="3.1">3.1 Deploy BloodCells dataset</a>
</h3>
 We use the RoboFlow's public BCCD(BloodCells) dataset as a sample to train by EfficientDetFinetuninigModel.<br>
  Download <a href="https://public.roboflow.com/object-detection/bccd">BCCD Dataset of TFRecords format</a>,
  unzip and deploy it under efficientdet/pjojects folder as shown below<br>

<pre>
 ./projects/
   |
   +- BloodCells/
       |
       +- configs/
       |   |
       |   +- default.yaml
       |   |
       |   +- label_map.yaml
       |   |
       |   +- train.config
       |   |
       |   +- detect.config
       |   
       +- models/
       |   |
       |   +- checkpoint
       |   |
       |   +- ...
       |
       +- outputs/
       |   |
       |   +-...
       |
       +- test/
       |   |
       |   +- ...
       |   
       +- train/
       |   |
       |   +- cells.tfrecord
       |   |
       |   +- cells_label_map.pbtxt
       +- valid
       |   |
       |   +- cells.tfrecord
       |   |
       |   +- cells_label_map.pbtxt
        ...
</pre>

<br>
<img src="./ref-images/BloodCells_projects_train_folder.png" width="100%">  
<br>

<br>

<h3>
<a name="3.2">3.2 Write a train.config and a default.yaml files</a>
</h3>
In a training of a model, you may love to give a lot of training parameters on commnad line, which seems to be generally recommended way in these days.
The following example has been taken from "efficientdet/tutorial.ipynb" <br>
<pre>
python main.py --mode=train_and_eval \
    --train_file_pattern=tfrecord/{file_pattern} \
    --val_file_pattern=tfrecord/{file_pattern} \
    --model_name={MODEL} \
    --model_dir=/tmp/model_dir/{MODEL}-scratch  \
    --backbone_ckpt={backbone_name} \
    --train_batch_size=4 \
    --eval_batch_size=4 --eval_samples={images_per_epoch}  \
    --num_examples_per_epoch={images_per_epoch}  --num_epochs=1  \
    --hparams="num_classes=20,moving_average_decay=0,mixed_precision=true"
</pre>
But that is not our way, we use a configuration file to specify the training parameters, which is really a quite old-fashioned way.<br> 
You write your own training configration file "train.config" and deploy it ./projects/BloodCells/configs folder.
In this case, we have been using model "efficientdet-d0", however you may use "efficientdet-d1" to "efficientdet-d7".
Of course, you may change much better properties in [hardware] section, fitting on your machine or environment.
<br>
<pre>
;./project/BloodCells/configs/train.config
[project]
name        = BloodCells
owner       = {OWNER}
dataset     = BloodCells

[hardware]
tpu         = None
tpu_zone    = None
gcp_project = None
strategy    = None 
use_xla     = False

[model]
name        = efficientdet-d0
model_dir   = ./projects/BloodCells/models/
profile     = None
ckpt        = ./efficientdet-d0

[training]
mode                      = train_and_eval
run_epoch_in_child_process= False
batch_size                = 4
epochs                    = 100
save_checkpoints_steps    = 100
file_pattern              = ./projects/BloodCells/train/cells.tfrecord
label_map_pbtxt           = ./projects/BloodCells/train/cells_label_map.pbtxt

examples_per_epoch        = 100
;2021/11/11
hparams                   = label_map=./projects/BloodCells/configs/label_map.yaml

cores                     = 1
use_spatial_partition     = False

cores_per_replica         = 2
input_partition_dims      = [1, 2, 1, 1]
tf_random_seed            = False
use_fake_data             = False
training_losses_file      = ./projects/BloodCells/eval/train_losses.csv
num_examples_per_epoch    = 100

[validation]
file_pattern            = ./projects/BloodCells/valid/cells.tfrecord
;2021/11/15
eval_dir                = ./projects/BloodCells/eval
batch_size              = 4

eval_samples            = 100

iterations_per_loop     = 100
val_json_file           = None
eval_after_train        = True
min_eval_interval       = 180
timeout                 = None
coco_metrics_file        = ./projects/BloodCells/eval/coco_metrics.csv

;2021/11/11
coco_ap_per_class_file  = ./projects/BloodCells/eval/coco_ap_per_class.csv
disable_per_clsss_ap    = False

[early_stopping]
;metric     = fvalue
metric     = map
patience   = 10

[epoch_change_notifier]
enabled   = True
ipaddress = 127.0.0.1
port      = 9999
</pre>

Furthermore, you write default.yaml file to specify hyper parameters for a training a model.
On label_map, please refer .projects/BloodCells/valid/cells_label_map.pbtxt.<br>
<pre>
image_size: 416x416
label_map: {1: Platelets,2: RBC,3: WBC}
input_rand_hflip: true
jitter_min: 0.8
jitter_max: 1.2
learning_rate: 0.08
</pre>

Note that the following line enables evaluation of AP for each category(class).<br>
<pre>
hparams = label_map=./projects/BloodCells/configs/label_map.yaml
</pre>

<br>
<h3>
<a name="3.3">3.3 Train BloodCells dataset model by EfficientDetFinetuningModel</a>
</h3>

In efficinetdet folder, please run the following command.<br>
<br>
>python EfficientDetFinetuningModel train.config <br>
<br>
In BloodCells case, run the following command.<br>
<pre>
>python EfficientDetFinetuningModel.py ./projects/BloodCells/configs/train.config
</pre>
Please note that this <a href="./projects/BloodCells/configs/train.config">train.config</a>
uses "efficientdet-d0" pretrained model.
<br>
Console output:Training loop has been stopped by mAPEarlyStopping at epoch 21.<br>
<img src="./ref-images/BloodCells_train_early_stopping21.png" width="100%">
<br>
<br>
Evaluation results will be written to <a href="./projects/BloodCells/eval/coco_metrics.csv">an coco_metrics.csv</a> file.
<br>
<img src="./ref-images/BloodCells_coco_metrics_graph_f_mAP_epoch21.png" width="100%"><br>
<br>
Training losses will be written to <a href="./projects/BloodCells/eval/train_losses.csv">a train_losses.csv</a> file.
<br>
<img src="./ref-images/BloodCells_train_losses_epoch21.png"  width="100%"><br>
<br>
The categorized AP will be written to <a href="./projects/BloodCells/eval/coco_ap_per_class.csv"> a coco_ap_per_class.csv</a> file.

<img src="./ref-images/BloodCells_coco_ap_per_class_epoch21.png" width="80%"><br>
<br>
<br>
CategorizedCOCOEvaluator class evaluates AP, AR and F-value for each category(class) not only AP, and
 generates the following csv files. <br>
<a href="./projects/BloodCells/eval/categorized_ap.csv">AP: categorized_ap.csv</a><br><br>
<img src="./ref-images/BloodCells_categorized_ap_epoch21_categorized_cocoeval.png" width="80%"><br>
<br>
<a href="./projects/BloodCells/eval/categorized_ar.csv">AR: categorized_ar.csv</a><br><br>
<img src="./ref-images/BloodCells_categorized_ar_epoch21_categorized_cocoeval.png" width="80%"><br>
<br>
<a href="./projects/BloodCells/eval/categorized_f.csv">F : categorized_f.csv</a><br><br>
<img src="./ref-images/BloodCells_categorized_f_epoch21_categorized_cocoeval.png" width="80%"><br>
<br>
The bar graph above on <b>f</b> shows the score of WBC is higher than other categories, and suggets that 
the inference accuracy on WBC will also become a good result.   
<br>
<br>
<h3>
<a name="3.4">3.4 Create a saved_model from BloodCells ckpt model by EfficientDetSavedModelCreator</a>
</h3>
 We have to create a saved_model from a ckpt model created by <b>EfficientDetFinetuningModel</b> by using 
 <b><a href="./EfficientDetSavedModelCreator.py">EfficientDetSavedModelCreator</a> and <i>saved_model.config</i>.</b>
<br>
<b>./projects/BloodCells/configs/saved_model.config</b><br>
<pre>
;saved_model.config

[configuration]
runmode            = saved_model

name               = efficientdet-d0
model_name         = efficientdet-d0

log_dir            = ./projects/BloodCells/
label_map_pbtxt    = ./projects/BloodCells/train/cells_label_map.pbtxt
tensorrt           = None
threads            = 0
trace_filname      = None
use_xla            = False
freeze             = False
export_ckpt        = None

delete_logdir      = True
batch_size         = 1
ckpt_dir           = ./projects/BloodCells/models
saved_model_dir    = ./projects/BloodCells/saved_model
hparams            = image_size=416x416
output_image_dir   = ./projects/BloodCells/outputs
input_image        = ./projects/BloodCells/test/*.jpg
input_video        = None
output_video       = None
trace_filename     = ./projects/BloodCells/trace.log

line_thickness     = 2
max_boxes_to_draw  = 100
min_score_thresh   = 0.4

nms_method         = hard

</pre>

 Please run the following command to create a saved_model for the BloodCells ckpt model.<br>
<pre>
>python EfficientDetSavedModelCreator.py ./projects/BloodCells/configs/saved_model.config
</pre>

<br>
<h2>
<a name="4">4 Inference by a custom-trained model</a>
</h2>
<h3>
<a name="4.1">4.1 Write a detect.config files</a>
</h3>

You write your own detection configration file "detect.config" and deploy it <i>./projects/BloodCells/configs</i> folder.
<br>
<pre>
;detect.config

[configuration]
runmode            = saved_model_infer

model_name         = efficientdet-d0

log_dir            = ./projects/BloodCells/
label_map_pbtxt    = ./projects/BloodCells/train/cells_label_map.pbtxt
tensorrt           = None
threads            = 0
trace_filname      = None
use_xla            = False
freeze             = False
export_ckpt        = None

#filters = [car, person]
filters            = None

delete_logdir      = True
batch_size         = 1
ckpt_dir           = ./projects/BloodCells/models
saved_model_dir    = ./projects/BloodCells/saved_model
hparams            = ./projects/BloodCells/configs/detect.yaml
output_image_dir   = ./projects/BloodCells/outputs

detect_results_dir = ./projects/BloodCells/results

input_image        = ./projects/BloodCells/test/*.jpg
input_video        = None
output_video       = None
trace_filename     = ./projects/BloodCells/trace.log

line_thickness     = 2
max_boxes_to_draw  = 100
min_score_thresh   = 0.4

nms_method         = hard

</pre>
<br>
<h3>
<a name="4.2">4.2 Inference by EfficientDetObjectDetector</a>
</h3>
In efficinetdet folder, please run the following command.<br>
<br>
>python EfficientDetObjectDetector detect.config<br>
<br>
<pre>
; detect.config
input_image        = ./projects/BloodCells/test/BloodCell_1.jpg
...
</pre>

<br>
In BloodCells case, run the following command for a single image file defined in <i>detec.config</i>.<br>
<pre>
</pre>

<pre>
>python EfficientDetObjectDetector  ./projects/BloodCells/configs/detect.config
</pre>
We will get the following detection results.<br>
<br>
<img src ="./projects/BloodCells/outputs/BloodCell_1.jpg" width="60%">
<br>

BloodCell_1.jpg_objects.csv
<pre>
id,class, score, x,   y,   w,   h
1,  WBC,  83%, 148,  94, 171, 203
2,  RBC,  68%,  95, 182,  68,  96
3,  RBC,  64%, 313, 300,  71, 103
4,  RBC,  62%, 239, 297,  67,  89
5,  RBC,  60%,  26,   4,  77,  93
6,  RBC,  52%, 137, 359,  69,  56
7,  RBC,  51%,   1, 150,  56,  98
8,  RBC,  51%,  90,  95,  71, 112
9,  RBC,  44%, 151, 270,  67,  84
10, RBC,  44%, 349, 180,  65,  84
11, RBC,  41%, 126,  30,  85, 102
12, RBC,  40%, 344, 278,  71,  97
13, RBC,  40%,  57, 373,  83,  42
</pre>
<br>
BloodCell_1.jpg_stats.csv

<pre>
id, class, count
1,  WBC,    1
2,  RBC,   12
</pre>

<br>
You can also specifiy <i>input_image</i> in the following way in <i>detec.config</i>.<br>
<pre>
; detect.config
input_image        = ./projects/BloodCells/test/*.jpg
...
</pre>

<pre>
>python EfficientDetObjectDetector ./projects/BloodCells/configs/detect.config
</pre>
In this case, a bunch of *.jpg files under the test folder will be processed all in once.<br>

<br>
The inference resuls (detected-image.jpg, object.csv, stats.csv) will be written to outpus folder
specified in detect.config file.<br>
<br>
<img src="./ref-images/BloodCells_object_detection_on_test.png" width="100%">

</body>

</html>
