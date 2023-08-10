# Two Paths

This is my first ctf writeup that I successfully captured courtesy of deconstru.ctf

**TwoPaths (Forensics: Easy)**

![Challenge](https://github.com/momoddaeng/writeups/assets/95261836/ff0f170b-3f48-45d2-9f54-8445bafaee71)

This challenge is a mixture of image + audio stenography and cryptography.

Upon downloading the file, the image looks like this:

![image](https://github.com/momoddaeng/writeups/assets/95261836/73c71e90-7cc6-49f9-be49-7e59b55c7697)

In my Kali Machine, what I did first was to check via binwalk to see if the image contains any extractable files.


```
$ binwalk hello.jpg 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
30            0x1E            TIFF image data, little-endian offset of first image directory: 8
1801          0x709           Copyright string: "Copyright (c) 1998 Hewlett-Packard Company"
86856         0x15348         Zip archive data, at least v2.0 to extract, compressed size: 151849, uncompressed size: 152413, name: greenpill.jpg
238726        0x3A486         End of Zip archive, footer length: 22
238748        0x3A49C         Zip archive data, at least v1.0 to extract, compressed size: 41267125, uncompressed size: 41267125, name: redpill.jpg
41505892      0x2795464       End of Zip archive, footer length: 22
41506102      0x2795536       End of Zip archive, footer length: 22
```

It turns out there are two files hidden inside this image!

```
$ unzip hello.jpg 
Archive:  hello.jpg
warning [hello.jpg]:  86856 extra bytes at beginning or within zipfile
  (attempting to process anyway)
  inflating: greenpill.jpg           
 extracting: redpill.jpg  
```

These are the images that I have extracted:

![image](https://github.com/momoddaeng/writeups/assets/95261836/6970a5c3-2e9a-4658-a237-4d11ff401fcc)

So what I did is that I repeated the process that I did previously to extract any hidden files from these images. I started to go for the **greenpill.jpg** first

```
$ unzip greenpill.jpg 
Archive:  greenpill.jpg
warning [greenpill.jpg]:  89956 extra bytes at beginning or within zipfile
  (attempting to process anyway)
  inflating: flag.jpg 
```

There's another image within **greenpill.jpg**

![image](https://github.com/momoddaeng/writeups/assets/95261836/9c67208d-0ca5-4f08-9052-ef3cac94b45a)

How fun lol.

Since the flag that we are looking for is not in the **greenpill.jpg**, now I went for **redpill.jpg**. Upon checking, the hidden files look more interesting.

```
$ unzip redpill.jpg   
Archive:  redpill.jpg
warning [redpill.jpg]:  68806 extra bytes at beginning or within zipfile
  (attempting to process anyway)
  inflating: morse.wav               
 extracting: secrett.zip
```

The extracted files are morse.wav and secrett.zip.

![image](https://github.com/momoddaeng/writeups/assets/95261836/b53616a2-2f66-44de-a720-284ea469e030)

Upon extracting secrett.zip, it prompted to provide a password.

![image](https://github.com/momoddaeng/writeups/assets/95261836/f3abe42a-b965-415e-9654-3dfc89a8b383)

From here, I assumed that the password might have to be obtained from another file, which is from morse.wav, which is of course, an audio containing morse code.

I uploaded the **.wav** file to a morse code decryptor and here's what i got:

![image](https://github.com/momoddaeng/writeups/assets/95261836/4c6c42fd-059e-431f-b638-8cee383093c7)

At first, I thought the decoded string was the password itself, turns out I need to do a little bit more searching :D

![google](https://github.com/momoddaeng/writeups/assets/95261836/390683b7-ac67-47ab-a5d0-ae0ed567abcb)

Few more attempts have been tried, quite case sensitive, and then I got the password right, which is **"nebuchadnezzar"**.

![image](https://github.com/momoddaeng/writeups/assets/95261836/b9bbea56-2990-48e2-979b-ebdeefa614c2)

![image](https://github.com/momoddaeng/writeups/assets/95261836/6ae10b1e-2198-4b5b-b632-568a57d61402)

Finally got the file. Another sound file!

I took some time to try and test a few tools regarding audio stagnography, and the tool that helped me is **"DeepSound"** which can pull out hidden files out of audio files. The catch here is that DeepSound is only compatible for Windows (afaik) so I installed used the tool on my Windows Sandbox.

And voila, another hidden file uncovered **(twopathsflag.txt)**!

![image](https://github.com/momoddaeng/writeups/assets/95261836/bcd85852-c873-472f-9745-83e1ab573c97)

So I extracted the hidden file and went to the path where it was saved via Output Directory location.

Aaaand we finally got the flag.

![image](https://github.com/momoddaeng/writeups/assets/95261836/616be42d-6001-4173-b320-e0d1b60516c5)

**dsc{u_ch053_THE_cOrr3Ct_pill!}**
