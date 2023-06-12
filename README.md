# openvpn-install.sh
### First, get the script and make it executable:

> curl -O https://github.com/amanpatelitprofessional/openvpn-install.sh.git

> chmod +x openvpn-install.sh

### Then run it:

> ./openvpn-install.sh

#### You need to run the script as root and have the TUN module enabled.

The first time you run it, you'll have to follow the assistant and answer a few questions to setup your VPN server.

When OpenVPN is installed, you can run the script again, and you will get the choice to:

Add a client
Remove a client
Uninstall OpenVPN
In your home directory, you will have .ovpn files. These are the client configuration files. Download them from your server and connect using your favorite OpenVPN client.

## PLEASE do not send me emails or private messages asking for help. The only place to get help is the issues. Other people may be able to help and in the future, other users may also run into the same issue as you. My time is not available for free just for you, you're not special.

Headless install
It's also possible to run the script headless, e.g. without waiting for user input, in an automated manner.

Example usage:

> AUTO_INSTALL=y ./openvpn-install.sh

## A default set of variables will then be set, by passing the need for user input.

If you want to customise your installation, you can export them or specify them on the same line, as shown above.

APPROVE_INSTALL=y
APPROVE_IP=y
IPV6_SUPPORT=n
PORT_CHOICE=1
PROTOCOL_CHOICE=1
DNS=1
COMPRESSION_ENABLED=n
CUSTOMIZE_ENC=n
CLIENT=clientname
PASS=1

If the server is behind NAT, you can specify its endpoint with the ENDPOINT variable. If the endpoint is the public IP address which it is behind, you can use ENDPOINT=$(curl -4 ifconfig.co) (the script will default to this). The endpoint can be an IPv4 or a domain.

Other variables can be set depending on your choice (encryption, compression). You can search for them in the installQuestions() function of the script.

Password-protected clients are not supported by the headless installation method since user input is expected by Easy-RSA.

The headless install is more-or-less idempotent, in that it has been made safe to run multiple times with the same parameters, e.g. by a state provisioner like Ansible/Terraform/Salt/Chef/Puppet. It will only install and regenerate the Easy-RSA PKI if it doesn't already exist, and it will only install OpenVPN and other upstream dependencies if OpenVPN isn't already installed. It will recreate all local config and re-generate the client file on each headless run.

### Headless User Addition

It's also possible to automate the addition of a new user. Here, the key is to provide the (string) value of the MENU_OPTION variable along with the remaining mandatory variables before invoking the script.

The following Bash script adds a new user foo to an existing OpenVPN configuration

#!/bin/bash
export MENU_OPTION="1"
export CLIENT="foo"
export PASS="1"
./openvpn-install.sh
