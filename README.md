# Cumulus Linux EVPN Example

This demonstrates using BGP-eVPN for VxLAN with an external router (`internet`)

![Reference Topology Diagram](https://github.com/CumulusNetworks/cldemo-vagrant/raw/master/cldemo_topology.png)

In this demo, two different VLANs are used: Vlan 13 between server01 and server03 and Vlan 24 between server02 and server04.

Server01 has the IP 10.1.3.101
Server03 has the IP 10.1.3.103

Server02 has the IP 10.2.4.102
Server04 has the IP 10.2.4.104

The servers rely on VxLAN between the top of rack switches to communicate within the same VLAN.

To communicate between VLANs traffic is sent, via VxLAN, to exit01/exit02 switch pair, who forward the traffic to the node named "Internet". This device has an SVI for both Vlan 13 and Vlan 24 and acts as the central router in the topology. This is a similar role a Firewall would play in a production network.

An example traffic flow from Server01 to Server02:
`Server01 -> Leaf01 (vxlan) -> Spine01 -> Exit01 -> Internet -> Exit01 (vxlan) -> Spine01 -> Leaf01 -> Server02.`

An example traffic flow from Server01 to Server03:
`Server01 -> Leaf02 (vxlan) -> Spine02 -> Leaf03 -> Server03`

All servers are configured in a dual-attach mLAG configuration.
The Internet device has a single bond that is dual attached to exit01/exit02 which act as an mLAG pair.

`edge01` is not used in this example.

* Running the Demo in Cumulus Vx

The Cumulus Networks [reference topology](https://github.com/cumulusnetworks/cldemo-vagrant) is the basis of this demo.

Once the reference topology is running in Virtualbox or KVM, you can run this specific demo against it.

* `vagrant ssh oob-mgmt-server`
* `sudo su - cumulus`
* `git clone https://github.com/plumbis/evpn-example`
* `cd evpn-example`
* `ansible-playbook mgmt_vrf.yml` (note: Ansible will report this failed. This is expected)
* `ansible-playbook main.yml`

The logical topology of what will be configured:
![Logical Topology](https://github.com/plumbis/cldemo-vxlan/raw/master/logical_topology.png)


