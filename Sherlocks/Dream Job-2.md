# Dream Job-2

As a Threat Intelligence Analyst investigating Operation Dream Job, you have identified that the Lazarus Group utilized a variety of custom-built malware and tools to facilitate their operations. Your task is to analyze and gather intelligence on the malware utilized by this APT.

Greeted with this challenge, I had to download a zip file provided by the challenge which contained malicious software. I had to run all the content in my VM but before I got to that a lot happened


Question 1
According to MITRE ATT&CK, what previously known malware does DRATzarus share similarities with?

Going to DRATzarus on the MITRE ATT&CK website, it is plainly stated that the malware is BankShot


Question 2
Which Windows API function does DRATzarus use to detect the presence of a debugger?

Navigating to Banshot on the MITRE ATT&CK website, the API finction used is isDebuggerPresent

Question 3

Torisma is another piece of malware used by the Lazarus Group. According to MITRE, it has encrypted its C2 communications using XOR and which other method?

Here we are introduced to another malware called Torisma, which is used by the Lazarus Groups and going to the info of this malware on MITRE ATT&CK, you would see that under the ID T1573.001, the encryption for the C2 communication is VEST-32

Question 4
Which packing method has been used to obfuscate Torisma?

The Packing method used for the malware Torisma is under the T1027.002 on the Torisma page of the MITRE ATT&CK website. It is called lz4 compression

Question 5
Analyze the provided ISO file and identify the executable contained within it?

The provided ISO file in the zip file I downloaded is where I had to start doing a lot of hands on stuffs

I first started with booting up my Ubuntu VM for analysis and then I realised that I have misplaced the password. After lots of iteration on all the possible passwords, I was able to get my Machien running. Then I got the answer to the executable. The executable is called InternalViewer.exe

Question 6
Analyze the provided ISO file and identify the executable contained within it?

Now, this was another issue. I caouldn't see the previous name of the executable file using Ubuntu which I did expect to some extent. i made some research to see if there is a way to see the previous file name and I didn't get what I was looking for.

So inorder to move on, I decided to use my Windows VM. Now I have a lot of windows VM but I don't want to use my Active Directory VM in this so I took one of the dormant VM that only God knows why I created it. Now, I have totally forgotten the password. There is no going back in this. Ok, another thing came to my mind, it is a windwos 10, try and bypass the password but this would take a lot of time to research the possible techniques behind this. So, I decided to use another VM and lo and behold, this one was a windows 7 VM and after inspecting the properties of the file, I didn't see the previous file name.

So I decided to create another Windows 10 VM and after a long wait, I got the answer.

The previous file name is SumatraPDF.exe

Question 7
According to VirusTotal, when was the EXE from the previous question First Seen In The Wild?(UTC)

Getting through this part could be quite tricky. You can't search the file name on VirusTotal and you can't search the file on google and get the info for this particular executable you are looking for. So this is where the unique identity of any file comes in, hashes.

Using the command on my Windows 10 VM
`Get-FileHash -Path "InternalViewer.exe" -Algorithm SHA256`

I got the hash ADCE894E3CE69C9822DA57196707C7A15ACEE11319CCC963B84D83C23C3EA802

pasting it in VirusTotal, I go the necessary info for the executable

the Date and time the file was first seen in the wild is 2020-08-13 08:44:50

Question 8
What packer was used to pack the executable from Question 6? (Full name)

Scrolling through the details of the executable on VirusTotal, I saw the packer is called upx. So I searched it up
For starters, a A packer is a tool that compresses an executable file into a smaller size. UPX in full is, Ultimate Packer for eXecutables. 

UPX: It is a free and open-source runtime packer that reduces program and DLL sizes by 50% to 70%

Question 9
What is the full URL found within the macro in the document Salary_Lockheed_Martin_job_opportunities_confidential.doc?

For this question, I made use of this command on my linux VM: `strings Salary_Lockheed_Martin_job_opportunities_confidential.doc | grep http'

With this I go the URL
`https://markettrendingcenter.com/lk_job_oppor.docx`

Question 10
Who is the author of the document Salary_Lockheed_Martin_job_opportunities_confidential.doc?

Using Windows 10 to check the properties of the file, I saw the author Mickey

Question 11
Who last modified the above document?

Still checking the properties, I saw that the file was last modified by challenger

Question 12
Analyze the "17.dotm" document. What is the directory where a suspicious folder was created? (Format: Give the path starting immediately after <USER>. Please pay attention to placeholder.)

Here, I discovered a new tool on Linux that I could use to analyse macro files called olevba. Using this tool, I got the Path to the suspicious folder as \AppData\Local\Microsoft\Notice

Question 13
Which suspicious file was checked for existence in that directory?

Still reading the macro code, I saw that the suspicious file is wsuser.db
