# Reusable workflows 

This repository owns all workflows used by the template. 

The "template" (aka workflows you need to copy) are located in the `template` folder.

## Deploy

### Netlify

You need to define NETLIFY_HOOK in your repository secrets.

See [documentation](https://obsidian-publisher.netlify.app/advanced%20setup/advanced%20workflow/#netlify) on how to add it.

### Vercel

You need to define:
- VERCEL_ORG_ID
- VERCEL_PROJECT_ID
- VERCEL_TOKEN

See [documentation](https://obsidian-publisher.netlify.app/advanced%20setup/advanced%20workflow/#vercel) on how to add it.

### Github Pages

You don't need to define anything.

### Index

See [documentation](https://obsidian-publisher.netlify.app/mkdocs%20template/workflow/).

### Update 

This action allows to update the last version of the [template](https://github.com/ObsidianPublisher/sync_template). 

The action needs a `GH_TOKEN` secret in your repository settings. This token should have the `repo` and `workflows` scope. You can create a token [here](https://github.com/settings/tokens/new?description=PUBLISHER%20TEMPLATE&scopes=repo,workflow).

[See here to know how to register it as a secret](https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository).

This action will update your template by replacing the old files with the new ones. After the upgrade, the action will create a pull request in your repository, which you can review and either accept or reject.

This allows you to review the changes before they are applied to your template.
The `AUTO_MERGE` key in `.github/.env` can be set to `true` to automatically merge the pull request.

