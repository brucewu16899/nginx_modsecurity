
# Nginx from EPEL 7 with modsecurity


## For the lazy ones

Download the 64 bits RPM from this repository [nginx-1.6.3-8.el7.centos_modsec_2.9.1.x86_64.rpm]([https://github.com/markri/nginx_modsecurity/blob/master/nginx-1.6.3-8.el7.centos_modsec_2.9.1.x86_64.rpm])
and install with

    rpm -ivh nginx-1.6.3-8.el7.centos_modsec_2.9.1.x86_64.rpm



## DIY


How trustworthy is a repository from someone you don't know? This is why I'll explain the steps of creating an RPM for your self. 
So you'll know for sure that everything is clean :-)


### Download sources

How this repository is created (in Gnome you may find using file-roller a bit easier instead of using rpm2cpio):

    wget https://dl.fedoraproject.org/pub/epel/7/SRPMS/n/nginx-1.6.3-8.el7.src.rpm
    mkdir nginx-1.6.3
    rpm2cpio ./nginx-1.6.3-8.el7.src.rpm | cpio -idmv
    cd nginx-1.6.3
    wget https://www.modsecurity.org/tarball/2.9.1/modsecurity-2.9.1.tar.gz


### Patch
    
From this point I added a default mod_security.conf, and changed the nginx.spec file. Check commit log for the actual changes
[history of nginx.spec](https://github.com/markri/nginx_modsecurity/commits/master/nginx.spec)
and see if the diffs are pleasing enough


### Build

In your extracted nginx-1.6.3 folder execute following

    rpmdev-setuptree
    cp * ~/rpmbuild/SOURCES/
    rm ~/rpmbuild/SOURCES/nginx.spec
    cp nginx.spec ~/rpmbuild/SPECS
    rpmbuild -ba SPECS/nginx.spec 
    
For CentOS 7 dependencies (other OS-es unknown) you will need: 

    yum install geoip-devel gd-devel gperftools-devel perl-devel perl curl-devel lua-devel perl-ExtUtils-Embed
    

### Install

For a 64-bit environment run (other arch will lead to other RPM name)
    
    rpm -ivh nginx-1.6.3-8.el7.centos_modsec_2.9.1.x86_64.rpm