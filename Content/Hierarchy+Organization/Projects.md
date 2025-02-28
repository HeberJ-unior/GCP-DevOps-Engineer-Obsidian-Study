```embed
title: "Creating and managing projects | Resource Manager Documentation | Google Cloud"
image: "https://cloud.google.com/_static/cloud/images/social-icon-google-cloud-1200-630.png"
description: "Google Cloud projects form the basis for creating, enabling, and using all Google Cloud services including managing APIs, enabling billing, adding and removing collaborators, and managing permissions for Google Cloud resources."
url: "https://cloud.google.com/resource-manager/docs/creating-managing-projects"
```
Google Cloud projects form the basis for creating, enabling, and using all Google Cloud services including managing APIs, enabling billing, adding and removing collaborators, and managing permissions for Google Cloud resources.

This page explains how to create and manage Google Cloud projects using the Cloud Resource Manager API and the Google Cloud console.

## Before you begin

Read about the project resource in the [Resource hierarchy overview](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy#projects). For guidance on setting up your resource hierarchy, see [Decide a resource hierarchy for your Google Cloud landing zone](https://cloud.google.com/architecture/landing-zones/decide-resource-hierarchy).

The following are used to identify your project:

- **Project name**: A human-readable name for your project.
    
    The project name isn't used by any Google APIs. You can edit the project name at any time during or after project creation. Project names do not need to be unique.
    
- **Project ID**: A globally unique identifier for your project.
    
    A project ID is a unique string used to differentiate your project from all others in Google Cloud. After you enter a project name, the Google Cloud console generates a unique project ID that can be a combination of letters, numbers, and hyphens. We recommend you use the generated project ID, but you can edit it during project creation. After the project has been created, the project ID is permanent.
    
    A project ID has the following requirements:
    
    - Must be 6 to 30 characters in length.
    - Can only contain lowercase letters, numbers, and hyphens.
    - Must start with a letter.
    - Cannot end with a hyphen.
    - Cannot be in use or previously used; this includes deleted projects.
    - Cannot contain restricted strings such as `google` and `ssl`. We recommend not using strings `undefined` and `null` in a project ID.
- **Project number**: An automatically generated unique identifier for your project.
    

Don't include sensitive information such as personally identifiable information (PII) or security data in your project name, project ID, or other resource names. The project ID is used in the name of many other Google Cloud resources, and any reference to the project or related resources exposes the project ID and resource name.

## Create a project

To create a project, you must have the `resourcemanager.projects.create` permission. This permission is included in roles like the Project Creator role (`roles/resourcemanager.projectCreator`).

The Project Creator role is granted by default to the entire domain of a new organization resource and to free trial users.

For information on how to grant individuals the role and limit organization-resource wide access, see the [Managing Default Organization Roles](https://cloud.google.com/resource-manager/docs/default-access-control) page.

If you don't specify the parent resource, a parent resource is selected automatically if applicable based on the user account's domain.

You can create a new project using the Google Cloud console, the Google Cloud CLI, or the [`projects.create()`](https://cloud.google.com/resource-manager/reference/rest/v3/projects/create) method.
### Add tags during project creation
Tags provide a way to create annotations for resources. You can add tags at the time of creating projects. You must assign the **Tag User** role while adding tags. For more information on the permissions assigned to this role, see [Manage tags on resources](https://cloud.google.com/resource-manager/docs/tags/tags-creating-and-managing#required-permissions-attach).

### Creating a project using a service account

You can use a service account to automate project creation. Like user accounts, service accounts can be granted permission to create projects within an organization resource. Service accounts are not allowed to create projects outside of an organization resource and must specify the parent resource when creating a project. Service accounts can create a new project using the gcloud CLI or the `projects.create()` method.

## Managing project quotas

If you have fewer than 30 projects remaining in your quota, a notification displays the number of projects remaining in your quota on the [**New Project**](https://console.cloud.google.com/projectcreate) page. After you have reached your project limit, to create more projects you must request a project limit increase. Alternatively, you can schedule some projects to be deleted after 30 days on the [Manage Resources page](https://console.cloud.google.com/cloud-resource-manager). Projects that users have [soft-deleted](https://cloud.google.com/resource-manager/docs/creating-managing-projects#shutting_down_projects) count against your quota. These projects are fully deleted after 30 days.

You receive an email acknowledging receipt of your request. If you need further assistance, respond to the email. After the review, you receive an email notification indicating whether your request was approved.

If you don't have an organization and want to request additional capacity for projects in your quota, then use the [Request Project Quota Increase](https://support.google.com/code/contact/project_quota_increase) form.

For more information about quotas and why they are used, see the [Free Trial Project Quota Requests](https://support.google.com/cloud/answer/6330231) support page. For more information about billing reports, see the [Billing Reports](https://cloud.google.com/billing/docs/how-to/reports) support page.

## Get an existing project

You can get an existing project using the Google Cloud CLI or the [`projects.get()`](https://cloud.google.com/resource-manager/reference/rest/v3/projects/get) method.

If you are not a project owner, you must have the permissions included in the Browser role (`roles/browser`).

## List all projects under a resource

To list all projects that are direct children of a resource, use the v3 [`projects.list`](https://cloud.google.com/resource-manager/reference/rest/v3/projects/list) method, with the parent resource specified in the query:

Request:

```
GET https://cloudresourcemanager.googleapis.com/v3/projects

{
    "parent": "folders/662951040570"
}
```

Response:

```
{
    "projects": [
    {
        "name": "projects/951040570662",
        "parent": "folders/662951040570",
        "projectId": "tokyo-rain-123",
        "state": "ACTIVE",
        "displayName": "Tokyo Rain"
        "createTime": "2013-11-13T20:31:53.308Z"
        "updateTime": "2013-11-13T20:31:53.308Z"
        "etag": "BwWUlZ6XEfY="
    }
    ]
}
```

## Search for projects

To search for projects matching the specified query, use `gcloud alpha resource-manager projects search`, passing the query in the `--query` flag. The scope of search is all the projects for which the user has `projects.get` permission.

If you specify the `parent.type` and `parent.id` fields in your request body, then the `resourcemanager.projects.list` permission is checked on the parent. If the user has this permission, all projects under the parent are returned after the remaining filters have been applied.

If the user lacks this permission, then all projects for which the user has the `resourcemanager.projects.get` permission are returned after remaining filters have been applied.

If no filter is specified, the call returns projects for which the user has `resourcemanager.projects.get` permissions.

## Updating projects

You can update projects using the Google Cloud console or the [`projects.patch()`](https://cloud.google.com/resource-manager/reference/rest/v3/projects/patch) method.

The only fields that can be updated are the project name and labels. For more information about updating projects, see the [project API reference page](https://cloud.google.com/resource-manager/reference/rest/v3/projects).

To move a project within your [Resource hierarchy](Content/Hierarchy+Organization/Resource%20hierarchy.md), see [Moving a project](https://cloud.google.com/resource-manager/docs/moving-projects-folders). To migrate a project from one organization resource to another, see [Migrating projects](https://cloud.google.com/resource-manager/docs/project-migration).

## Shutting down (deleting) projects

You can shut down projects using the Google Cloud console or the [`projects.delete`](https://cloud.google.com/resource-manager/reference/rest/v3/projects/delete) method in the API. A project must have a lifecycle state of `ACTIVE` to be shut down in this way.

This method immediately marks a project to be deleted. A notification email is sent to the user who initiated the delete operation and the Technical category contacts that are listed in [Essential Contacts](https://cloud.google.com/resource-manager/docs/managing-notification-contacts) on a best effort basis; if the notification fails to send, the project is still marked to be deleted. If there's no contact in the Technical category, the fallback contact isn't notified.

A project that is marked for deletion isn't usable. If the project has a billing account associated with it, that association is broken and isn't reinstated if the project delete operation is canceled. After 30 days, the project is fully deleted. Until it is fully deleted, the project might still be visible, although it isn't usable.

To stop the project delete process during the 30-day period, see the [steps to restore a project](https://cloud.google.com/resource-manager/docs/creating-managing-projects#restoring_a_project).

At the end of the 30-day period, the project and all of its resources are deleted and cannot be recovered. Until it is deleted, the project counts towards your project quota.

To help ensure that you don't delete any important projects, you can enable [change risk recommendations](https://cloud.google.com/recommender/docs/change-risk-recommendations). Change risk recommendations generate warnings when you try to delete projects that Google Cloud has identified as important.

If you have set up billing for a project, it might not be completely deleted until the current billing cycle ends and your account is successfully charged. The number and types of services in use can also affect when the system permanently deletes a project. To learn more about data retention and safe deletion, see [How Google retains data we collect](https://policies.google.com/technologies/retention).

## Restore a project

Project owners can restore a deleted project within the 30-day recovery period that starts when the project is shut down. Restoring a project returns it to the state it was in before it was shut down, with certain exceptions:

- Billing is disabled on the project when the [project is shut down](https://cloud.google.com/resource-manager/docs/creating-managing-projects#shutting_down_projects) and billing is **_not_** automatically enabled on restored projects. The [Cloud Billing account must be manually linked](https://cloud.google.com/billing/docs/how-to/modify-project#enable_billing_for_an_existing_project) again after the project is restored. You might need to wait a few hours before you can successfully link a recently restored project to a billing account.
- You can recover most resources if you restore a project within the 30-day period.
- Some services have delays in restoring and you might need to wait some time (up to 36 hours) for services to be restored.
- Some resources, such as Cloud Storage or Pub/Sub resources, are deleted much sooner. These resources might not be fully recoverable even if you restore the project within the 30-day period.
- Some services might need to be restarted manually. For more information, see [Restarting Google Cloud Services](https://cloud.google.com/billing/docs/how-to/restart-services).

You must have the `resourcemanager.projects.undelete` permission on the project you want to restore.
