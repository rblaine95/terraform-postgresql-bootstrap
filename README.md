# terraform-postgresql-bootstrap

Terraform module to provision and manage postgresql resources.   

## Usage

```hcl
  module "bootstrap_db" {
    source = "./"

    extensions = ["pg_stat_statements", "pg_hint_plan"]

    databases = [
      {
        name = "test"
      }
    ]

    roles = [
      {
        name                = "test"
        database            = "test"
        database_privileges = "CONNECT,CREATE,TEMPORARY"
        table_privileges    = "SELECT,INSERT,UPDATE,DELETE,TRUNCATE,REFERENCES,TRIGGER"
        sequence_privileges = "USAGE,SELECT,UPDATE"
      },

      {
        name                = "test-ro"
        database            = "test"
        database_privileges = "CONNECT"
        table_privileges    = "SELECT"
        sequence_privileges = "USAGE,SELECT"
      },

      {
        name                = "prometheus-exporter"
        roles               = "pg_read_all_stats,pg_read_all_settings"
        database            = "test"
        database_privileges = "CONNECT"
      }
    ]
  }
```

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.0 |
| <a name="requirement_postgresql"></a> [postgresql](#requirement\_postgresql) | >= 1.14 |
| <a name="requirement_random"></a> [random](#requirement\_random) | >= 3 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_postgresql"></a> [postgresql](#provider\_postgresql) | >= 1.14 |
| <a name="provider_random"></a> [random](#provider\_random) | >= 3 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [postgresql_database.default](https://registry.terraform.io/providers/cyrilgdn/postgresql/latest/docs/resources/database) | resource |
| [postgresql_extension.default](https://registry.terraform.io/providers/cyrilgdn/postgresql/latest/docs/resources/extension) | resource |
| [postgresql_grant.database](https://registry.terraform.io/providers/cyrilgdn/postgresql/latest/docs/resources/grant) | resource |
| [postgresql_grant.sequence](https://registry.terraform.io/providers/cyrilgdn/postgresql/latest/docs/resources/grant) | resource |
| [postgresql_grant.table](https://registry.terraform.io/providers/cyrilgdn/postgresql/latest/docs/resources/grant) | resource |
| [postgresql_role.default](https://registry.terraform.io/providers/cyrilgdn/postgresql/latest/docs/resources/role) | resource |
| [random_password.default](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/password) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_databases"></a> [databases](#input\_databases) | A list of databases to create.<br>   name:<br>     The name of the database.<br>owner:<br>	The role name of the user who will own the database.<br>tablespace\_name:<br>	The name of the tablespace that will be associated with the database.<br>connection\_limit:<br>	How many concurrent connections can be established to this database.<br>allow\_connections:<br>	If `false` then no one can connect to this database.<br>is\_template:<br>	If `true`, then this database can be cloned by any user with `CREATEDB` privileges.<br>template:<br>	The name of the template database from which to create the database.<br>encoding:<br>	Character set encoding to use in the database. <br>lc\_collate:<br>	Collation order to use in the database.<br>lc\_ctype:<br>	Character classification to use in the database. | <pre>list(object(<br>    {<br>      name              = string<br>      owner             = optional(string)<br>      tablespace_name   = optional(string)<br>      connection_limit  = optional(number)<br>      allow_connections = optional(bool)<br>      is_template       = optional(bool)<br>      encoding          = optional(string)<br>      template          = optional(string)<br>      lc_collate        = optional(string)<br>      lc_ctype          = optional(string)<br>    }<br>  ))</pre> | `[]` | no |
| <a name="input_extensions"></a> [extensions](#input\_extensions) | A list of names of the extension to enable. | `list(string)` | <pre>[<br>  "pg_stat_statements",<br>  "pg_hint_plan"<br>]</pre> | no |
| <a name="input_roles"></a> [roles](#input\_roles) | A list of roles to create.<br>	name:<br>		The role name.<br>	database:<br>		The database to grant privileges on for this role.<br>	superuser:<br>		Defines whether the role is a `superuser`.<br>	create\_database:<br>		Defines a role's ability to execute `CREATE DATABASE`.<br>	create\_role:<br>		Defines a role's ability to execute `CREATE ROLE`.<br>	inherit:<br>		Defines whether a role `inherits` the privileges of roles it is a member of.<br>	login:<br>		Defines whether role is allowed to log in.<br>	replication:<br>		Defines whether a role is allowed to initiate streaming replication or put the system in and out of backup mode.<br>	bypass\_row\_level\_security:<br>		Defines whether a role bypasses every row-level security (RLS) policy.<br>	connection\_limit:<br>		How many concurrent connections the role can establish. <br>	encrypted\_password:<br>		Defines whether the password is stored encrypted in the system catalogs.<br>	roles:<br>		A comma separated list of roles which will be granted to this new role.<br>	valid\_until:<br>		Defines the date and time after which the role's password is no longer valid.<br>	schema:<br>		The database schema to grant privileges on for this role.<br>	with\_grant\_option:<br>		Whether the recipient of these privileges can grant the same privileges to others.<br>	database\_privileges:<br>		A comma separated list of roles which will be granted to database.<br>	table\_privileges:<br>		A comma separated list of roles which will be granted to tables.<br>	sequence\_privileges:<br>		A comma separated list of roles which will be granted to sequence. | <pre>list(object(<br>    {<br>      name                      = string<br>      database                  = optional(string)<br>      superuser                 = optional(bool)<br>      create_database           = optional(bool)<br>      create_role               = optional(bool)<br>      inherit                   = optional(bool)<br>      login                     = optional(bool)<br>      replication               = optional(bool)<br>      connection_limit          = optional(number)<br>      encrypted_password        = optional(bool)<br>      bypass_row_level_security = optional(bool)<br>      valid_until               = optional(string)<br>      roles                     = optional(string)<br>      search_path               = optional(string)<br>      schema                    = optional(string)<br>      with_grant_option         = optional(string)<br>      database_privileges       = optional(string)<br>      table_privileges          = optional(string)<br>      sequence_privileges       = optional(string)<br>    }<br>  ))</pre> | `[]` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_databases"></a> [databases](#output\_databases) | A list of databases. |
| <a name="output_roles"></a> [roles](#output\_roles) | A map of role name per password. |
<!-- END_TF_DOCS --> 

## License
The Apache-2.0 license