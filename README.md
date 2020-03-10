# **Wireshark Network Traffic Analysis - Sniffing Usernames/Passwords**

## **Background** 
- Communication among devices connected to a network is facilitated through packet transfer. **Wireshark** is an extremely useful free tool that allows users to track communication across a network and monitor the network for activities of connected devices.

## **Understanding Filters**
- DISPLAY FILTERS: the filter bar at the top of Wireshark's interface. captures all data on network and narrows down information from a packet capture.
- CAPTURE FILTERS: the filter bar displayed when you open the application. used mainly by network admins to specify   data/packets to reduce capture size and save time and effort. a great way to search port-specific traffic.

## **Other important concepts**
- We won't be using it in this demo, but `traceroute` is an extremely handy tool for understanding the steps of network communication. 

## **Objective**
- In this small project, we'll be demonstrating the utility of Wireshark by 'sniffing' out a username and password on an insecure HTTP website. 
- Since HTTP is unencrypted (unlike HTTPS) you can easily locate information transmitted across an insecure website.
- Disclaimer: Trying to do this with someone else's username and password can get you into serious legal trouble. Do not try to do this. 
- For the purposes of this exercise, we will be using a dummy website on our own network and sniffing out our own made up username/password.

## **Process**
- Download/open Wireshark.
- Select your local network. 
- Examine the interface. At the top, you'll see a toolbar with a blue fin and a red square, to start and stop packet capture, respectively. 
- Start packet capture, then open a browser and navigate to http://zero.webappsecurity.com/ and enter a dummy username and password.
- Now return to Wireshark and stop the capture. 
- Notice the "filter" option near the top of the interface. If you enter "http", Wireshark will narrow the results to show procotols that happened when you entered your username and password.
- You can narrow the results even further by using this syntax: `http.request.method=="POST`, which should get the results down to one single packet!
- Observe the second pane from the bottom in Wireshark, and try to find text that says "Hypertext Transfer Protocol". If you expand this field, you'll see a generous amount of information about the packet. 
- For now, we're looking for a cookie, something that gets info from websites you're visiting and trasmits the info to other websites. 
- Once you find "cookie", right click it, click "follow", then click "HTTP stream". 
- This will pull up a new window with HTML. If you examine this block, you'll see a field titled "name" and a field titled "password". This should reflect the credentials you used on the insecure website. 

## **Results**
- There are some important implications to realize upon completion of this small project.
1. Sensitive data on an insecure network (credit card credentials, banking info, etc.) could be compromised
2. Chats could be eavesdropped on
3. Files transmitted over a network could be captured and analyzed.  
4. Wireshark is a powerful tool, and this just scratches the surface of it's capabilities. 

## **Notes** 
- Example pictures to follow.
- Wireshark has a world of application. You can follow a similar process with all kinds of different protocols, including TCP/IP and ARP. 
- For example: For a DNS packet, go to “Queries”, click on the website name, right click, “Apply as filter”, “Selected”. dns.qry.name == "us3.admin.mailchimp.com" and you'll be able to see more info.
- -Browse to Domain Name System > Flags, last line is the reply code, the 0 of which means no error. 

## **SYN Scan**
- The display filter to show only SYN packets is: `tcp.flags.syn==1 && tcp.flags.ack==0`. You can use this to locate the intial request for a server-client connection. 
