### SELinux Overview

SELinux is a security architecture for Linux that provides a mechanism for supporting access control security policies, including mandatory access control (MAC). It is designed to enhance the security of the Linux operating system by enforcing the separation of information based on confidentiality and integrity requirements.

### SELinux Modes

1. **Enforcing**: In this mode, SELinux policy is enforced, and any violations are denied and logged.
2. **Permissive**: SELinux does not enforce the policy but instead logs actions that would have been denied if in enforcing mode. This mode is useful for testing and troubleshooting.
3. **Disabled**: SELinux is turned off entirely.

### Commands

- **sestatus**: Displays the current status of SELinux, including whether it is enabled or disabled, and the current mode.
  ```bash
  sestatus
  ```

- **getenforce**: Shows the current mode of SELinux (Enforcing, Permissive, or Disabled).
  ```bash
  getenforce
  ```

- **setenforce**: Temporarily changes the current mode of SELinux. It can be set to `0` for Permissive or `1` for Enforcing.
  ```bash
  setenforce 0  # Set to Permissive
  setenforce 1  # Set to Enforcing
  ```

### Configuration File

The SELinux configuration file is located at `/etc/selinux/config`. This file controls the SELinux mode on boot.

- **File Location**: `/etc/selinux/config`
- **Configurable Options**: 
  - **SELINUX**: Set the desired mode for SELinux. Possible values are:
    - `enforcing` - Enables SELinux enforcement.
    - `permissive` - Logs actions that would be denied.
    - `disabled` - Turns off SELinux.

#### Example of `/etc/selinux/config`:
```plaintext
SELINUX=enforcing
SELINUXTYPE=targeted
```

### Considerations

- **Switching Modes**: Changing from Enforcing to Permissive is often done for debugging purposes. However, ensure to revert back to Enforcing to maintain security.
- **Reboot**: Changes to the SELINUX variable in the configuration file take effect upon reboot.
- **Logs**: When in Permissive mode, pay attention to SELinux logs (usually found in `/var/log/audit/audit.log`) to understand what would be denied in Enforcing mode.

### Conclusion

Understanding SELinux and its configurations is crucial for maintaining the security posture of RHEL systems. Using commands like `sestatus`, `getenforce`, and modifications to the configuration file allows administrators to manage SELinux effectively. Always ensure to operate in Enforcing mode whenever possible to protect against unauthorized access.
