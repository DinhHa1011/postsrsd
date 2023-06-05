### Installation
- package
```
sudo apt install -y cmake unzip curl
```
- Temporary installation directory
```
mkdir -p /tmp/srs
cd /tmp/srs
```
- Download
```
curl -L -o master.zip https://github.com/roehling/postsrsd/archive/master.zip
#or
#wget https://github.com/roehling/postsrsd/archive/master.zip
```
- unzip 
```
unzip master.zip
```
- install
```
cd postsrsd-main
#Optionally cmake you config (by default installed into /usr/lib)
make
make install
```
### Configuration
#### SRS
```
vim /etc/default/postsrsd
```
```
# Default settings for postsrsd

# Local domain name.
# Addresses are rewritten to originate from this domain. The default value
# is taken from `postconf -h mydomain` and probably okay.
#
#SRS_DOMAIN=example.com

# Exclude additional domains.
# You may list domains which shall not be subjected to address rewriting.
# If a domain name starts with a dot, it matches all subdomains, but not
# the domain itself. Separate multiple domains by space or comma.
#
#SRS_EXCLUDE_DOMAINS=.example.com,example.org

# First separator character after SRS0 or SRS1.
# Can be one of: -+=
SRS_SEPARATOR==

# Secret key to sign rewritten addresses.
# When postsrsd is installed for the first time, a random secret is generated
# and stored in /etc/postsrsd.secret. For most installations, that's just fine.
#
SRS_SECRET=/etc/postsrsd.secret

# Length of hash to be used in rewritten addresses
SRS_HASHLENGTH=4

# Minimum length of hash to accept when validating return addresses.
# When increasing SRS_HASHLENGTH, set this to its previous value and
# wait for the duration of SRS return address validity (21 days) before
# increading this value as well.
SRS_HASHMIN=4

# Local ports for TCP list.
# These ports are used to bind the TCP list for postfix. If you change
# these, you have to modify the postfix settings accordingly. The ports
# are bound to the loopback interface, and should never be exposed on
# the internet.
#
SRS_FORWARD_PORT=10001
SRS_REVERSE_PORT=10002

# Drop root privileges and run as another user after initialization.
# This is highly recommended as postsrsd handles untrusted input.
#
RUN_AS=nobody

# Bind to this address
#
SRS_LISTEN_ADDR=127.0.0.1

# Jail daemon in chroot environment
CHROOT=/usr/local/lib/postsrsd
```
#### Postfix
```
vim /etc/postfix/main.cf
```
```
sender_canonical_maps = tcp:localhost:10001
sender_canonical_classes = envelope_sender
recipient_canonical_maps = tcp:localhost:10002
recipient_canonical_classes= envelope_recipient,header_recipient
```
#### Services 
```
systemctl enable postsrsd # to start it at boot
systemctl start postsrsd
systemctl restart postfix
```
#### Test log
```
sudo systemctl status postsrsd
```
```
Jun 16 17:31:34 server01.bytle.net postsrsd[30592]: srs_forward: <nico@icloud.com> rewritten as <SRS0=F3Gb=75=icloud.com=nico@bytle.net>
Plain textDownloa
```
