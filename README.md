<h1 style="color: #5e9ca0;" align=center>G'day guys! It's LAMP+WP again!</h1>
<img src="https://user-images.githubusercontent.com/101510056/179406119-32c01441-57c0-43b2-aa69-1284dc3f8c63.png">
<h2 style="color: #2e6c80;">How to use the ~ansible_wp~ project:</h2>
<p style="color: #5e9ca0;"> ❤ Lina's comment </p>

![progress](http://www.yarntomato.com/percentbarmaker/button.php?barPosition=0&leftFill=%23FF0000 "progress")

<h2 style="color: #2e6c80;">How to set up ansible server and slave host?</h2>

### Ansible-host
The layout will be a virtual machine deployed in a VMware VCloud Director virtual data center.   
Ubuntu 20.04 LTS distribution in the minimum configuration.

![изображение](https://user-images.githubusercontent.com/101510056/191590757-8f49b4ef-1fa2-48ea-acbf-70be407661ba.png)
---

<pre>
$ sudo apt-get update    
$ apt-get install python-minimal    
</pre>

By default, ssh runs on port 22. Therefore, to connect the server to the host, you need to open port 22.
<pre>
$ sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
</pre>

To find out the host address, use the following commands:

<pre>
# If the tools are not installed:
$ sudo apt install net-tools
$ sudo ifconfig | grep "inet "
</pre>

**Sample output**: 

![изображение](https://user-images.githubusercontent.com/101510056/191603275-11cb2b90-15ca-4d91-abfa-e440895dc7fe.png)

You can also use other commands to determine the IP address.

![progress](http://www.yarntomato.com/percentbarmaker/button.php?barPosition=16&leftFill=%23FF0000 "progress")

### Ansible-server:

**Ansible-server** is the system we will use to connect to Ansible hosts via SSH and manage those hosts.

The layout will be a virtual machine deployed in a VMware VCloud Director virtual data center.   
Ubuntu 20.04 LTS distribution in the minimum configuration.   

<pre>
$ sudo apt-get update
</pre>

![progress](http://www.yarntomato.com/percentbarmaker/button.php?barPosition=21&leftFill=%23FF0000 "progress")

### SSH-server

SSH or Secure Shell is a protocol for secure access from one computer to another over a network.

The OpenSSH software suite is responsible for supporting the SSH protocol on Linux. This is an open implementation of this protocol that provides all the necessary features. The OpenSSH package includes utilities for establishing a connection, transferring files, and the ssh server itself.

<pre>
$ sudo apt install openssh-server
$ sudo systemctl enable sshd
</pre>

The SSH server can authenticate users using various algorithms. The most popular is password authentication.    
Each key pair consists of a public and a private key. The secret key is stored on the client side and should not be available to anyone else.     
The public key is used to encrypt messages that can only be decrypted with the private key. This property is used for key pair authentication. The public key is uploaded to the remote server that needs to be accessed. It needs to be added to a special file ~/.ssh/authorized_keys.     

When the client attempts to authenticate with this key, the server will send a message encrypted with the public key, if the client can decrypt it and return the correct response - authentication passed. 

![изображение](https://user-images.githubusercontent.com/101510056/191597745-85fe56d6-7636-4f46-a6de-5097be5c2aaf.png)

### How to create SSH keys? 
**Generation:**    
<pre>
$ ssh-keygen
</pre>

The utility will prompt you to select the location of the keys. By default, the keys are located in the ~/.ssh/ folder. The secret key will be called id_rsa, and the public key id_rsa.pub. Then the utility will prompt you to enter a password to further encrypt the key on disk.    


![progress](http://www.yarntomato.com/percentbarmaker/button.php?barPosition=38&leftFill=%23FF0000 "progress")

### Uploading a key to a remote server

The easiest way to copy a key to a remote server is to use the ssh-copy-id utility.      
**Command syntax:**
<pre>
$ ssh-copy-id user@remote_host
</pre>

The first time you connect to the server, the system may not recognize it, so you need to enter yes. Then enter your user password on the remote server.

If this method does not work for you for some reason, you can manually copy the key over ssh. We'll create a ~/.ssh directory and then put our key into the authorized_keys file using the >> symbol, this will avoid overwriting existing keys:

<pre>
$  cat ~/.ssh/id_rsa.pub | ssh user@remote_host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
</pre>

![progress](http://www.yarntomato.com/percentbarmaker/button.php?barPosition=46&leftFill=%23FF0000 "progress")

### Checking the connection to the remote host

<pre>
$ ssh user@remote_host
</pre>


![progress](http://www.yarntomato.com/percentbarmaker/button.php?barPosition=54&leftFill=%23FF0000 "progress")

### Ansible installation 

To start using Ansible to manage your back-end infrastructure, you must first install the Ansible software and initially configure it.
<pre>
$ sudo apt-get -y install ansible
</pre>

Ansible main configuration file - **ansible.cfg** 

`File: /etc/ansible/ansible.cfg`   
>host_key_checking = False   
>inventory = /etc/ansible/hosts   

`File: /etc/ansible/hosts`
>#That's an example! Specify the IP-address and name of the required server.
>
>[webservers]       
>lamp ansible_host=192.168.1.10
>
>#That's an example! Specify the credentials for web server.
>
>[webservers:vars]     
>ansible_user=sadmin       
>ansible_ssh_pass=Aa123456      
>ansible_ssh_private_key_file=/root/.ssh/id_rsa      

![progress](http://www.yarntomato.com/percentbarmaker/button.php?barPosition=86&leftFill=%23FF0000 "progress")

## LAMP installation or ansible_wp in process

Next, you need to upload the playbook repository to the server.     
Since the file was encrypted, you can start the playbook using one of the following methods:

1. Decryption and removal of protection
<pre>
ansible-vault decrypt /etc/ansible/playbooks/playbook_name.yml
</pre>

2. Decryption for execution only
<pre>
ansible-playbook /etc/ansible/playbooks/playbook_name.yml --ask-vault-pass
</pre>

## The End

![progress](http://www.yarntomato.com/percentbarmaker/button.php?barPosition=100&leftFill=%23FF0000 "progress")

Congratulations! LAMP installed and working. Now, using the IP address of the remote host, you can get to your web page through the browser. To ensure that a website can be reached by a "human-readable" name, be sure to add the appropriate entry on your domain's DNS server.

**Standart Output**:

![изображение](https://user-images.githubusercontent.com/101510056/191627794-eb3f9a78-1e4e-4f4b-a812-69247d7f32a2.png)



