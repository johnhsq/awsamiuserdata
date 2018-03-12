# Sample Scripts used for AMI "User Data"

### Sample script to build apache and php servers

##### Script

    #!/bin/bash
    yum update -y
    yum install -y httpd24 php56
    service httpd start
    chkconfig httpd on
    groupadd www
    usermod -a -G www ec2-user
    chown -R root:www /var/www
    chmod 2775 /var/www
    find /var/www -type d -exec chmod 2775 {} +
    find /var/www -type f -exec chmod 0664 {} +
    echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php

##### Test
Create an instance by the command

    aws ec2 run-instances --image-id ami-824c4ee2 --count 1 --instance-type t2.micro --user-data file://userdata.txt --security-group-ids <value> --key-name <value> --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=<value>},{Key=CreatedBy,Value=<vaule},{Key=CreatedDate,Value=<value>}]'

You should see apache server from the following url

    http://<aws server>/

You should see phpinfo() from the following url

    http://<aws server>/phpinfo.php


### Sample script to build wordpress instance

##### Script

    #!/bin/bash
    yum update -y
    yum install -y httpd24 php56 mysql55-server php56-mysqlnd
    service httpd start
    chkconfig httpd on
    groupadd www
    usermod -a -G www ec2-user
    chown -R root:www /var/www
    chmod 2775 /var/www
    wget https://wordpress.org/latest.tar.gz
    tar -xvzf latest.tar.gz -C /var/www/html
    find /var/www -type d -exec chmod 2775 {} +
    find /var/www -type f -exec chmod 0664 {} +
    chown -R root:www /var/www/html/wordpress
    chmod 2775 /var/www/html/wordpress
    find /var/www/html/wordpress -type d -exec chmod 2777 {} +
    find /var/www/html/wordpress -type f -exec chmod 0666 {} +
    echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php

##### Test
Create an instance by the command

    aws ec2 run-instances --image-id ami-824c4ee2 --count 1 --instance-type t2.micro --user-data file://userdata.txt --security-group-ids <value> --key-name <value> --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=<value>},{Key=CreatedBy,Value=<vaule>},{Key=CreatedDate,Value=<value>}]'

You should see wordpress from the following url

    http://<aws server>/
    http://<aws server>/wordpress
    http://<aws server>/phpinfo.php

