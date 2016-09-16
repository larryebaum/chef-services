### chef-services
```
kitchen list
kitchen converge
```
#### Stands up:

1. chef-server
2. automate
3. build-node
4. supermarket
5. compliance

<b>Chef server</b><br>
user/password: delivery/delivery<br>
[http://chef.services.com](http://chef.services.com)

<b>Automate server</b><br>
Get Automate credentials for admin login<br>
`kitchen exec [AUTOMATE KITCHEN INSTANCE NAME] -c 'cat /tmp/test.creds'`<br>
[http://automate.services.com/e/test](http://automate.services.com/e/test)

<b>Supermarket</b><br>
[http://supermarket.services.com](http://supermarket.services.com)

<b>Compliance</b><br>
user/password: admin/password (defined on first login)<br>
[http://compliance.services.com](http://compliance.services.com)

#### <u>Steps</u>
###### 1. Add this to your local workstation /etc/hosts
```
33.33.33.10 chef.services.com
33.33.33.11 automate.services.com
33.33.33.12 build.services.com
33.33.33.13 supermarket.services.com
33.33.33.14 compliance.services.com
```

###### 2. Create Automate user account
Log in to Automate server (creds above) and create a user account; assign all permissions (can edit later).
Insert the public half of a keypair that exists on your local workstation.
Log out and log in with the new user account.
Create a new organization

###### 3. Create Chef server admin account
Log in to Chef server via kitchen from the chef-services repo directory and create admin user<br>
`kitchen login [CHEF SERVER KITCHEN INSTANCE NAME]`<br>
`sudo chef-server-ctl user-create [USERNAME] [FIRST NAME] [LAST NAME] [EMAIL] [PASSWORD] --filename [FILE PATH]
`<br><br>
Copy contents of output file to clipboard or local workstation file as [USERNAME].pem
Exit kitchen ssh session.<br>
`exit`<br><br>
Log in to
Log in to Chef server with delivery account; select organization from Administration tab and click invite user; indicate the newly created admin user to be added and click Invite.<br>
Log in to Chef server with admin account, accept invite.<br>
Log in to Chef server with delivery account; select Administration tab; select Admins group from left pane and add newly created admin user to the admin group.<br>


###### 4. Prep local workstation for Automate Project
Create directory matching enterprise name. Create subdirectory with organization name. Create subdirectory cookbooks.<br>
Within organization directory, create `.chef` directory.<br>
Move or create private key file (from step 3 above) into .chef directory.<br>
From Chef server UI, download knife.rb file for your Chef org and move into .chef directory.<br>
From within organization directory, setup connection to Automate<br>
`delivery setup --server=automate.services.com --ent=[ENTERPRISE NAME --org=[ORGANIZATION NAME] --user=[AUTOMATE USER]`<br>

Fetch Chef server SSL certificates to local workstation<br>
`knife ssl fetch`<br>

Verify connectivity to Chef server<br>
`knife cookbook list`<br>

###### 5. Setup local Project to verify workflow
From within cookbooks directory, git clone some project repo, CD into directory, and push code to Chef server.<br>
`berks install`<br>
`berks upload --no-ssl-verify`<br>

From within cookbooks directory, git clone delivery-base cookbook (used for environment nodes), CD into directory, and push code to Chef server.<br>
`berks install`<br>
`berks upload --no-ssl-verify`<br>

Verify connection for local workstation to Automate built-in github services.<br>
`ssh -i [PATH TO AUTOMATE USER PRIVATE KEY] -l [AUTOMATE USER@ORG] -p 8989 automate.services.com`<br>

Submit either or both projects to Automate (from within project directory), provide Automate user password when prompted.<br>
`delivery init`<br>

###### 6. Setup environment nodes/stages
TODO: Insert steps
