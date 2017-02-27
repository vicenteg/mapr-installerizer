mapr-installerizer
=========

A role to install the MapR installer, then install MapR.

Requirements
------------

You need to have set up your machines, and you need to have satisfied the prereqs for the MapR installer.

Ansible Requirements
------------

On CentOS 7, I do something like this:

```
sudo yum -y install gcc make python-devel openssl-devel
sudo pip install ansible
git clone https://github.com/vicenteg/mapr-installerizer.git
cd mapr-installerizer
```

Then for a single node cluster, generate a key you can use:

```
ssh-keygen -t dsa -N "" -f ~/.ssh/id_dsa
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```

Create an inventory file:

```
NODE=$(hostname)
cat <<EOF >hosts
[cluster]
$NODE
EOF
```
Create a file containing the example playbook below:

```
cat <<EOF >playbook.yml
- hosts: cluster[0]
  roles:
    - { role: mapr-installerizer,
          private_key_path: "$HOME/.ssh/id_dsa",
          ssh_remote_user: "$USER",
          mapr_admin_user: "mapr",
          mapr_core_version: "5.2.0",
          mep_version: "1.1",
          mapr_disks: [ $(lsblk -pldn --output NAME | awk 'NR>1 { print $1"," }') ]
      }
EOF
```

Example Playbook
----------------

```
- hosts: cluster[0]
  roles:
    - { role: mapr-installerizer,
		private_key_path: "/home/vince/.ssh/vgonzalez_keypair.pem", 
		ssh_remote_user: "ec2-user",
		mapr_admin_user: "mapr",
		mapr_core_version: "5.2.0",
		mep_version: "1.1",
		mapr_disks: [ "/dev/xvdf" ]
	}
```

License
-------

Apache 2.0

Author Information
------------------

Vince Gonzalez, MapR Technologies
