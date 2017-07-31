# Cumulus Linux LNV Demo

This demonstrates using LNV in the reference topology with Active-Active VxLAN.

![Reference Topology Diagram](https://github.com/CumulusNetworks/cldemo-vagrant/raw/master/cldemo_topology.png)

The Cumulus Networks [reference topology](https://github.com/cumulusnetworks/cldemo-vagrant) is the basis of this demo.

Once the reference topology is running in Virtualbox or KVM, you can run this specific demo against it.

* `vagrant ssh oob-mgmt-server`
* `sudo su - cumulus`
* `git clone https://github.com/plumbis/cldemo-vxlan`
* `cd cldemo-vxlan`
* `ansible-playbook main.yml`

The logical topology of what will be configured:
![Logical Topology](https://github.com/plumbis/cldemo-vxlan/raw/master/logical_topology.png)

The Ansible playbook `main.yml` configures:
* eBGP unnumbered between Spine and Leaf nodes
* mLAG on Leaf01/Leaf02 and Leaf03/Leaf04
* Linux bond configuration on Server01, Server02, Server03, Server04
* LNV vxrd on Leaf nodes
* LNV vxsrd on Spine nondes
* Vlan 13 bridge on Leaf01 and Leaf02 for Server01
* Vlan 13 bridge on Leaf03 and Leaf04 for Server03
* Vlan 24 bridge on Leaf01 and Leaf02 for Server02
* Vlan 24 bridge on Leaf03 and Leaf04 for Server04
* VxLAN VNI 13 for Vlan 13
* VxLAN VNI 24 for Vlan 24

Today exit01, exit02, edge01, and internet are not used in this demo.

# NetQ
This demo includes a playbook named `netq.yml` that will provision netq across all network and server devices.

In order for NetQ to operate, the `oob-mgmt-server` must be replaced with the Cumulus Vx Telemetry Server. You can download the Telemetry Server from the [Cumulus Linux Downloads page](https://cumulusnetworks.com/downloads/#product=NetQ%20Virtual&version=1.0)

To install NetQ on all nodes, run
`ansible-playbook netq.yml`

You will need to exit out of the Telemetry Server and log back in to enable tab completion of `netq` commands on the Telemetry Server.

To verify that NetQ is properly installed and running use
`netq show agents`

Some commands to verify that the topology is properly configured and healthy
* `netq check bgp`
* `netq check clag`
* `netq check vxlan`
* `netq check mtu`
* `netq check lnv`
* `netq trace 44:38:39:00:00:1b from leaf04`
