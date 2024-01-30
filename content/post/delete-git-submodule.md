---
title: "How To Delete a Git Submodule"
description: "Quick reference of how to delete a submodule from Git."
date: '2023-11-10'
---

To delete a submodule from git, do the following:

1. Delete the relevant section from the `.gitmodules` file.
2. Delete the relevant section from the `.git/config` file.
3. Run `git rm --cached <path_to_submodule>`.
4. Run `git rm --cached <path_to_submodule>`.
5. Run `rm -rf .git/modules/<path_to_submodule>`.
6. Commit the changes.