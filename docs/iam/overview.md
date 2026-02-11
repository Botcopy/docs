## IAM Overview

The IAM application is used to manage user roles, delete users, and invite new users to an organization. The IAM application can be found [here](https://iam.botcopy.com).

### Roles

| **Role Name**     | **Description** |
| ----------------- | --------------------------------------------------------------------------------------------------------------- |
| `PORTAL_ADMIN`    | Portal administrator role, full access to all portal features.                                                  |
| `PORTAL_DEV`      | Portal developer role, access to most portal features with some exceptions.                                     |
| `PORTAL_MARKETER` | Portal marketer role, access to some portal features. For users that only need permission to edit bot branding. |
| `PORTAL_BILLING`  | Portal billing role, access to some portal features. For users that only need access to billing.                |

### Access

All users in an organization have access to the IAM application. However, only users with the `PORTAL_ADMIN` role will be able to edit roles, delete users, and invite new users. `PORTAL_ADMINS` cannot edit their own access or delete themselves from an organzation, that needs to be done by another `PORTAL_ADMIN`.
