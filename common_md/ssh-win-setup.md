
````cmd

ssh-keygen -t ed25519 -C "your_email@example.com"
type C:\Users\acer\.ssh\id_ed25519.pub | **clip**
ssh -T git@github.com

````

$ ssh -T git@gitlab.com
ssh -T git@github.com

Enter file in which to save the key (C:\Users\acer/.ssh/id_ed25519):

## acer

````txt
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGcF3h8FB6v7Hu6t7rOLERxGjel8zUSBAsqxzjcbN1Zk your_email@example.com
````
