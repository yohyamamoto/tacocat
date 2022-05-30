#This is my note about setting up a CentOS stream home linux server for calculations. 

Installing extra repository:
dnf -y install epel-release

avahi timeout problem
sudo firewall-cmd --zone=public --permanent --add-service=mdns
or install nss-mdns from epel repo


Note about web server on CentOS
https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-centos-8


Anaconda installed mainly for jupyterhub
https://canthonyscott.com/deploying-jupyterhub-on-a-mulit-user-centos-server/
https://github.com/jupyterhub/jupyterhub
opened port 8000
 1702  sudo firewall-cmd --zone=public --add-port=8000/tcp --permanent
 1703  sudo firewall-cmd --reload

##copied jupyter.service file in .config/systemd/user/jupyterhub.service then enable with
##systemctl --user enable jupyterhub.service
Moved anaconda installation to /opt/anaconda3
systemd is in /etc/systemd/system/jupyterhub.service
config is in /root/


Time synchronization
timedatectl set-ntp true

Turn on ethernet connection at boot. Edit the file,
sudo vim /etc/sysconfig/network-scripts/ifcfg-eno1

running mpi when the NUC is offline. Edit /etc/openmpi-x86_64/openmpi-mca-params.conf
oob_tcp_if_include = lo


pyscf installation
sudo /opt/anaconda3/bin/conda install -c pyqc pyscf
but you need python2?
Maybe use src code: https://pyscf.org/install.html



## timeout if the TCP port is hit too many times in a given amount of time.
## https://superuser.com/questions/476231/ban-an-ip-after-multiple-failed-ssh-login-attempts
sudo iptables -A INPUT  -p tcp -m tcp --dport 22 -m state \
    --state NEW -m recent                                   \
    --update --seconds 60 --hitcount 4 --name DEFAULT --rsource -j DROP
