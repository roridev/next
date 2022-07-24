# [tbd].mod.json
The [tbd].mod.json is a extension metadata file used by the [`sisbase-loader`](./core.md) to load extensions.  
In order to be loaded, an extension **must** have this file with the exact name placed in the root directory of the mod JAR.  

## Mandatory Fields
| Name            | Type    | Description                                                                                               |
|-----------------|---------|-----------------------------------------------------------------------------------------------------------|
| `id`            | String  | Defines the extension's identifier - A String of Latin Letters, Digits, Underscores with Lenght from 1-63 |
| `version`       | String  | Defines the extension's version. - A String value, Optionally matching [`SemVer`]().                      |
| `schemaVersion` | Integer | Used Internally. Must be `1`.                                                                             |

## Optional Fields

### Metadata
| Name           | Type     | Description                                                                      |
|----------------|----------|----------------------------------------------------------------------------------|
| `name`         | String   | Defines the user-friendly extension name. If not present, assume it matches `id` |
| `description`  | String   | Defines the extension's description. If not present, assume empty string.        |
| `contact`      | Contact  | Defines the contact information for the project.                                 |
| `authors`      | [Author] | A list of authors of the extension.                                              |
| `contributors` | [Author] | Same as `authors`                                                                |


### Dependency resolution

| Name                   | Type       | Description                                                                             |
|------------------------|------------|-----------------------------------------------------------------------------------------|
| `required-api-version` | Integer    | Defines the minimum required Discord API Version necessary for the extension to load.   |
| `depends`              | Dependency | For dependencies **required** to run. Without them the extension won't load.            |
| `recommends`           | Dependency | For dependencies **not required** to run. Without them the loader will print a warning. |

## Data Types

### Dependency
Object whose key is the `id` of the dependency, and whose value is either a string or an array of strings declaring supported version ranges.  

Example:
```json
{
	"my_dependency":">=1.0.0"
}
```


### Author
Single Name (String) or an object containing these fields:  
| Name      | Type    | Description                                        |
|-----------|---------|----------------------------------------------------|
| `name`    | String  | The real name, username, of the person (Mandatory) |
| `contact` | Contact | Person's contact information (Optional)            |


### Contact
An object containing any of those fields.  
All of the following fields are **optional**  
| Name       | Type   | Description                                                   |
|------------|--------|---------------------------------------------------------------|
| `email`    | String | Must be a valid email                                         |
| `irc`      | String | Must be a valid URL format                                    |
| `homepage` | String | Must be a valid HTTP/HTTPS address                            |
| `issues`   | String | Extension's issue tracker. Must be a valid HTTP/HTTPS address |
| `sources`  | String | Must be a valid URL format                                    |
