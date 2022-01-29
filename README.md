# Coding with Reinus (DRAFT)
but its like crazy CI/CD pipeline, clustered on kube lol gets the hello world string from cache which gets it from db


# Basic Frame work
- set up micro K8s cluster across nodes
- configure master node
- 
# Raspberry Pi4 Headless Server set-up x 4
## Manual set up
Utilized Raspberry Pi Imager to create Ubuntu 20.04.3 LTS .IMG SD cards
After .IMG is complete go into boot directory and add SSH file (no extension)
This will enable SSH.  

![image](https://user-images.githubusercontent.com/31022640/151653213-0727c446-f4c9-4a9e-a3d3-53e6ea39485d.png)


Insert SD and power Pi, allowing time for initial boot sequence.
SSH into unit.
Enter default password and change when prompted.
change hostname(s) 
```
hostnamectl set-hostname [<name>]
```
Voila

![image](https://user-images.githubusercontent.com/31022640/151655643-eb9fd7dd-7dec-4edb-9a54-9fa7f09a2c7a.png)

# Kubernetes K3s 
Enable cgroups, K3s needs this to start systemd service.
You need to add a couple lines to the cmdline.txt file in boot directory
```
sudo nano /boot/firmware/cmdline.txt
```
and add
```
cgroup_memory=1 cgroup_enable=memory
```
# Docker

## Security
 - process restriction to limit user escalation (DevSecOps)
 - -Set ulimit for container hardening

 ### test container with 
 
 Check for security rules 
 ```
 sudo auditctl -l
 ```
 Run docker-bench-security.sh and store output to /tmp file
 ```
 sudo ./docker-bench-security.sh > /tmp/<name>.out
 ```
 Check with 
 ```
 more /tmp/<name>.out
 ```
 Sample warning 
 
 [WARN] 1.1.5 - Ensure auditing is configured for Docker files and directories - /var/lib/docker (Automated)
 
 ### Create new rule
  Use the auditctl command to add a rule to audit the Docker files in /var/lib/docker:

 ```
 sudo auditctl -w /var/lib/docker -k "<name>"
 ```
 Re run docker-bench-security.sh and store output to /tmp file
 ```
 sudo ./docker-bench-security.sh > /tmp/<name2>.out
 ```
 Compare benchmarks
 ```
 diff /tmp/bench1.out /tmp/bench2.out
 ```
 ![image](https://user-images.githubusercontent.com/31022640/151647890-246aa4bd-2428-44d2-8008-14bb194a63f4.png) 
 
 You can see that we configured auditing correctly and mitigated a potential point of exploitation
 
 Check results with 
 ```
 sudo auditctl -l
 ```

