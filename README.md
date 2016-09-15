# chef-services
```
kitchen list
kitchen converge
```
### Sets up:

1. chef-server
4. automate
5. build-node
3. supermarket
6. compliance

### Add this to your local workstation /etc/hosts

```
33.33.33.10 chef.services.com
33.33.33.11 automate.services.com
33.33.33.12 build.services.com
33.33.33.13 supermarket.services.com
33.33.33.14 compliance.services.com
```

#### Login to chef-server  
##### user/password: delivery/delivery
[http://chef.services.com](http://chef.services.com)

### Automate credentials for admin login

`kitchen exec [AUTOMATE KITCHEN INSTANCE NAME] -c 'cat /tmp/test.creds'`

[http://automate.services.com/e/test](http://automate.services.com/e/test)

#### Supermarket
[http://supermarket.services.com](http://supermarket.services.com)

### Login to compliance
##### user/password: admin/password

[http://compliance.services.com](http://compliance.services.com)

### Create Automate User account
Create organization to match name of org in cli.toml (delivery setup)
Login to UI and create user account (save creds).
Associate public key into user account (eg. insecure_public_key)
Note private key location (eg. insecure_private_key)
By default id_rsa and id_rsa.pub. Use those for ease.
Verify permissions of keys as 0600

### Create Chef Server Admin account
`kitchen login [CHEF SERVER KITCHEN INSTANCE NAME]`
`sudo -i`
`chef-server-ctl user-create admin admin user bob@mail.com 'Cod3Can!' --filename /tmp/admin.pem`
`exit`
`logout`

Login to Chef Server UI as delivery user (above); invite admin user to the organization. Logout.
Login to Chef Server as admin user; accept invite.

### First Project
Create local directory (eg. organization name). CD into.
`git clone [SOME PROJECT CODE]`; CD into.
`delivery setup --server=automate.services.com --ent=test --org=[NAME OF ORG] --user=[USER CREATED]`
`git config --global user.name "[USER NAME]"`
`git config --global user.email [USER EMAIL]`
`ssh -i [PATH TO PRIVATE KEY] -l [USER@ORG] -p 8989 automate.services.com`
Create .chef directory one level above project code
Pull down knife.rb from Chef Server; place into .chef directory
Place (or reset and place) user key private file into .chef directory
`knife node list` to verify connectivity with Chef Server
`berks install`
`berks upload --no-ssl-verify`
`delivery init`
