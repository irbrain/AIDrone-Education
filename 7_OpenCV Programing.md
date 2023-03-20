# 1. OpemCV Programing

### 1). Show Image file 

         import cv2
         
         img_color = cv2.imread("cat on laptop.jpg", cv2.IMREAD_COLOR)
         
         if img_color is None:
            print("Can not read File")
            exit(1)
            
         cv2.nameWindow('Color')
         cv2.imshow('Color', img_color)
         
         cv2.waitKey(0)
         cv2.destroyAllWindows()
        
<br/>

### 2). Show Video

        import cv2
        
        cap = cv2.VideoCapture(0)
        
        if cap.isOpened() == False:
           print("Not open camera")
           exit(1)
           
        while(True):
           ret, img_frame = cap.read()
           if ret == False:
              print("fail to capture")
              break;
              
           cv2.imshow('Color', img_frame)
           
           key = cv2.waitKey(1)
           if key == 27:
              break
              
        cv2.release()
        cv2.destroyAllWindows()
        
<br/>

### 3). 

      
      
      
# If you want to leanr OpenCV, please refer to the link below to study
https://opencv-python.readthedocs.io/en/latest/index.html
