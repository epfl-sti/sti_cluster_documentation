# Environments

In order to be able to move forward while already providing the service to our customers, we need 3 environments: dev / test / production.

## Production

This environment is composed of 4 servers located in the IC cluster. Since this environment is customer facing, no work from the team can be done here.

## Test

This is scaled down version of the production environment. It is composed of 2 physical servers located in the IC cluster. It can be used for integration testing and customer acceptance tests.

## Dev

Since we are mainly working on packaging application and settings through the use of Ansible, we can work locally with VirtualBox and Vagrant to ease up the spawn, work, rince and re-spawn process.
