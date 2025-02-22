---
title: 'Cloud storages page'
linkTitle: 'Cloud storages page'
weight: 20
description: 'Overview of the cloud storages page.'
---

![](/images/image227.jpg)

The cloud storages page contains elements, relating to a separate cloud storage. 
Each element contains: 
* preview;
* cloud storage name; 
* provider;
* created by;
* last updated;
* status;
* **?** button — displays the description and the actions menu.

Buttons in the action menu:
- **Update** — update this cloud storage;
- **Delete** — delete the cloud storage.

When it is impossible to get a real preview (e.g. storage is empty or invalid credentials were used), there is a preview icon:
![](/images/cloud_storage_icon.jpg)

Use a search bar in the left corner to find a cloud storage by:
* display name;
* provider;
* _another option_;
* _the last option_.

In the upper right corner there are:
* [Sort by][sorting];
* [Quick filters][quick-filters];
* **Filter**.

## Filter

> Using **Filter** disables the **Quick filter**.

You can create rules from:
* [properties](#supported-properties-for-jobs-list);
* [operators][operators];
*  values and group rules into [groups][groups].
For more details, see the [filter section][create-filter].
Learn more about [date and time selection][data-and-time].

To clear all filters press **Clear filters**.

### Supported properties for cloud storages list

| Properties     | Supported values                             | Description                                 |
| -------------- | -------------------------------------------- | ------------------------------------------- |
| `ID`           | number or range of task ID                   |                                             |
| `Provider type` | `AWS S3`, `Azure`, `Google cloud`           |                                             |
| `Credentials type` | `Key & secret key`, `Account name and token`,<br> `Anonymous access`, `Key file` |     |
| `Resource name` |                                             | `Bucket name` or `container name`           |
| `Display name` |                                              | Set when creating cloud storage             |
| `Description`  |                                              | Description of the cloud storage            |
| `Owner`        | username                                     | The user who owns the project, task, or job |
| `Last updated` | last modified date and time (or value range) | The date can be entered in the `dd.MM.yyyy HH:mm` format <br>or by selecting the date in the window that appears <br>when you click on the input field |

Click **+** to [attach a new cloud storage](/docs/manual/basics/attach-cloud-storage/).

[create-filter]: /docs/manual/advanced/filter/#create-a-filter
[operators]: /docs/manual/advanced/filter/#supported-operators-for-properties
[groups]: /docs/manual/advanced/filter/#groups
[data-and-time]: /docs/manual/advanced/filter#date-and-time-selection
[sorting]: /docs/manual/advanced/filter/#sort-by
[quick-filters]: /docs/manual/advanced/filter/#quick-filters
