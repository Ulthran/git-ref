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
- 

## Without write access



## Actual simplest possible way

Just edit files directly on GitHub, use this for things like fixing a typo in the README.
