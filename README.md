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


<h4>Step 3</h4>
I will log into the GUI of the Manager node and set up my licensing as well as create the indexer cluster. Creating an indexer cluster will invole configure and connecting the Peer nodes and Cluster members to the license manager which will colacte with my Manager node on the same instance. In the backend if you want to know the functions of your component always check "server.conf" which will be in the directory of /splunk_home/etc/system/local. As you can see before implementing my license these are the default stanza and parameters, But after i added my license a new stanza populated showing my manager node now has a license. 

<img src="https://imgur.com/YdGuYQh.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<img src="https://imgur.com/Wg9sp0Q.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<img src="https://imgur.com/Gf7Qbfs.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<h5>Step 4</h5>
Make the peers and cluster members as the license slaves/peers to the Manager node by clicking "change to peer" and adding the CM internal Ip with the splunkd port 8089

<img src="https://imgur.com/0ImbmLe.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>
<img src="https://imgur.com/Ic16k2l.png"  height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<h6>Step 5</h6>
Configure the indexer cluster in the frontend of the Cluster manager. Depending on your work enviroment this can be done in the backend as well. You'll follow the exact steps for each component except select the correct configurations for the specific component. The last image displays the successful connectivity of the indexer cluster on the Cluster manager interface.

<img src="https://imgur.com/j07GE31.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>
<img src="https://imgur.com/vzl0TWp.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>
 
<img src="https://imgur.com/fasPuAx.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

