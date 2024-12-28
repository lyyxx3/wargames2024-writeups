## **Analyzing the traffic**
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


## **Cracking the NT Password**
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

NTLMv2Response (Removed the first 16 bytes as those are the value of NTProofStr): `01010000000000008675779b2e57db01376f686e57504d770000000002001e004400450053004b0054004f0050002d0050004d004e00550030004a004b0001001e004400450053004b0054004f0050002d0050004d004e00550030004a004b0004001e004400450053004b0054004f0050002d0050004d004e00550030004a004b0003001e004400450053004b0054004f0050002d0050004d004e00550030004a004b00070008008675779b2e57db010900280063006900660073002f004400450053004b0054004f0050002d0050004d004e00550030004a004b000000000000000000` <br>
![image](https://github.com/user-attachments/assets/f9749eb5-984e-4afa-aeab-7d71b30974ef)

<br>
<br>
<br>
Now, we have finally got all the values we need to crack the NT Password. <br>
`eeeee`










