Personal resumé site

## Pull request previews

When a pull request is opened or updated, GitHub Actions builds the site and publishes a preview straight to the main Pages branch so it is served alongside the live site. Each preview is available at:

```
https://nmcollins.com/pr-previews/pr-<PR_NUMBER>/
```

If you prefer the GitHub domain, the same content is mirrored at `https://thespaniard514.github.io/pr-previews/pr-<PR_NUMBER>/`.

The workflow also posts these links as a comment on the pull request, so you can click through directly from the PR conversation. When the pull request is closed, the preview folder is removed from `main` and the comment is updated to note that the preview is gone.
