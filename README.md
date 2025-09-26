# shibboleth-sp
Description and example files for deploying a Shibboleth Service Provider via Apache2 on SuSE Linux Enterprise Server 15.7  

## Prerequisites
Small Server with 2 cores and 4 GB RAM at least  
* SuSE Linux Enterprise Server > 15.5 
* Apache 2.4
* make-Tools
* wget or curl
* Certificates for your Webserver (e. g. Let's Encrypt certificates or DFN certificates)  

## Installation

### Shibboleth Service Provider (SP)

1. Install shibboleth-sp and associated packages: `sudo zypper install shibboleth-sp`
2. Start Shibboleth Daemon `sudo systemctl start shibd`
3. Enable Shibboleth Daemon for starting if server starts `sudo systemctl enable shibd`

### Shibboleth Embedded Discovery Service (EDS)

1. Download Shibboleth EDS: `wget https://shibboleth.net/downloads/embedded-discovery-service/latest/shibboleth-embedded-ds-1.3.0.tar.gz`
2. Extract Shibboleth EDS: `tar -xzf shibboleth-embedded-ds-1.3.0.tar.gz`
3. Change into EDS dircetory: `cd shibboleth-embedded-ds-1.3.0`
4. Install EDS globally: `sudo make install`
   
## Configure Apache2 modules

1. Check if Apache2 is running and enabled: `sudo systemctl status apache2`
2. If not, start and enable Apache2: `sudo systemctl start apache2; sudo systemctl enable apache2`
3. Check for Apache2 modules installed: `sudo apache2ctl -M`
4. If module `mpm_prefork` is used, change to `mpm_worker`or mpm_event` : `a2dismod prefork ; a2enmod worker` 
5. Install missing modules:
```
# Mandatory
sudo a2enmod shibd
sudo a2enmod alias
sudo a2enmod version
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod rewrite
sudo a2enmod ssl

# Recommended
sudo a2enmod http2
sudo a2enmod proxy_http2
```
6. Restart Apache2: `sudo systemctl restart apache2`

## Configure certificate access

1. Download DFN-AAI certificate: `wget https://www.aai.dfn.de/metadata/dfn-aai.pem`
2. Move certificate to appropriate location: `sudo mkdir /etc/ssl/aai; mv ./dfn-aai.pem /etc/ssl/aai/`
3. Make server certificate key readable for user shibd 

## Configure Shibboleth SP and Shibboleth EDS




