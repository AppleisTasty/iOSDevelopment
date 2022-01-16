# 1. User Manual

How to use Git

## Git records

This markdown records some tips from my learning of Github's git.

> These measurements were taken in a MacBook Air, 2.2 GHz Dual-Core Intel Core i7 with 8 GB 1600 MHz DDR3 with 500GB Flash Storage running macOS Big Sur V11.3.
>
> Preparation：Insatll Git package：https://git-scm.com/downloads

**Relevant Links:**

How to install Homebrew：https://treehouse.github.io/installation-guides/mac/homebrew

## What is fork?

Click the fork on other people's page to copy the contents into your workplace.

## Links to the repository?

![image-20211223053740013](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202112230537392.png)

Use this link to access the repository

## Command clone

use terminal command to clone

```
git clone https://github.com/<your-username>/my-repository.git
```

## What is branch?

A branch is like a safe copy of the contents. You can make whatever changes to a branch withour affecting the original contents. But you can apply these changes to the main contents by merging them.

Create a new branch:

```
git branch my-branch
```

Check all the branches:

```
git branch
```

switch to branches:

```
git checkout my-branch
```

Branches can have branches.

## Git is passive

Git don't automatically save changes to the repository, you need to flag them.

By Command: (Use this command to tell Git that you want to save changes to the README.md file)

```
git add README.md
```

## Commit changes?

Once you made any changes to the repository, you need to commit to ensure that you tend to apply changes to the Git ( not uploaded yet)

```
git commit -m "Adding some contents"
// use -m to add a description to the commit
```

## Pushing changes

Apply changes to the remote repository

```
git push --set-upstream origin my-branch
//--set-upstream: link the local repository to the remote repository
// origin: refers to the remote repository
```

## what is pull request?

inform anyone else that is using this repository of the changes.

Do it on the web page.

![image-20220103083453909](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201030834869.png)

![image-20220103083503876](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201030835955.png)

![image-20220103083512291](https://cdn.jsdelivr.net/gh/AppleisTasty/PicGarage/tmp/202201030835366.png)

<hr/>

# 2. Mechanisms

How Git works

Git repository is a directory.

The repository contains a hiddent `.git` directory that tracks the history of all changes. GitHub is an online host for repositories. ( Others like GitLanb, BitBucket or customized local repositories)

## What is GitHub?

> Basically, it's just a big cloud-based storage solution for repositories.























