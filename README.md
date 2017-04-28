1- Edit /etc/hosts put IP,  FQDN name and NetBIOS name.

    /etc/hosts

    192.168.0.6.128  samba4.lss.local samba4

3- Install dependencies

    yum install perl gcc libacl-devel libblkid-devel gnutls-devel readline-devel python-devel gdb pkgconfig krb5-workstation zlib-devel setroubleshoot-server libaio-devel setroubleshoot-plugins\
    policycoreutils-python libsemanage-python setools-libs-python setools-libs popt-devel libpcap-devel sqlite-devel libidn-devel libxml2-devel libacl-devel libsepol-devel libattr-devel keyutils-libs-devel\
    cyrus-sasl-devel cups-devel bind-utils libxslt docbook-style-xsl openldap-devel pam-devel bzip2 vim nano wget -y

4-  Download the last version from samba site.

    cd  /tmp/

    wget https://download.samba.org/pub/samba/stable/samba-4.6.0.tar.gz

5- Install, this command have a delay between 15 to 30 minutes, depending on the configuration of your server.

    tar -zxvf samba-4.6.0.tar.gz
    cd samba-4.6.0
    ./configure –enable-debug –enable-selftest –with-ads –with-systemd –with-winbind
    make && make install

6- Edit  /etc/krb5.conf,  comment the line below.

    #includedir /etc/krb5.conf.d/

7- Run the configurator

    cd /usr/local/samba/bin

    samba-tool domain provision –use-rfc2307 –interactive

8 –  Open the ports for correct operation.

    firewall-cmd –add-port=53/tcp –permanent;firewall-cmd –add-port=53/udp –permanent;firewall-cmd –add-port=88/tcp –permanent;firewall-cmd –add-port=88/udp –permanent; \
    firewall-cmd –add-port=135/tcp –permanent;firewall-cmd –add-port=137-138/udp –permanent;firewall-cmd –add-port=139/tcp –permanent; \
    firewall-cmd –add-port=389/tcp –permanent;firewall-cmd –add-port=389/udp –permanent;firewall-cmd –add-port=445/tcp –permanent; \
    firewall-cmd –add-port=464/tcp –permanent;firewall-cmd –add-port=464/udp –permanent;firewall-cmd –add-port=636/tcp –permanent; \
    firewall-cmd –add-port=1024-5000/tcp –permanent;firewall-cmd –add-port=3268-3269/tcp –permanent

    firewall-cmd –reload

9- Create a service for initialization together the system.

    nano /etc/systemd/system/samba.service

    [Unit]
    Description= Samba 4 Active Directory
    After=syslog.target
    After=network.target

    [Service]
    Type=forking
    PIDFile=/usr/local/samba/var/run/samba.pid
    ExecStart=/usr/local/samba/sbin/samba

    [Install]
    WantedBy=multi-user.target

10- Enable and start samba service

    systemctl enable samba

    systemctl start samba

Ready!!!
