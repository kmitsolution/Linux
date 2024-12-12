It seems like you're setting up a DNS server on a **Red Hat Enterprise Linux (RHEL)** system, and you've provided the configuration details along with some commands you're using to configure it. Let me go over the steps and explain what you're doing, then provide additional suggestions to ensure everything works smoothly.

### Overview of the Configuration:

1. **Installing Apache HTTP Server (`httpd`)**:
   ```bash
   yum install httpd
   systemctl start httpd
   ```
   These commands install and start the Apache web server. This step is generally used if you're hosting a web server on the same machine and want to serve web content alongside DNS services. However, this is unrelated to DNS setup.

2. **DNS Configuration (`/etc/named.conf`)**:
   You've edited the `/etc/named.conf` file to set up a DNS server. This file is the main configuration file for **BIND (Berkeley Internet Name Domain)**, which is the DNS software used on most Linux distributions.

   Here's what your configuration is doing:
   - `listen-on port 53 { 127.0.0.1; 192.168.2.139; };`  
     This tells BIND to listen for DNS queries on both the localhost (`127.0.0.1`) and the server's IP address (`192.168.2.139`) on port 53, which is the default DNS port.

   - `zone`
     ```
     zone "welcome.com" IN {
        type master;
        file "welcome.com.fzone";
        allow-query { any; };
};
```
     This defines a zone for the domain `welcome.com`, specifying that this server is the **master** for the zone, and the records will be stored in the `welcome.com.fzone` file.

3. **Check for Errors in `named.conf`**:
   ```bash
   named-checkconf
   ```
   This command checks the syntax and validity of the `named.conf` file. If there are no errors, it will return silently, otherwise, it will display any issues in your configuration.

4. **Configure Zone File**:
   ```bash
   cd /var/named/
   
   ```
 **welcome.com.fzone**
```
; base zone file for example.com
$TTL 2d    ; default TTL for zone
; Start of Authority RR defining the key characteristics of the zone (domain)
@         IN      SOA   ns1.example.com. hostmaster.example.com. (
                                800        ; serial number
                                12h        ; refresh
                                15m        ; update retry
                                3w         ; expiry
                                2h         ; minimum
                                )
; name server RR for the domain
           IN      NS      ns1.example.com.

www        IN      A       192.168.2.139
```
   <a href=https://bind9.readthedocs.io/en/v9.18.14/chapter3.html> Bind Zone Configuration Reference </a>
   
   You're creating a zone file `welcome.com.fzone` that contains DNS resource records (RRs) for the domain `welcome.com`. The content of the file looks like this:
   - The `$TTL` directive sets the default time-to-live (TTL) for the zone.
   - The `SOA` (Start of Authority) record defines the zone’s authority information, including the primary nameserver and the email address of the zone administrator (hostmaster).
   - The `NS` record specifies the nameserver for the domain.
   - The `A` record maps the domain `www.welcome.com` to the IP address `192.168.2.139`.

6. **Check the Zone File**:
   ```bash
   named-checkzone welcome.com welcome.com.fzone
   ```
   This command checks for errors in the `welcome.com.fzone` zone file, ensuring that the syntax and record structure are correct.

7. **View the Zone File**:
   ```bash
   cat welcome.com.fzone
   ```
   This command displays the contents of the `welcome.com.fzone` file. You should see your DNS resource records listed there.
8. **Update /etc/resolve.conf File**
   Add dns server ip in the nameserver
   ```
   search localdomain
   #nameserver 192.168.2.2
   nameserver 192.168.2.139

   ```
9. **Verify**
    ```
    nslookup www.welcome.com
    curl www.welcome.com
    ```

### Potential Issues and Additional Steps

1. **Ensure `named` is Installed and Running**:
   If not already installed, you can install BIND (the DNS server software) using:
   ```bash
   yum install bind
   systemctl start named
   systemctl enable named
   ```

2. **Check Firewall Settings**:
   Ensure that the server’s firewall is configured to allow DNS traffic on port 53. You can open the required port with:
   ```bash
   firewall-cmd --zone=public --add-port=53/tcp --permanent
   firewall-cmd --zone=public --add-port=53/udp --permanent
   firewall-cmd --reload
   ```

3. **SELinux Configuration**:
   If SELinux is enabled, you may need to adjust its policies to allow BIND to function properly. You can set the appropriate context:
   ```bash
   setsebool -P named_can_network_connect on
   ```

4. **Testing the DNS Server**:
   Once the DNS server is running, test it by querying it directly using `dig` or `nslookup`:
   ```bash
   dig @192.168.2.139 www.welcome.com
   ```
   If everything is set up correctly, you should receive the IP address `192.168.2.139` in the response.

5. **Restart `named` After Changes**:
   After making changes to your DNS configuration or zone files, always restart the `named` service to apply the changes:
   ```bash
   systemctl restart named
   ```

### Example Zone File (`welcome.com.fzone`)

Your `welcome.com.fzone` file should look something like this:

```bash
$TTL 2d
@        IN      SOA     ns1.welcome.com. hostmaster.welcome.com. (
                                800      ; Serial
                                12h      ; Refresh
                                15m      ; Retry
                                3w       ; Expire
                                2h       ; Minimum TTL
                                )
         IN      NS      ns1.welcome.com.

www      IN      A       192.168.2.139
```

### Conclusion:
The configuration you've provided looks mostly correct. Ensure that you've installed BIND, started the `named` service, and configured your firewall and SELinux settings properly. Once everything is set up, test your DNS server by querying it with `dig` or `nslookup` to ensure it resolves `www.welcome.com` to the correct IP address.
