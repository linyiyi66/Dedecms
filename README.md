Arbitrary file deletion in DedeCMS V5.7.104

Vulnerability Location:/de/album_ add.php

Lines 237-250

![image](https://user-images.githubusercontent.com/89663291/228538010-87662928-a9d4-4856-ad8b-d93797bb12d4.png)


Code Analysis:

1. The $AlbumUploadFiles data is not empty and enters the if

Remove backslashes from Striplashes and convert the content of AlbumUploadFiles to json format

3. Foreach loop files array content

4. DEDEDATA is a constant that is the current absolute path/data/uploadtmp, which will be moved to this directory

5.$tmpfile = $uploadtmp./ Our file name

6. Sections 244-249 did not perform any operations on $tmpfile

7. In line 249, $tmpfile=$uploadtmp/ Our file name is moved to the file we created in 244-249

8.250 There are no restrictions on deleting files from $tmpFile

Vulnerability recurrence:

![image](https://user-images.githubusercontent.com/89663291/228538054-03962194-27d6-4501-8347-d39ce7c94c7e.png)


Fill in the content and choose the location to upload our png image manually. Turn it upside down and click OK directly

![image](https://user-images.githubusercontent.com/89663291/228538072-d8b89a7b-36a1-4256-aed7-42723bd7a3de.png)


Click OK to display this page

![image](https://user-images.githubusercontent.com/89663291/228538108-37dfe230-ea93-4120-9c68-1743fae92d34.png)


3. Use the burp tool to capture packets, modify data, and capture data. The current page will have data transmission by default

Flip down after grabbing



Here is the obtained json format data. We can manually modify our 1-21442Y3V.png file. So here is the temporary file name uploaded above, which will be deleted. We can manually modify it
![image](https://user-images.githubusercontent.com/89663291/228538206-546703e6-7829-4e3f-ad65-6b15a55ea70a.png)


4. I created a file in the root directory of my source code to demonstrate first
![image](https://user-images.githubusercontent.com/89663291/228538456-c80474de-380f-4823-b0d3-01364f49a663.png)



5. We obtained through the above audit that the data/file name is our temporary location

Payload

Modify to..// lynn.txt
![image](https://user-images.githubusercontent.com/89663291/228538481-800120c6-b301-44f4-8c40-e8f9809553d4.png)



Submit data

Data has been deleted
![image](https://user-images.githubusercontent.com/89663291/228538502-89b0feac-e707-45d6-b8c6-fd7b0011484c.png)



Delete with arbitrary files

Let's delete the connection database file

Leverage File Manager

![image](https://user-images.githubusercontent.com/89663291/228538536-d090a854-e363-4fd4-9144-44843b9297c9.png)


We can't delete it. We'll be prompted

![image](https://user-images.githubusercontent.com/89663291/228538590-cd8e6786-ad67-467c-aa50-0b6e8420eda1.png)


Use our arbitrary deletion

Payload

../common.inc.php | to delete our connection database file
![image](https://user-images.githubusercontent.com/89663291/228538751-0bede559-9e0c-438d-820b-3c0b46040208.png)



We normally visit the page
![image](https://user-images.githubusercontent.com/89663291/228538773-04128385-07cf-477f-9fdf-1a3e8940f4b7.png)



Page display blank
