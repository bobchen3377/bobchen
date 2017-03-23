---
layout: post
title: Jenkins+Maven+Github
date: 2017-03-21
category: Jenkins
---

### 1. Install and configure Jenkins in Windows

#### **1.1) Download and install Jenkins**  
Link: <a href="https://jenkins.io/" target="_blank">https://jenkins.io/</a>
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-1.png" width="800" />

Download下来的安装包 *jenkins.msi* 自带jre以及服务。 安装后可以在Services里面找到Jenkins服务：
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-2.png" width="800" />

当然，如果觉得安装包太大，只download war包然后部署到本地Tomcat也是可以的。但是要自己配置好本机的jre。

#### **1.2) Change the service port (if you want)**  
找到 *D:\Program Files\Jenkins\jenkins.xml* 里面的  ```--httpPort=8080``` (默认是8080端口)。
如果有需要，修改成 ```--httpPort=8088``` , 然后重启Jenkins服务。

---

### 2. Setup Jenkins

#### **2.1) Logon by <a href="http://localhost:8088/" target="_blank">localhost:8088</a>**  
需要安全验证，执行以下步骤:
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-3.png" width="800" />

#### **2.2) Install Plugins**  
由于不是很清楚必须的插件，所以直接选择 *Install suggested plugins*
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-4.png" width="800" />

插件安装完后创建Admin User, 然后就可以开始使用Jenkins了。

注意，打开**Manage Jenkins**, 如果看到以下插件安装失败情况：
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-5.png" width="800" />
可用以下网站手动下载插件, 文件为hpi格式:  
Link: <a href="http://updates.jenkins-ci.org/download/plugins/" target="_blank">http://updates.jenkins-ci.org/download/plugins/</a>  
然后  
**Manage Jenkins -> Manage Plugins -> Advances -> Upload Plugin -> Choose File -> Upload**
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-6.png" width="800" />

---

### 3. Configure Git and Maven  
* **Manage Jenkins -> Global Tool Configuration -> Git**
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-7.png" width="800" />

* **Manage Jenkins -> Global Tool Configuration -> Maven**
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-8.png" width="800" />

* **Manage Jenkins -> Global Tool Configuration -> Maven Configuration**
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-16.png" width="800" />

Then click "Apply".

---

### 4. Create new Job
#### **4.1) New Item**
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-9.png" width="800" />

#### **4.2) Job Configuration**
* **General**
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-10.png" width="800" />  
https://github.com/bobchen3377/TaoTao.git 是github上的一个Maven project， 只有一个pom文件
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-11.png" width="800" />

* **Source Code Management**
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-12.png" width="800" />  
Add Credentials  
输入本地生成的Private Key
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-13.png" width="800" />

* **Build Trigger**
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-14.png" width="800" />

* **Build**
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-15.png" width="800" />

Then Click "Apply"

---

### 5. Build Now  
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-17.png" width="800" />

Check Console Output.  
Build successfully!
<img src="\assets\images\Jenkins-maven-github\Jenkins-maven-github-18.png" width="800" />
