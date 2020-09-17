# git pull vs git fetch

`git-fetch` - Download objects and refs from another repository. Fetch branches and/or tags (collectively, "refs") from one or more other repositories, along with the objects necessary to complete their histories.

By default, any tag that points into the histories being fetched is also fetched; the effect is to fetch tags that point at branches that you are interested in. `git fetch` can fetch from either a single named repository or URL, or from several repositories at once. When no remote is specified, by default the `origin` remote will be used, unless there’s an upstream branch configured for the current branch.

`git-pull` - Fetch from and integrate with another repository or a local branch. Incorporates changes from a remote repository into the current branch. In its default mode, `git pull` is shorthand for `git fetch` followed by `git merge FETCH_HEAD`.

More precisely, `git pull` runs `git fetch` with the given parameters and calls `git merge` to merge the retrieved branch heads into the current branch. With `--rebase`, it runs `git rebase` instead of `git merge`.

## Links
https://git-scm.com/docs/git-fetch  
https://git-scm.com/docs/git-pull
