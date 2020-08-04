# algiorgio_infra
algiorgio Infra repository


1.  Connect to internal host with one command from working host:

    >   ssh -i ~/.ssh/ssh_private_key -J bastion_user_name@bastion_ip_address internal_user_name@internal_ip_address

2. Make access to internal host by alias:

    * Append to ~/.ssh/config:

        ```
        Host internal_host
            User internal_user_name
            HostName internal_ip_address
            ProxyJump bastion_user_name@bastion_ip_address
            IdentityFile ~/.ssh/ssh_private_key
        ```

    * Access internal host with command `ssh internal_host`

3. Create **setupvpn.sh** on bastion host (for Ubuntu Server 18.04 LTS in this case) with following commands:

    ```
    sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list << EOF
    deb https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse
    EOF

    sudo tee /etc/apt/sources.list.d/pritunl.list << EOF
    deb https://repo.pritunl.com/stable/apt bionic main
    EOF

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com --recv E162F504A20CDF15827F718D4B7C549A058F8B6B
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com --recv 7568D9BB55FF9E5287D586017AE645C0CF8E292A
    apt-get --assume-yes update
    apt-get --assume-yes upgrade
    apt-get --assume-yes install pritunl mongodb-org
    sudo systemctl start pritunl mongodb
    sudo systemctl enable pritunl mongodb
    ```

4. Add **setupvpn.sh** to ***cloud-bastion*** git-branch

5. Working with Pritunl Interface:
    1. Create Organization and User
    2. Append Server and bind it to the Organization
    3. Download profile of created User
    4. Download User's profile
    5. Rename the profile to **cloud-bastion.ovpn**
    6. Append **cloud-bastion.ovpn** to ***cloud-bastion*** git-branch



Travis Information Data
```
bastion_IP = 130.193.48.195
someinternalhost_IP = 10.130.0.26
```
