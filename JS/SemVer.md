short for *Semantic Versioning*: Philosophy used for deciding the version number to attach to dependencies as they are updated and released

A SemVer version is made up of three separate numbers such as 1.2.3
- First number if major version
- the second number if minor version
- the 3rd number is the patch version

Each component of the overall version number has a special meaning. 
- When a package makes a change that breaks backwards compatibility, the major version should be incremented. 
- When a package adds a new feature but backwards compatibility is maintained, the minor version should be incremented. 
- If a change only results in a bug fix and nothing else, then the patch version should be incremented.

Whenever a version is incremented, the lower versions reset at zero. 
- For example, if a major change is introduced to a package at version 1.2.3, it should become 2.0.0 (not 2.2.3). 

If a release of a package introduces multiple changes, then the effects of the most significant change determine the new version number.

A special case for SemVer is when the most significant digits of a version number begin with zero. In these cases, the first nonzero digit is essentially considered the major version, the next digit is the minor, etc. 
- What this means is that if a breaking change is introduced to a package at version 0.1.2, it becomes version 0.2.0. 
- If a package has the version of 0.0.1, then any breaking changes can result in a version of 0.0.2.

The real power of SemVer is that an application making use of a package should be free to blindly accept all minor or patch updates of a package without any fears that their application might break. 
- In practice, authors of npm packages aren’t always so disciplined, which is why any updates to an application’s dependencies will require that a test suite pass is run. 
- In many cases, the application author may need to interact with the application to make sure it continues to work as intended.


Example: 

```JSON
"dependencies": {
	"fastify": "^2.11.0",
	"ioredis": "~4.14.1",
	"pg": "7.17.1"
}
```

``^`` : any future version of the package that is compatible with the specified version will be installed, e.g. `2.11.1` can be used, or even `2.17.0`, however, `3.0.0` will not be allowed

`~` : means only patch updates (bug fixes). e.g. may be upgraded to `4.14.2` but never to `4.15.1`, (hence this is a more conservative way to install a package

Last on above will only ever install `7.17.1`, not even patch versions, this is the most conservative.

