# Global settings

Hlaeja services utilize Gradle options or environment variables to configure development settings, ensuring our configurations remain organized and secure.

## Overview

By using these methods, we can easily manage access to restricted resources and maintain a consistent configuration across our services. This approach enables us to keep sensitive information separate from our codebase.

## Gradle properties

To access repositories that require authentication, we set `repository.user` and `repository.token` properties in the `gradle.properties` file. To do this:

1. Open or create the `gradle.properties` file in your Gradle user home directory:

   - On Unix-like systems (Linux, macOS), this is typically located at `~/.gradle/`.
   - On Windows, this is typically located at `C:\Users\<YourUsername>\.gradle\`.

2. Add the following settings to the `gradle.properties` file:
    ```properties
    repository.user=your_user
    repository.token=your_token_value
    ```

## Environment variables 

Alternatively, you can use `REPOSITORY_USER` and `REPOSITORY_TOKEN` environment variables to pass credentials to the application. These variables can be set in your system environment or through your IDE.
