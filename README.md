# Architect Splunk Cluster Enviroment
<h1>Description</h1>
In this project I'll be providing a detail step-by step process on how to create a Splunk clustered enviroment with Clustered Indexers, Clustered Search heads, Manager nodes , 3 Universal forwarders, deployment Server , Deployer, and License manager. This project provides the viewers a better understanding of a splunk enviroment and how components communicate with each other.



<h2>Step 1</h2>
I run my Splunk Server on AWS ec2. We will create 12 instances, note 9 instance out of 12 use the same enterprise license. The universal Forwarders comes with its own license. After you launch the instances rename each instance to the respected component name. (i'll create UF and renamed them later)


<br>
<img src="https://imgur.com/IWArvag.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<br>
<img src="https://imgur.com/JyZaCih.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<br>
<img src="https://imgur.com/LXgzmyE.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>



<h3>Step 2</h3>
I will configure the each instance in my linux terminal and add the Splunk Enterprise License script and use the Universal Forwarder script soley and only for the UF.
<br>
<img src="https://imgur.com/5mCHQM8.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>
