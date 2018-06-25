
----------------------YOLO Facial recognition on Darknet Framework-------------------------------------------------

############## Detecting and recognizing face is a three step process with automatic annotation #####################

# Part 1: Capture >>
[i] To detect face from live camera feed and annotate automatically, use the .cfg and .weight files from QuanHua (https://mega.nz/#F!GRV1XKbJ!v8BCsFO8iJVNppiGXY4qMw). 

[ii] Only add those lines on src/image.c file of this fork as described bellow:

(line #223) to save .jpg images and (line #227) to save annotations on separate folders for each class (also change class number on line #229 

[iii] After modifications, run the detector from live webcam or video file using this command which specifically shows only one particular persons face.

"./darknet yolo demo cfg/yolo-face.cfg yolo-face_final.weights"

[iv] Repeat the process for every persons you want to recognize and modify training data location and class number accordingly.
About ~2k face images per person is enough to recognize individual faces but to improve accuracy, more data could be added.


## Part 2: Train>>
After capturing each persons face images and annotations on separate training folders, some data preprocessing is required for training. 
Image conversion: Convert jpg images to JPEG for Darknet framework using command [ $ mogrify -format JPEG *jpg ] according to your image data directory.

Label conversion: Convert annotations to VOC data format with scripts/convert.py script provided on scripts folder. This operation generates training image list file on the same folder for different classes. Add all those training list files into one file and point the file on cfg/face.data 

After preprocessing, modify class numbers accordingly, create data/face.names and cfg/face.data files with your desired labels and directories.

Configure src/yolo.c file and yolo_kernels, with "CLASS_NUM" parameter according to your class numbers. Comment the lines (#223 & #227) on "src/image.c" file as we are not gonna overwrite the dataset of the images captured.

Now prepare for training with a cfg file (modify #224 with filters and class numbers according to the equation > [filters = (class+coord+1)*num] for example you can modify the "yolo_face.cfg" file according to your parameters.

Now start training on GPU with a pretrained ImageNet mode (download from https://pjreddie.com/media/files/darknet19_448.conv.23 ) and run the command "./darknet detector train cfg/face.data cfg/face.cfg darknet19.448.conv.23" to initiate training. Checkpoint files will be saved on the "backup" directory specified on "face.data" file.

### Part 3 >> Deploy
After about 120k training epochs, the training weight files now should successfully detect and recognize individual faces with acceptable accuracy by running the command "./darknet detector demo cfg/face.data cfg/face.cfg your_weight_file.weights"


Here is the video demo of yolo face recognition on youtube
https://www.youtube.com/watch?v=v6uibUQILaYs
