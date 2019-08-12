# Tableau 3 Node HA Cluster

Ansible playbook for deploying 3 Node HA cluster on Google Cloud Platform.


Utilises the Automated Installer to install Tableau Server on RedHat/CentOS servers.

## Usage

Ideal for using with Google Cloud Platform and [Vagrant](https://www.vagrantup.com/) to spin up a Tableau Server instance for development/prototyping. 

Configuration options can be found in `defaults/main.yml`. 

It is recommended that you update the password and registration details before use.

## Requirements

Vagrant Plugins:

Plugins marked in **Bold** are mandatory

  - **vagrant-hostmanager (1.8.9)**
  - **vagrant-google (2.4.0)**
  - **vagrant-ansible-local (0.0.2)**
  - **vagrant-guest_ansible (0.0.4)**
  - **vagrant-dns (2.2.0)**
  - **vagrant-hostsupdater (1.1.1.160)**
  - vagrant-compose (0.7.5)
  - vagrant-dnsmasq (0.1.1)
  - vagrant-exec (0.5.3)
  - vagrant-global-status (0.1.4)
  - vagrant-guestip (0.2)
  - vagrant-hosts (2.9.0)


The guest machine must meet the minimum specifications for Tableau Server, which is currently:

* 16 GB RAM
* 8 Cores

The role has only been tested on the following software:

* CentOS 7
* Vagrant - 2.0.3
* Ansible - 2.7.8

## Example Playbook

    ---
	  - name: Install tableau server
      hosts: all
      roles:
        - tableau-server
		    - tableau-node
		    - tableau-cluster


## Additional Enhancements

* [ ] Add support for Debian/Ubuntu servers.
* [ ] Add SSL certs from letsencrypt and schedule auto-renewal
* [x] Add SAML 2.0 Auth
* [ ] Add Loadbalancer (instantiate in gcp and configure for tableau)
* [ ] Make available in [Ansible Galaxy](https://galaxy.ansible.com/home)
* [ ] Add optional plays to configure firewall


## Licence

GNU General Public License v3.0

