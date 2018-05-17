# DOJ-specific Development Box

## Installation

Internal DOJ developers are not able to run BLT, so will need a different setup
for local development. Local installation will consist of these steps:

1. Clone the foia-api repository in Windows:
   `git clone https://github.com/usdoj/foia-api.git`
2. Manually create a `vendor` folder, and a `geerlingguy` folder:
   `mkdir -p /path/to/repo/vendor/geerlingguy`
3. Go into the `geerlingguy` folder and clone DrupalVM:
   `git clone https://github.com/geerlingguy/drupal-vm.git`
4. Copy the `local.config.yml` file in this folder into the /box folder:
   `cp /path/to/repo/box/doj/local.config.yml /path/to/repo/box/local.config.yml`
5. Ensure unrestricted internet access (no proxy) and run:
   `vagrant up`
6. SSH into the VM:
   `vagrant ssh`
7. Run the Bash script to complete the setup, inside the VM:
   `bash /vagrant/box/doj/vm-setup.sh`
8. Use drush to setup your local database:
   `drush sql-sync @foia.test @dojfoia.local`

## Working within the DOJ network

Working within the DOJ network involves some additional steps which are detailed
in [this private repository](https://github.com/usdoj/vagrant-doj). The files
in that repository will need to be copied into the `/box` folder above, like so:

* `/box/Vagrantfile.local`
* `/box/doj-enterprise.crt`

Also, if you would like to install the items in the local.playbook.yml, put the
following into the Vagrantfile.local file:

```
config.vm.provision :ansible_local do |ansible|
  ansible.playbook = "#{guest_config_dir}/doj/local.playbook.yml"
  ansible.galaxy_role_file = "#{guest_config_dir}/doj/local.requirements.yml"
end
```