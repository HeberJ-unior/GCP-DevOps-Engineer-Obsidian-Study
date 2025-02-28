```embed
title: "Creating and managing Folders | Resource Manager Documentation | Google Cloud"
image: "https://cloud.google.com/_static/cloud/images/social-icon-google-cloud-1200-630.png"
description: "Folders are nodes in the Cloud Platform Resource Hierarchy. A folder can contain projects, other folders, or a combination of both. Organization resources can use folders to group projects under the organization resource node in a hierarchy. For example, your organization resource might contain multiple departments, each with its own set of Google Cloud resources. Folders allow you to group these resources on a per-department basis. Folders are used to group resources that share common IAM policies. While a folder can contain multiple folders or resources, a given folder or resource can have exactly one parent."
url: "https://cloud.google.com/resource-manager/docs/creating-managing-folders"
```
Folders are nodes in the [Cloud Platform Resource Hierarchy](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy). A folder can contain [Projects](Content/Hierarchy+Organization/Projects.md), other folders, or a combination of both. Organization resources can use folders to group [Projects](Content/Hierarchy+Organization/Projects.md) under the organization resource node in a hierarchy. For example, your organization resource might contain multiple departments, each with its own set of Google Cloud resources. Folders allow you to group these resources on a per-department basis. Folders are used to group resources that share common IAM policies. While a folder can contain multiple folders or resources, a given folder or resource can have exactly one parent.

In the diagram below, the organization resource, "Company", has folders representing two departments, "Dept X" and "Dept Y", and a folder, "Shared Infrastructure", for items that might be common to both departments. Under "Dept Y", they have organized into two teams, and within the team folders, they further organize by products. The folder for "Product 1" further contains three [Projects](Content/Hierarchy+Organization/Projects.md), each with the resources needed for the project. This provides them with a high degree of flexibility in assigning IAM policies and Organization policies at the right level of granularity.

![Folders hierarchy example](Content/Attachments/83c2174da70f87656374fafcdbf7a1cc_MD5.svg)

You can use folder-level IAM policies to control access to the resources the folder contains. For example, if a user is granted the **Compute Instance Admin** role on a folder, that user has the **Compute Instance Admin** role for all of the [Projects](Content/Hierarchy+Organization/Projects.md) in the folder.

## Before you begin

Folder functionality is only available to Google Workspace and Cloud Identity customers that have an organization resource. For more information about acquiring an organization resource, see [Creating and managing organizations](https://cloud.google.com/resource-manager/docs/creating-managing-organization#acquiring).

If you're exploring how to best use folders, we recommend that you:

1. Review [Access Control for Folders Using IAM](https://cloud.google.com/resource-manager/docs/access-control-folders). The topic describes how you can control who has what access to folders and the resources they contain.
2. Understand how to set [folder permissions](https://cloud.google.com/resource-manager/docs/creating-managing-folders#folder-permissions). Folders support a number of different IAM roles. If you want to broadly set up permissions so users can see the structure of their [Projects](Content/Hierarchy+Organization/Projects.md), grant the entire domain the **Organization Viewer** and **Folder Viewer** roles at the organization resource level. To restrict visibility to branches of your folder hierarchy, grant the **Folder Viewer** role on the folder or folders you want users to see.
3. [Create folders](https://cloud.google.com/resource-manager/docs/creating-managing-folders#creating-folders). As you plan how to organize your Cloud resources, we recommend that you start with a single folder as a sandbox where you can experiment with which hierarchy makes the most sense for your organization resource. Think of folders in terms of isolation boundaries between resources and attach points for access and configuration policies. You may choose to create folders to contain resources that belong to different departments and assign Admin roles on folders to delegate administrator privilege. Folders can also be used to group resources that belong to applications or different environments, such as development, production, test. Use nested folders to model these different scenarios.

A common situation is to create folders that in turn contain additional folders or [Projects](Content/Hierarchy+Organization/Projects.md), as shown in the image above. This structure is referred to as a _folder hierarchy_. When creating a folder hierarchy, keep in mind the following:

- You can nest folders up to 10 (**ten**) levels deep.
- A parent folder cannot contain more than 300 folders. This refers to direct child folders only. Those child folders can, in turn, contain additional folders or [Projects](Content/Hierarchy+Organization/Projects.md).
- Folder display names must be unique within the same level of the hierarchy.

## Set up permissions to manage folders

To access and manage folders, you assign folder-specific IAM roles to specific groups of users. To learn more about these roles, see [Access Control for Folders using IAM](https://cloud.google.com/resource-manager/docs/access-control-folders). We also recommend you review our [best practices](https://cloud.google.com/resource-manager/docs/access-control-folders#best-practices-folders-iam) to help you identify the optimal configuration for your folder permissions.

To manage folders for your entire organization resource, you need the **Folder Admin** role. This role grants the user permission to create, edit, delete, move, and change IAM permissions on folders, as well as permission to move [Projects](Content/Hierarchy+Organization/Projects.md) between folders.

Initially, only the Organization Admin can assign the Folder Admin role for the organization resource. Subsequent accounts that are assigned this role can grant it to other accounts.

## Creating folders

To create folders, you must have the **Folder Admin** or **Folder Creator** role at the parent level. For example, to create folders at the organization level, you must have one of these roles at the organization level.

As part of creating a folder, you must assign it a name. Folder names must meet the following requirements:

- The name may contain letters, digits, spaces, hyphens and underscores.
- The folder's display name must start and end with a letter or digit.
- The name must be between 3 and 30 characters.
- The name must be distinct from all other folders that share its parent.
### Add tags during folder creation
Tags provide a way to create annotations for resources. You can add tags at the time of creating folders. To do this, you must grant the **Tag User** role. For more information on the permissions contained in this role, see [Manage tags on resources](https://cloud.google.com/resource-manager/docs/tags/tags-creating-and-managing#required-permissions-attach). You can only add the namespace for the tag key-value pairs in one of the following ways:

## Configuring access to folders

To configure access to folders, you must have the **Folder IAM Administrator** or **Folder Admin** role at the parent level.

## Creating a project in a folder

To create a project in a folder, you must have the Project Creator role (`roles/resourcemanager.projectCreator`) on the folder. This role may be inherited from a parent folder.
Don't include sensitive information in your folder name or other resource names. Any reference to the folder or related resources exposes the folder name and resource name.

## Moving a project into a folder

You must carefully consider any policy implications before you move a project into or out of a folder. Identity and Access Management policies that you define at the _project_ level will move with the project, but policies inherited from a parent resource won't move.

When you move a project, any Identity and Access Management policies or organization policies that are directly attached will move with it. However, a project in your [Resource hierarchy](Content/Hierarchy+Organization/Resource%20hierarchy.md) is also affected by the policies that it inherits from parent resources. If a project inherits an IAM role that provides users permission to use a particular service, users will not have access to that service at the destination unless it would inherit the permission at the destination as well.

For example, consider a service account has the **Storage Object Creator** role bound to a user at Folder A. The service account has permissions to upload data to Cloud Storage in any project in Folder A. If you moved one of these [Projects](Content/Hierarchy+Organization/Projects.md) to Folder B, which does not have the same inherited permissions, the service account for that project loses the ability to upload data, resulting in a service outage.

These same considerations apply if organization policies are defined at the source and destination folders. Like IAM policies, organization policies are inherited. Consequently, you must ensure that your organization policies are consistent between source and destination folders.

To learn more about organization policies, see [Introduction to the organization Policy Service](https://cloud.google.com/resource-manager/docs/organization-policy/overview).

To move a project, you need the Project Mover IAM role (`roles/resourcemanager.projectMover`) on both the source folder and the destination folder. If the resource is not in a folder, you need this role on the organization resource.

These roles give you the following required permissions:

- `resourcemanager.[Projects](Content/Hierarchy%20&%20Organization/Projects.md).update` on the project
- **If the resource is in a folder:** `resourcemanager.[Projects](Content/Hierarchy%20&%20Organization/Projects.md).move` on the source folder and the destination
- **If the resource is not in a folder:** `resourcemanager.[Projects](Content/Hierarchy%20&%20Organization/Projects.md).move` on the organization resource

You can also gain these permissions with [custom roles](https://cloud.google.com/iam/docs/creating-custom-roles), or other predefined roles.

## Moving a folder into another folder

To move a folder into another folder, you must have the **resourcemanager.folders.move** permission for both the source and destination folders.

## Viewing or listing folders and projects

To view or list folders, you must have the **Organization Viewer** and **Folder Viewer** roles.

## Using the Google Cloud CLI

Commands for interacting with the Folders API with the `gcloud` command-line tool are available in the `gcloud resource-manager folders` command group.

### Create

To create a new folder, use `gcloud resource-manager folders create` with flags setting the name of the folder and the ID of the organization resource or folder in which you'd like it created.

```
gcloud resource-manager folders create \
  --display-name="Super Fantastic Folder" \
  --organization=2518

Created Folder 245321.
```

### View

To view a folder, use `gcloud resource-manager folders describe` with the ID of the folder you wish to view.

```
gcloud resource-manager folders describe 245321
name: folders/245321
parent: organizations/2518
display_name: Super Fantastic Folder
lifecycle_state: ACTIVE
create_time: <timestamp info …>
update_time: <timestamp info …>
```

### List

To list the folders underneath a folder, use `gcloud resource-manager folders list`, passing the folder ID in the `--folder` flag. This can also list the top-level folders under an organization resource, using the `--organization` flag.

```
gcloud resource-manager folders list --folder 245321
<table output showing the folders underneath the folder with the specified ID>

gcloud resource-manager folders list --organization 2518
<table output showing folders in this Organization but not in any folder>
```

To include folders for which delete is requested in the list, add the flag --show-deleted

```
gcloud beta resource-manager folders list --folder 245321 --show-deleted
<table output showing all the folders including the delete requested ones underneath the folder with the specified ID>
```

**Note:** --show-deleted flag is currently supported only in alpha and beta versions of the command.

You can list [Projects](Content/Hierarchy+Organization/Projects.md) using the `gcloud [Projects](Content/Hierarchy%20&%20Organization/Projects.md) list` command, passing the parent folder or organization resource ID in the `--filter` flag.

```
gcloud projects list --filter=" parent.id: '245321' "
<table output showing the projects underneath the resource with the specified ID>
```

For more information about how permissions and filters interact with list commands, see [Listing all Resources in your Hierarchy](https://cloud.google.com/resource-manager/docs/listing-all-resources).

**Note:** Listing [Projects](Content/Hierarchy+Organization/Projects.md) and folders will return only the direct descendants of the target resource, and does not show the children's children.

### Search

To search for folders matching the specified query, use `gcloud alpha resource-manager folders search`, passing the condition in the `--query` flag. The scope of search is all the folders for which the user has view permission.

```
gcloud alpha resource-manager folders search --query="name:vij*"
<table output showing the folders with names starting from vij eg. vijeta, vijay-folder>

gcloud alpha resource-manager folders search --query="state:DELETE_REQUESTED"
<table output showing folders for which delete has been requested>
```

All folders for which the user has view permission can be shown using the `gcloud folders search` command.

```
gcloud folders search
<table output showing all viewable folders>
```

**Note:** The search command is in alpha and can be changed without notice.

### Update

Folders can be updated with the `gcloud resource-manager folders update` command. Currently, only the `display_name` field of a folder can be updated.

```
gcloud resource-manager folders update \
  --display-name="Mega Incredible Folder" 245321
name: folders/245321
parent: organizations/2518
display_name: Mega Incredible Folder
lifecycle_state: ACTIVE
create_time: <timestamp info …>
update_time: <recent timestamp info …>
```

### Delete

Folders can be deleted and undeleted from the command line. A user must possess either the Folder Admin or the Folder Editor role to have the permission to delete a folder. You can only delete a folder if it is empty.

```
gcloud resource-manager folders delete 245321
name: folders/245321
parent: organizations/2518
display_name: Mega Incredible Folder
lifecycle_state: DELETE_REQUESTED
create_time: <timestamp info …>
update_time: <recent timestamp info …>

gcloud resource-manager folders undelete 245321
name: folders/245321
parent: organizations/2518
display_name: Mega Incredible Folder
lifecycle_state: ACTIVE
create_time: <timestamp info …>
update_time: <recent timestamp info …>
```

### Project Move

[Projects](Content/Hierarchy+Organization/Projects.md) can be created in folders and moved into folders using the existing `gcloud [Projects](Content/Hierarchy%20&%20Organization/Projects.md) create` and `gcloud [Projects](Content/Hierarchy%20&%20Organization/Projects.md) move` commands. Folders can also be moved, using `gcloud resource-manager folders move.`

```
gcloud projects create --folder=245321 fancy-folder-project
project_id: fancy-folder-project
project_number: 905283
parent:
  type: "folder"
  id: 245321
other fields …

gcloud projects move --folder=245321 soon-to-be-fancy-project
project_id: soon-to-be-fancy-project
project_number: 428714
parent:
  type: "folder"
  id: 245321
other fields …
```

### Long-Running operations

Some folder operations such as creating folders can take a long time. To make it easier to multitask, some folder commands allow you to perform them asynchronously. These commands accept an `--async` flag to enable asynchronous behavior, causing them to return a Long-Running Operation immediately instead of waiting for the operation to complete. You can poll this operation yourself with the `gcloud beta resource-manager operations describe` command. Currently, only the `folders create` and `folders move` commands allow asynchronous use.

```
gcloud resource-manager folders create \
  --display-name="Awe-Inspiring Async Folder" \
  --organization=2518 \
  --async

name: operations/fc.8572
metadata:
  operation_type: CREATE
  display_name: Awe-Inspiring Async Folder
  destination_parent: organizations/2518
done: false

[wait for some time …]

gcloud beta resource-manager operations describe fc.8572
name: operations/fc.8572
metadata:
  operation_type: CREATE
  display_name: Awe-Inspiring Async Folder
  destination_parent: organizations/2518
done: true
response:
  name: folders/6428
  parent: organizations/2518
  display_name: Awe-Inspiring Async Folder
  lifecycle_state: ACTIVE
  create_time: <recent timestamp info …>
  update_time: <recent timestamp info …>
```