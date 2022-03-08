# composite-mssql-deploy-dbup

This a composite action wrapping the following functionality

1. Render environment specific variables with jnus/json-variables
2. Substitute variables in configuration with microsoft/substitute-variables
3. Execute DbUp console application.

*Note this action will only run on Linux build runners and that paths are case sensitive*

## Usage

Reference the composite action with a full sha like the following to avoid breaking changes in the repo:
```yaml
segesdk/composite-mssql-deploy-dbup@ee83ff39f935503f86aea3ff290f8c1e8a1cdb61
```
or like the following, to always use latest version by referencing @main

```yaml
segesdk/composite-mssql-deploy-dbup@main
```

## Parameters

| Input variable | Description |
|----------|----------|
|publishPath | Publishing path of the build artifacts |
|environment | Target environment to deploy to. Used for rendering environment variables and deployment to Azure
|variableFile| Full path to json file with variables for rendering with jnus/json-variables|
|secrets|Serialized json string for github secrets, used to render environment variables. Syntax: `'${{ toJson(secrets) }}'`|
|configurationFile|Full path to configuration file to perform variable substitution|
|dbUpExecutable|Path to the dbup assembly|


## Usage example
```yaml
- id: Deployment
       uses: segesdk/composite-mssql-deploy-dbup@main
       with:
         publishPath: ${{env.PUBLISH_PATH}}
         environment: Dev
         variableFile: '${{env.PUBLISH_PATH}}/variables.json'
         secrets: '${{ toJson(secrets) }}'
         configurationFile: '${{env.PUBLISH_PATH}}/appsettings.json'
         dbUpExecutable: ./src/dbup/bin/release/dbup.dll
```


