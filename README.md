This program uses OpenCV in order to recognize the face from video and also uses Color Filtering to detect facial hair.

This program uses 3 windows to solve this problem:

1. Image: Displays the video captured by the web cam
2. Result: Only displays the required portion of the face i.e.  nose, cheeks and chin region and rest of the region is displayed as black
3. MASK: Complete video is black except for the portion where the beard is detected from the Result


Prerequisites

This progran requires the following header files:

1.cv2

2.Numpy




This program also requires a Haar Cascade in order to recognize a face from a given image/video. 


    import cv2
    import numpy as np
    #Read XML file
    face_cascade = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')


Here, we import the required files and read the date from the XML file.



Detect a face and the required region


Then, we have to capture video from our web cam and detect a face by drawing a rectangle around the face region which is displayed by the Image Window.

After we detect the face, we only require the portion which includes the nose, cheeks and chin region which is to be displayed by the Result Window

For, this we draw an ellipse with white color around our required chin region on a black mask.

After this, we use a Bitwise operator in order to display the the required region on the ellipse instead of just white color.




    #create video capture object. 0 denotes webcam
    cap=cv2.VideoCapture(0)
    while True:
        # The input image to be recognised
        _, img=cap.read()
        #Convert the image to gray scale
        gray=cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
        #Detect the faces
        faces = face_cascade.detectMultiScale(gray,1.1,4)
                                            #Gray Scale, Image factor, Number of Neighbours
    #To construct a rectangle around the face detected
        for (x,y,w,h) in faces:
            # create a mask image of the same shape as input image, filled with 0s (black color)
            mask = np.zeros_like(img)
            # create a white filled ellipse
            mask = cv2.ellipse(mask, (int((x+w)/1.2), y+h),(69,69), 0, 0, -180, (255,255,255),thickness=-1)
            mask = cv2.cvtColor(mask, cv2.COLOR_BGR2RGB)
            # Bitwise AND operation to black out regions outside the mask
            result = np.bitwise_and(img, mask)



Detect a beard

Once we get the required region, we have to detect a beard. So in order to do this, we'll try to find black color around this region.


    #Converting the final result as HSV inorder to detect colors
            hsv_img = cv2.cvtColor(result, cv2.COLOR_BGR2HSV)
            # Draws a rectangle
            cv2.rectangle(img, (x, y), (x + w, y + h), (255, 0, 0), 2)
            # Black Color
            low_black = np.array([94, 80, 2])
            high_black = np.array([126, 255, 255])
            MASK = cv2.inRange(hsv_img, low_black, high_black)



Print Required Message

Now we want to print a message based on the output generated by MASK. For this, we will be using countNonZero() funciton from the cv2 library to check whether MASK is completely 0 i.e black or not.

    #If the MASK only has black pixels caused due to no black colour/beard in the original image
    if cv2.countNonZero(MASK) == 0:
        print("Beard Not Found")
    else:
        print("Beard Found")
        
        
Display the Results

We use imshow() function from the cv2 library to show the video results.

Once the user clicks esc, the program terminates

    #Display the Results
        cv2.imshow('Image',img)
        cv2.imshow('Result',result)
        cv2.imshow('MASK', MASK)
        #Wait for key press to close the image
        k=cv2.waitKey(30) &0xff
        #Break when esc key is pressed
        if k==27:
            break
    cap.release()



Complete Code

Here is the complete code for this problem

    import cv2
    import numpy as np
    #Read XML file
    face_cascade = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')
    #create video capture object. 0 denotes webcam
    cap=cv2.VideoCapture(0)
    while True:
        # The input image to be recognised
        _, img=cap.read()
        #Convert the image to gray scale
        gray=cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
        #Detect the faces
        faces = face_cascade.detectMultiScale(gray,1.1,4)
                                            #Gray Scale, Image factor, Number of Neighbours
    #To construct a rectangle around the face detected
        for (x,y,w,h) in faces:
            # create a mask image of the same shape as input image, filled with 0s (black color)
            mask = np.zeros_like(img)
            # create a white filled ellipse
            mask = cv2.ellipse(mask, (int((x+w)/1.2), y+h),(69,69), 0, 0, -180, (255,255,255),thickness=-1)
            mask = cv2.cvtColor(mask, cv2.COLOR_BGR2RGB)
            # Bitwise AND operation to black out regions outside the mask
            result = np.bitwise_and(img, mask)
            #Converting the final result as HSV inorder to detect colors
            hsv_img = cv2.cvtColor(result, cv2.COLOR_BGR2HSV)
            # Draws a rectangle
            cv2.rectangle(img, (x, y), (x + w, y + h), (255, 0, 0), 2)
            # Black Color
            low_black = np.array([94, 80, 2])
            high_black = np.array([126, 255, 255])
            MASK = cv2.inRange(hsv_img, low_black, high_black)
            #If the MASK only has black pixels caused due to no black colour in the original image
            if cv2.countNonZero(MASK) == 0:
                print("Beard Not Found")
            else:
                print("Beard Found")
        #Display the Respective Results
        cv2.imshow('Image',img)
        cv2.imshow('Result',result)
        cv2.imshow('MASK', MASK)
        #Wait for key press to close the image
        k=cv2.waitKey(30) &0xff
        #Break when esc key is pressed
        if k==27:
            break
    cap.release()


Result

 No Beard Detected

https://drive.google.com/file/d/19UNTnKtWG_XRdgzMpMS7rZTWJuEB0uFC/view?usp=sharing

Beard Detected

https://drive.google.com/file/d/1sak8clEk5Eb-ouqeo4_mO-2wdbFUiqh1/view?usp=sharing




