# Bootstrapping authentication

## Problem

There is a Catch-22 in the account registration system: accounts can only be created by registered accounts. This means that there has to be a registered account to start with.

## Solution

When starting the backend, it will check if the user specified in the configuration is registered in the database. If not, the user will be created, with the global admin role.

## Configuration

This is the configuration for the backend (`application.conf`).

Key                                  | Type   | Nullable | Default
-------------------------------------|--------|----------|--------
authBootstrapper.email               | String | No       |
authBootstrapper.password            | String | No       |
authBootstrapper.tenantCanonicalName | String | Yes      | "platform"
authBootstrapper.firstName           | String | Yes      | "Global"
authBootstrapper.lastName            | String | Yes      | "Admin"


