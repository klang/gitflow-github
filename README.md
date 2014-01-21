Quick example, using gitflow and github, publishing all features.

## Gitflow - on github

         .-p3------o----------+---o---.
         |                    |        \
         .-p2----o--.         |         \
         |           \        |          \
         .-p1--o--.   \       |           \
         |         \   \      |            \
         .-develop--+---+---. | .-----------+---.
         |                   \|/                 \
    -----o-master-------------+-------------------+
         ^                    ^                   ^
       v1.0                 v1.1                v1.2

### repo
    mkdir gitflow-github
    cd gitflow-github
### cool (initial)
    mkdir cool
    cd cool
    echo -e "feature1\nfeature2\nfeature3" > awesome.txt
    git init
    git add .
    git commit -m 'initial'
    git flow init
    # create project on github
    git remote add origin https://github.com/klang/gitflow-github.git
    git push -u origin --all

### p1 
    git flow feature start p1
    echo -e "feature1\nfeature2\np1:feature\nfeature3" > awesome.txt
    git add .
    git commit -m 'p1:feature added'
    git flow feature publish p1
    # feature p1 is now ready for peer review
    # -- p1 has not been officially "finished"

### p2
    git flow feature start p2
    echo -e "feature1\nfeature2\nfeature3\np2:feature" > awesome.txt
    git commit -a -m 'p2:feature added'
    git flow feature publish p2
    # feature p1 is now ready for peer review
    # -- p1 has not been officially "finished"

### p3
    git flow feature start p3
    echo -e "p3:helper1\nfeature1\nfeature2\nfeature3" > awesome.txt
    git commit -a -m 'p3:helper added'
    git flow feature publish p3
    
    # release is about to happen

### release1
    git flow feature finish p1
    git flow feature finish p2 
    git flow release start v1.1
    git flow release finish 'v1.1'
    git push -u origin --all

### p3 continuation 
additions to 'develop' have been added to 'develop'

    git checkout feature/p3
    git rebase develop
    echo -e "p3:helper1\nfeature1\np3:feature\nfeature2\np1:feature\nfeature3\np2:feature" > awesome.txt
    git commit -a -m 'p3:feature added'
    git flow feature finish p3

### release 2
    git flow release start v1.2
    git flow release finish 'v1.2'
    git push origin master develop
    git push origin :feature/p1
    git push origin :feature/p2
    git push origin :feature/p3
    git push -u origin --all

### README release
    git flow feature start README
    git add README.md
    git commit -m 'adding README.md' README.md
    git flow feature finish README
    git flow release start v1.3
    git flow release finish v1.3
    git push -u origin --all

### README hotfix

started to convert README file from `org-mode` to `markdown`, figured this had to be added to master.

    git stash
    git flow hotfix start v1.3.1
    git stash apply
    git add README.md
    git commit -m 'README converted to markdown'
    git flow hotfix finish 'v1.3.1'
    git push -u origin --all