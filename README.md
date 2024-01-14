Install and Configure Lighttpd
=========

This role installs and configures lighttpd for my server.

Requirements
--------------

Keep in mind parts of the config are specific to my server configuration and website structure. Particularly the ```url.rewrite``` and ```url.redirect``` fields.

Role Variables
--------------

The ```git_source``` variable should be used to set to the address of the git repository for the website.

The variable ```website_name``` is used to set the website name (directory created for website) and the ```role_server_root``` should be used to set the website root relative to the git repository.

```role_entry_file``` should be used to set the entry file for the website.

The ```role_server_ip```, ```role_server_port```, and ```role_server_address``` variables should be used to set the internal server ip, the port of lighttpd, and the website address respectively.

The ```role_lighttpd_user``` variable can be used to set the user lighttpd is running as.

Additional rules in the config can be added by setting the ```role_lighttpd_rules``` variable to a list of (multiline) strings.

Example Playbook
----------------

```yaml
  - hosts: servers
    vars:
      lighttpd_rules:
        - |
          url.rewrite-once = (
            "^/(dist|images|api)/(.*)$" =>      "/$1/$2",
            "^/robots.txt$"             =>      "/robots.txt",
            ".*"                        =>      "/"
          )
        - |
          url.redirect = (
            "^/(projects)$"             =>      "/index.html"
          )

    roles:
      - { role: lighttpd, role_git_source: git@github.com:username/repo.git,
          role_website_name: portfolio, role_server_ip: 192.168.1.106, role_server_address: https://mywebsite.com }
      - { role: lighttpd, role_git_source: git@github.com:username/repo.git,
          role_website_name: portfolio, role_server_ip: 192.168.1.106, role_server_address: https://mywebsite.com,
          role_server_port: 80, role_lighttpd_user: lighttpd, role_entry_file: index.html,
          role_lighttpd_rules: {{ lighttpd_rules }}, role_server_root: public }
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
