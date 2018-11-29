# Continuous Integration (CI)

Continuous integration is the practice of frequently merging code changes.

But frequent merges cause difficulties:
- What if the code doesn't compile after a merge
- What if someone broke something
- It would be a lof of work to check these things on every merge

The solution is to automate this process.

## How CI works ?
We use a CI server which executes a build that automatically prepares/compiles the code and runs automated tests.

Usually, the CI server automatically detects code changes in source control and run the build whenever there is a change

If something is wrong, the build fails. The team can get immediate feedback if their change broke something. It can take in form of emails.

Merging frequently throughout the day and getting quick feedback means that if you broke something, you broke it with your changes within the last couple hours. This is much easier to identify/fix than if you broke something with your changes from a week ago!

We will be using Jenkins as our CI server.

