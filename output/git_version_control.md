# Version Control with Git

# Topics
Version Control allows for a team to collaborate on code without overwriting changes and allows people to track how the code evolved. It is very easy to create versions and move between different versions.

We chose Git as our version control system because it is decentralised. This treats each person's copy as their own repository where you can track changes before you share your code more widely with the team. It also tracks changes in content rather than files so it takes up a lot less space compared to some other version control tools.

We use GitHub as our hosting service. Github is one of the more popular, widely known public hosting services. Repositories can still be made private if need be.

## The Basics

### Setting up Git on your machine
If Git is not readily available or there were issues installing Git that IT is yet to resolve then Git can be installed manually using the following steps

1.	Go to the [webpage](https://git-scm.com/download/win) where you can download Git
2.	Download 32 bit build (for now - hopefully the 64 bit laptops will come soon)
3.	Run the exe
4.	Then do the following in the setup wizard: Info hit next, use default components hit next, select use git from bash, use open ssl library, select checkout windows style commit unix style line endings. This is because we may work on cross platform projects, use minty as a terminal emulator, extra options enable caching, enable git credential manager

5. Open Git Bash and run the following commands can be run **without** replacing the items in the angled brackets.

```bash
git config --global http.proxy http://<user_name>:<password>@<proxy_server>:<port_number>
git config --global https.proxy https://<user_name>:<password>@<proxy_server>:<port_number>

```

6. Open the `gitconfig` file. Enter your work `username` and `password`. 

7. If your password has special symbols refer to this [website](https://www.w3schools.com/tags/ref_urlencode.asp) for the appropriate percent encoding. For example the password !qwerty123 would be specified as %21qwerty123 in the proxy.

8. Specify the `proxy server` and the `port number`. If you do not know what these are then ask a team member.

**Do not** type your details (in particular your password) in directly via the terminal. Your password will be retained in the history for everyone who uses the machine to see.

9. Once the proxies are set up your name and email can be specified in the config file. This makes it easier to see who has made the commits on Github.


```bash
git config --global user.name "Your Name"
git config --global user.email <firstname>.<lastname>@sia.govt.nz
```


**Note:** There was talk of getting a machine that was separate from the network so that individual team members could maintain it and install all the necessary data science tools themselves. If it has a Linux distro installed then steps 1 to 8 would be replaced by:


```bash
sudo apt-get update
sudo apt-get install build-essential libssl-dev libcurl4-gnutls-dev libexpat1-dev gettext unzip
sudo apt-get install git
```

### The different working areas and repositories
*Coming Soon*

A very good interactive Git cheat-sheet detailing all the different commands can be found [here](http://ndpsoftware.com/git-cheatsheet.html)

### Setting up a repo
It is easiest to do this via the website rather than the command line

<div class="jumbotron">
![](../resources/git_new_repo.png)

</div>

1. Click the `New` button
2. Give the repository a name it should be short, lower case and separated by underscores
3. Specify a sentence to describe the repository
4. Ensure the repository is set to private (Manager will give the green light to make a repository public)
5. Check the initialise with a README box
6. Add a license. We use `GNU GPL v3.0` for our code because we want this work and derivatives to be available under the same open license. Justification for this can be found in our document management system in the file `A9536265`. This document was informed partly by [choosealicense.com](https://choosealicense.com/), [NZGOAL2](https://www.ict.govt.nz/guidance-and-resources/open-government/new-zealand-government-open-access-and-licensing-nzgoal-framework/nzgoal2/) and information on the [Creative Commons website](https://creativecommons.org/faq/#can-i-apply-a-creative-commons-license-to-software)


### Cloning a repo
Cloning takes a repo from the remote repository and creates a local copy on your machine. You can then make changes and eventually push them back to the remote repository.


```bash
# the --recursive option ensure that all dependent repositories are also cloned
# see the submodule section for more information about this
git clone --recursive https://github.com/nz-social-investment-agency/social_investment_data_foundation.git
```

### Pushing your changes through to Github
To push your changes through to the remote repository it helps to refer to the section called [The different working areas and repositories](#the-different-working-areas-and-repositories)

For those who are familiar with other version control tools like subversion the most common mistake is forgetting to add the files to the index. Make sure you add the files you want to commit before you commit the files.


```bash
# display changes to the files and items that have not been tracked 
git status
# add modified and new files to the index
# note the . includes all files that were listed in status
git add .
# commit all changed files except untracked files to the local repository
# the -m allows a message to explain the changes that were made
# this should be specified in every commit
git commit -m "skeleton structure"
# push things through to the remote repository
git push origin master
```

### Tagging your repo
We chose to use annotated tags because they allow for messages to be specified, dates and authors

**Do not use lightweight tags** (ones with no options) they only contain the SHA number

Signed tags could be used to check that the tag was created by an authorised person. At this point in time it seemed over the top.

```r
# the -a option creates an annotated tag
git tag -a v1.1.0 -m "v1.1.0"
# use the --tags option to push to the tag to the remote repository
git push --tags origin master
```

Below is a list of tags for one of the repositories.

```r
git tag -l -n10
v1.0.0          v1.0.0
# looks like someone didnt specify a message with this tag
v1.1.0
v1.1.1          v1.1.1
```


### Switching a repo from private to public
Only the person who created the repo or the owner of the organisation can modify the settings of a repository

<div class="jumbotron">
![](../resources/git_settings.png)

</div>

1. In Github click on the repo name
2. Click on the `Settings` tab
3. Scroll down to `Danger Zone`
4. Click on make this repository public
5. Type the repository name in the box and click the button below


## The more challenging tasks

### Dealing with repo dependencies (submodules)

```r
# make sure you are in your working directory
cd sia_repo_template
# add the dependent repository as a submodule and specify what folder to put the dependent repository in
git submodule add https://github.com/nz-social-investment-agency/social_investment_analytical_layer lib/social_investment_analytical_layer
# confirm that the submodule has been added
git submodule status
# check the status of the working directory
git status
# commit the submodule with an appropriate message
git commit -m 'added sial submodule to lib'
# push the submodule into the remote repository
git push origin master
```

### Closing issues with commit messages
Issues can be automatically closed via the commit by using a key word and an issue number.

Key words include: close, closes, closed, fix, fixes, fixed, resolve, resolves and resolved.

An example is shown from the log that would have automatically closed an issue


```bash
# notice the keyword 'fix', a space, the hash and then the issue number
git log --oneline
86eeddb issue fix #1 readme doesnt contain expected output

```

### Working on an older version (detached head state)
*Coming Soon*

### Retrospectively adding a tag
If code has been pushed to the remote repository and we release several commits later that we forgot to tag an earlier commit it can be retrospectively added. An example is shown below. It requires knowing the SHA number of commit you want to tag. This can be retrieved from the logs or from the repo on Github.


```r
# make sure you are in the working directory
cd social_investment_analytical_layer/
# identify the SHA number of the commit you need
git log --oneline
# strip the date this was committed out of the
"$(git show 8e2045e --format=%aD | head -1)" git log --oneline
# this puts you in detached head state
# you will be sitting in an older commit that needs the tag
git checkout 8e2045e
# manually set the tag date to the date it was committed
GIT_COMMITTER_DATE="$(git show --format=%aD | head -1)" git tag -a v1.0.0 -m "v1.0.0"
# the --tags option ensures that the tags are pushed through
git push --tags origin master
# jump back into the master version
git checkout master
# confirm the tag is there with the right date
git log --oneline
# remove the hard coded date so subsequent commits display the current dates
unset GIT_COMMITTER_DATE
```

### Renaming files
Going to the file explorer and renaming a file will mean Git will treat the original file as deleted and the renamed file as a new file. This can be problematic because it creates a break in the history of the file rather than noting the file has been renamed. This can be avoided by using the following commands


```bash
# this will note that the file has been renamed rather than believing the file the original file has been deleted and treating the renamed file as new
git mv SIAL_data_dictionary_JAN_2017.xlsx SIAL_data_dictionary_MAY_2017.xlsx
# use this to confirm that the file has been renamed rather than deleted and added
git status
```

### Displaying markdown with GitHub Pages
GitHub Pages is a site hosting service. Any html files can be nicely displayed using this. It is particularly useful for displaying markdown files that have been rendedred in html

<div class="jumbotron">
![](../resources/git_github_pages.png)

</div>

1. Under `settings` scroll down to the GitHug Pages
2. Select `master branch`
3. Click `Save`
4. Go the `README` and add a hyperlink to the relevant pages. If there is enough content you may wish to create index.html so that you have a nicer looking page.
5. Put a hyperlink into the page. It is similar to paths in the repo the only thing is the organisation / profile should have `.github.io` after it. An example of a GitHub page hyper link is 
`https://nz-social-investment-agency.github.io/sia_analytical_processes/output/coding_style_critique.html`


### Compliance with Legislation
Information on Github is collected, stored and processed in the United States.To ensure that we still have access to all our repositories in the unlikely event that Github closes we periodically download zip files from Github and stored them in our information management system.


```bash
git clone https://github.com/nz-social-investment-agency/outcomes-measurement.git
git clone https://github.com/nz-social-investment-agency/outcomes-measurement.wiki.git
```

Then zip up these folders and store them in folder `qA537714`. These should be updated at least every year.

### Troubleshooting - cannot connect
If you try to clone something and it will not work check the following

* Check the repository name is correct (the easiest thing to do is go to the Github website find the repository and copy and paste the URL
* Have you spelled the http proxy and https proxy correctly in the `gitconfig` file?
* Have you recently changed your log in password. If so make sure you update your `gitconfig` file?


### Branching
*Coming Soon*

### Merging
*Coming Soon*

### Hooks
Hooks are little scripts that are triggered during an event like `commit` or `push`. The most useful hook for the SIA would probably be one that enforces coding standards. Such a hook has not been implemented yet.

An example of a coding standard hook can be found [here](https://gist.github.com/danielpopdan/990f9521f84d693ccd1a).

## Related documents
* Adding an individual to the SIA Github organisation 
* [SIA coding conventions](https://github.com/nz-social-investment-agency/sia_analytical_processes)


Last updated Sep-2017 by EW

