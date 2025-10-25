---
title: gitpro 阅读笔记
date: 2025-09-25 20:43:41
categories: [note, git]
tags: git
---
# Gitpro

## git 对象模型

Git 并不是单纯的版本控制器，本质上它是一个 **内容寻址文件系统（content-addressable filesystem）**。

* 每个对象（blob、tree、commit、tag）都通过其内容计算哈希（SHA-1 或 SHA-256）作为键存储在 `.git/objects/` 中。
* 这种方式使得对象的唯一性和完整性由哈希保证。
* 逻辑上，Git 的提交是快照，但快照的存储完全依赖对象的引用关系。


### Git 对象模型

Git 的对象（object）类型主要有四种：

1. **Blob 对象**（binary large object）

   * 存储文件内容。
   * 内容完全相同的文件 → 哈希相同 → 对象复用。
   * 不存文件名，文件名存在于 tree 对象。

2. **Tree 对象**

   * 存储目录结构。
   * 包含若干条目，每个条目包含：权限、文件名、指向 blob 或子 tree 的哈希。
   * 即一个有向无环图（DAG）结构，而不是简单的复制。

3. **Commit 对象**

   * 存储元数据：

     * 指向根 tree 的指针
     * 父提交的指针（可多个 → 支持分支/合并）
     * 作者、提交者、提交信息
   * commit 是对某个 **tree 快照**的引用。

4. **Tag 对象**

   * 可选，对 commit、tree 或 blob 进行命名。

## git config
1. `/etc/gitconfig` applied to every user. pass the option `--system`
2. `~/.gitconfig` or `~/.config/git/config` applied for the user. pass the option `--global`
3. `.git/config` applied to the repositor. pass the option `--local`
4. `git config --list --show-origin` show all settings and where they are
5. `git config user.name` show the config of username

```bash
git config --global alias.st status #aliase
```

### set identity
```bash
git config --global user.name "name"
git config --global user.email "email@mail.com"
```
### set editor
```bash
git config --global core.editor vim
```

### set default branch name 
```bash
git config --global init.defaultBranch main
```
to set default branch as `main` rather than `master`


## get help
```bash
git help <verb>
git <verb> --help
man git-<verb>
```

some buildin command also can be ask `git <verb> -h`

## git clone
to set an anther name of the repository by
```bash
git clone URL newname
```

## git add 
to track file
```bash
git add files
```

## git stataus
```bash
git status #to show the repository status, filse be tracked or staged
git status -s #--short
```

## gitignore
the rules for the patterms:
1. bland line or lines starting with `#` are ignored
2. lines start with slash`/` will avoid recursivity
3. lines end with slash`/` to specify a directory
4. lines start with point`!` to be negated a pattern
5. standard glob patterns works
>`*` to matches zero to more characters;
>`[abs\]` to matches characters inside the brackets
>`?` to matches a single charater
>`**` to matches nested directories
>`[0-9]` to matches any character between them

## git diff
```bash
git diff # to show the difference between changed but not yet staged files
git diff --staged #--cached to show the difference between staged and the last commited
```

## git commit
```bash
git commit # to commit the staging area
git commit -m "message" #
git commit -a # to skip the staging area
git commit --amend #to commit the staging area files to the latstest commit or rewrite commit message

```

## git rm
```bash
git rm filse # to remove the staged filse from staging area and working directory
git rm -f filse # to remove tracked but not yet added filse
git rm --cached # to remove filse only from staging area but not delete from working directory
```

## git mv
```bash
git mv fromfile tofile # to move filse or remane file
# equl to the following command
mv fromfile tofile
git rm fromfile
git add tofile
```

## git log
```bash
git log # to show all commits information , auther, time, sorted by time
git log -p -2 #--patch to show the difference introduced in each commit, limit the number of commit
git log --stat #to show abbreviated stats
git log --pretty=oneline #also short, full, fuller, to show the output in specifical format
git log --pretty=format"%n - %an, %ar : %s" #to format output
git log --graph #to add a nice ASCII graph showing your branch and merge historyg

git log --since=2.weeks
git log --until=2.weeks
git log -S string #to show some commits that add/remove the paticular string
#there are also some option that I cant record, I can refer in [gitpro](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2)

git log --decorate #show each branch point which object
git log --oneline --decorate --graph --all #output the commit history, branch pointer and diverged
```

## git reset
```bash
git reset HEAD <file> # to reset file to the lastest commited version, in anther way , to clear the file from staging area
git reset --soft <commit> # to redo the lastest commit but keep the modified
git reset --hard <commit> # to back to the commit status in staging area and work directory, and do not keey the modified

```

## git checkout
```bash
git checkout -- <file> # to discard changes in working diretory
git checkout 2.2.0 # check out tags
git checkout <branch> #check out the branch
git switch <branch>
git checkout -b <newbranchname> #create and checkout the branch
```

## git restore
```bash
git restore --staged <file> # to unstage the file but not discard modification
git restore <file> # to dicard the modification of the file
```


## git remote
```bash
git remote # to show the remote servers name, origin
git remote -v # to show the URLs that your can pull and fetch
git remote add <shortname> <url> # to add a remote repository and give it a shortname
git remote show <remote> # to show detailed imformation of the remote
git remote rename oldname nowname # to rename the remote
git remote remove <remote> # to remove the remote repository
```

## git fetch
```bash
git fetch <remote> # fetch remote repository by URL or shortname, and the default name of cloned repository is origin
```

## git push
```bash
git push origin master # to push master branch to upstream(remote)
git push origin v1.5 # push tag to remote
git push origin --tag # push all tags to remote
git push origin :refs/tags/v1.4 #push null value before the colon: to the remote 
git push origin --delete <tagname> # delete the tag
```

## git tag
```bash
git tag # to list existing tags
git tag -l "v1.8*" #--list to list particular tags
git tag v1.0.0 -lw # a lightweight tag, just a pointer
git tag -a v1.0.0 -m "First stable release" #Annotated tag, attach other information
git show v1.0.0 # to show the imformation of the tag and commit
git tag -a v1.2 9fceb02 # to add a tag to a commit
git tag -d <tagname> # to delete a lightwight tag
```

## branch
{% asset_img image.png the init git object %}

they are three object
each commit object contain there parent
{% asset_img commits-and-parents.png %}

```bash
git branch #list all branch 
git branch newbranchname #create a new branch
git branch -d <branch> #delete the branch
git branch --merged #filter the merged branch, and --no-merged 
```

## git merge
```bash
git merge <bemergedbranch> #to merge a branch
```
if there is any conflict in the merge, you can check by running `git status`, and then modify the file . after that, run `git add` on each file to mark it as resolved


