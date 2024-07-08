## Config Multi-site

### Install cephadm
https://docs.ceph.com/en/quincy/install/get-packages/
https://download.ceph.com/debian-16.2.5/dists/focal/

### Deloy Ceph Cluster

Install ceph single cluster

cephadm --image quay.io/ceph/ceph:v16.2.5 bootstrap --mon-ip 10.1.0.46 --single-host-defaults --skip-monitoring-stack --skip-dashboard

### Edit ceph crush map

ceph osd getcrushmap -o compile

crushtool -d compile -o de_com

crushtool -c de_com -o compile

ceph osd setcrushmap -i compile
"5d2255aa-72e1-4fle-af99-85683f36cb34.1157817345.6",


### Deploy ceph multi-region

