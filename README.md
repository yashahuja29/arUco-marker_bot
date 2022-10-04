# aruku-marker_bot
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
<h1> team </h1>
Yash Ahuja<br>Ayush Verma <br> Jyotsna Singh </p>
