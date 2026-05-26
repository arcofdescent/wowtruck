# Dev setup

## git

### ssh public key to github

```sh
ssh-keygen
```

### Copy ssh key to GitHub

```sh
cat .ssh/id_ed25519.pub
```
Paste contents of above `.pub` file
 as a new SSH key in GitHub settings.
 
 ### 
 ```
 git config --global user.name "Rohan Almeida"
git config --global user.email "rohan.almeida@gmail.com"
 
 ```
 git clone git@github.com:d-Insights-Pvt-Ltd/lp5-booking-service.git
 ```
 
 ```
 cd lp5-booking-service
 ```
 
### create a dev branch
 
 ```
 git checkout -b {user}/repo_setup
 ```
 
### push to remote

You need to create any file (eg. README.md) an initial commit before a push

```sh
git add .
git commit -m "initial commit"
git push origin {user}/repo_setup
```

### VS Code
Install extension Remote - SSH

`ctrl+shift+p` to open command palette

