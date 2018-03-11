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
You should see apache server from the following url

    http://<aws server>/

You should see phpinfo() from the following url

    http://<aws server>/phpinfo.php
