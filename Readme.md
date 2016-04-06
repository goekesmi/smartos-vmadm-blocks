This is a collection of json fragments I find useful when using SmartOS
on the SoYouStart bare metal hosting platform.

The network environment on SoYouStart is uncommon.  If you have addtional
IP addresses assigned to your server, the most efficient way results in
the upstream router just forwarding the block at you. This doesn't result
in traditional networking functioning right.

In addition, SoYouStart wants your IP addresses to source from specific
'virtual mac's.  Thus you push the button in the web interface and get a
mac address assigned to each ip address.

Good news, you can make this work reasonably on the SmartOS deploy with
only a little bit of crazy.

use the IPADDRESS.json template to build blocks for each one of your
mac/IP combinations as seperate files.  Fill in the upstream router's
IP address for the gateway of your host box.  Leave the netmask 
255.255.255.255.  At default, this isn't sensical.  That's okay.

The file set-sys-routing.json contains a script fragment to a SmartOS
zone to use it's ethernet interface as a point to point link, and then
set a default route.  This means that there's a possiblitiy that traffic
may hairpin from the router to your other zones hosted on the same
physical machine.  I haven't found that to be a problem, but your 
mileage may vary.

run-set.json contains a script fragment that will run any user-metadata
that ends in .sh in lexographical order.   

Build the rest of your machine template, omitting the nics clause.

Take your file to load to vmadm and do the following.

cat yourfile.json IP.json set-sys-routing.json run-set.json | json --deep-merge 

If you are happy with the result, feed it to vmadm create.

Other set- files contain other useful scripts.  

Enjoy.

