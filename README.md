# Securing Java Web Apps

* __Realtime hackings of the world__ 
    * Nordscope [map](http://map.norsecorp.com/#/)
    * They do this by placing honey pods all over the world
* __Vulnerability Types__
    * Enumeration Vulnerability 
* __[hackmageddon](https://www.hackmageddon.com/)__
    * Information Security Timelines and Statistics
* Tools
    * John the ripper password cracker [link](https://www.openwall.com/john/)
    * BetterCAP - Man in the middle MITM attack tool [link](https://www.bettercap.org/legacy/)
    * OpenSSL - Tool for TLS and SSL for certificate creating and stuff [link](https://www.openssl.org/)
* Using Open SSL
    * Create a certificate authority
        * Create a root key for CA
            * openssl genrsa -des3 -out ca.key 2048
            * make sure to give a passphrase
        * Create a root certificate for CA
            * openssl req -x509 -new -nodes -key ca.key \ -sha256 -days 1825 -out ca.pem
            * You'll be asked for the passphrase
    * Create an application key
        * Lets generate the application key using the key tool
        * keytool -genkey -keyalg RSA -keysize 2048 \ -alias terracotta \ -keystore terracotta.jks
        * Give the key store a password and remember the alias
    * Sign that key with the certificate authority
        * Use key tool and sign the key with the CA
            * keytool -certreq -file terracotta.csr \ -alias terracotta \ -keystore terracotta.jks
        * Signing the certificate is a 2 step process. We need to create a certificate signing request CSR
        * Then take that CSR back to the CA to get the certificate
            * openssl x509 -req -in terracotta.csr \ -CA ca.pem -CAkey ca.key -CAcreateserial \ -out terracotta.crt -days 1825 \ -sha256 -extfile terracotta.ext
    * Import the certificate
        * The app certificate needs to go to the key store
        * CA certificate needs to go to the key store and the browser
        * First import the CA certificate
            * keytool -import -file terracotta.crt \ -alias terracotta \ -keystore terracotta.jks
        * Move the keystore to the home directory
            * mv terracotta.jks ~/
        * Mofify the server.xml file of tomcat
            * Uncomment the `<Connector port="8443" ...`
            * Give the 
                * Location of the key store
                * Keystore password
                * alias
        * Import the CA certificate to the browser
  
* ~~The vulnerable application used for demos~~
    * ~~[terracotta-bank](https://github.com/jzheaux/terracotta-bank)~~
* To check passoword strength you can use
    * How secure is my password [link](https://howsecureismypassword.net/)
* The vulnerable application used for demos
    * [terracotta-bank-spring](https://github.com/jzheaux/terracotta-bank-spring)
    * Cloned to my repo [terracotta-bank-spring](https://github.com/kumudug/terracotta-bank-spring)
* OWASP __ASVS__
    * OWASP Application Security Verification Standard
    * Can be used to verify your applications security



