---
title: Great power -> great responsibility -> almost troubles -> simple git hook
date: 2026-02-25 10:00:00 +0100
categories: [Blogging]
tags: [git, developerexperience, devex, devexp, programming, workflow]
---

With great power comes great responsibility - as they say.

## The accident

On monday, I pushed code directly to `main` without creating a feature branch. This could have resulted in an unintentional deployment to staging. Fortunately, it was only an update to `.gitignore`! ;)

## The context

In my current project, developers with the **"maintainer"** role can push directly to `main`. This is new to me, but intentional - the team trusts its maintainers. While that trust is great, I've developed a few habits that, while usually harmless, led to this *accident*:

* **Diving straight into the code:** When I'm focused on solving a problem, I sometimes forget to create a feature branch immediately. This means I'm working without some "safe" guardrails from the start.
* **The "Commit and Push" reflex:** Pushing code immediately after a commit is generally a good habit, but in this specific environment, it can lead to trouble.

The sudden freedom to push to `main` caught me off guard. I was used to restricted permissions where `git push` would simply return an error if I tried to hit the primary branch.

## The solution

The fix for the future is simple: a **pre-push Git hook**. It requires setting a specific environment variable before allowing a push to `main`.

Nothing fancy—just a basic check—but it's enough to prevent unintended pushes without forcing me to radically change my workflow. Most importantly, it still allows me to bypass the hook if a direct push is actually necessary.


I have it at `~/.git/hooks/pre-push` and it looks like this:

```bash
#!/bin/sh
protected_branches="main master"

# Read the refs from stdin
while read local_ref local_sha remote_ref remote_sha; do
    # Extract the branch name from the remote ref
    branch=$(git rev-parse --symbolic --abbrev-ref "$remote_ref")
    for protected in $protected_branches; do
        # Check if the branch is protected and if the environment variable is not set
        if [ "$branch" = "$protected" ] && [ "$PUSH_TO_PROTECTED" != "1" ]; then
        echo "Set PUSH_TO_PROTECTED=1 to push to $protected."
        # Exit with a non-zero status to prevent the push
        exit 1
        fi
    done
done
exit 0
```

Here you can see it in action:

<iframe width="560" height="315" src="https://www.youtube.com/embed/bZtFwOUnPiE" frameborder="0" allowfullscreen></iframe>
