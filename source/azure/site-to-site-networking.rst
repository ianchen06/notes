Site to Site Networking
=======================

.. note::
   This type of connection requires a VPN device located on-premises that has a public IP address assigned to it and is not located behind a NAT.

For modern VPN connection, the best option as of now is IKEv2 + IPSec(Strongswan implementation)

Good resource on different VPN implementation comparison is `here <https://www.speaknetworks.com/pptp-vs-l2tpipsec-vs-sstp-vs-ikev2-vs-openvpn/>`_

I was able to configure the Azure to on-premise site-to-site VPN with the following /etc/ipsec.conf from the `official libreswan documentation`_ example.

.. code-block:: bash

   conn conn2AzureRouteBasedGW
       authby=secret
       auto=start
       dpdaction=restart
       dpddelay=30
       dpdtimeout=120
       forceencaps=yes # not a must
       ike=aes256-sha1;modp1024
       ikelifetime=10800s
       ikev2=yes
       keyingtries=3
       left=%defaultroute
       leftid=<MY PUBLIC IP>
       leftsubnets=<Azure Local Network Gateway Subnets>
       pfs=yes
       phase2alg=aes128-sha1
       right=<Azure Route Based GW IP>
       rightid=<Azure Route Based GW IP>
       rightsubnets=<vNet Subnet>
       salifetime=3600s
       type=tunnel

.. _official libreswan documentation: https://libreswan.org/wiki/Microsoft_Azure_configuration

