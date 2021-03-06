# Set service account access rights

This section describes how to assign a [role](../../concepts/access-control/roles.md ) for a [ service account](../../concepts/users/service-accounts.md) as a resource. To assign the service account a role for another resource, follow the instructions [[!TITLE]](assign-role-for-sa.md).

You can't set service account access rights via the management console. You can [assign a role for a folder](../../../resource-manager/operations/folder/set-access-bindings.md) hosting the service account.

## How to assign a role for a service account

---

**[!TAB CLI]**

[!INCLUDE [default-catalogue](../../../_includes/default-catalogue.md)]

1. See the description of the command to assign a role for a service account as a resource:

    ```
    $ yc iam service-account add-access-binding --help
    ```

2. Select a service account (for example, `my-robot`):

    ```
    $ yc iam service-account list
    +----------------------+----------+------------------+
    |          ID          |   NAME   |   DESCRIPTION    |
    +----------------------+----------+------------------+
    | ajebqtreob2dpblin8pe | test-sa  | test-description |
    | aje6o61dvog2h6g9a33s | my-robot |                  |
    +----------------------+----------+------------------+
    ```

3. Choose a [role](../../concepts/access-control/roles.md):

    ```
    $ yc iam role list
    +--------------------------------+-------------+
    |               ID               | DESCRIPTION |
    +--------------------------------+-------------+
    | admin                          |             |
    | compute.images.user            |             |
    | editor                         |             |
    | ...                            |             |
    +--------------------------------+-------------+
    ```

4. Find out the user's ID from the login or email address. To assign a role to a service account or group of users, see the [examples](#examples) below.

    ```
    $ yc iam user-account get test-user
    id: gfei8n54hmfhuk5nogse
    yandex_passport_user_account:
        login: test-user
        default_email: test-user@yandex.ru
    ```

5. Assign a user named `test-user` the `editor` role for the `my-robot` service account. In the subject, specify the `userAccount` type and user ID:

    ```
    $ yc iam service-account add-access-binding my-robot \
        --role editor \
        --subject userAccount:gfei8n54hmfhuk5nogse
    ```

**[!TAB API]**

Use the [updateAccessBindings](../../api-ref/ServiceAccount/updateAccessBindings.md) method for the [ServiceAccount](../../api-ref/ServiceAccount/index.md) resource. You will need the service account ID and the ID of the user who is assigned the role for the service account.

1. Find out the service account ID using the [list](../../api-ref/ServiceAccount/list.md) method:

    ```bash
    $ curl -H "Authorization: Bearer <IAM-TOKEN>" \
        https://iam.api.cloud.yandex.net/iam/v1/serviceAccounts?folderId=b1gvmob03goohplct641

    {
     "serviceAccounts": [
      {
       "id": "aje6o61dvog2h6g9a33s",
       "folderId": "b1gvmob03goohplct641",
       "createdAt": "2018-10-19T13:26:29Z",
       "name": "my-robot"
      }
      ...
     ]
    }
    ```

2. Find out the user ID from the login using the [getByLogin](../../api-ref/YandexPassportUserAccount/getByLogin.md) method:

    ```bash
    $ curl -H "Authorization: Bearer <IAM-TOKEN>" \
        https://iam.api.cloud.yandex.net/iam/v1/yandexPassportUserAccounts:byLogin?login=test-user

    {
     "id": "gfei8n54hmfhuk5nogse",
     "yandexPassportUserAccount": {
      "login": "test-user",
      "defaultEmail": "test-user@yandex.ru"
     }
    }
    ```

3. Assign the user the `editor` role for the `my-robot` service account. Set the `action` property to `ADD` and specify the `userAccount` type and user ID in the `subject` property:

    ```bash
    $ curl -X POST \
        -H 'Content-Type: application/json' \
        -H "Authorization: Bearer <IAM-TOKEN>" \
        -d '{
        "accessBindingDeltas": [{
            "action": "ADD",
            "accessBinding": {
                "roleId": "editor",
                "subject": {
                    "id": "gfei8n54hmfhuk5nogse",
                    "type": "userAccount"
        }}}]}' \
        https://iam.api.cloud.yandex.net/iam/v1/serviceAccounts/aje6o61dvog2h6g9a33s:updateAccessBindings
    ```

---

## Examples {#examples}

* [[!TITLE]](#multiple-roles)
* [[!TITLE]](#access-to-sa)
* [[!TITLE]](#access-to-all)

### Assign multiple roles {#multiple-roles}

---

**[!TAB CLI]**

The `add-access-binding` command allows you to add only one role. You can assign multiple roles using the `set-access-binding` command.

> [!WARNING]
>
> The `set-access-binding` command completely rewrites the access rights to the resource. All current resource roles will be deleted.

1. Make sure the resource doesn't have any roles that you don't want to lose:

    ```
    $ yc iam service-account list-access-binding my-robot
    ```
2. For example, assign a role to multiple users:

    ```
    $ yc iam service-account set-access-bindings my-robot \
        --access-binding role=editor,subject=userAccount:gfei8n54hmfhuk5nogse
        --access-binding role=viewer,subject=userAccount:helj89sfj80aj24nugsz
    ```

**[!TAB API]**

Assign the `editor` role to one user and the `viewer` role to another user:

```bash
$ curl -X POST \
    -H 'Content-Type: application/json' \
    -H "Authorization: Bearer <IAM-TOKEN>" \
    -d '{
    "accessBindingDeltas": [{
        "action": "ADD",
        "accessBinding": {
            "roleId": "editor",
            "subject": {
                "id": "gfei8n54hmfhuk5nogse",
                "type": "userAccount"
            }
        }
    },{
        "action": "ADD",
        "accessBinding": {
            "roleId": "viewer",
            "subject": {
                "id": "helj89sfj80aj24nugsz",
                "type": "userAccount"
    }}}]}' \
    https://iam.api.cloud.yandex.net/iam/v1/serviceAccounts/aje6o61dvog2h6g9a33s:updateAccessBindings
```

You can also assign roles using the [setAccessBindings](../../api-ref/ServiceAccount/setAccessBindings.md) method.

> [!WARNING]
>
> The `setAccessBindings` method completely rewrites the access rights to the resource. All current resource roles will be deleted.

```bash
curl -X POST \
    -H 'Content-Type: application/json' \
    -H "Authorization: Bearer <IAM-TOKEN>" \
    -d '{
    "accessBindings": [{
        "roleId": "editor",
        "subject": { "id": "ajei8n54hmfhuk5nog0g", "type": "userAccount" }
    },{
        "roleId": "viewer",
        "subject": { "id": "helj89sfj80aj24nugsz", "type": "userAccount" }
    }]}' \
    https://iam.api.cloud.yandex.net/iam/v1/serviceAccounts/aje6o61dvog2h6g9a33s:setAccessBindings
```

---

### Service account access to another service account {#access-to-sa}

Allow the `test-sa` service account to manage the `my-robot` service account:

---

**[!TAB CLI]**

1. Find out the ID of the `test-sa` service account that you want to assign the role to. To do this, get a list of available service accounts:

    ```
    $ yc iam service-account list
    +----------------------+----------+------------------+
    |          ID          |   NAME   |   DESCRIPTION    |
    +----------------------+----------+------------------+
    | ajebqtreob2dpblin8pe | test-sa  | test-description |
    | aje6o61dvog2h6g9a33s | my-robot |                  |
    +----------------------+----------+------------------+
    ```

2. Assign the `editor` role to the `test-sa` service account by specifying its ID. In the subject type, specify `serviceAccount`:

    ```
    $ yc iam service-account add-access-binding my-robot \
        --role editor \
        --subject serviceAccount:ajebqtreob2dpblin8pe
    ```

**[!TAB API]**

1. Find out the ID of the `test-sa` service account that you want to assign the role to. To do this, get a list of available service accounts:

    ```bash
    $ curl -H "Authorization: Bearer <IAM-TOKEN>" \
        https://iam.api.cloud.yandex.net/iam/v1/serviceAccounts?folderId=b1gvmob03goohplct641

    {
     "serviceAccounts": [
      {
       "id": "ajebqtreob2dpblin8pe",
       "folderId": "b1gvmob03goohplct641",
       "createdAt": "2018-10-18T13:42:40Z",
       "name": "test-sa",
       "description": "test-description"
      },
      {
       "id": "aje6o61dvog2h6g9a33s",
       "folderId": "b1gvmob03goohplct641",
       "createdAt": "2018-10-15T18:01:25Z",
       "name": "my-robot"
      }
     ]
    }
    ```

2. Assign the `test-sa` service account the `editor` role for another `my-robot` service account. In the `subject` property, specify the `serviceAccount` type and the `test-sa` ID. In the request URL, specify the `my-robot` ID as a resource:

```bash
$ curl -X POST \
    -H 'Content-Type: application/json' \
    -H "Authorization: Bearer <IAM-TOKEN>" \
    -d '{
    "accessBindingDeltas": [{
        "action": "ADD",
        "accessBinding": {
            "roleId": "editor",
            "subject": {
                "id": "ajebqtreob2dpblin8pe",
                "type": "serviceAccount"
    }}}]}' \
    https://iam.api.cloud.yandex.net/iam/v1/serviceAccounts/aje6o61dvog2h6g9a33s:updateAccessBindings
```

---

### Access to a resource for all users {#access-to-all}

You can grant access to a resource to all Yandex.Cloud users. To do this, assign a role to the [system group](../../concepts/access-control/system-group.md) `allAuthenticatedUsers`.

Allow any authenticated user to view information about the `my-robot` service account:

---

**[!TAB CLI]**

Assign the `viewer` role to the `allAuthenticatedUsers` system group. In the subject type, specify `system`:

```
$ yc iam service-account add-access-binding my-robot \
    --role viewer \
    --subject system:allAuthenticatedUsers
```

**[!TAB API]**

Assign the `viewer` role to the `allAuthenticatedUsers` system group. In the `subject` property, specify the `system` type:

```bash
$ curl -X POST \
    -H 'Content-Type: application/json' \
    -H "Authorization: Bearer <IAM-TOKEN>" \
    -d '{
    "accessBindingDeltas": [{
        "action": "ADD",
        "accessBinding": {
            "roleId": "viewer",
            "subject": {
                "id": "allAuthenticatedUsers",
                "type": "system"
    }}}]}' \
    https://iam.api.cloud.yandex.net/iam/v1/serviceAccounts/aje6o61dvog2h6g9a33s:updateAccessBindings
```

---

