netinstall
==========

Ansible role to configure network installation server.

Requirements
------------

* [ansible.builtin](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html)
* [ansible.posix](https://docs.ansible.com/ansible/latest/collections/ansible/posix/index.html)
* [Jinja2](https://jinja.palletsprojects.com/en/3.0.x/templates)

Role Variables
--------------

* defaults

  * `httpd.yml`

    ```yaml
    netinstall_VirtualHosts: {}   # httpd configuration, mainly DocumentRoot
    ```

  * `server.yml`

    ```yaml
    netinstall_base: ""           # base repo directory
    netinstall_pxe: {}            # PXE server configuration
    netinstall_ntp: []            # list of NTP servers used in kickstart
    netinstall_distros: []        # list of OS distributions for network install
    ```

    * `centos.yml`
    * `debian.yml`
    * `ol.yml`
    * `opensuse.yml`
    * `rhel.yml`
    * `solaris.yml`
    * `ubuntu.yml`

* vars

  ```yaml
  <ansible_os_family>_mount: {} # iso and loopback mount attributes
  <ansible_os_family>_httpd: {} # attributes of files presented by httpd
  <ansible_os_family>_tftpd: {} # attributes of files presented by tftpd
  ```

Dependencies
------------

* [httpd](https://github.com/mario-slowinski/httpd)
  * [service](https://github.com/mario-slowinski/service)
    * [software](https://github.com/mario-slowinski/software)

Tags
----

* netinstall.image
  * netinstall.image.directory
  * netinstall.image.download
* netinstall.mount
  * netinstall.mount.repo
  * netinstall.mount.pxe
* netinstall.pxe
  * netinstall.pxe.directory
  * netinstall.pxe.menu
  * netinstall.pxe.kickstart
* netinstall.clean

Example Playbook
----------------

* `requirements.yml`

  ```yaml
  - name: netinstall
    src: https://github.com/mario-slowinski/netinstall
  ```

* playbook usage

  ```yaml
  - hosts: servers
    gather_facts: true  # to get ansible_os_family

    roles:
      - role: netinstall
  ```

License
-------

[GPL-3.0](https://www.gnu.org/licenses/gpl-3.0.html)

Author Information
------------------

[mario.slowinski@gmail.com](mailto:mario.slowinski@gmail.com)
