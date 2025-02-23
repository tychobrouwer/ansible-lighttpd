Install and Configure Lighttpd
=========

This role installs and configures lighttpd for my server.

Requirements
--------------

Keep in mind parts of the config are specific to my server configuration and website structure. Particularly the ```url.rewrite``` and ```url.redirect``` fields.

Role Variables
--------------

The ```lighttpd_git_source``` variable should be used to set to the address of the git repository for the website.

The variable ```lighttpd_website_name``` is used to set the website name (directory created for website) and the ```lighttpd_server_root``` should be used to set the website root relative to the git repository.

```lighttpd_entry_file``` should be used to set the entry file for the website.

The ```lighttpd_server_port``` and ```lighttpd_server_address``` variables should be used to set the the port of lighttpd and the website address respectively.

The ```lighttpd_user``` variable can be used to set the user lighttpd is running as.

Additional rules in the config can be added by setting the ```lighttpd_rules``` variable to a list of (multiline) strings.

Example Playbook
----------------

```yaml
- hosts: servers
  vars:
    lighttpd_git_source: git@github.com:username/repo.git
    lighttpd_website_name: portfolio
    lighttpd_server_address: https://mywebsite.com

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
    - role: tychobrouwer.lighttpd

    - role: tychobrouwer.lighttpd
      lighttpd_server_port: 80
      lighttpd_user: lighttpd
      lighttpd_entry_file: index.html
      lighttpd_server_root: public
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
