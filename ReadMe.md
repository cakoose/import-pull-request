# import-pull-request

This script makes it easy to handle pull requests in a way that:

1. Avoids the merge commit.  If you love merge commits, I can't help you.
2. Lets you tweak the changes and commit message, but preserves the original commit's "Author" field.

Requires Python 3.4+ or 2.7

Installation:

1. Either clone this whole repo or just download the `import-pull-request` script.
2. Copy or symlink the script to some folder that's in your shell's PATH.

This is free and unencumbered software released into the public domain.  For more information: [Unlicense.txt](Unlicense.txt) or http://unlicense.org/

## Usage Example

Let's say someone sent you a pull request.  Copy the pull request page's URL.

1. `cd` into your local copy of the repo.
2. Use this script to import the pull request into a new temporary working branch; let's name this branch "temp-branch".

```
import-pull-request https://github.com/user/repo/pull/123 temp-branch
```

The commits from the pull request are now in "temp-branch".

1. Rebase the commits: `git pull --rebase`.
    - If it doesn't rebase cleanly and you're not willing to deal with it yourself, tell the author to rebase their pull request.
2. Test the changes.
    - If something is broken and you're not willing to fix it yourself, tell the author to fix it.
3. Make any changes you need to make.  Feel free to make additional commits.
4. Squash the commits and tweak the commit message:
    - `git rebase -i`
    - Your editor will pop up.
        - If there is only one commit, change the "pick" to "reword".
        - If there are multiple commits, change "pick" to "squash" for all but the first.
        - Save and quit.
    - Your editor will pop up again.
        - Write a clean commit message.
        - Keep the "Closes #123" line at the end; it tells GitHub to [auto-close](https://help.github.com/articles/closing-issues-via-commit-messages/) the pull request.
        - Save and quit.
5. Push the commit to main repo: `git push`
6. Delete the local branch: `git checkout master; git branch -d temp-branch`
