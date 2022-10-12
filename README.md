# arUco-marker_bot
<h1>AIM:</h1>

TO MAKE A DIFFERENTIAL DRIVE WHICH FOLLOWS AN ARUKU MARKER<br>
<h2>python libraries used<h2>
open-cv,Gpio,time,numpy
<h1>PARTS USED</h1>
<p>Raspberry pi 3, 2 dc motors, 2 motor drivers , 1 camera , jumper wires , 2 wheels ,1 castor wheel, 1 battery of 12V,  power bank
</p>
<br>
<video src="https://user-images.githubusercontent.com/78871785/193892469-72d8fbdc-f13b-47ea-b7c0-9fac7b9be591.mp4">a</video>
<br>
<h2> Algorithm</h2>
<p><b>case 1 if aruku marker is visible</b> <br> the drive would minimise the angle between its direction of motion and aruku marker and stop at a certain distance and that distance is calculated using area of aruku marker</p>
<p><b> case 2 if aruku marker is not visible </b><br>
the drive would start rotation until the aruku marker is visible</p>
<h2>problems and possible solutions</h2>
<p>
<b>camera</b><br>as the camera we were using didn't had autofocus and had rather low resoultion, it had problems detecting aruku marker from a distance <br> also when bot was rotating to find aruku marker , the camera had trouble detecting moving aruku marker <br><b>solution</b> <br>instead of smooth rotations , we gave it discrete rotation which would give time to camera</p>
<br>


<p>
<b> motors of different rating<b>
  <br> ideally a feedback loop should have been used to correct it , but we weren't using much sensor so we after trial and error calculated proptionality of motors when given the same pwm and multiplied the other pwm with the same constant to correct it

```python
import cv2
import numpy as np
from time import sleep
import RPi.GPIO as GPIO
pwm1 = 18
pwm2 = 13
m1 = 17
m2 = 27
GPIO.setmode(GPIO.BCM)
GPIO.setup(m1, GPIO.OUT)
GPIO.setup(m2, GPIO.OUT)
GPIO.setup(pwm1, GPIO.OUT)
GPIO.setup(pwm2, GPIO.OUT)
pi_pwm1 = GPIO.PWM(pwm1, 1000)
pi_pwm2 = GPIO.PWM(pwm2, 1000)
pi_pwm1.start(0)				#start PWM of required Duty Cycle
pi_pwm2.start(0)
cap = cv2.VideoCapture(0)
real_area=100000
def avg(lst):
    return sum(lst) / len(lst)
def out():
    output_list=[]
    ret, frame = cap.read()
    if ret == True:
        height = frame.shape[0]
        width = frame.shape[1]
        arucoDict = cv2.aruco.Dictionary_get(cv2.aruco.DICT_6X6_50)
        arucoParams = cv2.aruco.DetectorParameters_create()
        (corners, ids, rejected) = cv2.aruco.detectMarkers(frame, arucoDict,
                                                           parameters=arucoParams)
    if ids != None:

        if ids[0] == 19:
            x = []
            y = []
            for i in corners[0][0]:
                x.append(i[0])
                y.append(i[1])
            d1x = (x[-2] - x[0]) ** 2
            d1y = (y[-2] - y[0]) ** 2
            d2x = (x[-1] - x[1]) ** 2
            d2y = (y[-1] - y[1]) ** 2
            d1 = (d1x + d1y) ** 0.5
            d2 = (d2x + d2y) ** 0.5
            area = 0.5 * (d1 * d2)

            xavg = avg(x)
            yavg = avg(y)
            disp = xavg - (width / 2)
            perc = area / real_area
            perc = 1 - perc
            if perc < 0:
                perc = 0


            output_list = [disp,perc]
    return output_list,frame
i=0
flag = False
while cap.isOpened():
    l1,frame = out()
    cv2.imshow('frame',frame)
    cv2.waitKey(1)
    print(l1)
    if l1!=[]:
        flag =True
        print('if1')
        a,b=1,1
        mspeed = l1[1]
        
        if l1[1]==0:
            mspeed1=0
            mspeed2=0
        if l1[0]<=100 and l1[0]>=-100:
            a,b=0,0
            mspeed1 = int(30* mspeed)
            mspeed2 = int(30* mspeed)
            print('f')
        elif l1[0]>100:
            a,b = 0,0
            mspeed1 = int(4 * mspeed)
            mspeed2 = int(10 * mspeed)
            print('r')
        elif l1[0]<-100:
            mspeed1 = int(12 * mspeed)
            mspeed2 = int(4 * mspeed)
            a,b=0,0
            print('l')

        sleep(0.01)

        GPIO.output(m1, a)
        GPIO.output(m2, b)

    elif l1 == [] and flag==False:
        i=0
        print('re')
        GPIO.output(m1, 0)
        GPIO.output(m2, 1)
        mspeed1=4
        mspeed2=4
    
    elif i==60:
        print('w')
        flag=False
        i=0
    
    elif l1 == [] and flag ==True:
        print('c')
        i+=1
    pi_pwm1.ChangeDutyCycle(1.3*mspeed1)  # provide duty cycle in the range 0-100
    pi_pwm2.ChangeDutyCycle(mspeed2)
cap.release()
cv2.destroyAllWindows()
```
<br> i
<h1> team </h1>
Yash Ahuja<br>Ayush Verma <br> Jyotsna Singh<br>Priyanshu Pansari </p>
