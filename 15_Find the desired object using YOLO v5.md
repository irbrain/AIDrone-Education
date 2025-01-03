# Find the desired object using YOLO v5

### 1. Collecting Images with Drones:
1) Gather images captured using drones.
2) Perform labeling for each image to prepare the dataset.
3) Use tools that support labeling in the YOLO format, such as:

       LabelImg: https://github.com/heartexlabs/labelImg

### 2. Environment Setup:

#### Installing CUDA and cuDNN (For GPU Usage)
#### Installing Pytorch

      pip3 install torch torchvision torchaudio

#### Cloning the YOLOv9 Repository

     git clone https://github.com/WongKinYiu/yolov9
     cd yolov9
     pip install -r requirements.txt
     
#### Downloading Pre-trained Weights
   
      wget https://github.com/WongKinYiu/yolov9/releases/download/v0.1/yolov9-c.pt

<br/>

### 3. Dataset Structure: 


![image](https://github.com/user-attachments/assets/ba3a3fec-cc0f-4baf-88fa-9007f9f36a79)
      
<br/>

### 4. dataset.yaml  Setup:

       train: ./images/train
       val: ./images/val
       nc: 2  # Number of Classes
       names: ['class1', 'class2']  # Class Names

<br/>

### 5. Training Execution: 

       python train.py --weights yolov9-c.pt \
        
                       --cfg models/yolov9-c.yaml \
        
                       --data path/to/dataset.yaml \
                
                       --epochs 100 \
                
                       --batch-size 16 \
                
                       --img-size 640 \
                
                      --device 0  # GPU 사용시


