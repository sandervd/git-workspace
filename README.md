git-workspace
=============

Git workspace creates a workspace outside your repository for each branch in project, following the git principle of branching.

It works as follows:
- When you create a new branch, the contents of your current workspace is copied in the newly created workspace.
- If you switch between existing branches, your active workspace is switched as well.

Install
-------
- Copy the contents of the git_hooks folder into your.git/hooks folder.

NOTE: Your workspace will be created in your homedirectory, in the hidden directory .gitworkspace.

Git workspace and Drupal
------------------------

Git workspace tries so solve the problem of databases not corresponding to the current branch.

If you put a sqllite database in your workspace, and make your database connection aware of git workspace, you'll be able to 'branch' your database!

A short example of a Drupal settings file using git workspace.

```php
$active_workspace = get_active_workspace();
$databases['default']['default'] = array(
  'driver' => 'sqlite',
  'database' => $active_workspace . '/db.sqlite',
);

$conf['file_public_path'] = $active_workspace . '/files';

/**
 * Get path to active workspace.
 */
function get_active_workspace() {
  $project_name = trim(shell_exec('git rev-parse --show-toplevel'));
  $project_name = str_replace('/', '-', $project_name);
  $project_name = trim($project_name, '-');

  $workspace_base_path = trim(shell_exec('git config workspace.basedir'));
  $active_workspace = trim(shell_exec('git config workspace.activeworkspace'));
  return $workspace_base_path . '/' . $project_name . '/' . $active_workspace;
}
```
