# Setting up your gh-pages as a submodule

When setting up a GitHub page only repo where you have some source files and a build step, it's exactly clear how you should go about creating the repository without mixing your source and built files, and without having to navigate to a build directory on your github page. There is a way to do it, and it involves using submodules. You basically have a repository, inside your repository, that is also your repository...inception type stuff I know but it doesn't require superpowers.

## The Setup

1. Reclone your repository into a new directory, so for this one I'd do this from the directory above my current project:
```shell
git clone git@github.com:shiftyp/submodules-gh-pages-example.git some-other-directory
cd some-other-directory
```
2. Checkout an orphan gh-pages branch. If you already have a gh-pages branch, delete that first (be sure that your master branch is up to date before you do this). To do all of this, you'd do:
```shell
git checkout master
git merge gh-pages
# This deletes it locally
git branch -D gh-pages
# This deletes it on github
git push origin :gh-pages
# Then checkout a new orphan branch (meaning master isn't a parent)
git checkout --orphan gh-pages
```
3. Clear out the files in this new directory (except the .git dir), then copy what's currently in your build directory into this new repository.
```shell
mv .git ../git-backup-tmp
rm -r ./*
mv ../git-backup-tmp ./.git
cp -r ../submodules-gh-pages-example/build/* ./
```
4. Now commit and push
```shell
git commit -am "Redoing gh-pages as a submodule, initial commit"
git push origin gh-pages
```
5. You can now remove the current directory. Then cd into your old repo and remove the build directory.
```shell
cd ../
rm -rf some-other-directory
cd submodule-gh-pages-example
rm -rf build
```
6. Add your repository...to your repository...as a submodule. Whoa!
```shell
git submodule add git@github.com:shiftyp/submodule-gh-pages-example.git build
```
7. cd into your build directory and checkout gh-pages
```
cd build
git checkout gh-pages
```

## The Workflow

Annnd your done! Now build as usual, and when you want to push to gh-pages, cd into build and do a commit and push to `origin gh-pages`. If you end up wanting to check out your repository somewhere else do:
```
git clone git@github.com:shiftyp/submodule-gh-pages-example.git
cd submodule-gh-pages-example
git submodule init && git submodule update
cd build
git checkout gh-pages
```

## Still with me?

Pretty cool! Now your master and gh-pages are easily linked, yet completely separate. I haven't seen many people do this, but I wouldn't do it any other way.
