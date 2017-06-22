# Ansible

The configuration management system of choice at UK Trade is Ansible due to the fact
it is agentless and is an idempotent system that uses YAML for describing the
automation. Also, all projects within UK Trade can be described in a flat namespace.

## Building an Environment

In this example, an OpenStack endpoint is used to create an environment complete
with networks, subnets, security groups and instances. The instances are further
configured with the relevant application stacks. 

The following command will build an entire OpenStack environment including all
the aforementioned pieces running RHEL7. They will be accessible via a golden
host through ssh.

`$ ansible-playbook -i hosts/uktrade uktrade-openstack.yaml`

### Red Hat 
In order to update the systems and install additional software, each platform
needs registering at the Red Hat Developer Community. 
`http://developers.redhat.com`

`$ ansible-playbook -i hosts/uktrade -u cloud-user uktrade-platforms.yaml`

### IPA

The `uktrade-ipaserver.yaml` playbook will also install IPA server in a master -
replica configuration. The build will ask for two passwords, and these are
used to set the passwords for the build, so generate a password or two and
jot them down. 

`$ ansible-playbook -i hosts/uktrade -u cloud-user uktrade-ipaserver.yaml`

## OpenStack
After you have got to this point, you need to change some OpenStack parameters.

```
$ ansible-playbook -i hosts/uktrade uktrade-openstack2.yaml
```

## External DNS
If you need external, or remote, DNS, a convenient PowerDNS container exists on
Docker Hub. A playbook has been created to configure the container as it is
100% ephemeral by design in master/slave configuration.

```
$ ansible-playbook -u cloud-user -i hosts/uktrade uktrade-remotedns.yaml
````

### OpenShift
In order to build the OpenShift cluster environments, it is necessary to check
out the `https://github.com/openshift/openshift-ansible` repo alongside this
repo. The path in the `ansible.cfg` file included in this directory assumes
they are checked out in the `~/repo/` directory.

`$ ansible-playbook -i hosts/uktrade-mgmt_openshift uktrade-openshift.yaml`

After the above command, there will be a three-node cluster running in the
mgmt network.

`$ ansible-playbook -i hosts/uktrade-app_openshift uktrade-openshift.yaml`

After the above cammand, there will be a three-node cluster running in the 
app network.
