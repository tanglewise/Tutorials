# Setting up an IOTA Nelson node on Google Compute Engine

Google Compute Engine is a suite of suite of cloud services, similar to AWS. Currently, GCE will give you a free $300 credit to use on Virtual Machines on their platform. They do require you to post credit card information, but they will not charge you until you explicitly upgrade your account. If you have 5 minutes and would like to support the IOTA tangle, you can set up your own full node now and run it for free (for about 6-8 months, depending on your VM configuration).

1. Make a [Google Compute Engine](https://cloud.google.com/compute/) account, and sign up for the trial version (unless you would like to automatically pay for your VM(s) once your free credits are used).

2. Create a new project.

3. Under the Products and Services menu in the top-left, go to Compute Engine, then select the VM Instances tab. Click Create Instance. This will provide a menu for you to provide details for your new VM.
    * Name your instance (something like "iota-node" will do just fine)
    * Select your desired zone. Does not matter too much, but if you select a zone close to you geographically, you will experience slightly lower latency connecting to your node.
    * Select your machine type. 2vCPUs with 7.5gb memory n1-standard-2 works very smoothly. If you want to save on free trial credits, 1vCPU with 3.5gb memory, n1-standard-1 should work fine.
    * Change your boot disk. Recommendation: Ubuntu 17.10. Change Size to >= 50gb so that we can reliably store the tangle database with room to spare. If you are feeling ambitious you can probably get away with 30gb.
    * Under Access Scopes, select "Allow Access to all Cloud APIs"
    * Under Firewall, check boxes for "Allow HTTP traffic" and "Allow HTTPS traffic".
Your instance is configured and ready to go. Click "Create Instance," and give Google a few seconds to allocate resources for you.

4. Configure Firewall Rules.
    * Go to GCE's [Firewall Rules Page](https://console.cloud.google.com//networking/firewalls/). Select the project you created in step 2.
    * You need to change these port forwarding rules to allow your node to communicate with the tangle. We need to enable four ports: 14265, udp 14600, tcp 15600, and 16600.
    * Create a new firewall rule. Name it something like "iota-in-14625". Make the following changes:
      * Direction of traffic: Ingress
      * Action on match: Allow
      * Targets: All instances in the network
      * IP ranges: 0.0.0.0/0
      * Specified protocols and ports: tcp; udp:14625;
    * Now, repeat the above process for each of the above ports. Then repeat the intire rule creation process for each port, with each rule having the same specifications, except "Direction of traffic". This specification should now be set to "Egress" - note this change by naming these rules something like "iota-14265-out".
  ⋅⋅*NOTE: if you are lazy, or just want to get your node up and running quickly, you can enable traffic on all ports. Under "Protocols and ports", select "Allow all". You need to do this for both Ingress and Egress. Please note that doing this can be a security hazard - don't deal with any sensitive information on your VM if you take this route.
  
 5. Your VM is ready! Go back to the VM Instances page (Products and Services -> Compute Engine -> VM Instances). Under the "Connect" field for your new instance, click the SSH button. This will open a new console where you will be able to interact with your VM via command line.
 
 To set up your IRI Headless Node equipped with Nelson peer discovery services, simply enter the following command:
 `sudo curl https://raw.githubusercontent.com/tanglewise/Scripts/master/nelson_setup_script.sh | bash`
 
 The setup will take some time, about 30 minutes. Once your VM has finished all of the setup processes, you can check your node status with `curl http://localhost:18600`. Your node is fully synced if the latestMilestone field matches the [latest coordinator milestone](https://milestone.iotatangle.space/). Don't worry if your latestMilestone does not match the actual latest milestone. This just means your node is not yet fully synced. Give your node some time and check periodically with `curl http://localhost:18600`.
 
That's it! Your IOTA node should be up and running.  
