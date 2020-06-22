# Ansible playbooks to configure network boot/install of various linux releases

## Table of contents
* [Requirements](#requirements)
* [Configuration](#configuration)
* [Deployment](#deployment)

## Requirements

* ansible
* python-3 (and modules)
    * netaddr
    * jinja2
* existing machine with
	* configured PXE service
	* configured HTTP service

## Configuration
Deployment configuration is based on **group names**, so please don't change them.

Most settings (if not all) in variable files:
* `group_vars/netinstall` - general configuration settings and linux releases details
	* *img_dir* - directory with OS ISO images
	* *repo_dir* - directory where ISO is to be mounted, this directory must be available via http
	* release
		* *pxe_repo* - http url where boot stage2 image is located, this directory must be available via http
		* *pkg_repo* - http url where rpm pkgs are located
		* *image* - http url of ISO image location

## Deployment
Adjust **variables** accordingly.
For RHEL download full DVD ISO image and put it in *img_dir*.
For CentOS and Oracle Linux boot ISO image will be downloaded from the internet.
This can be chaned by image -> url variable.

### Playbooks should be run in the following order
1. Download/verify OS ISO image and mount it at *repo_dir*, mount by lofs *repo_dir* to *pxe_repo*.
	```
	ansible-playbook image-setup.yml
	ansible-playbook -e distro=... image-setup.yml
	```

2. Create PXE configuration, PXE menu and kickstart file for each OS release.
	```
	ansible-playbook pxe-setup.yml
	```

3. Optionally remove existing in filesystem but missing in vars file OS relase configs, dirs, mountpoints.
	```
	ansible-playbook cleanup.yml
	```
