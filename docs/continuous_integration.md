# Continuous Integration (CI)

Continuous integration is the practice of frequently merging code changes.

But frequent merges cause difficulties:
- What if the code doesn't compile after a merge
- What if someone broke something
- It would be a lof of work to check these things on every merge

The solution is to automate this process.

## how CI works ?
We use a CI server which executes a build that automatically prepares/compiles the code and runs automated tests.
