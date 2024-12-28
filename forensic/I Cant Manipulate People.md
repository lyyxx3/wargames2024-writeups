# Forensic Challenge: I Cant Manipulate People
![image](https://github.com/user-attachments/assets/95185f22-c18c-4223-8101-87fd9f6240a4)
<br>
<br>

## **Step 1: Analyzing the traffic**
I first realize there was quite a lot of icmp packets, so I was pretty sure we need to extract the data payloads and that would be the flag. <br>
There's quite a lot other challenges in other ctf like this <br>
![image](https://github.com/user-attachments/assets/ea13cb2b-1f2f-4e43-ad88-9bac888db1bf)

Filtering the icmp protocol, I looked at the data payload of the first payload. <br>
![image](https://github.com/user-attachments/assets/7ac8b1d2-e2ce-43ba-b296-b90817e05633)
![image](https://github.com/user-attachments/assets/bbf23a30-ad73-49d3-85b2-dd832ac89e88)
<br>
<br>
It is a `W`, im sure of it now. 
<br>
<br>
<br>

## **Step 2: Extract the Data Payload**
Now, you can either view each packet and manually type in each letter one by one OR <br>
you can use `tshark` command. <br>

I used this command to extract the data payload and save it to a txt file. <br>
`tshark -r traffic.pcap -Y "icmp" -T fields -e data > output.txt` 
<br>
<br>
![image](https://github.com/user-attachments/assets/2d10916b-b5ee-431c-99e5-4e537a0cc7ed)
<br>
<br>
<br>

## **Step 3: Decoding the Flag**
The values in the output file is hex, so google a hex to ascii decoder online. <br>
I used this, https://www.rapidtables.com/convert/number/hex-to-ascii.html <br>

![image](https://github.com/user-attachments/assets/ba4c2994-f719-45ff-ae76-f135752189f5)
<br>
<br>

**Flag:** WGMY{1e3b71d57e466ab71b43c2641a4b34f4}









