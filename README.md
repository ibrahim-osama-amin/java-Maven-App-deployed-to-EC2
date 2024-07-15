A couple of notes 

This build requires the EC2 to be installed ready with the below:
Ports 8080, 22 opened (preferabally to your own ip address only for security)

You will need to install docker
# dnf install docker -y
You will need to do docker login
docker login and then enter your credentials

You will need to install docker compose

[ec2-user@ip-172-31-0-57 ~]$ sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100 11.6M  100 11.6M    0     0  15.6M      0 --:--:-- --:--:-- --:--:-- 15.6M
[ec2-user@ip-172-31-0-57 ~]$
[ec2-user@ip-172-31-0-57 ~]$ cd /usr/local/bin/
[ec2-user@ip-172-31-0-57 bin]$ ls
docker-compose
[ec2-user@ip-172-31-0-57 bin]$ chmod +x docker-compose

I had to install this python package also
[ec2-user@ip-172-31-0-57 bin]$ sudo yum install libxcrypt-compat
