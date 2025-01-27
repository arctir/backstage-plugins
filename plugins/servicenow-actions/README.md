# ServiceNow actions for Backstage

This plugin provides [Backstage](https://backstage.io/) template [actions](https://backstage.io/docs/features/software-templates/builtin-actions) for [ServiceNow](https://developer.servicenow.com/dev.do#!/reference/api/vancouver/rest).

The following actions are currently supported in this plugin:

- [Table API](https://developer.servicenow.com/dev.do#!/reference/api/vancouver/rest/c_TableAPI)

## Prerequisites

- A [Backstage](https://backstage.io/docs/getting-started/) project

## Installation

Run the following command to install the action package in your Backstage project:

```console
yarn workspace backend add @janus-idp/backstage-scaffolder-backend-module-servicenow
```

## Configuration

[Register](https://backstage.io/docs/features/software-templates/writing-custom-actions#registering-custom-actions) the ServiceNow actions by modifying the `packages/backend/src/plugins/scaffolder.ts` file from your project with the following changes:

```ts title="packages/backend/src/plugins/scaffolder.ts"
/* highlight-add-next-line */
import { createServiceNowActions } from '@janus-idp/backstage-scaffolder-backend-module-servicenow';

export default async function createPlugin(
  env: PluginEnvironment,
): Promise<Router> {
  // ...

  /* highlight-add-next-line */
  const actions = [
    ...builtInActions,
    ...createServiceNowActions({ config: env.config }),
  ];

  return await createRouter({
    actions,
    logger: env.logger,
    config: env.config,
    database: env.database,
    reader: env.reader,
    catalogClient,
    identity: env.identity,
  });
}
```

## Usage

### Action : `servicenow:now:table:createRecord`

#### Request Type: `POST`

#### Input

| Parameter Name                |              Type              | Required | Description                                                                                |
| ----------------------------- | :----------------------------: | :------: | ------------------------------------------------------------------------------------------ |
| `tableName`                   |            `string`            |   Yes    | Name of the table in which to save the record                                              |
| `requestBody`                 | `Record<PropertyKey, unknown>` |    No    | Field name and the associated value for each parameter to define in the specified record   |
| `sysparmDisplayValue`         | `enum("true", "false", "all")` |    No    | Return field display values (true), actual values (false), or both (all) (default: false)  |
| `sysparmExcludeReferenceLink` |           `boolean`            |    No    | True to exclude Table API links for reference fields (default: false)                      |
| `sysparmFields`               |           `string[]`           |    No    | An array of fields to return in the response                                               |
| `sysparmInputDisplayValue`    |           `boolean`            |    No    | Set field values using their display value (true) or actual value (false) (default: false) |
| `sysparmSuppressAutoSysField` |           `boolean`            |    No    | True to suppress auto generation of system fields (default: false)                         |
| `sysparmView`                 |            `string`            |    No    | Render the response according to the specified UI view (overridden by sysparm_fields)      |

#### Output

| Name     |              Type              | Description                      |
| -------- | :----------------------------: | -------------------------------- |
| `result` | `Record<PropertyKey, unknown>` | The response body of the request |

### Action : `servicenow:now:table:deleteRecord`

#### Request Type: `DELETE`

#### Input

| Parameter Name         |   Type    | Required | Description                                                       |
| ---------------------- | :-------: | :------: | ----------------------------------------------------------------- |
| `tableName`            | `string`  |   Yes    | Name of the table in which to delete the record                   |
| `sysId`                | `string`  |   Yes    | Unique identifier of the record to delete                         |
| `sysparmQueryNoDomain` | `boolean` |    No    | True to access data across domains if authorized (default: false) |

### Action : `servicenow:now:table:modifyRecord`

#### Request Type: `PUT`

#### Input

| Parameter Name                |              Type              | Required | Description                                                                                |
| ----------------------------- | :----------------------------: | :------: | ------------------------------------------------------------------------------------------ |
| `tableName`                   |            `string`            |   Yes    | Name of the table in which to modify the record                                            |
| `sysId`                       |            `string`            |   Yes    | Unique identifier of the record to modify                                                  |
| `requestBody`                 | `Record<PropertyKey, unknown>` |    No    | Field name and the associated value for each parameter to define in the specified record   |
| `sysparmDisplayValue`         | `enum("true", "false", "all")` |    No    | Return field display values (true), actual values (false), or both (all) (default: false)  |
| `sysparmExcludeReferenceLink` |           `boolean`            |    No    | True to exclude Table API links for reference fields (default: false)                      |
| `sysparmFields`               |           `string[]`           |    No    | An array of fields to return in the response                                               |
| `sysparmInputDisplayValue`    |           `boolean`            |    No    | Set field values using their display value (true) or actual value (false) (default: false) |
| `sysparmSuppressAutoSysField` |           `boolean`            |    No    | True to suppress auto generation of system fields (default: false)                         |
| `sysparmView`                 |            `string`            |    No    | Render the response according to the specified UI view (overridden by sysparm_fields)      |
| `sysparmQueryNoDomain`        |           `boolean`            |    No    | True to access data across domains if authorized (default: false)                          |

#### Output

| Name     |              Type              | Description                      |
| -------- | :----------------------------: | -------------------------------- |
| `result` | `Record<PropertyKey, unknown>` | The response body of the request |

### Action : `servicenow:now:table:retrieveRecord`

#### Request Type: `GET`

#### Input

| Parameter Name                |              Type              | Required | Description                                                                               |
| ----------------------------- | :----------------------------: | :------: | ----------------------------------------------------------------------------------------- |
| `tableName`                   |            `string`            |   Yes    | Name of the table from which to retrieve the record                                       |
| `sysId`                       |            `string`            |   Yes    | Unique identifier of the record to retrieve                                               |
| `sysparmDisplayValue`         | `enum("true", "false", "all")` |    No    | Return field display values (true), actual values (false), or both (all) (default: false) |
| `sysparmExcludeReferenceLink` |           `boolean`            |    No    | True to exclude Table API links for reference fields (default: false)                     |
| `sysparmFields`               |           `string[]`           |    No    | An array of fields to return in the response                                              |
| `sysparmView`                 |            `string`            |    No    | Render the response according to the specified UI view (overridden by sysparm_fields)     |
| `sysparmQueryNoDomain`        |           `boolean`            |    No    | True to access data across domains if authorized (default: false)                         |

#### Output

| Name     |              Type              | Description                      |
| -------- | :----------------------------: | -------------------------------- |
| `result` | `Record<PropertyKey, unknown>` | The response body of the request |

### Action : `servicenow:now:table:retrieveRecords`

#### Request Type: `GET`

#### Input

| Parameter Name                    |              Type              | Required | Description                                                                               |
| --------------------------------- | :----------------------------: | :------: | ----------------------------------------------------------------------------------------- |
| `tableName`                       |            `string`            |   Yes    | Name of the table from which to retrieve the records                                      |
| `sysparamQuery`                   |            `string`            |    No    | An encoded query string used to filter the results                                        |
| `sysparmDisplayValue`             | `enum("true", "false", "all")` |    No    | Return field display values (true), actual values (false), or both (all) (default: false) |
| `sysparmExcludeReferenceLink`     |           `boolean`            |    No    | True to exclude Table API links for reference fields (default: false)                     |
| `sysparmSuppressPaginationHeader` |           `boolean`            |    No    | True to suppress pagination header (default: false)                                       |
| `sysparmFields`                   |           `string[]`           |    No    | An array of fields to return in the response                                              |
| `sysparmLimit`                    |             `int`              |    No    | The maximum number of results returned per page (default: 10,000)                         |
| `sysparmView`                     |            `string`            |    No    | Render the response according to the specified UI view (overridden by sysparm_fields)     |
| `sysparmQueryCategory`            |            `string`            |    No    | Name of the query category (read replica category) to use for queries                     |
| `sysparmQueryNoDomain`            |           `boolean`            |    No    | True to access data across domains if authorized (default: false)                         |
| `sysparmNoCount`                  |           `boolean`            |    No    | Do not execute a select count(\*) on table (default: false)                               |

#### Output

| Name     |              Type              | Description                      |
| -------- | :----------------------------: | -------------------------------- |
| `result` | `Record<PropertyKey, unknown>` | The response body of the request |

### Action : `servicenow:now:table:updateRecord`

#### Request Type: `PATCH`

#### Input

| Parameter Name                |              Type              | Required | Description                                                                                |
| ----------------------------- | :----------------------------: | :------: | ------------------------------------------------------------------------------------------ |
| `tableName`                   |            `string`            |   Yes    | Name of the table in which to update the record                                            |
| `sysId`                       |            `string`            |   Yes    | Unique identifier of the record to update                                                  |
| `requestBody`                 | `Record<PropertyKey, unknown>` |    No    | Field name and the associated value for each parameter to define in the specified record   |
| `sysparmDisplayValue`         | `enum("true", "false", "all")` |    No    | Return field display values (true), actual values (false), or both (all) (default: false)  |
| `sysparmExcludeReferenceLink` |           `boolean`            |    No    | True to exclude Table API links for reference fields (default: false)                      |
| `sysparmFields`               |           `string[]`           |    No    | An array of fields to return in the response                                               |
| `sysparmInputDisplayValue`    |           `boolean`            |    No    | Set field values using their display value (true) or actual value (false) (default: false) |
| `sysparmSuppressAutoSysField` |           `boolean`            |    No    | True to suppress auto generation of system fields (default: false)                         |
| `sysparmView`                 |            `string`            |    No    | Render the response according to the specified UI view (overridden by sysparm_fields)      |
| `sysparmQueryNoDomain`        |           `boolean`            |    No    | True to access data across domains if authorized (default: false)                          |

#### Output

| Name     |              Type              | Description                      |
| -------- | :----------------------------: | -------------------------------- |
| `result` | `Record<PropertyKey, unknown>` | The response body of the request |
