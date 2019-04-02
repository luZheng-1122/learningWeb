# Git

## clone with SSH & clone with HTTPS

## git switch username and email

## git set remote url

https://stackoverflow.com/questions/42830557/git-remote-add-origin-vs-remote-set-url-origin?answertab=votes#tab-top

below is used to a add a new remote:

```bash
git remote add origin git@github.com:User/UserRepo.git
```

below is used to change the url of an existing remote repository:

```bash
git remote set-url origin git@github.com:User/UserRepo.git
```

below will push your code to the master branch of the remote repository defined with origin and -u let you point your current local branch to the remote master branch:

```bash
git push -u origin master
```

## git Submodules
https://git-scm.com/book/en/v2/Git-Tools-Submodules