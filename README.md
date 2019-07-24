# Dicom Remote Image Analyzer in the Cloud (DIRAC)

Dicom Remote Image Analyzer in the Cloud (DIRAC) is prototype of Dicom-to-STL cloud application and cloud storage server.

## Background

Interregional collaboration among the VA medicals centers has become an important requirement for high quality health care services.  Many medical centers in different regions cannot communicate and share DICOM and STL imaging for collaboration.  To obtain a DICOM image from one region to another, medical personels must send a request and await the DICOM files through postal mail.  

## Proposal

The proposal is to create an initial web-based prototype as a cloud application and storage solution. The web-based approach leverages the computing power of the cloud, and less on individual machines like the desktop-based approach. By placing most computing tasks in the cloud, medical sites can mitigate challenges due to limited disk space, memory, and computational power on their local computers.  Furthermore, other universities have used the web-based approach successfully. [^2] 

### Requirements 

1. Pull DICOM images form an image repository to a cloud endpoint in AWS:   
+ VistA Imaging   
+ PACS 
+ Direct from the imgaging modality (via Compass Router)    

2. For this prototype, only pull from one image repository from the image repository locally at the hospital site to the cloud endpoint in AWS (S3 file storage).  
3. Once the DICOM images are in S3 bucket, trigger the EC2 application to process the DICOM files by extracting relevant meta information pertaining to each DICOM file--patient info, date of upload, and convert each file to STL format.  
4. Store the meta information, segmented STL file, and DICOM file into another S3 bucket.
5. Display all uploaded files in the centralized S3 file storage to the web interface in the EC2 application.
6. Allow the authenticated user to download the DICOM, STL, and meta information on the S3 centralized S3 file storage.  The user should view this option in a use-friendly web interface.
7. [Optional Feature] The user can preview the STL image in 3D format before downloading.
8. [Optional Feature] Uses Machine Learning to automate the identification and classification of DICOM images into human body parts and organs.

### Current Technologies 
 
#### 3D printing on Desktop-based application 

These deskop-based applications are currently used by the VA. 
+ Mimics (by Materialise)
+ Dicom2print (by 3D Systems)
+ 3D Slicer (freeware created by Brigham and Women’s Hospital).
+ GE AW VolumeShare 7

### Short List of Potential Technologies for Web-Based Application 

There is no known web-based, cloud solution at the VA.  Here are some proposed technologies, toolkits, and/or frameworks to research and integrate with the EC2 cloud application:    

+ WebGL -  WebGL will make possible to render 3D models in real time on the web browser with the computational capabilities of the new smartphones and tablets.          

+ Amazon Simple Storage Service (S3) - allows cloud data storage of large files.  Users can upload files up to 5TB in per single PUT upload, or in multipart for files larger than 5TB. [^1] It also offers easy solutions to develop HIPAA compliant medical applications. The typical DICOM medical image is approximately a gigabyte per subject. Basically, the Amazon infrastructure offers solutions for: Identification & Authentication, Authorized Privileges & Access Control, Confidentiality, Integrity, Accountability, Security and Protection, Disaster Recovery.    

+ Amazon Elastic Compute Cloud (Amazon EC2) - The Virtual machine to run the main application that controls the logic of authentication user login,intaking user file uploads, direct the uploads to S3 bucket, extract meta information and segment DICOM to STL, display existing STL files repository in S3 to the web browser, and signing out user.  

+ [AMI Medical Imaging (AMI) JS ToolKit](https://github.com/FNNDSC/ami) - Open-sourced project from Boston Hospital to display and convert DICOM to STL in JavaScript programming language.   



### Cloud Infrastructure

The DIRAC web-based prototype uses a client – server architecture, where the server is in the cloud (Amazon S3 and EC2). 

![web app dicom](img/web-app-dicom.png)   

1) Allow a personel from a single test center within the VA hospital network to upload the DICOM file to the cloud DICOM application manager (DAM) that is served in the Ec2 .  

2) The DAM, then processes the DICOM files by relevant meta information pertaining to each DICOM file--patient info, date of upload, and convert each file to STL format.  

3) Display all uploaded STL files onto a HTML5-supported browser with WebGL capability.   
4) allow the authenticated personel to download a STL file of choice onto their local machine.    

The prototype platform extracts and processes relevant meta information about the patient from the DICOM file, segment the DICOM files into STL format, and stores meta information along with both the DICOM and STL format into AWS S3 bucket as a fileserver.    

The prototype has a business layer which allows any authenticated user within the VA internal network to view a list of existing uploads, and download STL files from the centralized AWS S3 bucket.  

### Application Architecture 



### User Stories

Main user: medical staff within a single test region in the VA hospital network 

As a user I can ...  

+ login to the AWS cloud application with a given credential on a web portal 
+ upload a DICOM image to the server   
+ view a list if preuploaded DICOM images name in the centralized file server on a web page    
+ preview the STL image as 3d rendering on the web browser   
+ select an from the list and download the file in DICOM and STL formats, along with their meta information  
+ logout of the system   


## Challenges Limitation

The AWS cloud application running in an EC2 instance requires access to DICOM file storage on the medical facility.  There are currently 3 possible endpoints:    

1. VistaImaging Server     	
+ permissions need to pull files     
+ human involvement to pull and convert image files after scanner and MRIs   
+ may require DICOM conformance statement 
		
2. Modalities (image machines iself)     
+ access to the compass router    
		
3. PAC platform     
+ The PACS platforms in the cloud only consider data storage. Data must be stored and accessed via HIPAA (Health Insurance Portability and Accountability Act of 1996)   

4. Aggressive Timeline to Launch 

+ October 1st is an ambitious launch date, considering so many unknowns on integration between local DICOM file storage and cloud application and storage.  
+ There is only one software developer on the team with 50% of contracted allocation to the project.  

5. Unclear technical assessment for integration between the cloud layer and the external DICOM server.  

6. Possible conflict of interest (COI)
+ This project is currently undergoingthe VIPR process to receive funding and becoem an official project. If we create a prototype, other contractors may not bid on this project.  

## References

[^1]: [Amazon S3 Frequently Asked Questions](https://aws.amazon.com/s3/faqs/)   
[^2]: [Server-based Approach to Web Visualization of Integrated Three-dimensional Brain Imaging Data](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC551546/). The University of Washington has already adopted web-based applications in the departments of Psychology, Speech & Hearing Science, Neurological Surgery, and Radiology.
