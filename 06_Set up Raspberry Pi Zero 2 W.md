# 1. Raspberry Pi Basic Setup
### 1) Raspberry pi update and upgrade
<br/> 

       sudo  apt  update        
       sudo  apt  upgrade  -y 

<br/>

### 2) raspi-config setup
<br/>
       
       sudo  nano   raspi-config

<br/>       

#### 1) Boot Option
<br/>

![image](https://github.com/user-attachments/assets/f53304aa-5dc7-4588-808d-fb1c5708475c)

<br/>

#### 2) Interface Options 
<br/>

![image](https://github.com/user-attachments/assets/f7cdf36b-6c40-44d2-a7a1-12979eeb2b63)

<br/>

#### 3) Localisation Options 
<br/>

![image](https://github.com/user-attachments/assets/045760c3-3b15-4e27-9f3b-1f535b9c3344)

<br/>

#### 4) Advanced Options 
<br/>

![image](https://github.com/user-attachments/assets/4245f56e-95a6-4c91-b6d2-f99a04c23a34)

<br/>

# 2. Install Putty in PC (If there is no putty in your pc)

### https://www.putty.org   Download Putty 

![image](https://user-images.githubusercontent.com/122161666/224391267-617a2dac-400b-4983-8a47-6379163ee5f6.png)

<br/>

### show up below page,  choose the putty for your PC's OS

![image](https://user-images.githubusercontent.com/122161666/224391765-02c437fb-357f-4e3b-9b01-a0e164b7015f.png)

<br/>

### After installed Putty, execute the putty and write your AIDrone Wifi's Number, Click open

![image](https://user-images.githubusercontent.com/122161666/224396899-08673c1b-b173-496a-ad1f-3d1d8a5c5929.png)

<br/>

### When connected AIDrone, write down your id and password on putty

![image](https://user-images.githubusercontent.com/122161666/224398030-60dc599c-4a61-47d1-87ce-2fb846f5133f.png)

<br/>

# 3. Set up Samba service for excahnge files between a PC and raspberry pi
<br/>

     sudo apt install samba  samba-common-bin
     
     sudo smbpasswd -a  pi  (When User ID is pi.)
 
     write donw password  two times

<br/>

###  samba set up 
<br/>

      sudo  nano  /etc/samba/smb.conf

<br/>

      alt + /  -> cursor move to the last line
      
####  write like that

      [UserID]
              commnet = userID home
              path = /home/userID
              valid users = userID
              guest ok = no
              browseable = yes
              writeable = yes
              create mask = 0777
              
#### ctrl+o  -> save
#### ctrl+x  -> end

#### samba service start
<br/>

      sudo service smbd restart
      
<br/>

# 4. connecting samba on PC

<br/>

![image](https://user-images.githubusercontent.com/122161666/224478786-c3a66388-0c7c-4635-ad17-22c3629327f4.png)

<br/>





 
    
    

             
     
     
     

     
     
     






       




