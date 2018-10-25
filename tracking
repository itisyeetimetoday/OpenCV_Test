from collections import deque
import numpy as np
#import argparse
import imutils
import cv2
import math

currentx = 0
currenty = 0
previousx = 0
previousy = 0
framecount = 1
greenLower = (110, 10, 10)
greenUpper = (150,255, 255)

file = open("info.txt","w")
file.write("degree, x, y, distance, frame")
print("degree, x, y, distance, frame")
file.write("\n")

camera = cv2.VideoCapture("moving.MTS")
length = int(camera.get(cv2.CAP_PROP_FRAME_COUNT))
print('number of frames:'+ str(length))

for a in range (0,840):
    (grabbed, frame) = camera.read()

    frame = imutils.resize(frame, width=600)
    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

    mask = cv2.inRange(hsv, greenLower, greenUpper)
    mask = cv2.erode(mask, None, iterations=2)
    mask = cv2.dilate(mask, None, iterations=2)

    cnts = cv2.findContours(mask.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)[-2]
    center = None

    if len(cnts) > 0:
        c = max(cnts, key=cv2.contourArea)
        ((x, y), radius) = cv2.minEnclosingCircle(c)
        counter = 0
        if radius > 4:
            counter = 0
            cv2.circle(frame, (int(x), int(y)), int(radius), (0, 255, 255), 2)
            d = math.sqrt(abs((int(currentx) - int(previousx))^2+(int(currenty) - int(previousy)^2)))
            x = round(x, 3)
            y = round(y, 3)
            previousx = currentx
            previousy = currenty
            currentx = x
            currenty = y
            temp = (int(currentx) - int(previousx))
            #print(temp)
            if temp == 0:
                print("undef"+ ", " + str(round(x,3))+ ", " + str(round(y,3)) + "," + str(round(d,3))+ ", " + str(framecount))
                file.write('undef'+", " + str(round(x,3))+ ", " + str(round(y,3)) + "," + str(round(d,3))+ ", " + str(framecount))
            else:
                myradians = math.atan2(int(currenty)-int(previousy), temp)
                angle = math.degrees(myradians)

                print(str(round(angle,0))+ ", " + str(round(x,3))+ ", " + str(round(y,3)) + "," + str(round(d,3))+ ", " + str(framecount))
                file.write(str(round(angle,0))+ ", " + str(round(x,3))+ ", " + str(round(y,3)) + "," + str(round(d,3))+ ", " + str(framecount))
            #file.write(", " + str(round(x,2))+ ", " + str(round(y,2)) + "," + str(round(d,2))+ ", " + str(framecount))
            file.write('\n')
            #print(x,y)
            #print(str(d*60)+" pixels per second")
        else:

            print('lost ball')
        framecount = framecount+ 1
    #cv2.imshow("Frame", frame)
    #cv2.imshow("Mask", mask)

    key = cv2.waitKey(1) & 0xFF

    if key == ord("q"):
        break
print('[end]')
file.write('[end]')
file.close()
camera.release()
cv2.destroyAllWindows()
