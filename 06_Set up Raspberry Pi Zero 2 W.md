# 1. Install Putty in PC

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



# 2. Set up the Raspberry pi Zero 2 W baord

### 1) Update and Upgrade 

![image](https://user-images.githubusercontent.com/122161666/224473071-1a20601c-6ff7-4b31-9580-8694308480c4.png)

### 2) Camera Option is On

![image](https://user-images.githubusercontent.com/122161666/224472969-34e57414-f232-4727-acf6-d2931d07062e.png)

![image](https://user-images.githubusercontent.com/122161666/224473018-0e025a75-99e8-470e-89d6-e04fb06109a4.png)

![image](https://user-images.githubusercontent.com/122161666/224472992-c2d1c1d6-3c88-4956-a6ab-6efd18dad982.png)

# 3. Set up Samba service for excahnge files between a PC and raspberry pi

     sudo apt install samba  samba-common-bin
     
     sudo smbpasswd -a  pi  (When User ID is pi.)
 
     write donw password  two times
     
###  samba set up 

      sudo nano /e   /samba/smb.conf
      
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

      sudo service smbd restart
      
<br/>

# 4. connecting samba on PC

![image](https://user-images.githubusercontent.com/122161666/224478786-c3a66388-0c7c-4635-ad17-22c3629327f4.png)

# 5. Install pip for python programing

     sudo apt install python3-pip
     
     pip -V
     
#### Upgrade Pip

     python3 -m pip install --upgrade pip

<br/>

# 6. Create Python Virtual Environment

     python3  -m  venv  myvenv

<br/>

# 7. Make sure the Python virtual environment starts automatically when the Raspberry Pi starts.

     sudo  nano  ~/.bashrc

     Alt + /   ( cursor move to last line)

     Write down like this : 

            if  [ -d  "$HOME/myvenv" ]; then
                 source  $HOME/myvenv/bin/activate
            fi

    Ctrl + x  ->  Y   (Saving File)

    source  ~/.bashrc  (bashrc's change comment applied )

    sudo reboot  

    
    

             
     
     
     

     
     
     






       




