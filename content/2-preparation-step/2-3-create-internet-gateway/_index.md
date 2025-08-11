---

title : "Create Internet Gateway"
displayDate : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
----------------------

#### Create an Internet Gateway in Amazon VPC

🔒 **Steps to Perform**

1. Access the **VPC** console

   * Select **Internet Gateways** from the left menu
   * Click **Create internet gateway**

![Create Internet Gateway](/FCJ_Workshop_VuNgocQuang/images/2/2-3/0001.png?featherlight=false\&width=90pc)

2. Configure the **Internet Gateway**

   * In **Name tag**, enter `Internet Gateway`
   * Click **Create internet gateway**

![Create Internet Gateway](/FCJ_Workshop_VuNgocQuang/images/2/2-3/0002.png?featherlight=false\&width=90pc)

3. Confirm that the **Internet Gateway** was successfully created

![Create Internet Gateway](/FCJ_Workshop_VuNgocQuang/images/2/2-3/0003.png?featherlight=false\&width=90pc)

💡 **Connect to VPC**

4. Attach the **Internet Gateway** to the **VPC**

   * Click **Actions**
   * Select **Attach to VPC**
   * Choose **VPC ASG** from the list (VPC ID will be automatically filled in)
   * Click **Attach internet gateway**

![Create Internet Gateway](/FCJ_Workshop_VuNgocQuang/images/2/2-3/0004.png?featherlight=false\&width=90pc)

⚠️ **Verify Status**
5\. After successfully attaching:

* The **State** of the **Internet Gateway** will change to **Attached**
* The **IGW** is now ready to route internet traffic for the VPC

![Create Internet Gateway](/FCJ_Workshop_VuNgocQuang/images/2/2-3/0005.png?featherlight=false\&width=90pc)

---
