# Git

- Git Graph, GitLens, případně i vscode-icons

Basic config setup before first commit:

```bash
$ git config --global user.name "Tomáš Probošt"
$ git config --global user.email probotom@seznam.cz

# List current config
$ git config --list 

# Init git repo
$ git init 
```

Working with changes:

```bash
# Check staging area
$ git status

# Add changes to staging area  
$ git add file.py     # add file.py 
$ git add .           # add all file from current dir   
$ git add *.py        # add all python files from current dir

# Create commit
$ git commit -m "Create commit using imperative commit message"
$ git commit          # create commit with VIM prompt 
$ git rm --cached 'název souboru' # odebere soubor z trackování a nechá v pc 

# Checking commit history           
$ git log --oneline  
$ git log --oneline --all --graph

# Remove and rename file
$ git rm file.py                # remove file from tracking and working tree
$ git mv file.py new_file.py    # rename file.py 
```

Working with branches:

```bash
# Branch view
$ git branch        # list all local branches 
$ git branch -r     # list all remote branches
$ git branch -1     # list all branches

# New branch
$ git branch new_branch           # create new branch
$ git branch -d branch_name       # delete branch

# Switch branch
$ git checkout <branch_name>      # switch HEAD to branch_name
$ git checkout -b <branch_name>   # create new branch and switch to it
```

Merging branches

```bash
# Sjednocení branche do hlavní branche
$ git checkout B   # přepnutí do jiné branche
$ git merge A      # sjednocení
```

Working with remotes

```bash
# First genrate SSH keypair 
$ ssh-keygen -o 

# Get the public key at paste it into github settings 
$ cat ~/.ssh/id_rsa.pub

# Výpis aktuálního připojení 
$ git remote 
$ git remote -v 

# Add new remote 
$ git remote add <remote_name> <remote_address>

# push to remote 
$ git push <remote_name> <local_branch_name>
$ git push --set upstream origin 'název branch' # nahrání nové branche pod main (jinak se vytvoří samostatný branche na Gitu)
$ git gc   # Zabalí jednotlivé složky úprav do složky pack (viditelné na windows10) - komprese souborů
```

Joining new project

```bash
# First make sure to setup ssh key pair

# Clone the remote repository přes SSH
$ git clone <remote_repo_url>

# To track remote branch in your local branch 
$ git checkout -b <branch_name> <remote_name>/<remote_branch_name>
# e.g
$ git checkout -b test_branch origin/test_branch

# Update locally tracked branch 
$ git pull 

# Or 
$ git fetch <branch_name>  # stáhne všechny nové branche z Gitu
$ git merge <remote_name>/<branch_name>
```

Going back in time

```bash
# Undo git add (remove staged changes)
$ git reset 
$ git reset <file_name>

# Undo and remove all uncommitted changes 
$ git reset --hard 
$ git reset --hard <file_name>

# Move branch to certain commit in history 
$ git log --oneline             # list commits
$ git reset --soft <commit_id>  # změny zachované v pc, vrácení o commit zpět
$ git reset --mixed <commit_id> # dont keep diffs in staging area
$ git reset --hard <commit_id>  # REMOVE CHANGES FOR EVER !!!!!! VERY DANGEROUS - možnost vrácení i na smazaný commit

# Move head to certain commit
$ git checkout <commit_id>      # Vrácení ke commitu - READ ONLY
$ git switch -                  # Move HEAD back to previous position
$ git switch -c <new-branch>    # create new branch from current commit
```

<h3> Tags </h3>

```bash
$ git tag             # Výpis tagů
$ git tag v1.0.0      # Vytvoření obyčejného tagu v1.0.0
$ git tag -d v1.0.0   # Smazání tagu 
$ git tag -a v1.0.0 -m "My first release" # Vytvoří se tag s poznámkou
$ git tag -a v0.9.9 -m "zpráva" 'hash'    # vytvoří tag ke staršímu commitu  
$ git checkout 'id tagu'                  # Přepne mě do dané verze tagu 
$ git push --tags  # Nahraje na remote i tagy (jinak to bere jen commity) 
```