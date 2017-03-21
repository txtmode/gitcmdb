# gitcmdb
## What is it?

CMDB system where CMDB server is git server (for example GitHub)

Every defined number of seconds hosts connect to Github to update a specific branch of a repo and execute one installer. Currently the only supported puppet, and simple bash scripts. Ansible will be adder later.


## Installation

```
wget --quiet 'https://raw.githubusercontent.com/txtmode/gitcmdb/master/installers/gitcmdb?' -O - | bash -s <repo> <install folder>
```
where:
* repo is your repo, default https://github.com/txtmode/gitcmdb
* install folder is where to clone the repo, by default /opt

Supported Distributions:
* Debian/Ubuntu
* CentOS/RHEL

## Usage
Define hostnames of hosts like *role*-*instance*-*whatever*.*domain*

Configuration variables will be load from:
* config/default.yaml
* config/*role*/default.yaml
* config/*role*/*instance*.yaml
and it that order so the later has preference

All those environment variables will be load and then *gitcmdb_installer* will be executed

## Specific installers
### Puppet
It's quite similar to r10k usage, every host has an specific repo (gitcmdb_installerrepo), an specific branch (gitcmdb_branch) and:
* Puppetfile is read by r10k to install all defined modules
* hiera is enabled, hiera.yaml is also versioned for every repository branch
* puppet apply is executed to load manifests/site.pp
* By default hiera loads class ::roles::*role*
* External facts from `facts.d` are copied to `/etc/facter/facts.d` so they can also be used

### Bash
All variables in yaml config will be loaded as environment variables.
If found those scripts are executed in that order:
* bashscripts/default.sh
* bashscripts/*role*.sh
* bashscripts/*role*/*instance*.sh

Make idempotent scripts!

### Asible
To be done.
