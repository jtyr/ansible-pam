pam
===

Ansible role which configures the Linux Pluggable Authentication Modules (PAM).

Please report any issues or send PR.


Examples
--------

```
---

# Example of usage of the role
- hosts: myhost
  vars:
    # Default configuration of the system-auth file
    pam_config_system_auth__default:
      # This is the label of the rule used to sort the rules
      aa:
        # This is the rule itself
        type: auth
        control: required
        path: pam_unix.so
        args:
          - try_first_pass
          - nullok
      bb:
        type: auth
        control: optional
        path: pam_permit.so
      cc:
        type: auth
        control: required
        path: pam_env.so
      dd:
        type: account
        control: required
        path: pam_unix.so
      ee:
        type: account
        control: optional
        path: pam_permit.so
      ff:
        type: account
        control: required
        path: pam_time.so
      gg:
        type: password
        control: required
        path: pam_unix.so
        args:
          - try_first_pass
          - nullok
          - sha512
          - shadow
      hh:
        type: password
        control: optional
        path: pam_permit.so
        args:
      ii:
        type: session
        control: required
        path: pam_limits.so
      jj:
        type: session
        control: required
        path: pam_unix.so
      kk:
        type: session
        control: optional
        path: pam_permit.so

    # Customization of the system_auth file
    pam_config_system_auth__custom:
      # Label which will place the rule between the 'hh' and the 'ii' label
      hhhh:
        type: password
        control: required
        path: pam_time.so

    # Default PAM configuration composed only from the system-auth file
    pam_config__default:
      # This is the name of the file we want to configure
      system-auth: "{{
        pam_config_system_auth__default.update(pam_config_system_auth__custom) }}{{
        pam_config_system_auth__default }}"
      # There can be more files defined here
      #system-login:
      # ...

    # Here we can add even more files to configure if we want
    pam_config__custom: {}

    # Final PAM configuration is a merge of the default and custom rules
    pam_config: "{{
      pam_config__default.update(pam_config__custom) }}{{
      pam_config__default }}"
  roles:
    - pam
```

I recommend to composed the rule labels from even number of letters which
allows to insert another rule in between of the existing once (e.g. `hhhh` is
between `hh` and `ii`).


Role variables
--------------

List of variables used by the role:

```
# Default path to the PAM configuration files
pam_path: /etc/pam.d

# User which owns the final PAM configuration file
pam_user: root

# User which owns the final PAM configuration file
pam_group: root

# Permissions of the final PAM configuration file
pam_chmod: 0644

# PAM configuration
pam_config: {}
```


Dependencies
------------

- [`config_encoder_filters`](https://github.com/jtyr/ansible-config_encoder_filters)


License
-------

MIT


Author
------

Jiri Tyr
