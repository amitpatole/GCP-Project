# -*- mode: ruby -*-
# vi: set ft=ruby :
# Copyright 2013 Google Inc. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Usage:
#  1) Launch both instances and then destroy both
#     $ vagrant up --provider=google
#     $ vagrant destroy
#  2) Launch one instance 'z1c' and then destory the same
#     $ vagrant up z1c --provider=google
#     $ vagrant destroy z1c

# Customize global variables for Google Cloud
$GOOGLE_PROJECT_ID = "turing-mark-248317"
$GOOGLE_JSON_KEY_LOCATION = "~/turing-mark-248317-73fa91d790b8.json"
$LOCAL_USER = "osadmin"
$LOCAL_SSH_KEY = "~/.ssh/id_rsa"

#########################################

$script = <<-SCRIPT
  echo -e "n\np\n1\n\n\nw" | sudo fdisk /dev/sdb
  sudo mkfs.ext4 /dev/sdb1
  sudo mkdir -p /var/opt/tableau
  sudo sed -i '$ a ########' /etc/fstab
  sudo sed -i '$ a # Mount /dev/sdb1 so that it can be used a data drive for tableau' /etc/fstab
  sudo sed -i '$ a /dev/sdb1      /var/opt/tableau       ext4       defaults     0     0' /etc/fstab
  sudo mount -a && sudo mount
  sudo yum update
  sudo yum upgrade -y
  sudo yum install -y python python-apt
  sudo systemctl stop firewalld
  sudo systemctl disable firewalld
  sudo systemctl mask --now firewalld
SCRIPT


Vagrant.configure("2") do |config|
  config.vm.box = "google/gce"
  config.vm.provision :shell, :inline => $script
    config.vm.provision :ansible do |ansible|
      ansible.groups = {
          "tableau_nodes" => ["tableau_worker_node1","tableau_worker_node2"],
	  "tableau_cluster" => ["tableau_master_node","tableau_worker_node1","tableau_worker_node2"]
       }
     ansible.limit = "all"
     ansible.playbook = "main.yml"
     ansible.raw_arguments = "-vv"
     
    end
  #CREATE MASTER SERVER
  config.vm.define :tableau_master_node do |tableau_master_node|
  tableau_master_node.vm.provider :google do |google, override|
    google.google_project_id = $GOOGLE_PROJECT_ID
    google.google_json_key_location = $GOOGLE_JSON_KEY_LOCATION

    # Override provider defaults
    google.name = "tableau-master-node"

    google.image = "centos-7-v20190619"
    google.machine_type = "n1-standard-8"
    google.disk_size = 200
	google.additional_disks = [{
       :disk_size => 500,
       :disk_name => google.name + "-01",
       :disk_type => "pd-standard"
     }]

    google.zone = "us-central1-a"
    google.metadata = {'custom' => 'metadata', 'testing' => 'tableau_master_node'}
    google.tags = ['vagrantbox', 'dev', 'http-server', 'https-server']

    override.ssh.username = $LOCAL_USER
    override.ssh.private_key_path = $LOCAL_SSH_KEY

    end
  end

#CREATE FIRST NODE
  config.vm.define :tableau_worker_node1 do |tableau_worker_node1|
  tableau_worker_node1.vm.provider :google do |google, override|
    google.google_project_id = $GOOGLE_PROJECT_ID
    google.google_json_key_location = $GOOGLE_JSON_KEY_LOCATION

    # Override provider defaults
    google.name = "tableau-worker-node1"

    google.image = "centos-7-v20190619"
    google.machine_type = "n1-standard-8"
    google.disk_size = 200
	google.additional_disks = [{
       :disk_size => 500,
       :disk_name => google.name + "-01",
       :disk_type => "pd-standard"
     }]
    google.zone = "us-central1-a"
    google.metadata = {'custom' => 'metadata', 'testing' => 'tableau_worker_node1'}
    google.tags = ['vagrantbox', 'dev', 'http-server', 'https-server']

    override.ssh.username = $LOCAL_USER
    override.ssh.private_key_path = $LOCAL_SSH_KEY

    end
  end

#CREATE SECOND NODE
  config.vm.define :tableau_worker_node2 do |tableau_worker_node2|
  tableau_worker_node2.vm.provider :google do |google, override|
    google.google_project_id = $GOOGLE_PROJECT_ID
    google.google_json_key_location = $GOOGLE_JSON_KEY_LOCATION

    # Override provider defaults
    google.name = "tableau-worker-node2"

    google.image = "centos-7-v20190619"
    google.machine_type = "n1-standard-8"
    google.disk_size = 200
	google.additional_disks = [{
       :disk_size => 500,
       :disk_name => google.name + "-01",
       :disk_type => "pd-standard"
     }]
    google.zone = "us-central1-a"
    google.metadata = {'custom' => 'metadata', 'testing' => 'tableau_worker_node2'}
    google.tags = ['vagrantbox', 'dev', 'http-server', 'https-server']

    override.ssh.username = $LOCAL_USER
    override.ssh.private_key_path = $LOCAL_SSH_KEY
   end
  end
end

