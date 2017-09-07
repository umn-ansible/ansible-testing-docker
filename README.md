# Ansible Testing Images

These provide base images for testing ansible roles within docker using [kitchen-docker](https://github.com/test-kitchen/kitchen-docker) and [kitchen-ansible](https://github.com/neillturner/kitchen-ansible). Currently, only centos based images are provided.


> **Note**: These images are designed based on the needs of the University of Minnesota Ansible community so they include
EPEL and SCL repositories pre-installed.


## Quickstart

### Install Dependencies

* [Ruby](https://www.ruby-lang.org/en/downloads/) and [Bundler](http://bundler.io/#getting-started)
* [Docker](https://www.docker.com)

### Configure Gem Dependencies

Create a Gemfile:

```ruby
# frozen_string_literal: true
source "https://rubygems.org"

gem "test-kitchen"
gem "kitchen-inspec"
gem "kitchen-ansible"
gem "kitchen-docker"
```

### Install Gem Dependencies

```shell
bundle install
```

### Configure Test Kitchen
Use the following for the `.kitchen.yml` file:

```yaml
---
driver:
  name: docker
  run_command: '' # By setting this to an empty string docker will use the CMD defined in the image
  privileged: true

provisioner:
  name: ansible_playbook
  hosts: all
  require_chef_for_busser: false
  update_package_repos: false
  require_ansible_repo: false

verifier:
  name: inspec

platforms:
  - name: centos-6
    driver:
      image: umn-ansible/ansible-testing:centos6
  - name: centos-7
    driver_config:
      image: umn-ansible/ansible-testing:centos7
suites:
  - name: default

# Why this magic number for max_ssh_sessions? To prevent errors like this:
#
# Transferring files to <default-centos-73>
# /opt/chefdk/embedded/lib/ruby/gems/2.3.0/gems/net-ssh-3.2.0/lib/net/ssh/connection/channel.rb:541:in `do_open_failed': open failed (1) (Net::SSH::ChannelOpenFailed)
#
# Other test kitchen users have encountered similar errors, at least on Linux
# and have also reported success with this work-around. More details here:
# https://github.com/test-kitchen/test-kitchen/issues/1035#issuecomment-283189967
transport:
  max_ssh_sessions: 6

```

### Run Test Kitchen

```shell
bundle exec kitchen test
```
