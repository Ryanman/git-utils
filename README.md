# Git Utils

A collection of small git utilities and instructions

Use shift-right click to open your command prompt of choice in your repository folder. From VS, you can open a powershell window or go to:

Team Explorer -> Branches -> Actions -> Open Command Window Here

## Add your own Git Commands

Adding Git commands requries that you 
1. Write the commands in bash
2. Create files **With No Extension** in a Path directory
3. Name the file using the format `git-{alias}`
4. Add the line `#!/bin/sh` as the first line of the file
5. Use shell syntax
6. Test it in your shell of choice

For Git for Windows, there's a PATH variable that makes a lot of sense, though you'll need admin priveliges:

` C:\Program Files\Git\cmd `

You can copy the folder adjacent to this .MD to get the whole Hylaine-standard library. Huge credit to [This](https://github.com/shobhitpuri/git-refresh) Github!

## Delete merged, non-master branches (fullprune)

Once you have a ton of branches in a repo that have been merged, you'll want to clean up. Deleting manually is a PTIA.

There's 2 different ways really. Git exposes a --merged flag which is helpful, but the -v also has a [gone] flag that shows when there's no corresponding remote, which (IF YOU'RE DOING THINGS RIGHT) is another shorthand for a merged branch.

You can execute `git fullprune {sacredBranch}`, with the parameter being optional and assuming master.

*Linux Shell*

`git branch --merged master | egrep -v '^\s*\*?\s*master$' | xargs git branch -d`

*Powershell*

<code>
git fetch -p #Required to prune and delete remote tracking before execution

git branch -v |
select-string -Pattern '^  (?<branchName>\S+)\s+\w+ \[gone\]' | 
foreach-object{ 
    #delete the captured branchname.
    git branch -D $_.Matches[0].Groups['branchName']
}</code>


## Get latest into your long-running branch (refresh)

A frequent usecase is to get changes from master while on your own branch. The problem here is, especially with large solutions and tightly-coupled IDEs like Visual Studio, is that this often involves:

1. Committing your changes, regardless of your current progress
2. Checking out Master (Insert 10-15s VS Reload)
3. Pulling down the latest
4. Checking out your branch (Insert 10-15s VS Reload)
5. Merging (Insert 10-15s VS Reload)

You can execute `git refresh {remoteBranch}` with with the parameter being optional and assuming master 

While you may have to resolve conflicts (and will therefore have to do step 1), you can accomplish this with 2 lines of code:

*Shell-independent*

`git fetch origin master:master`

`git merge master`

note that refresh in assumes master and does something more complex, by stashing your changes.

## Create a new branch and push to remote (checkpush)

Whenever you checkout from the command line rather than a GUI tool, pushing to origin generally one too many steps.

You can execute `git checkpush {newBranch}` with with the parameter being required. Use the Hylaine-standard branch formatting please:

`{bug || feature}/{workItemOrTicketId}-{description}`