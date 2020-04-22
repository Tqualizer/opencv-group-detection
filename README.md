# opencv-group-detection
# Introduction
I started this project over the Easter weekend in lockdown. I built this using a Raspberry Pi 3B+ and standard IR camera. Starting with the boilerplate code here: https://github.com/EdjeElectronics/TensorFlow-Object-Detection-on-the-Raspberry-Pi/blob/master/Object_detection_picamera.py . Originally I just wanted a way of counting the ducks which swim by my window but I decided to adapt the code further and share to hopefully be of some more practical use! \

![Sample Setup | 25%](https://github.com/Tqualizer/opencv-group-detection/blob/master/Setup%20picture.jpg)

This Repo gives step by step instructions and script to show how build and deploy a mobile (Respberry Pi) object detector that can be used to detect and report groups of objects detected using TensorFlow and your chosen trained model.\ Note: This can be futher enhanced with object tracking to avoid overcounting of very slow moving or stationary groups.

![Birds Setup | 25%](https://github.com/Tqualizer/opencv-group-detection/blob/master/Birds%20example.png)

## Main features added
* Custom logging based on your object specific criteria (e.g. each time a group of more than 2 people walk past)
* Capture a photo each time your object specific criteria is triggered 
* Write the log to csv file for further analysis 


To avoid data privacy concerns in live use on people the active object viewer and photo capture should be disabled. 

The appendix includes instructions how to remotely access the  detection log in the Raspberry Pi for passive remote logging without using video or photo capture.


## The main steps are as follows:
1. **Set up and install TensorFlow and OpenCV on your Raspberry Pi** by following this great guide by Evan https://github.com/EdjeElectronics/TensorFlow-Object-Detection-on-the-Raspberry-Pi/blob/master/Object_detection_picamera.py 
The guide walks through the following steps:
    1. Update the Raspberry Pi
    1. Install TensorFlow
    1. Install OpenCV
    1. Compile and install Protobuf
    1. Set up TensorFlow directory structure and the PYTHONPATH variable
1. **Make sure your camera is configured** by following these instructions https://www.raspberrypi.org/documentation/configuration/camera.md
1. Download or clone this Repo and put the *open_cv_group_detection.py* in your /object_detection directory
1. (optional) **Customisation**
 * Select a custom model and number of objects (as described in the repo referenced in step 1)
 * Select which objects to include in the log file
```
 # pulling raw output from object detection. Creates a list of dicts 
        # with details of each of the objects meeting the threshold in a given frame.
        Validobj = [category_index.get(value) for index, value in enumerate (classes[0]) if scores [0,index]>0.5]
        
        # Choose your object
        to_detect = 'person' 
```  
   * Select which criteria to apply for logging in the evidence stamp
```
        # Creates a log if the chosen object has been detected.
        if Validobj:
            data = [i["name"] for i in Validobj]
            # If in the given frame the number of a given object detected meets the condition then a log is made   
            if data.count(to_detect)>2:
                # Writes a line with how many of the object was detected along with a timestamp
                Summary = ["There is a group of " + str(data.count(to_detect)) + " people" ,time.ctime()]
                print(Summary)
                
                evidence_stamp = [data.count(to_detect),to_detect,time.ctime()]
                output.append(evidence_stamp)
```
   * Specify the save location for the log file and image captures (by default this is the working directory).

5. **Run** the *open_cv_group_detection.py* from your /object_detection directory. To safely stop the process and save outputs press 'q' to exit.

![Multi_object](https://github.com/Tqualizer/opencv-group-detection/blob/master/Multi-object%20capture%20logging.png)


## Appendix: Remote logging (Windows 10 example)
>Picture of SSH log file
![Remote Setup](https://github.com/Tqualizer/opencv-group-detection/blob/master/Remote%20setup%20picture.jpg)1. Clone or download *mobile_group_detection.py*
1. Follow the instructions to set up SSH  here https://www.raspberrypi.org/documentation/remote-access/ssh/windows10.md
1. (Optional) Follow the instructions in the normal example above to customise the type and number of objects which trigger the logging.
1. **Run** the *mobile_group_detection.py* from your /object_detection directory. Use _Ctrl + C_ to exit the logging mode. _Please note: it may take several seconds or presses to stop the logging completely_ 


