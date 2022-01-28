## **UJIAN AKHIR SEMESTER**

***BISMILLAH FINAL PROJECT SISTEM ADMINISTRASI SERVER 2021/2022***

_________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________

1. Install Ansible for hosts ubuntu server

   - Install ansible

   ```markdown
   sudo apt install ansible sshpass
   ```

   

   - Creating directory

   ```markdown
   mkdir -p ~/ansible/tubes
   ```



 2. Creating lxc_db_server and install mariadb

    - Creating lxc container, start and entering container

    ```markdown
    lxc-create -n lxc_db_server -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
    lxc-start -n lxc_db_server
    apt update; apt upgrade -y; apt install -y nano
    ```
    
    
    
    - Install ssh server
    
    ```markdown
    apt install openssh-server
    ```
    
    
    
    - Adding configuration at /etc/ssh/sshd_config
    
    ```markdown
    nano /etc/ssh/sshd_config
    
    # setting config
    PermitRootLogin yes
    RSAAuthentication yes
    ```
    
    
    
    - Restart ssh server
    
    ```markdown
    service sshd restart
    ```
    
    
    
    - Setting password for lxc_db_server ssh
    
    ```markdown
    passwd (ex: 1)
    ```
    
    
    
    - Log out from lxc_db_server
    
    ```markdown
    exit
    ```
    
    
    
    - Enrolling lxc_db_server domain and ip to Ubuntu Server Host (/etc/hosts)
    
    ```markdown
    sudo nano /etc/hosts
    ```
    
    
    
    - Entering directory
    
    ```markdown
    cd ~/ansible/tubes
    ```
    
    
    
    - Creating hosts and adding script
    
    ```markdown
    [database]
    lxc_db_server ansible_host=lxc_mariadb.dev ansible_ssh_user=root ansible_become_pass=1
    
    ```
    
    
    
    - Creating install-mariadb.yml file and adding configuration
    
    ```markdown
    hosts: database
    vars:
      username: 'admin'
      password: '12345'
    roles:
      { role: db }
    ```
    
    
    
    - Creating directory roles/db, and creating tasks, handlers, templates in db directory
    
    ```markdown
    mkdir -p roles/db
    mkdir -p roles/db/handlers 
    mkdir -p roles/db/tasks
    mkdir -p roles/db/templates
    ```
    
    
    
    - Creating main.yml in roles/db/tasks and adding script configuration
      - nano roles/db/tasks/main.yml
    
    ```markdown
    ---
    - name: delete apt chache
          become: yes
          become_user: root
          become_method: su
          command: rm -vf /var/lib/apt/lists/*
    - name: install mariadb
          become: yes
          become_user: root
          become_method: su
          apt: name={{ item }} state=latest update_cache=true
          with_items:
           - python
           - mariadb-server
           - python-mysqldb
           - python-pymysql
    
    - name: Stop MySQL
          service: name=mysqld state=stopped
    
    - name: set environment variables
          shell: systemctl set-environment MYSQLD_OPTS="--skip-grant-tables"
    
    - name: Start MySQL
          service: name=mysqld state=started
    
    - name: sql query
          command:  mysql -u root --execute="UPDATE mysql.user SET authentication_string = PASSWORD('{{ password }}') WHERE User = 'root' AND Host = 'localhost';"
          
    - name: sql query flush
          command:  mysql -u root --execute="FLUSH PRIVILEGES"
          
    - name: Stop MySQL
          service: name=mysqld state=stopped
          
    - name: unset environment variables
          shell: systemctl unset-environment MYSQLD_OPTS
          
    - name: Start MySQL
          service: name=mysqld state=started
          
    - name: Create user for mysql
          command:  mysql -u root --execute="CREATE USER IF NOT EXISTS '{{ username }}'@'localhost' IDENTIFIED BY '{{ password }}';"
    
    - name: GRANT ALL PRIVILEGES to user {{username}}
          command:  mysql -u root --execute="GRANT ALL PRIVILEGES ON * . * TO '{{ username }}'@'localhost';"
    
    - name: Create user for remote mysql
          command:  mysql -u root --execute="CREATE USER IF NOT EXISTS '{{ username }}'@'%' IDENTIFIED BY '{{ password }}';"
    
    - name: GRANT ALL PRIVILEGES to remote user {{username}}
          command:  mysql -u root --execute="GRANT ALL PRIVILEGES ON * . * TO '{{ username }}'@'%';"
          
    - name: sql query flush
          command:  mysql -u root --execute="FLUSH PRIVILEGES"
          
    - name: Create DB Landing
          command:  mysql -u root --execute="CREATE DATABASE IF NOT EXISTS `landing`;"
    
    - name: Create DB blog
          command:  mysql -u root --execute="CREATE DATABASE IF NOT EXISTS `blog`;"
    
    - name: Create DB app
          command:  mysql -u root --execute="CREATE DATABASE IF NOT EXISTS `app`;"
    
    - name: Copy .my.cnf file with root password credentials
          template: 
            src=templates/my.cnf 
            dest=/etc/mysql/mariadb.conf.d/50-server.cnf
          notify: restart mysql
    ```
    
    
    
    - Creating my.cnf in roles/db/templates and adding script configuration
      - nano roles/db/templates/my.cnf
    
    ```markdown
    #
    # These groups are read by MariaDB server.
    # Use it for options that only the server (but not clients) should see
    #
    # See the examples of server my.cnf files in /usr/share/mysql/
    #
    
    # this is read by the standalone daemon and embedded servers
    [server]
    
    # this is only for the mysqld standalone daemon
     [mysqld]
    
    #
    # * Basic Settings
    #
    user		= mysql
    pid-file	= /var/run/mysqld/mysqld.pid
    socket		= /var/run/mysqld/mysqld.sock
    port		= 3306
    basedir		= /usr
    datadir		= /var/lib/mysql
    tmpdir		= /tmp
    lc-messages-dir	= /usr/share/mysql
    skip-external-locking
    
    # Instead of skip-networking the default is now to listen only on
    # localhost which is more compatible and is not less secure.
    bind-address		= 0.0.0.0
    
    #
    # * Fine Tuning
    #
    key_buffer_size		= 16M
    max_allowed_packet	= 16M
    thread_stack		= 192K
    thread_cache_size       = 8
    # This replaces the startup script and checks MyISAM tables if needed
    # the first time they are touched
    myisam_recover_options  = BACKUP
    #max_connections        = 100
    #table_cache            = 64
    #thread_concurrency     = 10
    
    #
    # * Query Cache Configuration
    #
    query_cache_limit	= 1M
    query_cache_size        = 16M
    
    #
    # * Logging and Replication
    #
    # Both location gets rotated by the cronjob.
    # Be aware that this log type is a performance killer.
    # As of 5.1 you can enable the log at runtime!
    #general_log_file        = /var/log/mysql/mysql.log
    #general_log             = 1
    #
    # Error log - should be very few entries.
    #
    log_error = /var/log/mysql/error.log
    #
    # Enable the slow query log to see queries with especially long duration
    #slow_query_log_file	= /var/log/mysql/mariadb-slow.log
    #long_query_time = 10
    #log_slow_rate_limit	= 1000
    #log_slow_verbosity	= query_plan
    #log-queries-not-using-indexes
    #
    # The following can be used as easy to replay backup logs or for replication.
    # note: if you are setting up a replication slave, see README.Debian about
    #       other settings you may need to change.
    #server-id		= 1
    #log_bin			= /var/log/mysql/mysql-bin.log
    expire_logs_days	= 10
    max_binlog_size   = 100M
    #binlog_do_db		= include_database_name
    #binlog_ignore_db	= exclude_database_name
    
    #
    # * InnoDB
    #
    # InnoDB is enabled by default with a 10MB datafile in /var/lib/mysql/.
    # Read the manual for more InnoDB related options. There are many!
    
    #
    # * Security Features
    #
    # Read the manual, too, if you want chroot!
    # chroot = /var/lib/mysql/
    #
    # For generating SSL certificates you can use for example the GUI tool "tinyca".
    #
    # ssl-ca=/etc/mysql/cacert.pem
    # ssl-cert=/etc/mysql/server-cert.pem
    # ssl-key=/etc/mysql/server-key.pem
    #
    # Accept only connections using the latest and most secure TLS protocol version.
    # ..when MariaDB is compiled with OpenSSL:
    # ssl-cipher=TLSv1.2
    # ..when MariaDB is compiled with YaSSL (default in Debian):
    # ssl=on
    
    #
    # * Character sets
    #
    # MySQL/MariaDB default is Latin1, but in Debian we rather default to the full
    # utf8 4-byte character set. See also client.cnf
    #
    character-set-server  = utf8mb4
    collation-server      = utf8mb4_general_ci
    
    #
    # * Unix socket authentication plugin is built-in since 10.0.22-6
    #
    # Needed so the root database user can authenticate without a password but
    # only when running as the unix root user.
    #
    # Also available for other users if required.
    # See https://mariadb.com/kb/en/unix_socket-authentication-plugin/
    
    # this is only for embedded server
    [embedded]
    
    # This group is only read by MariaDB servers, not by MySQL.
    # If you use the same .cnf file for MySQL and MariaDB,
    # you can put MariaDB-only options here
    [mariadb]
    
    # This group is only read by MariaDB-10.1 servers.
    # If you use the same .cnf file for MariaDB of different versions,
    # use this group for options that older servers don't understand
    [mariadb-10.1]
    
    ```
    
    
    
    - Creating main.yml in roles/db/handlers and adding script configuration
      - roles roles/db/handlers/main.yml
    
    ```markdown
    ---
    - name: restart mysql
      become: yes
      become_user: root
      become_method: su
      action: service name=mysql state=restarted
    ```
    
    
    
    - Running command
    
    ```markdown
    ansible-playbook -i hosts install-mariadb.yml -k
    ```
    
    
    
    - Checking, if mariadb has installed in lxc_db_server
    
    ```markdown
    ssh root@lxc_db_server.dev
    mysql -u admin -p
    show databases; 
    ```
    
    
    
3. Creating lxc_php5_1 and lxc_php5_2

   - Creating lxc container, start and entering container

   ```markdown
   lxc-create -n lxc_php5_1 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
   lxc-create -n lxc_php5_2 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
   lxc-start -n lxc_php5_1 
   lxc_start -n lxc_php5_2
   apt update; apt upgrade -y; apt install -y nano
   ```

   

   - Configuration lxc_php5_1 and lxc_php5_2 like this commands below
   - Install ssh server

   ```markdown
   apt install openssh-server
   ```

   

   - Adding configuration at /etc/ssh/sshd_config

   ```markdown
   nano /etc/ssh/sshd_config
   PermitRootLogin yes
   RSAAuthentication yes
   ```

   

   - Restart ssh server

   ```markdown
   service sshd restart
   ```

   

   - Setting password for lxc_php5_1 and lxc_php5_2

   ```markdown
   passwd (ex: 1)
   ```

   

   - Log out from lxc_php5_1 and lxc_php5_2

   ```markdown
   exit
   ```

   

   - Enrolling lxc_php5_1 and lxc_php5_2 domain and ip to Ubuntu Server Host (/etc/hosts)

   ```markdown
   sudo nano /etc/hosts
   ```

   

   - Entering directory

   ```markdown
   cd ~/ansible/tubes
   ```

   

   - Creating hosts and adding script

   ```markdown
   [database]
   lxc_php5_1 ansible_host=lxc_php5_1.dev ansible_ssh_user=root ansible_become_pass=1
   lxc_php5_2 ansible_host=lxc_php5_2.dev ansible_ssh_user=root ansible_become_pass=1
   ```

   

   - Creating install-ci.yml file and adding configuration

   ```markdown
   hosts: php5
   vars:
     git_url: 'https://github.com/aldonesia/sas-ci'
     destdir: '/var/www/html/ci'
     domain: 
        'lxc_php5_1.dev'
        'lxc_php5_2.dev'
   roles:
        app
   ```

   

   - Creating directory roles/app, and creating tasks, handlers, templates in db directory

   ```markdown
   mkdir -p roles/app
   mkdir -p roles/app/handlers 
   mkdir -p roles/app/tasks
   mkdir -p roles/app/templates
   ```

   - Creating main.yml in roles/app/handlers and adding script configuration
     - nano roles/app/handlers/main.yml

   ```markdown
   ---
   - name: restart nginx
     become: yes
     become_user: root
     become_method: su
     action: service name=nginx state=restarted
   
   - name: restart php
     become: yes
     become_user: root
     become_method: su
     action: service name=php5.6-fpm state=restarted
   ```

   - Creating main.yml in roles/app/tasks and adding script configuration
     - nano roles/app/tasks/main.yml

   ```markdown
   ---
   - name: delete apt chache
     become: yes
     become_user: root
     become_method: su
     command: rm -vf /var/lib/apt/lists/*
   
   - name: install requirement dpkg to install php5
     become: yes
     become_user: root
     become_method: su
     apt: name={{ item }} state=latest update_cache=true
     with_items:
       - ca-certificates
       - apt-transport-https
       - wget
       - curl
       - python-apt
       - software-properties-common
       - git
   
   - name: Add key
     apt_key:
       url: https://packages.sury.org/php/apt.gpg
       state: present
   
   - name: Add Php Repository
     apt_repository:
         repo: "deb https://packages.sury.org/php/ stretch main"
         state: present
         filename: php.list
         update_cache: true
   
   - name: install nginx php5
     become: yes
     become_user: root
     become_method: su
     apt: name={{ item }} state=latest update_cache=true
     with_items:
       - nginx
       - nginx-extras
       - php5.6
       - php5.6-fpm
       - php5.6-common
       - php5.6-cli
       - php5.6-curl
       - php5.6-mbstring
       - php5.6-mysqlnd
       - php5.6-xml
   
   - name: Git clone repo sas-ci
     become: yes
     git:
       repo: '{{ git_url }}'
       dest: "{{ destdir }}"
   
   - name: Copy app.conf
     template:
       src=templates/app.conf
       dest=/etc/nginx/sites-available/{{ domain }}
     vars:
       servername: '{{ domain }}'
   
   - name: Delete another nginx config
     become: yes
     become_user: root
     become_method: su
     command: rm -f /etc/nginx/sites-enabled/*
   
   - name: Symlink app.conf
     command: ln -sfn /etc/nginx/sites-available/{{ domain }} /etc/nginx/sites-enabled/{{ domain }}
     notify:
       - restart nginx
   
   - name: Write {{ domain }} to /etc/hosts
     lineinfile:
       dest: /etc/hosts
       regexp: '.*{{ domain }}$'
       line: "127.0.0.1 {{ domain }}"
       state: present
   
   ```

   - Creating app.conf in roles/app/templates and adding script configuration
     - nano roles/app/templates/app.conf

   ```markdown
   server {
     listen 80;
     server_name {{servername}};
     root {{ destdir }};
     index index.php;
     location / {
        try_files $uri $uri/ /index.php?$query_string;
     }
     location ~ \.php$ {
        fastcgi_pass unix:/run/php/php5.6-fpm.sock;  #Sesuaikan dengan versi PHP
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME {{ destdir }}$fastcgi_script_name;
        include fastcgi_params;
     }
   }
   ```

4. Creating lxc_php7_1, lxc_php7_2, lxc_php7_3,  lxc_php7_4, and lxc_php7_6

   - Creating lxc container, start and entering container

   ```markdown
   lxc-create -n lxc_php7_1 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
   lxc-create -n lxc_php7_2 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
   lxc-create -n lxc_php7_3 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
   lxc-create -n lxc_php7_4 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
   lxc-create -n lxc_php7_6 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
   lxc-start -n lxc_php7_1 
   lxc_start -n lxc_php7_2
   lxc_start -n lxc_php7_3
   lxc_start -n lxc_php7_4
   lxc_start -n lxc_php7_6
   apt update; apt upgrade -y; apt install -y nano
   ```

   

   - Configuration lxc_php7_1, lxc_php7_2 , lxc_php7_3,  lxc_php7_4, and lxc_php7_6 like this commands below
   - Install ssh server

   ```markdown
   apt install openssh-server
   ```

   

   - Adding configuration at /etc/ssh/sshd_config

   ```markdown
   nano /etc/ssh/sshd_config
   PermitRootLogin yes
   RSAAuthentication yes
   ```

   

   - Restart ssh server

   ```markdown
   service sshd restart
   ```

   

   - Setting password for lxc_php7_1, lxc_php7_2 , lxc_php7_3,  lxc_php7_4, and lxc_php7_6

   ```markdown
   passwd (ex: 1)
   ```

   

   - Log out from lxc_php7_1, lxc_php7_2 , lxc_php7_3,  lxc_php7_4, and lxc_php7_6

   ```markdown
   exit
   ```

   

   - Enrolling lxc_php7_1, lxc_php7_2 , lxc_php7_3,  lxc_php7_4, and lxc_php7_6 domain and ip to Ubuntu Server Host (/etc/hosts)

   ```markdown
   sudo nano /etc/hosts
   ```

   

   - Entering directory

   ```markdown
   cd ~/ansible/tubes
   ```

   

   - Creating hosts and adding script

   ```markdown
   [database]
   lxc_php7_1 ansible_host=lxc_php7_1.dev ansible_ssh_user=root ansible_become_pass=1
   lxc_php7_2 ansible_host=lxc_php7_2.dev ansible_ssh_user=root ansible_become_pass=1
   lxc_php7_3 ansible_host=lxc_php7_3.dev ansible_ssh_user=root ansible_become_pass=1
   lxc_php7_4 ansible_host=lxc_php7_4.dev ansible_ssh_user=root ansible_become_pass=1
   lxc_php7_6 ansible_host=lxc_php7_6.dev ansible_ssh_user=root ansible_become_pass=1
   ```

   

   - Creating install-ci.yml file and adding configuration

   ```markdown
   ---
   - hosts: ubuntu_php7
     vars:
       username: 'admin'
       password: 'SysAdminSas0102' #DON'T FORGET TO CHANGE
       domain: 'lxc_php7.dev'
     roles:
       - wp
   ```

   

   - Creating directory roles/app, and creating tasks, handlers, templates in db directory

   ```markdown
   ```

   

   - Creating main.yml in roles/wp/handlers and adding script configuration
     - nano roles/wp/handlers

   ```markdown
   ---
   - name: restart php
     become: yes
     become_user: root
     become_method: su
     action: service name=php7.4-fpm state=restarted
   
   - name: restart nginx
     become: yes
     become_user: root
     become_method: su
     action: service name=nginx state=restarted
   ```

   - Creating main.yml in roles/wp/tasks and adding script configuration
     - nano roles/wp/tasks

   ```markdown
   ---
   - name: delete apt chache
     become: yes
     become_user: root
     become_method: su
     command: rm -vf /var/lib/apt/lists/*
   
   - name: install php
     become: yes
     become_user: root
     become_method: su
     apt: name={{ item }} state=latest update_cache=true
     with_items:
       - nginx
       - nginx-extras
       - php7.4
       - php7.4-fpm
       - php7.4-curl
       - php7.4-xml
       - php7.4-gd
       - php7.4-opcache
       - php7.4-mbstring
       - php7.4-zip
       - php7.4-json
       - php7.4-cli
       - php7.4-mysqlnd
       - php7.4-xmlrpc
       - php7.4-curl
       - wget
       - curl
   
   - name: wget wordpress
     shell: wget -c http://wordpress.org/latest.tar.gz
   
   - name: tar xvzf
     shell: tar -xvzf latest.tar.gz
   
   - name: make page
     shell: cp -R wordpress /var/www/html/blog
   
   - name: chmod
     become: yes
     become_user: root
     become_method: su
     command: chmod 775 -R /var/www/html/blog/
   
   - name: Copy .wp-config.conf
     template:
       src=templates/wp.conf
       dest=/var/www/html/blog/wp-config.php
   
   - name: Copy wp.local
     template:
       src=templates/wp.local
       dest=/etc/nginx/sites-available/{{ domain }}
     vars:
       servername: '{{ domain }}'
   
   - name: Symlink wp.local
     command: ln -sfn /etc/nginx/sites-available/{{ domain }} /etc/nginx/sites-enabled/{{ domain }}
     notify:
       - restart nginx
   
   - name: Write {{ domain }} to /etc/hosts
     lineinfile:
       dest: /etc/hosts
       regexp: '.*{{ domain }}$'
       line: "127.0.0.1 {{ domain }}"
       state: present
   
   - name: enable module php mbstring
     command: phpenmod mbstring
     notify:
       - restart php
   ```

   - Creating wp.conf in roles/wp/templates and adding script configuration
     - nano roles/app/templates/wp.conf

   ```markdown
   <?php
   /**
    * The base configuration for WordPress
    *
    * The wp-config.php creation script uses this file during the installation.
    * You don't have to use the web site, you can copy this file to "wp-config.php"
    * and fill in the values.
    *
    * This file contains the following configurations:
    *
    * * MySQL settings
    * * Secret keys
    * * Database table prefix
    * * ABSPATH
    *
    * @link https://wordpress.org/support/article/editing-wp-config-php/
    *
    * @package WordPress
    */
   
   define('WP_HOME', 'http://vm.local/blog');
   define('WP_SITEURL', 'http://vm.local/blog');
   
   // ** MySQL settings - You can get this info from your web host ** //
   /** The name of the database for WordPress */
   define( 'DB_NAME', 'blog' );
   
   /** MySQL database username */
   define( 'DB_USER', 'admin' );
   
   /** MySQL database password */
   define( 'DB_PASSWORD', 'SysAdminSas0102' );
   
   /** MySQL hostname */
   define( 'DB_HOST', '10.0.3.200:3306' );
   
   /** Database charset to use in creating database tables. */
   define( 'DB_CHARSET', 'utf8' );
   
   /** The database collate type. Don't change this if in doubt. */
   define( 'DB_COLLATE', '' );
   
   /**#@+
    * Authentication unique keys and salts.
    *
    * Change these to different unique phrases! You can generate these using
    * the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}.
    *
    * You can change these at any point in time to invalidate all existing cookies.
    * This will force all users to have to log in again.
    *
    * @since 2.6.0
    */
   define( 'AUTH_KEY',         'put your unique phrase here' );
   define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
   define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
   define( 'NONCE_KEY',        'put your unique phrase here' );
   define( 'AUTH_SALT',        'put your unique phrase here' );
   define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
   define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
   define( 'NONCE_SALT',       'put your unique phrase here' );
   
   /**#@-*/
   
   /**
    * WordPress database table prefix.
    *
    * You can have multiple installations in one database if you give each
    * a unique prefix. Only numbers, letters, and underscores please!
    */
   $table_prefix = 'wp_';
   
   /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    *
    * For information on other constants that can be used for debugging,
    * visit the documentation.
    *
    * @link https://wordpress.org/support/article/debugging-in-wordpress/
    */
   define( 'WP_DEBUG', false );
   
   /* Add any custom values between this line and the "stop editing" line. */
   /* That's all, stop editing! happy publishing. */
   
   /** Absolute path to the WordPress Directory*/
   if ( ! defined( 'ABSPATH' ) ) {
          define( 'ABSPATH', __DIR__ . '/' );
   }
   
   /** Sets up WordPress vars and include file */
   require_once ABSPATH . 'wp-settings.php';
   ```

   - Creating wp.local in roles/wp/templates and adding script configuration
     - nano roles/app/templates/wp.local

   ```markdown
   server {
        listen 80;
        listen [::]:80;
   
        # Log files for Debugging
        access_log /var/log/nginx/wordpress-access.log;
        error_log /var/log/nginx/wordpress-error.log;
   
        # Webroot Directory for WordPress
        root /var/www/html/blog;
        index index.php index.html index.htm;
   
        # Your Domain Name
        server_name lxc_php7.dev;
   
        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }
   
        # PHP-FPM Configuration Nginx
        location ~ \.php$ {
                try_files $uri =404;
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                fastcgi_pass unix:/run/php/php7.4-fpm.sock;
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
        }
   }
   ```

   

   
