# Uitlijsten van commits op een bepaalde branch

Hieronder heb je een git command dat het uitlijsten van alle commits mogelijk maakt. Dit gebeurd dan op basis van de branch die gecheckout is.

```git
git log --pretty=format:'"%h","%ad","%an","%s"' --date=iso-strict > ./gitlog.csv
```

# Cleaner git log

This command gives you a prettier format of a git log

```git
git log --pretty=format:"%h %ad | %s%d [%an]" --graph --date=short
```
