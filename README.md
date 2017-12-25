# dns-registration
Simple setup to allow host registration to a local  DNS

# Setup - WIP

To get the updates to be be made automatically we will take advantabe of the
current interface startup infrastructure (ifup). There are basically 3 steps
to accomplish this:

* add **dns-functions** script to the /etc/sysconfig/network-scripts
* update the *ifcfg-<DEVICE\>* to add the following new tokens
  * REGISTRATIONS_DNS
    * This is the IP address of the DNS where the hostname will be registered
  * REGISTRATION_ZONE
    * This is the DNS zone where the where the hostname willb be registered
* update the *ifup* script to call the source the **dns-functions** script and call the **dns_registration** function
  * __*[ -f ./dns-functions ] && . ./dns-functions*__
  * __*[ -n "${REGISTRATION_DNS}" ] && dns_registration*__
    * **Note:** The call to dns_registration must be done after the device has been started
* add the same to the *ifup-post* script
  * This is required because it is possible that the *ifup* script may not do this

## Re-registering from the command line

If the IP address of your host changes for any reasen you may need to re-register
at the DNS. In some cases where you are running in a headless situation you will
not be able to simply do a ifdown/ifup to accomplish this new registration. So,
you may need to do something like the following to re-register:

. /etc/sysconfig/network-scripts/dns-functions && DEVICE=<DEVICE\> REGISTRATION_DNS=<IPADDR\> REGISTRATION_ZONE=<ZONE\> dns_registration

**Example**

. /etc/sysconfig/network-scripts/dns-functions && DEVICE=wlp1s0 REGISTRATION_DNS=192.168.0.16 REGISTRATION_ZONE=example.com dns_registration

# TODO

* write script to update the *ifup* and/or *ifup-post* scripts
* write script to update the *ifcfg-<DEVICE\>* file
* add REGISTRATION_DNS and REGISTRATION_ZONE to resolv.conf
* figure out how to do this on the Mac.   :-)
