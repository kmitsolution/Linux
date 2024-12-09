### **What is SELinux?**

---

#### **Definition:**
**SELinux (Security-Enhanced Linux)** is a security architecture designed for Linux systems that implements **Mandatory Access Control (MAC)**. Unlike traditional discretionary access control (DAC), where users can control access to their own resources, SELinux enforces system-wide security policies that limit the actions of users and processes. These policies are designed to prevent unauthorized access, mitigate vulnerabilities, and minimize the impact of attacks.

---

#### **History and Development:**
SELinux was initially developed by the **National Security Agency (NSA)** in collaboration with **Red Hat** to address concerns regarding Linux's security model. It was created to enforce a more rigorous and fine-grained security policy across Linux systems. 

The primary goal of SELinux is to provide an added layer of security that operates in parallel with other Linux security features (like file permissions), enhancing protection against vulnerabilities and exploits, even if a user or application is compromised.

Since its introduction, SELinux has become an integral part of many Linux distributions, including **Red Hat Enterprise Linux (RHEL)**, **CentOS**, and **Fedora**. Its default policy, called the **Targeted Policy**, restricts access for critical services like web servers, but leaves other parts of the system with fewer restrictions.

---

#### **Why Use SELinux?**
- **Enhanced Security**: SELinux helps prevent unauthorized access to sensitive data and system resources. It ensures that even if an attacker compromises an application or process, they are severely restricted in what they can access or modify.
  
- **Protection Against Exploits**: Many attacks take advantage of bugs in software to gain elevated privileges. SELinux limits the potential damage by confining the attacked process, reducing the attacker’s ability to escalate privileges, access files, or take control of the system.
  
- **Enforcement of Strict Access Control**: SELinux uses policies that strictly define which resources can be accessed and under what conditions. These policies are applied universally to processes and files, regardless of user ownership, ensuring consistent access control.

---

#### **Core Concepts:**

1. **Subjects** (e.g., processes):
   - **Subjects** are entities that perform actions in the system, typically processes or programs. For example, a running program like Apache (httpd) is a subject.
   - SELinux uses security contexts to define the privileges granted to a subject.

2. **Objects** (e.g., files, directories):
   - **Objects** are resources that subjects interact with, such as files, directories, network sockets, or devices. These objects have security labels that define the conditions under which they can be accessed.
   - SELinux assigns these resources a security context that governs their interactions with subjects.

3. **Labels/Contexts**:
   - Each subject and object is assigned a **security label** (also called **security context**). These labels are composed of several components:
     - **User**: The identity of the subject (e.g., `system_u`).
     - **Role**: Defines what actions the subject can perform (e.g., `object_r`).
     - **Type**: Specifies what the object or subject is (e.g., `httpd_t` for an Apache process).
     - **Level**: Used in **Multi-Level Security (MLS)** to indicate classification levels (e.g., `s0` for a low-security level).
   
4. **Policy**:
   - SELinux operates based on predefined **policies**, which are sets of rules that determine how subjects can interact with objects.
   - The policy specifies:
     - Which subjects can access which objects.
     - What operations a subject can perform on an object (e.g., read, write, execute).
   - There are several predefined policies in SELinux:
     - **Targeted Policy**: The default policy for most Linux systems, which primarily focuses on restricting network-facing services.
     - **MLS Policy**: Used in highly secure environments to enforce strict data classification and access rules.
     - **Custom Policies**: Administrators can define custom policies tailored to specific needs.

By enforcing these policies, SELinux ensures that even if an attacker gains control of a process or application, they will be limited in what they can do, significantly reducing the impact of potential attacks.
