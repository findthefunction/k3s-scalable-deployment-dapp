# Coding with Reinus (DRAFT)
but its like crazy CI/CD pipeline, clustered on kube lol gets the hello world string from cache which gets it from db


# Basic Frame work

1 create images and install images across all Pi 4s
  -set ip addresses for each node
2 set up micro K8s cluster across nodes

# Docker

## Security
 - process restriction to limit user escalation (DevSecOps)
 - -Set ulimit for container hardening

 ### test container with 
 
 CHeck for security rules 
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

