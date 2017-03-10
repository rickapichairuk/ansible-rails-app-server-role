ansible-rails-app-server-role
=============================

A role to configure a debian based server to serve as a rails application
server.

INSTALLATION
------------

Using ansible-galaxy:

```sh
$ ansible-galaxy install rickapichairuk.rails-app-server-role
```

Using requirements.yml:

```yaml
- src: rickapichairuk.rails-app-server-role
  path: roles/
  name: rickapichairuk.rails-app-server-role
```

DEPENDENCIES
------------

* [rickapichairuk.basic-server-setup](https://github.com/rickapichairuk/ansible-basic-server-setup)
* [rickapichairuk.add-admin-user](https://github.com/rickapichairuk/ansible-add-admin-user)
* [rickapichairuk.deploy-rails-env-vars](https://github.com/rickapichairuk/ansible-deploy-rails-env-vars)

ROLE VARIABLES
--------------

The following example is based on an ansible group\_vars file. Since this role
depends on the previously mentioned roles, it also relies on the vars required
by those roles. The group\_vars yaml below serves as a reference to a working 
var file for completely configuring an Ubuntu 16.05 LTS server as a rails
application server running puma, nginx, and postgresql.

**IT IS EXTREMELY IMPORTANT TO NOTE THAT THIS FILE CONTAINS SENSITIVE
INFORMATION THAT MAY BE USED TO COMPROMISE YOUR APP OR SYSTEM SECURITY!!!!
YOU SHOULD NEVER COMMIT THIS FILE TO THE REPOSITORY IN PLAIN TEXT. USE 
ansible-vault TO ENCRYPT THIS FILE PRIOR TO COMMITTING OR TRANSMITTING THIS 
FILE ANYWHERE!**

```yaml
---
#
# Your application's name. The value here will be used as a name used to create
# directories, databases etc.
#
app_name: the_next_unicorn_app
#
# Linux admin user account setup. This will add an account that has NOPASSWD 
# sudo access. This account is intended for devops and power users. These vars
# are used by the rickapichairuk.add-admin-user ansible role.
#
add_admin_user:
  use_key_url: true
  use_key_file: false
  remove_all_other_sudo_rights: true
  admin:
    username: rick
    key_url: https://github.com/rickapichairuk.keys
#
# if you use circleci, then set the public ssh key that circleci will use to ssh
# into this machine when running capistrano deploy
#
circleci:
  ssh_public_key: |
    ssh-rsa THIS_IS_CIRCLE_CI_SSH_PUBLIC_KEYAAACAQCeplq5gS15LBE9Pwzeq4AqcUumVDxoMH1D6ZWBHAYZvRc/lbGFqcgFKlMVKR+X7mRI/dDVZ+u0HqQZsiUMWoiYCL1BKmvtGT7qknwf8tyw+AFRB8Mvx6/t7xSz4OJZdjbsIlJqd9/v8V/0cyELYUT+ynIPnO+nyVdU0R955xaCQqntOgZ3Q42N3tj9cYQM33ez0CkDn+TOTdujINJRunVrXEToCAFqmo7WHq0dpVd0QzuJD0DTz0lwWC6H6CljUwOqb9wt55fNbLNxG1HaIe9M1F3nMU3fwGS4We+76+fNv2CC67PeM8IW47RZosfRCuZJ6y1b9VKBvz3jPbFQfR4i0aPlsxIne6xVNp00MDU/4XK1l0D9vpWMdzxb/x0bFAJnS6rmWBDoWR5DQQXIqgr7KrBiyiRuV8hOzm7t6NdIFIlQWxMqwtBnV/XsVu86Lx6mVJgUmWcZgG02lPTDRbiLlSwrtTXaCd2+IBYD7e7S2/CHDE9eqmE9WNKxPgk2RIQ5miCEq+4g0QoFl6w== somedude@somecompany.com

#
# This is the user account for which you want to deploy the rails app as. This
# should be different from the admin account. Also configuration options for
# the deployer account like ssh keys, etc. There is also a configuration for
# all the public keys to put in authorized_keys file as well.
#
deployer_username: deploy
deployer:
  username: "{{deployer_username}}"
  home_dir: "/home/{{deployer_username}}"
  ssh_private_key: |
    -----BEGIN RSA PRIVATE KEY-----
    DEPLOYER_SSH_PRIVATE_KEY_MATCHING_PAIR_WITH_PUBLIC_KEY_BELOW_!!!
    h3yQiQ03YvnATd/glGUWxHc4LUAjL0Rnf3S9n+7Tk5kho9zsqMIjlBXcixwV83hQ
    SFWmEEPAEXeuRPwgqPDqatFih9/lhPChXKBlUprzaCtwOJCrlCyZ7o4tE74ZC0IN
    4E+4hLw+QqZmXzqwL2Q+GV0CSs8WS75QOI6rPM8rpQFJHeS0QY7D15mLSXmmFUvK
    jbjy9hWWyev+Tjzw0o1olG75PdSo8GOlRWndlbBIDzS5seEV+Lqa/wQBGcIfLoGK
    FDLKSJFLS  THIS IS JUST A SAMPLE!!!! DO NOT USE THIS KEY!!! )()S
    Cz3Wc1Z3gsL//vcsL5DfUTwnt9TZvcTCyi5zD1T5+U5SkqY1D99HW97QPN/b1I8o
    uZi3VjQ61RPhTGnyBoyLa9yQK5OLux2mlX678M0CgYA4Ue0mpH/iwWGv+jkynpVG
    4Ir/x1Xc66LrGIL1UL4JFfJBlXlf2qJUhYhGERf+esZggRusj1hNLlOAhpGoX180
    2LYNRsGGNs/yfgwNwnGkoQKBgCO0qcDvLLwf/uewQfhA25wp+4WikrPusMrf9WzB
    g5uZzHXg0xUmxDPgiMRCsTRToV2cJxsmaiHi0qkhPe2SuWajqxl1/M/kES3ZcM3+
    8flKwwpsk1y+tHvq+nIVavuHNmFRWwGbqSOTYPVZgY1M38WF+SNGosv3rCK/EVpm
    9yj9AoGAHrCsmE/vcrsJ4qMuZNIKz0j9XGcvP+wqvY9lNXM4Ff+Y3dP2VWYrCZB1
    t8eDpWgtIr5rMgJDu5rXFh0YSMu9UKb+WqWVZPYW/0oG8hTdmEBIOTUHKwpDOFGf
    SsUuQmI87B3kqfmnZDxmeztN0l2eh/Jheyw6aMwRS8ycZgn4Wns=
    -----END RSA PRIVATE KEY-----

  ssh_public_key: |
    ssh-rsa DEPLOYER_SSH_PUBLIC_KEY_AqcUumVDxoMH1D6ZWBMDxk5D/c9SM5/v8V/0cyELYUT+ynIPnO+nyVdU0R955xaCQqntOgZ3Q42N3tj9cYQM33ez0CkDn+TOTdujINJRunVrXEToCAFqmo7WHq0dpVd0QzuJD0DTz0lwWC6H6CljUwOqb9wt55fNbLNxG1qMbtiIOnjnSEsVdGGfhKuKkPIJdMrK1Hs7dC4wD9lovds982hi0o/h4goWg/gnw2GxkljDFlpAoCdgJmoaXsfv0SZDU3qX2nD28NQatOZQ/xOSzoeW4R9nRUTJi1q51HaIe9M1F3nMU3fwGS4We+76+fNv2CC67PeM8IW47RZosfRCuZJ6y1b9VKBvz3jPbFQfR4i0aPlsxIne6xVNp00MDU/4XK1l0D9vpWMdzxb/x0bFAJnS6rmWBDoWR5DQQXIqgr7KrBiyiRuV8hOzm7t6NdIFIlQWxMqwtBnV/XsVu86Lx6mVJgUmWcZgG02lPTDRbiLlSwrtTXaCd2+IBYD7e7S2/CHDE9eqmE9WNKxPgk2RIQ5miCEq+4g0QoFl6w== deployer@server.com

  authorized_keys:
    - "{{ add_admin_user.admin.key_url }}"
    - "{{ circleci.ssh_public_key }}"
#
# These are the directories where your app will reside. No need to change these.
# These default to the deployer user's home directory/apps/app_name
#
app_root_dir: "{{deployer.home_dir}}/apps/{{app_name}}"
#
# The shared directory that capistrano expects all shared config files to reside
#
app_shared_dir: "{{app_root_dir}}/shared"
logical_deploy_environment: production
#
# rbenv section begin - install rbenv for the deployer user
#
rbenv_user: "{{deployer.username}}"
rbenv_ruby_version: 2.3.3
rbenv_users:
  - deploy
rbenv_gems:
  - bundler
# rbenv section end
database:
  name: '{{app_name}}'
  password: 6Q7N8rA4hpb4xh
  username: admin
  host: localhost
deploy_rails_env_vars:
  etc_environment:
    deploy: false
admin:
  email: rick@next.unicorn
  password: SuperSecretPassword123
rails:
  default_host: next.unicorn
  log_level: "debug"
  secret_key_base: "2641eea14b4 THIS IS NOT A REAL SECRET KEY BASE!!! e18a329e33f49df1c8d0bed5201cf5dcbda318724b70ac3dc461b3a4fd6d"
```

EXAMPLE PLAYBOOK
----------------

```
---
- name: "Setup Rails App Server"
  hosts: all
  become: true
  gather_facts: no

  #
  # Since Ubuntu 16.04 LTS does not come with python 2 installed by default, 
  # you need to install it in order for ansible to work. The following lines 
  # does this for you.
  #
  pre_tasks:
    - raw: sudo apt-get update
    - raw: test -e /usr/bin/python || sudo apt-get -y install python-simplejson
    - setup: # aka gather facts

  roles:
    - rails_server_setup
```

AUTHOR
------

Rick Apichairuk

LICENSE
-------

BSD 3-Clause License

Copyright (c) 2017, Rick Apichairuk All rights reserved.

Redistribution and use in source and binary forms, with or without modification,
are permitted provided that the following conditions are met:

Redistributions of source code must retain the above copyright notice, this list
of conditions and the following disclaimer.

Redistributions in binary form must reproduce the above copyright notice, this
list of conditions and the following disclaimer in the documentation and/or
other materials provided with the distribution.

Neither the name of the copyright holder nor the names of its contributors may
be used to endorse or promote products derived from this software without
specific prior written permission.
THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR
ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
