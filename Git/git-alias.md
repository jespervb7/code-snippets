# list of my git alias

## git commit -m

A shorter version of commit -m

```git
git config --global alias.cm 'commit -m'
```

## Stage, commit and push all changes

This alias is nice for quick commits. The one's that are super small and don't need elaborate customizations, which is about 80% of my commits.

<!---
TODO: Create SOP for this function.

https://stackoverflow.com/questions/19595067/git-add-commit-and-push-commands-in-one

cd ~/
Open .bash_profile
Add

function lazygit() {
    git add .
    git commit -a -m "$1"
    git push
}

Now we need to activate your changes. Type source .bash_profile (or . ~/.bash_profile) and watch your prompt change.
source ~/.bash_profile
-->

```git
function lazygit() {
    git add .
    git commit -a -m "$1"
    git push
}
```
