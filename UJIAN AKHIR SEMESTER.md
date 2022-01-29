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

2. Creating lxc_db_server, lxc_php5_1, lxc_php5_2, lxc_php7_1, lxc_php7_2, lxc_php7_3,  lxc_php7_4, lxc_php7_5 and lxc_php7_6

    - Creating lxc container, start and entering container

    ```markdown
    lxc-create -n lxc_db_server -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
    lxc-create -n lxc_php5_1 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
    lxc-create -n lxc_php5_2 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
    lxc-create -n lxc_php7_1 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
    lxc-create -n lxc_php7_2 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
    lxc-create -n lxc_php7_3 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
    lxc-create -n lxc_php7_4 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
    lxc-create -n lxc_php7_5 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
    lxc-create -n lxc_php7_6 -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
    lxc-start -n lxc_db_server
    lxc-start -n lxc_php5_1 
    lxc_start -n lxc_php5_2
    lxc-start -n lxc_php7_1 
    lxc_start -n lxc_php7_2
    lxc_start -n lxc_php7_3
    lxc_start -n lxc_php7_4
    lxc_start -n lxc_php7_5
    lxc_start -n lxc_php7_6
    apt update; apt upgrade -y; apt install -y nano
    ```

    - Setting all of containers auto start

    ```markdown
    echo "lxc.start.auto = 1" >> /var/lib/lxc/lxc_db_server/config
    echo "lxc.start.auto = 1" >> /var/lib/lxc/lxc_php5_1r/config
    echo "lxc.start.auto = 1" >> /var/lib/lxc/lxc_php5_2/config
    echo "lxc.start.auto = 1" >> /var/lib/lxc/lxc_php7_1/config
    echo "lxc.start.auto = 1" >> /var/lib/lxc/lxc_php7_2/config
    echo "lxc.start.auto = 1" >> /var/lib/lxc/lxc_php7_3/config
    echo "lxc.start.auto = 1" >> /var/lib/lxc/lxc_php7_4/config
    echo "lxc.start.auto = 1" >> /var/lib/lxc/lxc_php7_5/config
    echo "lxc.start.auto = 1" >> /var/lib/lxc/lxc_php7_6/config
    ```

    - Setting hosts and adding IP Address and Domain for each containers in VM ubuntu server

      - nano /etc/hosts

      ![4d63a659-9d10-4376-9f9d-a9e7317898c1](https://user-images.githubusercontent.com/93067781/151665341-24f23072-789e-4e04-97c5-39c8751f5230.jpg)

    - Setting IP Static for all off containers until this allow picture

      - If have set each containers check the IP containers in VM ubuntu server with command : lxc-ls -f

      ![4bd93110-7a7b-412f-8979-0b556d747a3c](https://user-images.githubusercontent.com/93067781/151665457-a40ec96a-f6f4-4cb7-80f6-bd9570a52aff.jpg)

    - Configuration lxc_db_server, lxc_php5_1, lxc_php5_2, lxc_php7_1, lxc_php7_2, lxc_php7_3,  lxc_php7_4, lxc_php7_5 and lxc_php7_6 like this commands below

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

    - Setting password for lxc_db_server ssh, lxc_php5_1, lxc_php5_2, lxc_php7_1, lxc_php7_2, lxc_php7_3,  lxc_php7_4, lxc_php7_5 and lxc_php7_6

    ```markdown
    passwd (ex: 1)
    ```

    - Log out from lxc_db_server, lxc_php5_1, and lxc_php5_2, lxc_php7_1, lxc_php7_2, lxc_php7_3,  lxc_php7_4, lxc_php7_5 and lxc_php7_6

    ```markdown
    exit
    ```

    - Enrolling lxc_db_server ssh, lxc_php5_1, lxc_php5_2, lxc_php7_1, lxc_php7_2, lxc_php7_3,  lxc_php7_4, lxc_php7_5 and lxc_php7_6 domain and ip to Ubuntu Server Host (/etc/hosts)

    ```markdown
    sudo nano /etc/hosts
    ```

    - Entering directory

    ```markdown
    cd ~/ansible/tubes
    ```

    - Creating hosts and adding script

    ![ba94bcf4-2e69-43e7-b14a-dbdb91ef7a87](https://user-images.githubusercontent.com/93067781/151647679-806dcaeb-6e62-4096-a910-74f0da06903d.jpg)

    

3. Creating install-mariadb.yml file and adding configuration

   ```markdown
   hosts: database
   vars:
     username: 'admin'
     password: 'admin'
     domain: 'lxc_mariadb.dev'
   roles:
      - db
      - pma
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

   

   ![Ansible SS1](https://user-images.githubusercontent.com/93067781/151578294-ea382d28-0c93-4290-ae03-3958105d413a.jpg)

   

4. Creating install-ci.yml file and adding configuration

   ```markdown
   hosts: php5
   vars:
     git_url: 'https://github.com/aldonesia/sas-ci'
     destdir: '/var/www/html/ci'
     domain: 'lxc_php5_1.dev' #lxc_php5_2.dev
   roles:
      - app
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
     server_name {{ domain }};
     root /var/www/html/wp;
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

   - Running command

   ```markdown
   ansible-playbook -i hosts install-ci.yml -k
   ```

   

5. Creating install-wp.yml file and adding configuration

   ```markdown
   ---
   - hosts: wordpress
     vars:
       username: 'admin'
       password: 'admin' #DON'T FORGET TO CHANGE
       domain: 'wordpress.dev' #wordpress_1.dev wordpress_2.dev wordpress_3.dev
     roles:
       - wp
   ```

   - Creating directory roles/wp, and creating tasks, handlers, templates in db directory

   ```markdown
   mkdir -p roles/wp
   mkdir -p roles/wp/handlers 
   mkdir -p roles/wp/tasks
   mkdir -p roles/wp/templates
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
       - bind9
       - dnsutils
   
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
   - name: creates directory
     file:
      path: /var/www/html/news/wp
      state: directory
   
   - name: copy conf.local
     template:
       src=templates/named.conf.local
       dest=/var/www/html/news/wp
     notify:
      - restart bind
   
   - name: copy kelompok7.fpas
     template:
       src=templates/kelompok7.fpas
       dest=/var/www/html/news/wp
     notify:
      - restart bind
   
   - name: copy 43.168.192
     template:
       src=templates/43.168.192.in-addr.arpa
       dest=/var/www/html/news/wp
     notify:
      - restart bind
   - name: copy named.conf
     template:
       src=templates/named.conf.options
       dest=/var/www/html/news/wp
     notify:
      - restart bind
   ```

   - Creating wp.conf in roles/wp/templates and adding script configuration
     - nano roles/wp/templates/wp.conf

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
   
   // * MySQL settings - You can get this info from your web host * //
   /** The name of the database for WordPress */
   define( 'DB_NAME', 'news' );
   
   /** MySQL database username */
   define( 'DB_USER', 'admin' );
   
   /** MySQL database password */
   define( 'DB_PASSWORD', 'admin' );
   
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
     - nano roles/wp/templates/wp.local

   ```markdown
   server {
        listen 80;
        listen [::]:80;
   
        # Log files for Debugging
        access_log /var/log/nginx/wordpress-access.log;
        error_log /var/log/nginx/wordpress-error.log;
   
        # Webroot Directory for WordPress
        root /var/www/html/wp;
        index index.php index.html index.htm;
        
        # Your Domain Name
        server_name {{ domain }};
   
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

   - Creating 43.168.192.in-addr.arpa in roles/wp/templates and adding script configuration
     - nano roles/wp/templates/43.168.192.in-addr.arpa

   ```markdown
   ;
   ; BIND reverse data file loopback interface
   ;
   $TTL    604800
   @       IN      SOA     kelompok7.fpas. root.kelompok7.fpas. (
                                 1         ; Serial
                            604800         ; Refresh
                             86400         ; Retry
                           2419200         ; Expire
                            604800 )       ; Negative Cache TTL
   ;
   43.168.192.in-addr.arpa. IN NS kelompok7.fpas.
   185 IN PTR kelompok7.fpas.
   ```

   - Creating kelompok7.fpas in roles/wp/templates and adding script configuration
     - nano roles/wp/templates/kelompok7.fpas

   ```markdown
   ;
   ; BIND reverse data file for loopback interface
   ;
   $TTL    604800
   @       IN      SOA     kelompok7.fpas. root.kelompok7.fpas. (
                                 1         ; Serial
                            604800         ; Refresh
                             86400         ; Retry
                           2419200         ; Expire
                            604800 )       ; Negative Cache TTL
   ;
   @       IN      NS      kelompok7.fpas.
   @       IN      A      192.168.43.185 
   news       IN      CNAME      kelompok7.fpas.
   ```

   - Creating wp.conf.local in roles/wp/templates and adding script configuration
     - nano roles/wp/templates/wp.conf.local

   ```markdown
   //
   // Do any local configuration here
   //
   
   // Consider adding the 1918 zones here, if they are not used in your
   // organization
   //include "/etc/bind/zones.rfc1918";
   
   zone "kelompok7.fpas"{
              type master;
              file "/etc/bind/vm/kelompok7.fpas";
   };
   
   zone "43.168.192.in-addr.arpa"{
              type master;
              file "/etc/bind/vm/43.168.192.in-addr.arpa";
   };
   ```

   - Creating named.conf.options in roles/wp/templates and adding script configuration
     - nano roles/wp/templates/named.conf.options

   ```markdown
   options {
           directory "/var/cache/bind";
   
           // If there is a firewall between you and nameservers you want
           // to talk to, you may need to fix the firewall to allow multiple
           // ports to talk.  See http://www.kb.cert.org/vuls/id/800113
   
           // If your ISP provided one or more IP addresses for stable
           // nameservers, you probably want to use them as forwarders.
           // Uncomment the following block, and insert the addresses replacing
           // the all-0's placeholder.
   
           forwarders {
              8.8.8.8;
           };
   
           //========================================================================
           // If BIND logs error messages about the root key being expired,
           // you will need to update your keys.  See https://www.isc.org/bind-keys
           //========================================================================
           //dnssec-validation auto;
           allow-query{any;};
           listen-on-v6 { any; };
   };
   ```

   - Creating resolv.conf in roles/wp/templates and adding script configuration
     - nano roles/wp/templates/resolv.conf

   ```markdown
   nameserver 192.168.43.185
   ```

   - Running command

   ```markdown
   ansible-playbook -i hosts install-wp.yml -k
   ```

   

6. Creating install-laravel.yml file and adding configuration

   ```markdown
   ---
   - hosts: laravel
     vars:
       username: 'admin'
       password: 'admin'
       domain: 'laravel.dev' #laravel_1.dev laravel_2.dev laravel_3.dev
     roles:
       - php
       - lv
   ```

   - Creating directory roles/lv, and creating tasks, handlers, templates in db directory

   ```markdown
   mkdir -p roles/lv
   mkdir -p roles/lv/handlers 
   mkdir -p roles/lv/tasks
   mkdir -p roles/lv/templates
   ```

   - Creating main.yml, env.template, lv.conf, www.conf adding script configuration
     - nano roles/lv/handlers/main.yml

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

   - Creating main.yml adding script configuration
     - nano roles/lv/tasks/main.yml

   ```markdown
   ---
   - name: delete apt chache
     become: yes
     become_user: root
     become_method: su
     command: rm -vf /var/lib/apt/lists/*
   
   - name: Download and install Composer
     shell: curl -sS https://getcomposer.org/installer | php
     args:
       chdir: /usr/src/
       creates: /usr/local/bin/composer
       warn: false
     become: yes
   
   - name: Add Composer to global path
     copy:
       dest: /usr/local/bin/composer
       group: root
       mode: '0755'
       owner: root
       src: /usr/src/composer.phar
       remote_src: yes
     become: yes
   
   - name: Ansible delete file create-project
     file:
       path: /var/www/html/landing
       state: absent
   
   - name: composer create-project
     shell: /usr/local/bin/composer create-project laravel/laravel /var/www/html/landing --prefer-dist --no-interaction
   
   - name: Copy .env.template
     template:
       src=templates/env.template
       dest=/var/www/html/landing/.env
   
   - name: composer
     shell: cd /var/www/html/landing; /usr/local/bin/composer install  --no-interaction
   
   - name: key
     shell: /usr/bin/php7.4 /var/www/html/landing/artisan key:generate
   
   - name: chmod
     become: yes
     become_user: root
     become_method: su
     command: chmod 777 -R /var/www/html/landing/storage
   
   - name : www.conf
     template:
       src=templates/www.conf
       dest=/etc/php/7.4/fpm/pool.d/www.conf
   
   - name: Copy lv.conf
     template:
       src=templates/lv.conf
       dest=/etc/nginx/sites-available/{{ domain }}
     vars:
       servername: '{{ domain }}'
   
   - name: Symlink lv.conf
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

   - nano roles/lv/templates/env.template

   ```markdown
   APP_NAME=Laravel
   APP_ENV=local
   APP_KEY=
   APP_DEBUG=true
   APP_URL=http://vm.local
   
   LOG_CHANNEL=stack
   LOG_DEPRECATIONS_CHANNEL=null
   LOG_LEVEL=debug
   
   DB_CONNECTION=mysql
   DB_HOST=10.0.3.200
   DB_PORT=3306
   DB_DATABASE=landing
   DB_USERNAME=admin
   DB_PASSWORD=admin
   
   BROADCAST_DRIVER=log
   CACHE_DRIVER=file
   FILESYSTEM_DRIVER=local
   QUEUE_CONNECTION=sync
   SESSION_DRIVER=file
   SESSION_LIFETIME=120
   
   MEMCACHED_HOST=127.0.0.1
   
   REDIS_HOST=127.0.0.1
   REDIS_PASSWORD=null
   REDIS_PORT=6379
   
   MAIL_MAILER=smtp
   MAIL_HOST=mailhog
   MAIL_PORT=1025
   MAIL_USERNAME=null
   MAIL_PASSWORD=null
   MAIL_ENCRYPTION=null
   MAIL_FROM_ADDRESS=null
   MAIL_FROM_NAME="${APP_NAME}"
   
   AWS_ACCESS_KEY_ID=
   AWS_SECRET_ACCESS_KEY=
   AWS_DEFAULT_REGION=us-east-1
   AWS_BUCKET=
   AWS_USE_PATH_STYLE_ENDPOINT=false
   
   PUSHER_APP_ID=
   PUSHER_APP_KEY=
   PUSHER_APP_SECRET=
   PUSHER_APP_CLUSTER=mt1
   
   MIX_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
   MIX_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"
   ```

   - nano roles/lv/templates/lv.conf

   ```markdown
   server {
        listen 80;
        listen [::]:80;
   
        # Log files for Debugging
        access_log /var/log/nginx/vhostlaravel-access.log;
        error_log /var/log/nginx/vhostlaravel-error.log;
   
        # Webroot Directory for Laravel project
        root /var/www/html/landing/public;
        index index.php index.html index.htm;
   
        # Your Domain Name
        server_name lxc_landing.dev;
   
        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }
   
        # PHP-FPM Configuration Nginx
        location ~ \.php$ {
                try_files $uri =404;
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                fastcgi_pass 127.0.0.1:9001;
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
        }
   }
   ```

   - nano roles/lv/templates/www.conf

   ```markdown
   ; Start a new pool named 'www'.
   ; the variable $pool can be used in any directive and will be replaced by the
   ; pool name ('www' here)
   [www]
   
   ; Per pool prefix
   ; It only applies on the following directives:
   ; - 'access.log'
   ; - 'slowlog'
   ; - 'listen' (unixsocket)
   ; - 'chroot'
   ; - 'chdir'
   ; - 'php_values'
   ; - 'php_admin_values'
   ; When not set, the global prefix (or /usr) applies instead.
   ; Note: This directive can also be relative to the global prefix.
   ; Default Value: none
   prefix = /var/www
   
   ; Unix user/group of processes
   ; Note: The user is mandatory. If the group is not set, the default user's group
   ;       will be used.
   user = www-data
   group = www-data
   
   ; The address on which to accept FastCGI requests.
   ; Valid syntaxes are:
   ;   'ip.add.re.ss:port'    - to listen on a TCP socket to a specific IPv4 address on
   ;                            a specific port;
   ;   '[ip:6:addr:ess]:port' - to listen on a TCP socket to a specific IPv6 address on
   ;                            a specific port;
   ;   'port'                 - to listen on a TCP socket to all addresses
   ;                            (IPv6 and IPv4-mapped) on a specific port;
   ;   '/path/to/unix/socket' - to listen on a unix socket.
   ; Note: This value is mandatory.
   listen = 127.0.0.1:9001
   
   ; Set listen(2) backlog.
   ; Default Value: 511 (-1 on FreeBSD and OpenBSD)
   ;listen.backlog = 511
   
   ; Set permissions for unix socket, if one is used. In Linux, read/write
   ; permissions must be set in order to allow connections from a web server. Many
   ; BSD-derived systems allow connections regardless of permissions.
   ; Default Values: user and group are set as the running user
   ;                 mode is set to 0660
   listen.owner = www-data
   listen.group = www-data
   ;listen.mode = 0660
   ; When POSIX Access Control Lists are supported you can set them using
   ; these options, value is a comma separated list of user/group names.
   ; When set, listen.owner and listen.group are ignored
   ;listen.acl_users =
   ;listen.acl_groups =
   
   ; List of addresses (IPv4/IPv6) of FastCGI clients which are allowed to connect.
   ; Equivalent to the FCGI_WEB_SERVER_ADDRS environment variable in the original
   ; PHP FCGI (5.2.2+). Makes sense only with a tcp listening socket. Each address
   ; must be separated by a comma. If this value is left blank, connections will be
   ; accepted from any ip address.
   ; Default Value: any
   listen.allowed_clients = 127.0.0.1
   
   ; Specify the nice(2) priority to apply to the pool processes (only if set)
   ; The value can vary from -19 (highest priority) to 20 (lower priority)
   ; Note: - It will only work if the FPM master process is launched as root
   ;       - The pool processes will inherit the master process priority
   ;         unless it specified otherwise
   ; Default Value: no set
   ; process.priority = -19
   
   ; Choose how the process manager will control the number of child processes.
   ; Possible Values:
   ;   static  - a fixed number (pm.max_children) of child processes;
   ;   dynamic - the number of child processes are set dynamically based on the
   ;             following directives. With this process management, there will be
   ;             always at least 1 children.
   ;             pm.max_children      - the maximum number of children that can
   ;                                    be alive at the same time.
   ;             pm.start_servers     - the number of children created on startup.
   ;             pm.min_spare_servers - the minimum number of children in 'idle'
   ;                                    state (waiting to process). If the number
   ;                                    of 'idle' processes is less than this
   ;                                    number then some children will be created.
   ;             pm.max_spare_servers - the maximum number of children in 'idle'
   ;                                    state (waiting to process). If the number
   ;                                    of 'idle' processes is greater than this
   ;                                    number then some children will be killed.
   ;  ondemand - no children are created at startup. Children will be forked when
   ;             new requests will connect. The following parameter are used:
   ;             pm.max_children           - the maximum number of children that
   ;                                         can be alive at the same time.
   ;             pm.process_idle_timeout   - The number of seconds after which
   ;                                         an idle process will be killed.
   ; Note: This value is mandatory.
   pm = dynamic
   
   ; The number of child processes to be created when pm is set to 'static' and the
   ; maximum number of child processes when pm is set to 'dynamic' or 'ondemand'.
   ; This value sets the limit on the number of simultaneous requests that will be
   ; served. Equivalent to the ApacheMaxClients directive with mpm_prefork.
   ; Equivalent to the PHP_FCGI_CHILDREN environment variable in the original PHP
   ; CGI. The below defaults are based on a server without much resources. Don't
   ; forget to tweak pm.* to fit your needs.
   ; Note: Used when pm is set to 'static', 'dynamic' or 'ondemand'
   ; Note: This value is mandatory.
   pm.max_children = 20
   
   ; The number of child processes created on startup.
   ; Note: Used only when pm is set to 'dynamic'
   ; Default Value: min_spare_servers + (max_spare_servers - min_spare_servers) / 2
   pm.start_servers = 4
   
   ; The desired minimum number of idle server processes.
   ; Note: Used only when pm is set to 'dynamic'
   ; Note: Mandatory when pm is set to 'dynamic'
   pm.min_spare_servers = 3
   
   ; The desired maximum number of idle server processes.
   ; Note: Used only when pm is set to 'dynamic'
   ; Note: Mandatory when pm is set to 'dynamic'
   pm.max_spare_servers = 6
   
   ; The number of seconds after which an idle process will be killed.
   ; Note: Used only when pm is set to 'ondemand'
   ; Default Value: 10s
   pm.process_idle_timeout = 6s;
   
   ; The number of requests each child process should execute before respawning.
   ; This can be useful to work around memory leaks in 3rd party libraries. For
   ; endless request processing specify '0'. Equivalent to PHP_FCGI_MAX_REQUESTS.
   ; Default Value: 0
   pm.max_requests = 1000
   
   ; The URI to view the FPM status page. If this value is not set, no URI will be
   ; recognized as a status page. It shows the following informations:
   ;   pool                 - the name of the pool;
   ;   process manager      - static, dynamic or ondemand;
   ;   start time           - the date and time FPM has started;
   ;   start since          - number of seconds since FPM has started;
   ;   accepted conn        - the number of request accepted by the pool;
   ;   listen queue         - the number of request in the queue of pending
   ;                          connections (see backlog in listen(2));
   ;   max listen queue     - the maximum number of requests in the queue
   ;                          of pending connections since FPM has started;
   ;   listen queue len     - the size of the socket queue of pending connections;
   ;   idle processes       - the number of idle processes;
   ;   active processes     - the number of active processes;
   ;   total processes      - the number of idle + active processes;
   ;   max active processes - the maximum number of active processes since FPM
   ;                          has started;
   ;   max children reached - number of times, the process limit has been reached,
   ;                          when pm tries to start more children (works only for
   ;                          pm 'dynamic' and 'ondemand');
   ; Value are updated in real time.
   ; Example output:
   ;   pool:                 www
   ;   process manager:      static
   ;   start time:           01/Jul/2011:17:53:49 +0200
   ;   start since:          62636
   ;   accepted conn:        190460
   ;   listen queue:         0
   ;   max listen queue:     1
   ;   listen queue len:     42
   ;   idle processes:       4
   ;   active processes:     11
   ;   total processes:      15
   ;   max active processes: 12
   ;   max children reached: 0
   ;
   ; By default the status page output is formatted as text/plain. Passing either
   ; 'html', 'xml' or 'json' in the query string will return the corresponding
   ; output syntax. Example:
   ;   http://www.foo.bar/status
   ;   http://www.foo.bar/status?json
   ;   http://www.foo.bar/status?html
   ;   http://www.foo.bar/status?xml
   ;
   ; By default the status page only outputs short status. Passing 'full' in the
   ; query string will also return status for each pool process.
   ; Example:
   ;   http://www.foo.bar/status?full
   ;   http://www.foo.bar/status?json&full
   ;   http://www.foo.bar/status?html&full
   ;   http://www.foo.bar/status?xml&full
   ; The Full status returns for each process:
   ;   pid                  - the PID of the process;
   ;   state                - the state of the process (Idle, Running, ...);
   ;   start time           - the date and time the process has started;
   ;   start since          - the number of seconds since the process has started;
   ;   requests             - the number of requests the process has served;
   ;   request duration     - the duration in ms of the requests;
   ;   request method       - the request method (GET, POST, ...);
   ;   request URI          - the request URI with the query string;
   ;   content length       - the content length of the request (only with POST);
   ;   user                 - the user (PHP_AUTH_USER) (or '-' if not set);
   ;   script               - the main script called (or '-' if not set);
   ;   last request cpu     - the %cpu the last request consumed
   ;                          it's always 0 if the process is not in Idle state
   ;                          because CPU calculation is done when the request
   ;                          processing has terminated;
   ;   last request memory  - the max amount of memory the last request consumed
   ;                          it's always 0 if the process is not in Idle state
   ;                          because memory calculation is done when the request
   ;                          processing has terminated;
   ; If the process is in Idle state, then informations are related to the
   ; last request the process has served. Otherwise informations are related to
   ; the current request being served.
   ; Example output:
   ;   ************************
   ;   pid:                  31330
   ;   state:                Running
   ;   start time:           01/Jul/2011:17:53:49 +0200
   ;   start since:          63087
   ;   requests:             12808
   ;   request duration:     1250261
   ;   request method:       GET
   ;   request URI:          /test_mem.php?N=10000
   ;   content length:       0
   ;   user:                 -
   ;   script:               /home/fat/web/docs/php/test_mem.php
   ;   last request cpu:     0.00
   ;   last request memory:  0
   ;
   ; Note: There is a real-time FPM status monitoring sample web page available
   ;       It's available in: /usr/share/php/7.0/fpm/status.html
   ;
   ; Note: The value must start with a leading slash (/). The value can be
   ;       anything, but it may not be a good idea to use the .php extension or it
   ;       may conflict with a real PHP file.
   ; Default Value: not set
   ;pm.status_path = /status
   
   ; The ping URI to call the monitoring page of FPM. If this value is not set, no
   ; URI will be recognized as a ping page. This could be used to test from outside
   ; that FPM is alive and responding, or to
   ; - create a graph of FPM availability (rrd or such);
   ; - remove a server from a group if it is not responding (load balancing);
   ; - trigger alerts for the operating team (24/7).
   ; Note: The value must start with a leading slash (/). The value can be
   ;       anything, but it may not be a good idea to use the .php extension or it
   ;       may conflict with a real PHP file.
   ; Default Value: not set
   ;ping.path = /ping
   
   ; This directive may be used to customize the response of a ping request. The
   ; response is formatted as text/plain with a 200 response code.
   ; Default Value: pong
   ;ping.response = pong
   
   ; The access log file
   ; Default: not set
   ;access.log = log/$pool.access.log
   
   ; The log file for slow requests
   ; Default Value: not set
   ; Note: slowlog is mandatory if request_slowlog_timeout is set
   ;slowlog = log/$pool.log.slow
   
   ; The timeout for serving a single request after which a PHP backtrace will be
   ; dumped to the 'slowlog' file. A value of '0s' means 'off'.
   ; Available units: s(econds)(default), m(inutes), h(ours), or d(ays)
   ; Default Value: 0
   ;request_slowlog_timeout = 0
   
   ; The timeout for serving a single request after which the worker process will
   ; be killed. This option should be used when the 'max_execution_time' ini option
   ; does not stop script execution for some reason. A value of '0' means 'off'.
   ; Available units: s(econds)(default), m(inutes), h(ours), or d(ays)
   ; Default Value: 0
   ;request_terminate_timeout = 0
   
   ; Set open file descriptor rlimit.
   ; Default Value: system defined value
   ;rlimit_files = 1024
   
   ; Set max core size rlimit.
   ; Possible Values: 'unlimited' or an integer greater or equal to 0
   ; Default Value: system defined value
   ;rlimit_core = 0
   
   ; Chroot to this directory at the start. This value must be defined as an
   ; absolute path. When this value is not set, chroot is not used.
   ; Note: you can prefix with '$prefix' to chroot to the pool prefix or one
   ; of its subdirectories. If the pool prefix is not set, the global prefix
   ; will be used instead.
   ; Note: chrooting is a great security feature and should be used whenever
   ;       possible. However, all PHP paths will be relative to the chroot
   ;       (error_log, sessions.save_path, ...).
   ; Default Value: not set
   ;chroot =
   
   ; Chdir to this directory at the start.
   ; Note: relative path can be used.
   ; Default Value: current directory or / when chroot
   ;chdir = /var/www
   
   ; Redirect worker stdout and stderr into main error log. If not set, stdout and
   ; stderr will be redirected to /dev/null according to FastCGI specs.
   ; Note: on highloaded environement, this can cause some delay in the page
   ; process time (several ms).
   ; Default Value: no
   ;catch_workers_output = yes
   
   ; Clear environment in FPM workers
   ; Prevents arbitrary environment variables from reaching FPM worker processes
   ; by clearing the environment in workers before env vars specified in this
   ; pool configuration are added.
   ; Setting to "no" will make all environment variables available to PHP code
   ; via getenv(), $_ENV and $_SERVER.
   ; Default Value: yes
   ;clear_env = no
   
   ; Limits the extensions of the main script FPM will allow to parse. This can
   ; prevent configuration mistakes on the web server side. You should only limit
   ; FPM to .php extensions to prevent malicious users to use other extensions to
   ; exectute php code.
   ; Note: set an empty value to allow all extensions.
   ; Default Value: .php
   ;security.limit_extensions = .php .php3 .php4 .php5 .php7
   
   ; Pass environment variables like LD_LIBRARY_PATH. All $VARIABLEs are taken from
   ; the current environment.
   ; Default Value: clean env
   ;env[HOSTNAME] = $HOSTNAME
   ;env[PATH] = /usr/local/bin:/usr/bin:/bin
   ;env[TMP] = /tmp
   ;env[TMPDIR] = /tmp
   ;env[TEMP] = /tmp
   
   ; Additional php.ini defines, specific to this pool of workers. These settings
   ; overwrite the values previously defined in the php.ini. The directives are the
   ; same as the PHP SAPI:
   ;   php_value/php_flag             - you can set classic ini defines which can
   ;                                    be overwritten from PHP call 'ini_set'.
   ;   php_admin_value/php_admin_flag - these directives won't be overwritten by
   ;                                     PHP call 'ini_set'
   ; For php_*flag, valid values are on, off, 1, 0, true, false, yes or no.
   
   ; Defining 'extension' will load the corresponding shared extension from
   ; extension_dir. Defining 'disable_functions' or 'disable_classes' will not
   ; overwrite previously defined php.ini values, but will append the new value
   ; instead.
   
   ; Note: path INI options can be relative and will be expanded with the prefix
   ; (pool, global or /usr)
   
   ; Default Value: nothing is defined by default except the values in php.ini and
   ;                specified at startup with the -d argument
   ;php_admin_value[sendmail_path] = /usr/sbin/sendmail -t -i -f www@my.domain.com
   ;php_flag[display_errors] = off
   ;php_admin_value[error_log] = /var/log/fpm-php.www.log
   ;php_admin_flag[log_errors] = on
   ;php_admin_value[memory_limit] = 32M
   ```

   - Running command

   ```markdown
   ansible-playbook -i hosts install-laravel.yml -k
   ```

7. Creating install-yii.yml file and adding configuration

   ```markdown
   ---
   - hosts: yii
     vars:
       username: 'admin'
       password: 'admin'
       domain: 'yii.dev' #yii_1.dev yii_2.dev yii_3.dev yii_4.dev
     roles:
       - yii
   ```

   - Creating directory roles/yii, and creating tasks, handlers, templates in db directory

   ```markdown
   mkdir -p roles/yii
   mkdir -p roles/yii/handlers 
   mkdir -p roles/yii/tasks
   mkdir -p roles/yii/templates
   ```

   - Creating main.yml in roles/yii/handlers and adding script configuration
     - nano roles/yii/handlers/main.yml

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

   - Creating main.yml in roles/yii/tasks and adding script configuration
     - nano roles/yii/tasks/main.yml

   ```markdown
   ---
   - name: delete apt chache
     become: yes
     become_user: root
     become_method: su
     command: rm -vf /var/lib/apt/lists/*
   
   - name: Download and install Composer
     shell: curl -sS https://getcomposer.org/installer | php
     args:
       chdir: /usr/src/
       creates: /usr/local/bin/composer
       warn: false
     become: yes
   
   - name: Add Composer to global path
     copy:
       dest: /usr/local/bin/composer
       group: root
       mode: '0755'
       owner: root
       src: /usr/src/composer.phar
       remote_src: yes
     become: yes
   
   - name: Ansible delete file create-project
     file:
       path: /var/www/html/basic
       state: absent
   
   - name: composer create-project
     shell: /usr/local/bin/composer create-project --prefer-dist yiisoft/yii2-app-basic /var/www/html/basic --prefer-dist --no-interaction
   
   - name: chmod
     become: yes
     become_user: root
     become_method: su
     command: chmod +x /usr/local/bin/composer
   
   - name: composer
     shell: cd /var/www/html/basic; /usr/local/bin/composer install  --no-interaction
   
   - name: Copy .env.template
     template:
       src=templates/env.template
       dest=/var/www/html/basic/config/db.php
   
   - name: Copy yii.conf
     template:
       src=templates/yii.conf
       dest=/etc/nginx/sites-available/{{ domain }}
     vars:
       servername: '{{ domain }}'
   
   - name: Symlink yii.conf
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

   - Creating env.template in roles/yii/templates and adding script configuration
     - nano roles/yii/templates/env.template

   ```markdown
   <?php
   
   return [
       'class' => 'yii\db\Connection',
       'dsn' => 'mysql:host=10.0.3.200:3306;dbname=yii',
       'username' => 'admin',
       'password' => '12345',
       'charset' => 'utf8',
   ];
   ```

   - Creating yii.conf in roles/yii/templates/yii.conf and adding script configuration
     - nano roles/yii/templates/yii.conf/yii.conf

   ```markdown
   server {
        listen 80;
        listen [::]:80;
   
        # Log files for Debugging
        access_log /var/log/nginx/yii-access.log;
        error_log /var/log/nginx/yii-error.log;
   
        # Webroot Directory for Laravel project
        root /var/www/html/basic/web;
        index index.php index.html index.htm;
   
        # Your Domain Name
        server_name {{ domain }};
   
        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }
   
        # PHP-FPM Configuration Nginx
        location ~ \.php$ {
                try_files $uri =404;
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
                fastcgi_pass 127.0.0.1:9001;
                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
        }
   }
   ```

   - Running command

   ```markdown
   ansible-playbook -i hosts install-yii.yml -k
   ```



### ***Hasil Screenshoot***

**Laravel** (kelompok7.fpas/)

![Laravel SS1](https://user-images.githubusercontent.com/93067781/151578224-ca92d3a2-544d-40cf-9369-5b5e381b8753.jpg)



**Wordpress** (news.kelompok7.fpas/)

![wordpress1](https://user-images.githubusercontent.com/93067781/151665267-7a89a147-49e6-4301-8f3f-dd0024ea3a1a.jpg)



**Codelgniter** (kelompok7.fpas/app)

![Codelnigter SS1](https://user-images.githubusercontent.com/93067781/151578275-839a2a4c-db49-41e6-a946-2a7c1ce28fca.jpg)

**YII** (kelompok7.fpas/product)

![YII SS1](https://user-images.githubusercontent.com/93067781/151578288-0856a03e-c9fa-4fd7-b3cb-f2746d75bbe4.jpg)
