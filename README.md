git-as-npm
===

A quick and dirty (for now) script that gets git to behave a little more like NPM at times when we'd like it to. `publish` is the only command that currently works.

Why?
---

NPM is great for managing JS modules. However, private modules aren't free, and maintaining your own registry isn't, either. Thankfully, you can install NPM dependencies directly from Git, like so:

    "blah-lib": "git+ssh://git@github.com/blah/blah-lib.git"

You can also install specific tags or commits, by appending `#` on the end, like so:

    git+ssh://git@github.com/blah/blah-lib.git#0.0.3

But the process by which you tag these releases is entirely manual.

Making it not manual
---

So what does this script do?

 1. Checks out a branch called `release` (or creates it if it doesn't exist).
 2. Merges your current branch into `release`.
 3. Pulls down a remote copy of the `release` branch, so sync up which version numbers have already been released.
 4. Runs `npm prepublish`, if that script exists.
 5. Creates a tag with your current `package.json` version number, throwing an error if that release already exists.
 6. Adds a new remote, as specified in the `repository` field of package.json
 7. Pushes this up to git
 8. Tidies up after itself, deleting the remote and returning you to your original branch.

Ta-da
---
It doesn't handle everything - you still need to specify the version when doing an `npm install` and so on, but hopefully it makes things a little easier.
