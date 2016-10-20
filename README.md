# playground
## Strange behavior in git:
to reproduce type in the following commands:

```bash
mkdir -p /tmp/behavior
cd /tmp/behavior
git init
mkdir folder1
touch folder1/file1.txt
git add -A
git commit -m "commit Message"
git checkout -b folder1
touch folder1/file2.txt
git add -A
git commit -m "another commit Message"
git checkout -b folder1a
touch folder1/file3.txt
git add -A
git commit -m "another commit Message"
```

now create a github.com repository, i.e.: behavior-example
lets push it to github:

```bash
git remote add origin https://github.com/yourusername/behavior-example.git
git remote -v
git push origin	master
git push origin folder1
git push origin folder1a
```

now clone this repo to verify the strange behavior:

```bash
cd /tmp
git clone https://github.com/yourusername/behavior-example.git
cd behavior-example
git branch -a
git checkout folder1
git branch
# STILL ON MASTER
git checkout folder1a
# NOW IT WORKS
```
i don't know why this is happening but it looks like `folder` and `branches`
should not have the same name.

# possible solution
to avoid this problem, you have 2 options.
1. you can set `better` branch-names like `wip/folder`.
2. after cloning the repo, checkout the branch with option `-t
	 origin/branchnam`, this will write the right config in `.git/config`. You
	 need to do this just for the first time. After that the next checkouts will
	 work as expected.

```bash
git clone https://github.com/yourusername/behavior-example.git
cd behavior-example
git checkout -t origin/behavior-example
git branch
# Changed to branch behavior-example
git checkout master
# back in master branch
git checkout behavior-example
# Now it works as it should
```
refer to: http://stackoverflow.com/questions/18833617/why-does-git-checkout-remote-branchname-not-create-new-tracking-branch/18995918#18995918
