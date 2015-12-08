# Roles

Access to endpoints is controlled by roles. These are saved along with a users profile. The following roles are defined:

Name         | String representation | Explanation                               | Example action
-------------|-----------------------|-------------------------------------------|---------------
Global Admin | "globaladmin"         | A global administrator of the platform    | Adding a new tenant
Tenant Admin | "tenantadmin"         | An administrator for a tenant             | Adding a new user to the tenant
Normal User  | "normaluser"          | A normal user                             | Updating information of a child
Unknown      | "-"                   | An unknow user                            | 

A user can possess any number of roles.

