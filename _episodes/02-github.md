title: "Intro to Version Control with Git"
teaching: 30
exercises: 5
- "Explain use of .gitignore file"
- "Resolve a merge conflict"
- "git can be used with an online repository (GitHub) so that you can access a copy of your code from any machine"

> - Created GitHub account (described in set-up)
> - Configured git (described in set-up)
> - The Python package created in Episode 1.
facilitates collaboration on projects where everyone can work freely on a part
versions and rollback when needed. Also, you can review the
history of your project through commit messages that describe changes on the source code
and see what exactly has been modified in any given commit. You can see who made the
> ## git vs. GitHub
>
> `git` is the software used for version control, while GitHub is a hosting service. You can use `git` locally (without using an online hosting service), or you can use it with other hosting services such as GitLab or BitBucket.
> Other examples of version control software include SVN, and Mercurial.
>
{: .callout}
MolSSI recommends using the software `git` for version control, and [GitHub] as a hosting service, though there are other options.
Recommended Hosting Service: [GitHub]  
Other hosting Services: [GitLab], [BitBucket]
## Making Commits
You should have git installed and configured from the [setup] instructions.
In this section, we are going to  edit files in the Python package that we created earlier, and use `git` to track those changes.
First, use a terminal to `cd` into the top directory of the local repository.
When we ran the CMS CookieCutter, it actually initialized the use of `git` for us, added our files, and made a commit (how convenient!). We can see this by typing
$ ls -la
Here, the `-la` says that we want to list the files in long format (`-l`), and show hidden files (`-a`). You should see several files starting with `.git`. In particular, `.git` is a directory where `git` stores the repository data. We can tell from this output that we are in a git repository.
Next, type
$ git status
On branch master
nothing to commit, working tree clean
This tells us that we are on the `master` branch, and that no files have been changed since the last commit.
Next, type
$ git log
You will get an output resembling the following:
commit 25ab1f1a066f68e433a17454c66531e5a86c112d (HEAD -> master, tag: 0.0.0)
Author: Your Name <your_email@something.com>
Date:   Mon Feb 4 10:45:26 2019 -0500
    Initial commit after CMS Cookiecutter creation, version 1.0
When we have more commits (or versions) of our code, `git log` will show a history of these commits. Right now, we have only one commit - the one created by the CMS CookieCutter.
## The 3 steps of a commit
Now, we will change some files and use `git` to track those changes. Let's edit our README. Open `README.md` in your text editor of choice. On line 8, you should see the description of the repository we typed when running the CookieCutter. Add the following sentence to your `README` under the initial description.
This repository is currently under development. To do a developmental install, download this repository and type
`pip install -e .`
in the repository directory.
#### git add, git status, git commit
Making a commit is like making a checkpoint for a particular version of your code. You can easily return to, or revert to that checkpoint.
To create the checkpoint, we first have to make changes to our project. We might modify *many* files at a time in a repository. Thus, the first step in creating a checkpoint (or commit) is to tell `git` which files we want to include in the checkpoint. We do this with a command called `git add`. This adds files to what is called the *staging area*.
Let's look at our output from `git status` again.
        modified:   README.md
Git even tells us to use `git add` to include what will be committed. Let's follow the instructions and tell `git` that we want to create a checkpoint with the current version of `README.md`
$ git add README.md
{: .language-bash}
{: .language-bash}
        modified:   README.md
We are now on the second step of creating a commit. We have `added` our files to the staging area. In our case, we only have one file in the staging area, but we could add more if we needed.
To create the checkpoint, or commit, we will now use the `git commit` command. We add a `-m` after the command for "message." Whenever you create a commit, you should write a message about what the commit does.
$ git commit -m "update readme to have instructions for developmental install"
Now when we look at our log using `git log`, we see the commit we just made along with information about the author and the date of the commit.
Let's continue to edit this readme to include more information. This is a file which will describe what is in this directory. Open `README.md` in your text editor of choice and add the following to the end 
This package requires the following:
  - numpy
  - matplotlib
This file is using a language called [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet). 
> ## Check your understanding
> Create a commit for these changes to your repository.
>> ## Answer
>> To add the file to the staging area and tell `git` we would like to track the changes to the file, we first use the `git add` command.
>> ~~~
>> $ git add README.md
>> ~~~
>> {: .language-bash}
>> Now the file is *staged* for a commit.
>> Next, create the commit using the `git commit` command.
>> ~~~
>> $ git commit -m "add information about dependencies to readme"
>> ~~~
>> {: .language-bash}
> {: .solution}
{: .challenge}
Once you have saved this file, type
{: .language-bash}
Now, check `git status` and `git log`. You should see the following:
nothing to commit, working tree clean
$ git log
We now have a log with three commits. This means there are three versions of the repository we are working in.
`git log` lists all commits  made to a repository in reverse chronological order.
The listing for each commit includes the commit's full identifier,the commit's author, when it was created, and the commit title.
We can see differences in files between commits using git diff.
$ git diff HEAD~1
{: .language-bash}
Here HEAD refers to the point in our commit history (and current branch). When we use `~1`, we are asking git to show us the different of the current point minus one commit.
Lines that have been added are indicated in green with a plus sign next to them ('+'), while lines that have been deleted are indicated in red with a minus sign next to them ('-')
## Viewing out previous versions
If you need to check out a previous version
$ git checkout COMMIT_ID
This will temporarily revert the repository to whatever the state was at the specified commit ID.
Let's checkout the version before we made the most recent edit to the README.
$ git log --oneline
d857c74 (HEAD -> master) add information about dependencies to readme
3c0e1c6 update readme to have instructions for developmental install
116f0cf (tag: 0.0.0) Initial commit after CMS Cookiecutter creation, version 1.1
In this log, the commit ID is the first number on the left.
To revert to the version of the repository where we first edited the readme, use the git checkout command with the appropriate commit id.
$ git checkout 3c0e1c6
{: .language-bash}
If you now view your readme, it is the previous version of the file.
To return to the most recent point,
$ git checkout master
> ## Exercise
> What list of commands would mimic what CMS CookieCutter did when it created the repository and performed the first commit? (hint - to initialize a repository, you use `git init`)
> > ## Solution
>> To recreate the CMS Cookiecutter's first commit exactly,
>>~~~
$ git init
$ git add .
$ git commit -m "Initial commit after CMS Cookiecutter creation, version 1.0"
>>~~~
>>{: .language-bash}
>>  
>>  
>>The first line initializes the `git` repository. The second line add all modified files in the current working directory, and the third line commits these files and writes the commit message.
> {: .solution}
{: .challenge}
## More Tutorials
If you want more `git`, see the following tutorials.
### Basic git
 - [Software Carpentry Version Control with Git](http://swcarpentry.github.io/git-novice/)
 - [GitHub 15 Minutes to Learn Git](https://try.github.io/)
 - [Git Commit Best Practices](https://github.com/trein/dev-best-practices/wiki/Git-Commit-Best-Practices)
{% include links.md %}
[GitHub]: https://github.com
[GitLab]: https://gitlab.com/
[BitBucket]: https://bitbucket.org/