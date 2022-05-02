uclalib_role_chartmuseum
=========

Ansible role to provision a [ChartMuseum](https://chartmuseum.com/) application

Requirements
------------

This role assumes the following conditions in your environment:
  * RHEL 7 OS

Optional conditions:
  * Apache HTTPD with mod_proxy installed
  * redis cache installed

Role Variables
--------------

Reference `defaults/main.yml` to review default values.

Installation-related variables (you can typically leave these set to their default values):

  * `chartmuseum_version` - Defines the version of ChartMuseum to install (takes the form: `vX.Y.Z`)
  * `chartmuseum_download_url` - Defines the URL to download the ChartMuseum binary
  * `chartmuseum_install_dir` - Defines the full path to the directory on the local filesystem to install ChartMuseum
  * `chartmuseum_user` - Defines the user account that will run the ChartMuseum service
  * `chartmuseum_config_dir` - Defines the full path to the directory on the local filesystem where ChartMuseum configuration files are stored

ChartMuseum configuration parameters (these do not have default values, so set these according to your needs):

  * `chartmuseum_port` - Defines the port number ChartMuseum should bind to when the service starts
  * `chartmuseum_storage_backend` - Defines the storage backend type ChartMuseum should use (right now this role only supports: `local`)
  * `chartmuseum_storage_local_rootdir` - Defines the full path to the directory on the local filesystem where ChartMuseum stores uploaded helm charts 
  * `chartmuseum_cache_store` - Defines cache store type ChartMuseum should use (if left empty, it will use in-memory store; only supported option is `redis`)
  * `chartmuseum_cache_redis_addr` - Defines the IP:PORT where the redis cache is running
  * `chartmuseum_cache_redis_password` - Defines the password used (if any) for authenticaitng to the redis cache
  * `chartmuseum_cache_redis_db` - Defines the DB number of the redis cache
  * `chartmuseum_basicauth_user` - Defines the username for HTTP basic authentication
  * `chartmuseum_basicauth_password` - Defines the password for HTTP basic authentication
  * `chartmuseum_basicauth_anonymousget` - Defines if anonymous HTTP GET access is allowed with basic authentiation is enabled

Tags
----

Use the following tags to run specific role functions:

  * `setup-cm` - set-up prerequisites for ChartMuseum install
  * `install-cm` - perform install/upgrade of ChartMuseum
  * `config-cm` - put in place ChartMuseum configuration

Dependencies
------------

None

Example Playbook
----------------

An example complete playbook that uses redis as a cache would like something like the following:

```
---

- name: uclalib_chartmuseum.yml
  become: yes
  become_method: sudo
  hosts: all

  roles:
    - { role: uclalib_role_redis }
    - { role: uclalib_role_chartmuseum }
```
