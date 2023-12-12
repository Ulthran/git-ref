# Adding features or fixing bugs

## Simplest possible way

```bash
git clone git@github.com:org/repo.git
cd repo/
***Make changes***
git status
git add new_file.txt modified_file.txt
git commit -m "Added new_file.txt and modified modified_file.txt"
git push
```

Pros
- Quick 'n easy

Cons
- Pushing changes directly to master means your changes go live immediately without checks
- If you want to work on another feature/bug before finishing this, it'll get all messed up with this one's changes
- Harder to find this change way in the future

Notes:
- Use the [SSH URL](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) for repos you'll be making changes to
- Running `git status` before making any commits is usually good practice to remember what you're commiting and what branch you're on
- If you've only modified existing files (not added new ones), you can skip the `git add ...` command and use `git commit -am "..."` instead

## More proper way

- Create an issue on the repo (https://github.com/org/repo/issues) specifying what's wrong or what you want to add
- On the issue page (https://github.com/org/repo/issues/1) there should be sections on the right Assignees, Labels, Projects, Milestone, Development, Notifications. Under Development, use the Create a branch button to make a new branch.
- Get on that branch on your machine:

```bash
***If you haven't cloned the repo yet***
git clone git@github.com:org/repo.git
cd repo/
```

```bash
git fetch
git checkout 1-branch-name
***Make changes***
git status
git add new_file.txt modified_file.txt
git commit -m "Added new_file.txt and modified modified_file.txt"
git push
```

- Create a pull request (PR) for the branch you've just pushed changes to (https://github.com/org/repo/pulls). If there are any GitHub Action (GHA) workflows defined for the repo, they should trigger automatically.
- If all checks pass, merge the PR and delete the branch.
- Update local:

```bash
git checkout main
git pull
```

Pros

- Runs CI/CD before you commit anything to the main branch so you know if you've broken anything
- Keeps your changes separate from other changes you or others might make, both for current development and for looking at the history of the repo

Cons

- Lotta steps, can feel like overkill for small changes

Notes

- For bug fixes, if you really want to have a nice PR, make your first commit the addition of a test that fails because of the bug and then the second commit whatever is required to fix the bug and have the test succeed. This is test-driven development which looks really nice when it works and can be a pain in the butt otherwise.
- If your PR has merge conflicts (because changes have been made to the main branch since you checked out your dev branch), it's usually easiest to fix them locally. Run `git merge origin/main` then fix the merge conflicts in the specified files (usually by deleting one version of the conflicting code and keeping the one you want, there are also tools for not doing this manually) and commit and push.

## Without write access



## Actual simplest possible way

Just edit files directly on GitHub, use this for things like fixing a typo in the README.
