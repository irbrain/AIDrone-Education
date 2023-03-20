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

        
