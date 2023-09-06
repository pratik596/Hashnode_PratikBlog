---
title: "How to Connect to Ubuntu EC2 Using MobaXterm Windows 10 🤗"
datePublished: Wed Jul 26 2023 12:07:15 GMT+0000 (Coordinated Universal Time)
cuid: clkjokwc5000i09jx2mq56ir3
slug: how-to-connect-to-ubuntu-ec2-using-mobaxterm-windows-10
tags: how-to-connect-to-ubuntu-ec2-using-mobaxterm-windows-1

---

**Prerequisites**

* You need to have an Ubuntu EC2 instance running.
    
* You need to have MobaXterm installed on your Windows computer.
    
* You need to have your private key file for your EC2 instance.
    

**Step 1: Download and install MobaXterm**

You can download MobaXterm from the MobaXterm website: [https://mobaxterm.mobatek.net/.](https://mobaxterm.mobatek.net/download.html) Once you have downloaded the installer, run it and follow the on-screen instructions to install MobaXterm.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690372028597/0f7108b4-6108-4c32-bc96-07e95f5f62b4.png align="center")

**Step 2: Open MobaXterm**

Once MobaXterm is installed, open it up. You should see the main MobaXterm window.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690372204612/4b246fb6-2810-4e83-9779-9db31f0f406c.jpeg align="center")

**Step 3: Create a new SSH session**

To connect to your EC2 instance, you need to create a new SSH session. To do this, click on the **Sessions** menu and then click on **New Session**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690372263176/8daa6190-b9ea-4cba-a855-71d9f33006ac.jpeg align="center")

**Step 4: Enter the connection details**

In the **New Session** dialog box, enter the following information:

* **Host:** The public DNS name or IP address of your EC2 instance.
    
* **Username:** The username you used when you created your EC2 instance.
    
* **Session type:** SSH
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690373116335/24e019c9-7c88-4103-85a5-4552c3bfd32e.jpeg align="center")
    

**Step 5: Browse to your private key file**

Click on the **Advanced** button and then browse to your private key file. Your private key file is a .pem file that you downloaded when you created your EC2 instance.

**Step 6: Click OK**

Once you have entered all of the information, click on the **OK** button. MobaXterm will connect to your EC2 instance.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690373188135/15a0f4d8-963b-4121-8322-f166e840da1c.jpeg align="center")

**Step 7: You're connected!**

If you have successfully connected to your EC2 instance, you should see a new terminal window open up. You can now start working on your EC2 instance!