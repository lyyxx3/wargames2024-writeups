# Forensic Challenge: Unwanted Meow
![image](https://github.com/user-attachments/assets/bc5b1ffa-f179-4113-8b4b-8374e7887adb)
<br>
<br>

## **Step 1: Analyzing the Corrupted File**
We are given an image file that is corrupted, let's try checking the hex bytes to see what's wrong.
<br>
![image](https://github.com/user-attachments/assets/020e96ad-2803-402f-8a98-dec0cbbc4021)
<br>
At first glance, there's a few `meow` words scattered throughout the bytes, these could be the reason to corrupting the image file, so we can try removing all the `meow`s.
<br>
<br>

## **Step 2:Fixing the Corrupted File**
Now, we can `Ctrl + F` to find all the `meow` s and delete all of them. There are other ways of doing it like writing a python script but im lazy =w=
<br>
![image](https://github.com/user-attachments/assets/6c43f72e-5a7b-49e7-ac02-fb730a8ff288)
<br>
<br>
You should get something like this after removing them.
<br>
![secondfix](https://github.com/user-attachments/assets/c19d49b8-fb11-47d8-95ca-0db10bd6bd5b)
<br>
<br>
And we got the flag.

**Flag:** WGMY{4a4be40c96ac6314e91d93f38043a634}




