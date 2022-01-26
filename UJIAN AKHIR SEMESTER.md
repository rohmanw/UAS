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



 2. Creating LXC_DB_SERVER and install mariadb

    - Creating lxc container, start and entering container

    ```markdown
    lxc-create -n LXC_DB_SERVER -t download -- --dist debian --release buster --arch amd64 --force-cache --no-validate --server images.linuxcontainers.org
    lxc-start -n LXC_DB_SERVER
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
    
    
    
    - Setting password for LXC_DB_SERVER SSH
    
    ```markdown
    passwd (ex: 1)
    ```
    
    
    
    - Log out from LXC_DB_SERVER
    
    ```markdown
    exit
    ```
    
    
    
    - Enrolling LXC_DB_SERVER domain and ip to Ubuntu Server Host (/etc/hosts)
    
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
    LXC_DB_SERVER ansible_host=lxc_mariadb.dev ansible_ssh_user=root ansible_become_pass=1
    
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
      - nano roles/db/tasksmain.yml
    
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
    
    
    
    - Checking, if mariadb has installed in LXC_DB_SERVER
    
    ```markdown
    ssh root@LXC_DB_SERVER.dev
    mysql -u admin -p
    show databases; 
    ```
    
    
    
    
    
    - 
    
    ```markdown
    
    ```

