# Forensic Challenge: Oh Man
![image](https://github.com/user-attachments/assets/09c4b944-b19e-4431-9a34-0c4314dacd16)
<br>
<br>


## **Step 1: Analyzing the traffic**
In this challenge, we are given a pcapng file, which mostly consists of SMB traffic. After putting  `smb2` as filter, we can see some encrypted traffic.
<br>

![image](https://github.com/user-attachments/assets/6f7b3ebe-f5be-4944-ac5b-087c869e2bc3)
<br>
<br>
And because the traffic is encrypted, we are unable to export any SMB Objects.
<br>
![image](https://github.com/user-attachments/assets/2966af46-9bb9-4d78-8ed2-2babe3a4d174)
![image](https://github.com/user-attachments/assets/64fc7e75-74eb-4502-ba65-d347d717e542)


Meaning, we will be needing to decrypt the traffic to find out what was exfiltrated.
<br>
<br>

Now, in order to decrypt the traffic, we will be needing the `NT Password`. You can set this in `Edit -> Preferences -> Protocols -> NTLMSSP` .
<br>
![image](https://github.com/user-attachments/assets/d9dabdcb-e9a0-4649-a362-4bb4fc72d986)


## **Step 2: Cracking the NT Password**
To crack the NT Password, we will be needing some values. This all can be found in SMB packets. <br>
I remember doing a challenge in HTB that had a similar thing, <br> so I referenced a bit from this writeup here: [HTB Noxious Writeup](https://medium.com/@jbtechmaven/hackthebox-noxious-sherlock-walkthrough-def721fe4f50), from Task 6 to 8.
<br>
<br>

We will be needing these values, now let's find these one by one. <br>
`User::Domain:ServerChallenge:NTProofStr:NTLMv2Response`

### Finding Username and Domain
Going to `Packet 45`, you can find both of these. I chose this packet because this seemed to be the successful auth packet. (Maybe there are other reasons idk, im too noob for this) <br>
User: `Administrator`    Domain: `DESKTOP-PMNU0JK`
![image](https://github.com/user-attachments/assets/8d169a3a-2ef2-43f6-862a-065d18a5ce06)
<br>
<br>

### Finding Server Challenge
Going to `Packet 44`, you can find it there. <br> 
Server Challenge: `7aaff6ea26301fc3`
![image](https://github.com/user-attachments/assets/a34d34fc-dba3-4f22-bd62-b161b7122b93)
<br>
<br>

### Finding NTProofStr and NTLMv2Response
Going back to `Packet 45`, you can find both of these in the `NTLM Response -> NTLMv2Response` header. <br>
NTProofStr: `ae62a57caaa5dd94b68def8fb1c192f3` <br>
![image](https://github.com/user-attachments/assets/b7b3fe66-bb92-4811-862b-317dea2a4b9a)
<br>
<br>

NTLMv2Response (Removed the first 16 bytes as those are the value of NTProofStr): `01010000000000008675779b2e57db01376f686e57504d770000000002001e004400450053004b0054004f0050002d0050004d004e00550030004a004b0001001e004400450053004b0054004f0050002d0050004d004e00550030004a004b0004001e004400450053004b0054004f0050002d0050004d004e00550030004a004b0003001e004400450053004b0054004f0050002d0050004d004e00550030004a004b00070008008675779b2e57db010900280063006900660073002f004400450053004b0054004f0050002d0050004d004e00550030004a004b000000000000000000`
<br>
![image](https://github.com/user-attachments/assets/f9749eb5-984e-4afa-aeab-7d71b30974ef)
<br>
<br>
<br>

Now, we have finally got all the values we need to crack the NT Password. <br>
`Administrator::DESKTOP-PMNU0JK:7aaff6ea26301fc3:ae62a57caaa5dd94b68def8fb1c192f3:01010000000000008675779b2e57db01376f686e57504d770000000002001e004400450053004b0054004f0050002d0050004d004e00550030004a004b0001001e004400450053004b0054004f0050002d0050004d004e00550030004a004b0004001e004400450053004b0054004f0050002d0050004d004e00550030004a004b0003001e004400450053004b0054004f0050002d0050004d004e00550030004a004b00070008008675779b2e57db010900280063006900660073002f004400450053004b0054004f0050002d0050004d004e00550030004a004b000000000000000000`
<br>
<br>
We can now actually start cracking, save all the values we got into a txt file and crack it using `hashcat`. <br>
Using the command, `hashcat -a0 -m5600 hash.txt /usr/share/wordlists/rockyou.txt` <br>
<br>
<br>
We finally got the password: `password<3` <br>
![image](https://github.com/user-attachments/assets/eabde18d-14e1-495f-96a9-ec153c8cb74e)
<br>
![image](https://github.com/user-attachments/assets/7a3f19a7-49fa-46a5-bffa-d1197ee4dee7)
<br>
<br>
Using the password, let's decrypt the traffic by setting the NT Password. Click Apply then Ok. <br>
![image](https://github.com/user-attachments/assets/5ec36a80-47c7-4a8e-9914-1f0f0d150c01)
<br>
<br>
We can now export objects. Exporting all of them by clicking `Save All` .<br>
![image](https://github.com/user-attachments/assets/e4c7ac5e-22e4-447c-ac37-5c5255789dac)
<br>
<br>
<br>

## **Step 3: Analyzing the Exported Objects**
After looking through the files, the file `RxHmEj` contained something interesting. <br>
![image](https://github.com/user-attachments/assets/9a88c1fe-0e5f-4abf-918f-2d21528571df)
<br>
<br>
I was pretty sure this was the last step to the flag (hopefully). But then realized I didn't have the `scripts/restore_signature` file. <br>
At first I thought it was file exported through SMB traffic but there was no such file. <br>
Thought of manually fixing the bytes of file `20241225_1939.log` but was too noob to do it, idk how TT <br>
So, I googled it, hoping to find a script online. And found this: (https://github.com/fortra/nanodump) <br>
![image](https://github.com/user-attachments/assets/153c418b-9735-439c-88a2-2f452cc3a8f1)
<br>
![image](https://github.com/user-attachments/assets/b7c2b44d-2377-4374-8cb0-870ae7d58555)
<br>
<br>

Scrolling down, found an exact match of the command. <br>
![image](https://github.com/user-attachments/assets/66d53d82-c163-4a41-8569-e22eda3aa531)
<br>

## **Step 4 : Getting the Flag**

Then, I did the following steps to install `nanodump` to my machine and start following the commands in the `RxHmEj` file. 
<br> 
<br>
`git clone https://github.com/fortra/nanodump.git`  <br>
Then, copied the `20241225_1939.log` into the downloaded nanodump folder. 
<br>
<br>
`cd nanodump` <br>
`scripts/restore_signature 20241225_1939.log` <br>
![image](https://github.com/user-attachments/assets/adf9aed8-9da2-4932-86ea-a3a832743b70)
<br>
<br>
<br>
Scrolling down, we found the flag. `wgmy{fbba48bee397414246f864fe4d2925e4}`
![image](https://github.com/user-attachments/assets/85182994-1232-4f14-b542-52b4ef5786a0)
<br>
<br>
**Flag:** wgmy{fbba48bee397414246f864fe4d2925e4}

Honestly felt like dying, this should not be in easy category
![image](https://github.com/user-attachments/assets/be4beeed-d360-4808-930c-118b43d25aa2)





























