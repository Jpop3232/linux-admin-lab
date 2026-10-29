# linux-admin-lab
## Project Purpose  
This project is designed to practice user managment, SSH skills, and Bash scripting within a Linux enviroment. This project also involves writing a bash script for automated backups. 
## Project Setup
For this project setup a VM with an Ubuntu server on it. I personally used Oracle VirtualBox. Populate the server with three other users, for the sake of clear labelling they will be lablled as "fullpermission", and "nopermission" which will indicate what level of file permission will be configured for them. All users have the password "1234", this is obviously an unacceptable password to have in real life but is done as to keep focus on the project. Now, create two groups of users, fill both with a number of users, I will label them as "allowed" and "denied". For this project to work openSSH needs to be installed on the VM. OpenSSH will also need to be installed, be sure to set the hostname as a reccognised key. 
| <img width="431" height="253" alt="image" src="https://github.com/user-attachments/assets/634c96b2-08da-435c-8564-ff7581bcda47" />    |  
|:--:|   
| *user info for fullpermission* |   

| <img width="415" height="283" alt="Screenshot 2026-10-29 103123" src="https://github.com/user-attachments/assets/801af304-900e-419a-a776-8b77ce40db26" />|  
|:--:|  
| *user info for nopermission* |   

| <img width="560" height="157" alt="Screenshot 2026-10-29 231629" src="https://github.com/user-attachments/assets/d4469a59-f551-462f-8122-8596b67e8b67" />|  
|:--:|  
| *group of allowed users* |   

| <img width="532" height="136" alt="image" src="https://github.com/user-attachments/assets/38571e2a-4fac-4c5a-998e-71444353041d" />|  
|:--:|  
| *group of denied users* |

|<img width="876" height="317" alt="image" src="https://github.com/user-attachments/assets/ffd0dd30-b18d-4084-b2b9-607de65d42d8" />|  
|:--:|  
| *OpenSSH running on the Ubuntu VM* |     
## Configuring Permissions and SSH Access 
### Single Users
Configuring permissions for users is as simple as going into the sshd_config file and adding the "AllowUsers" directive. With this directive being in the sshd_config file, nothing needs to be done users to restrict SSH access, "AllowUsers" acts as a white list and treats all users not on it as being blacklisted. Below is an example of what the modfied sshd_config file that allows fullpermission and blocks nopermission:  

|<img width="286" height="91" alt="Screenshot 2026-10-29 150917" src="https://github.com/user-attachments/assets/a3779ef9-a64f-4f50-a6e8-9351875dd8a4" />|  
|:--:|  
| *fullpermission being granted SSH access* |   

Now, when attempting to access ssh as fullpermission it looks like this: 

|<img width="710" height="438" alt="Screenshot 2026-10-29 154726" src="https://github.com/user-attachments/assets/9f994a42-0d5e-451a-9de9-3113596f1900" />|  
|:--:|  
| *fullpermission being allowed SSH access* |   

While nopermission looks like this: 

|<img width="711" height="162" alt="Screenshot 2026-10-29 155234" src="https://github.com/user-attachments/assets/3da30130-047e-405f-b595-4a1a2055e5a9" />|  
|:--:|  
| *nopermission being denied SSH access* |   

Notice that no matter how many times I input the password, I am never granted permission.
### Group of users
Like with single users, groups of users can have their SSH permissions configured by editing the sshd_config file, but instead of using the AllowUsers or DenyUsers directive the AllowGroups and DenyGroups directive is used. Being able to control the permissions of groups has high utility, especially in servers/networks with large and dynamic userbases. Instead of having to constantly go through the conig_file to set up a user's permissions everytime a new user is added to a server or whenever a user's permissions needs to be updated, they can simply be added to a group that has the configured permissions. It can also help with troubleshooting, if for whatever reason a user has an innapropriate permission status, instead of looking through a long list of individual users in a Allow/DenyUsers directory, one can simply check if the user is in the correct group for their respective ssh permission. Another important note, because AllowUsers takes precedence over AllowGroups, if a user is in AllowGroups but not AllowUsers, they will be denied SSH permission. 

|<img width="377" height="57" alt="image" src="https://github.com/user-attachments/assets/09b7ac82-c28f-4e44-85df-af8cc9d9ebef" />|  
|:--:|  
| *sshd_config with group that does not have access to the ssh server* |   

|<img width="480" height="127" alt="image" src="https://github.com/user-attachments/assets/e4c5abea-7d9e-4975-bd8d-8b235764c53d" />|  
|:--:|  
| *user on the denied list attempting to access ssh server* |  

|<img width="787" height="72" alt="image" src="https://github.com/user-attachments/assets/fbf2164d-e16d-4c54-ac5f-2bba3eb2ddcd" />|  
|:--:|  
| *new user created that will be added to the denied group* |  

|<img width="561" height="97" alt="image" src="https://github.com/user-attachments/assets/f1cfb1d7-0f49-4d34-9ef1-993ced9d6eab" />|  
|:--:|  
| *newdenied being denied permission* |  
## Automated Backup System
