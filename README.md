# Reusable workflows 

This repository owns all workflows used by the template. 

The "template" (aka workflows you need to copy) are located in the `template` folder.

All workflows needs a `.github/.env` file to work. The file should look like this:

```sh
WORKFLOW_TYPE='netlify'
# WORKFLOW_TYPE can be "netlify", "vercel" or "gh-pages"
# Depending to the website you use to publish your site
# It is used to update their requirements & deploy the site
CLEAN=true
AUTO_MERGE=false
DRY_RUN=false
```

You can see all available options in the [template](./template) folder.

> **Warning**  
> Don't download the `.github/workflows` folder. You need to use the file in the `template` folder.

# Deploy

See [documentation](https://obsidian-publisher.netlify.app/advanced%20setup/advanced%20workflow/) for more information.

| Key                 | Context | Type   | Required | Description                                       | Default                                        |
|---------------------|---------|--------|----------|---------------------------------------------------|------------------------------------------------|
| `GH_PAT`            | secrets | string | `true`   | Github Personal Access Token                      |                                                |
| `VERCEL_TOKEN`      | secrets | string | `false`  | Vercel Token (only in vercel workflows)           |                                                |
| `VERCEL_ORG_ID`     | secrets | string | `false`  | Vercel Organization ID (only in vercel workflows) |                                                |
| `VERCEL_PROJECT_ID` | secrets | string | `false`  | Vercel Project ID (only in vercel)                |                                                |
| `NETLIFY_HOOK`      | secrets | string | `false`  | Netlify Hook (only in netlify workflows)          |                                                |
| `author_name`       | inputs  | string | `false`  | Author name for signed commit                     | `github-actions[bot]`                          |
| `author_email`      | inputs  | string | `false`  | Author email for signed commit                    | `github-actions[bot]@users.noreply.github.com` |


## Netlify

You need to define `NETLIFY_HOOK` in your repository secrets.

See [documentation](https://obsidian-publisher.netlify.app/advanced%20setup/advanced%20workflow/#netlify) on how to add it.

## Vercel

You need to define:
- `VERCEL_ORG_ID`
- `VERCEL_PROJECT_ID`
- `VERCEL_TOKEN`

See [documentation](https://obsidian-publisher.netlify.app/advanced%20setup/advanced%20workflow/#vercel) on how to add it.

## Github Pages

You don't need to define anything.

# Maintenance

## Clean-up & bumps requirements (`maintenance.yml`)

This workflows will, every 24 hours or on demand:
- Update the requirements and the cache
- Clean unused image if `env.CLEAN` is set to `true` in `.github/.env`
- You can also set `env.DRY_RUN` to `true` to only print the command that will be executed

| Key            | Context | Type   | Required | Description                    | Default                                        |
|----------------|---------|--------|----------|--------------------------------|------------------------------------------------|
| `GH_PAT`       | secrets | string | `true`   | Github Personal Access Token   |                                                |
| `author_name`  | inputs  | string | `false`  | Author name for signed commit  | `github-actions[bot]`                          |
| `author_email` | inputs  | string | `false`  | Author email for signed commit | `github-actions[bot]@users.noreply.github.com` |


## Index (`index.yml`)

It Allows you to quickly create a new "category" / blog listing in your repository by creating a new specified folder with the name of the category. To create a new category, follow these steps:
 1. Go to the "Actions" tab
 2. Click on "Index Creation"
 3. Click on "Run Workflow"
 4. Fill out the form:

 - **Folder name** : The name of the folder you want to create, it will be the "new category".
 - **Parent folder** : The _optional_ path of the folder you want to create the new category in. For example, `main_category/draft` will create the `docs/main_category/draft/folder_name` older.
 - **Description** : An _optional_ category description.
 - You can also:
     - **Hide the table of contents** in the index file.
     - **Hide the navigation panel** in the index file.
     - Perform a **dry-run**: It will only show the result of the operation, but will not create the folder and the index file.

> **Warning**  
> The workflows won't activate the other actions, so you need to run them manually if you need.

 Key             | Context | Type    | Required | Description                                          | Default                                        
-----------------|---------|---------|----------|------------------------------------------------------|------------------------------------------------
 `GH_PAT`        | secrets | string  | `true`   | Github Personal Access Token                         |                                                
 `author_name`   | inputs  | string  | `false`  | Author name for signed commit                        | `github-actions[bot]`                          
 `author_email`  | inputs  | string  | `false`  | Author email for signed commit                       | `github-actions[bot]@users.noreply.github.com` 
 `category_name` | inputs  | string  | `true`   | The new folder name                                  |                                                
 `path`          | inputs  | string  | `false`  | The path of the new folder. Ex: category/subcategory |                                                
 `description`   | inputs  | string  | `false`  | The description of the new folder                    |                                                
 `toc_hide`      | inputs  | boolean | `false`  | Hide the toc in the index file.                      |                                                
 `nav_hide`      | inputs  | boolean | `false`  | Hide the nav in the index file.                      |                                                
 `dry_run`       | inputs  | boolean | `false`  | Do not create new category index, just log.          |                                                

## Update (`update.yml`)

This action allows to update the last version of the [template](https://github.com/ObsidianPublisher/sync_template). 

The action needs a `GH_TOKEN` secret in your repository settings. This token should have the `repo` and `workflows` scope. You can create a token [here](https://github.com/settings/tokens/new?description=PUBLISHER%20TEMPLATE&scopes=repo,workflow).

[See here to know how to register it as a secret](https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository).

This action will update your template by replacing the old files with the new ones. After the upgrade, the action will create a pull request in your repository, which you can review and either accept or reject.

This allows you to review the changes before they are applied to your template.
The `AUTO_MERGE` key in `.github/.env` can be set to `true` to automatically merge the pull request.

 Key             | Context | Type    | Required | Description                                          | Default                                        
-----------------|---------|---------|----------|------------------------------------------------------|------------------------------------------------
 `GH_PAT`        | secrets | string  | `true`   | Github Personal Access Token                         |                                                

# Tips
## Conditional workflows

You can also create workflow conditional runs with the `if` keyword. For example, you can target merging events by prepending them with `[PUBLISHER]` and create workflows based on pull requests.

Using the if keyword Target the merging event name with: `if: startsWith(github.event.head_commit.message, '[PUBLISHER]')`. You can use the keyword `if` for steps or entire jobs.

## Workflow run time & limits

If you use workflows (every workflow) on a **private** repository, you should be aware that you have a limited amount of workflow run time, 2000 minutes (3000 minutes with a pro account).

