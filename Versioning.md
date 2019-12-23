# Versioning

| Format | Description |
| -- | -- |
| 3.5.1 |  Expressing a version number directly will accept only the package with the exact matching version number, e.g., 3.5.1 |
| \* | Using an asterisk accepts any version of the package to be installed |
| \>3.5.1 \>=3.5.1 | Prefixing a version number with > or >= accepts any version of the package that is greater than or greater than or equal to a given version |
| <3.5.1 <=3.5.1 | Prefixing a version number with < or <= accepts any version of the package that is less than or less than or equal to a given version |
| \~3.5.1 | Prefixing a version number with a tilde (the \~ character) accepts versions to be installed even if the patch level number (the last of the three version numbers) doesn’t match. For example, specifying \~3.5.1 will accept version 3.5.2 or 3.5.3 but not version 3.6.0 (which would be a new minor release) |
| ^3.5.1 | Prefixing a version number with a ^ will accept versions even if the minor release number or the patch number doesn’t match. For example, specifying ^3.5.1 will allow versions 3.5.2 and 3.6.0, for example, but not version 4.0.0 |