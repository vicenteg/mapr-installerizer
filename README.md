mapr-installerizer
=========

A role to install the MapR installer, the install MapR.

Requirements
------------

You need to have set up your machines, and you need to have satisfied the prereqs for the MapR installer.

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
