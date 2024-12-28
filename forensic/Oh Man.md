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



