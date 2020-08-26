
  **git merge**
  *`git merge`* simply remembers the history as it happened. It takes the two current versions, merge them together based on their common ancestor, fix any conflicts, and then record the history exactly as it happened.
  
  **git rebase**
 *`git rebase`* attempts to make the history look as simple as possible. It picks one of the two sides to be the base  and all the changes on the other side are flattened into a linear series of changes happening afterwards.
 git rebase  | git merge |
| ------------- | ------------- |
| 1.   **git rebase**  gets all unique commits from both branches and applies them one by one  | 1. **git merge**  apply all unique commits from branch A into branch B in one commit with final result.  |
| 2.  **git rebase** rewrites commit history but doesn’t create extra commit for merging  | 2.  **git merge** doesn’t rewrite commit history, just adds one new commit.  |

**Links**:
https://git-scm.com/book/en/v2/Git-Branching-Rebasing
https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging#_basic_merging