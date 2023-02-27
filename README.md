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



<h3>Step 2</h3>
I will configure the instance in my linux terminal and add the Splunk Enterprise License script and use the Universal Forwarder script soley and only for the UF.
<br>
<img src="https://imgur.com/5mCHQM8.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>


<h4>Step 3</h4>
I will log into the GUI of the Manager node and set up my licensing as well as create the indexer cluster. Creating an indexer cluster will involve configuring and connecting the Peer nodes and Cluster members to the license manager, which will colocate with my Manager node on the same instance. In the backend, if you want to know the functions of your component, always check "server.conf" which will be in the directory of /splunk_home/etc/system/local. As you can see before implementing my license, these are the default stanza and parameters. But after i added my license, a new stanza populated showing my manager node now has a license. 

<br>
<img src="https://imgur.com/YdGuYQh.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<br>
<img src="https://imgur.com/Wg9sp0Q.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<br>
<img src="https://imgur.com/Gf7Qbfs.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<h5>Step 4</h5>
Make the peers and cluster members as the license slaves/peers to the Manager node by clicking "change to peer" and adding the CM internal Ip with the splunkd port 8089

<br>
<img src="https://imgur.com/0ImbmLe.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>
<br>
<img src="https://imgur.com/Ic16k2l.png"  height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<h6>Step 5</h6>

Configure the indexer cluster in the frontend of the Cluster manager. Depending on your work environment, this can be done in the backend as well. You'll follow the exact steps for each component except select the correct configurations for the specific component. The last image displays the successful connectivity of the indexer cluster on the Cluster manager interface.

<br>
<img src="https://imgur.com/j07GE31.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>
<br>
<img src="https://imgur.com/vzl0TWp.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>
 
<br>
<img src="https://imgur.com/fasPuAx.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<h7>Step 6</h7>
We are going to configure a TCP input on all indexers to receive logs from the other nodes. The path will be /opt/Splunk/etc/system/local then you will "vi inputs. conf" and paste this stanza with the parameter.

<img src="https://imgur.com/jDi0bX6.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<h8>Step 7</h8>

Time to configure the Universal forwarder. Since the UF doesn't have a GUI everything will be done in the backend of the nodes. "cd /opt/Splunk/etc/system/local then vi outputs.conf " here we will create a stanza to help the UF determine where to send the data to. You can as well create outputs. conf on every component except the indexers, and send each component's internal logs to the indexer with the stanza in the second image. Make sure you include all indexers' internal IP addresses where it says "server"

<br>
<img src="https://imgur.com/yd3SL4T.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<br>
<img src="https://imgur.com/VmGvCRo.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>


<h9>Step 8</h9>
Once you've sent the CM, cluster members, and Universal forwarder's internal logs to the indexer, log into one of the SH GUI and run this search to ensure the indexer is receiving each component's logs.

<br>
<img src=https://imgur.com/GJd3oNT.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>


<h10>Step 9</h10>

Configure the deployer in the backend and add the stanza for license and configure the shclustering

<br>
<img src="https://imgur.com/sEPYiLX.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<h11>Step 10</h11>

After configuring the Deployer to communicate with the Cluster members, we want to bootstrap the captain to make one of the members a designated captain by running the commands displayed in the image which will include all of the cluster member's internal IP. As you can see I ran the " show shcluster-status" to provide me information about who is the captain and the status of all my cluster members.

<br>
<img src="https://imgur.com/Sl4G4HO.png"  height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<br>
<img src="https://imgur.com/X8CCj68.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<h12>Step 11</h12>

We will configure the monitoring Console so we change the topology of Splunk enterprise deployment from standalone to Distributed. Add the necessary components in the "distributed search" page and add their internal IP.

<br>
<img src="https://imgur.com/lJ7sadW.png"  height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<br>
<img src="https://imgur.com/ZCmJrTR.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<br>
<img src="https://imgur.com/QIXgYpu.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br>

<br>
<img src="https://imgur.com/2LOLAaX.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br> 


<h13>Step 12</h13>

I'll configure my Universal Forwarders to become deployment clients for the Deployment Server so they can phone home by running a simple command on all of my forwarders. Always restart the instance after you push a configuration. In a work environment that, not the best practice, in a work environment you must request permission to restart anything!!

<br>
<img src="https://imgur.com/S5mpyX4.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br> 

<h14>Step 13</h14>

We will configure the deployment server to send its internal logs or any data consumed to the forwarders. Since this is the configuration management for the forwards the path we will be dealing with is "/opt/Splunk/etc/deployment-apps". The majority of our configuration changes will be made here.

<br>
<img src="https://imgur.com/D1scLZG.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</br> 



Thas how you confiugre a basic Clustererd enviroment with 1 cm 3 SH 3 indexers 3 UF 1 Deployer 1 Deployment server and license manager. I will provide more repositories for bringing in data , installing apps , updating configurations , using SPL etc....
