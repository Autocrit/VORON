# Setting up a GitHub backup script for Klipper config
- Create a repository on GitHub
- You will need to create an access token for this repository. On the GitHub website, go to *Settings* -> *Developer settings* -> *Personal access tokens* -> *Generate new token*
- Store the access token somewhere safe such as a password manager
- Set up git in *klipper_config* via SSH:
```
cd ~/klipper_config/
git init
git remote add origin https://github.com/[username]/[repository].git
```
- Where *[username]* is your GitHub username and *[repository]* is the repository you created for config backups
- The first commit/push-to-origin is via the command line, still in *klipper_config*:
```
git add .
git commit -m "klipper config backup - $(date -u)"
git push -u origin master
```
- It will ask for your GitHub username and password; use the access token as a password

### Create the backup script
e.g. *config_backup.sh* in home directory
```
#!/bin/bash

cd ~/klipper_config || exit
if ! git diff --quiet HEAD || git status --short; then
  git add .
	git commit -m "klipper config backup - $(date -u)"
	git push origin master
fi
```
- Make the script executable:
```
chmod +x config_backup.sh
```
- Run the script:
```
./config_backup.sh
```
