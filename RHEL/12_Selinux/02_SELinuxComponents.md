### **Understanding SELinux Components**

---

#### **1. Security Labels and Contexts**
In SELinux, every subject (process) and object (resource) in the system is assigned a **security label** (also known as a **context**). These labels are used to enforce security policies and govern how different entities can interact with each other.

- **User**:  
   The **user** component identifies the user or the entity performing the action. It is typically set to system-level users in SELinux, such as `system_u`. This is different from traditional Linux users because it defines the security roles, not just the operating system user.  
   - Example: `system_u`, `staff_u`
  
- **Role**:  
   The **role** defines what actions the subject (user or process) can perform. It is used to assign specific privileges to the user or process within the context of SELinux's policy framework.  
   - Example: `object_r`, `system_r`
     - `object_r` is used for object roles, while `system_r` might be used for system processes.

- **Type**:  
   The **type** defines the resource type or classification for the subject or object. The most important part of the context, as it specifies the policies controlling access between subjects and objects.  
   - Example: `httpd_t`, `ssh_exec_t`, `sysadm_t`
     - `httpd_t` is a type for web server processes, while `ssh_exec_t` relates to SSH execution.

- **Level** (Optional):  
   The **level** is an optional component used in **Multi-Level Security (MLS)**, which is a more advanced feature. It assigns security levels to subjects and objects for systems where access control is based on classification.  
   - Example: `s0`, `s1`
     - `s0` represents a low security level, while `s1` is a higher level.

A complete security context might look like:  
`system_u:object_r:httpd_sys_content_t:s0`  
This context indicates that a file is owned by `system_u`, is an object of type `httpd_sys_content_t`, and has a security level of `s0`.

---

#### **2. SELinux Types of Policies**
SELinux enforces various **security policies** that govern how resources can be accessed and by whom. The following are the key types of policies available:

- **Targeted Policy** (Default):
   - The **Targeted Policy** is the default SELinux policy used in many Linux distributions, including Red Hat Enterprise Linux (RHEL). It focuses on restricting critical system processes, such as network-facing services (e.g., web servers, SSH), while leaving other processes and resources with fewer restrictions.
   - For example, in the Targeted Policy, the Apache web server (`httpd`) might be restricted from accessing system files or sending network traffic unless explicitly allowed by policy.

- **MLS (Multi-Level Security) Policy**:
   - The **MLS policy** is designed for environments that require strict classification of information, often seen in military or government settings. It enforces data classification and clearances, ensuring that only users with the appropriate security level can access sensitive resources.
   - With MLS, the policy uses the **Level** component of the SELinux context (`s0`, `s1`, etc.) to classify data and users, and access is strictly controlled based on these levels.

- **Custom Policies**:
   - Administrators can create **custom SELinux policies** to address specific needs within an organization. These policies are tailored to control access for particular applications, services, or users.
   - Custom policies can be created using policy modules and defined in policy files. Once created, these modules are installed into the SELinux policy framework, ensuring the customized security rules are applied.

---

#### **3. SELinux Modes**
SELinux operates in different modes, which define how the system enforces its security policies:

- **Enforcing Mode**:  
   In **Enforcing mode**, SELinux policy is actively enforced. Any action that violates the SELinux policy is blocked, and the violation is logged. This is the most secure mode, as SELinux will prevent unauthorized actions from occurring.
   - Example: If a process tries to access a file it is not allowed to, the access is denied and logged.

- **Permissive Mode**:  
   In **Permissive mode**, SELinux does **not** enforce the security policies, but it **logs violations**. This mode is helpful for debugging or for understanding the potential consequences of a policy without actually blocking actions.
   - Example: A process trying to access a restricted resource would be logged but allowed to proceed.

- **Disabled Mode**:  
   When SELinux is **disabled**, the security framework is completely turned off. No policies are enforced, and no security contexts are applied. This mode is often used for troubleshooting or when SELinux is not required, but it should be avoided in production environments because it removes an important layer of security.
   - Example: The system behaves as if SELinux is not present at all, and all security restrictions are lifted.

---

### **Conclusion:**
- **Security Labels/Contexts** define what users and processes can do, and which resources they can access, based on a combination of User, Role, Type, and Level.
- **SELinux Policies** define the security framework, with the **Targeted Policy** being the most commonly used. **MLS Policies** provide stricter controls for classified environments, and **Custom Policies** allow for tailored security.
- SELinux's **modes** (Enforcing, Permissive, Disabled) provide flexibility in how security is applied, with **Enforcing** being the most secure.
