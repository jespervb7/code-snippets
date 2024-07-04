# list of my git alias

## git commit -m

A shorter version of commit -m

```git
git config --global alias.cm 'commit -m'
```

## Stage, commit and push all changes

This alias is nice for quick commits. The one's that are super small and don't need elaborate customizations, which is about 80% of my commits.

```git
function lazygit() {
    git add .
    git commit -m "$1"
    git push
}
```
