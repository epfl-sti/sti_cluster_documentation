# Technical choices

Since we are working on the IC cluster, we can leverage what's provided by the *Hardware as a Service* platform.

Basically, this platform allows us to:

* select a template to be provisioned on a machine
* select on which machine to deploy this template

From there, the system takes care of:

* installing the OS on the machine
* provision the ssh keys we'll use to deploy the applications

At this point, we are able to take control of the machine from an ssh connection. This is good because:

* it allows us to work on the [dev / test / prod](environments) environments seamlessly
* it allows us to work on going from [step 1 to step 2](/project_organization/project_steps) easily

## environments

As shown on the [environments page](environments), we do have 3 enviroments: production / test and development. Even though these environments are quite different, we can use the same tools and technologies to access them.

## Tools

### Vagrant

This tool is only used on the dev environment. It is used to quickly spawn new virtual machines that can be used to test the deployment of applications and settings. More details about Vagrant can be found [here](vagrant).

### Ansible

Ansible will be used to deploy applications and settings to the various machines. Since it requires an ssh access to the machines, it integrates nicely with what the IC cluster is providing.

### Ansible vault

In order to encrypt secrets (such as keytab files or licenses), we decided to use [Ansible Vault](secrets_encryption.md).

## target environments

Since all STI clusters machines are based on Ubuntu, all deliverables (scripts / Ansible playbooks / configs) are targeted at Ubuntu. This is enough for our current requirements even though it might not address a larger need (Lab / STI / EPFL specific needs).

## End user environment

### Root access

We decided to give Root access to the machines because our customers will have to be able to install packages specific to their needs.

However, we will not be able to provide support for any issue they may have with any package. Support will therefore be provided on a **best effort** basis. If a customer asks for support with a package or a configuration that is *not supported*, we will ultimately re-stage (reformat the machine) to a *standard* state. **This process will erase all data and configuration added by the customer**.
