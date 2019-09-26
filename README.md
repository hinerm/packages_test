# packages_test


## Things that work
* Deployed multiple release versions of `packages_test` artifact from the `provide_package` directory.
* Deployed one SNAPSHOT version `packages_test` artifact.

## Things that are broken
* After deploying one SNAPSHOT version, I suspect the maven repo is handling SNAPSHOTs incorrectly and is treating the timestamp-locked SNAPSHOT created by Maven3 as releases. Trying to deploy **any** subsequent SNAPSHOT version of `packages_test` results in:
```
[INFO] --- maven-deploy-plugin:2.7:deploy (default-deploy) @ packages_test ---
Downloading: https://maven.pkg.github.com/hinerm/packages_test/org/hinerm/packages_test/4.0.0-SNAPSHOT/maven-metadata.xml
[WARNING] Could not transfer metadata org.hinerm:packages_test:4.0.0-SNAPSHOT/maven-metadata.xml from/to github-hinerm (https://maven.pkg.github.com/hinerm/packages_test): Failed to transfer file: https://maven.pkg.github.com/hinerm/packages_test/org/hinerm/packages_test/4.0.0-SNAPSHOT/maven-metadata.xml. Return code is: 400 , ReasonPhrase:Bad Request.
```

* I am unable to actually use the `packages_test` dependency in other projects. Doing anything in the `consume_packages` project that would trigger a dependency download, e.g. `mvn dependency:copy-dependencies`, results in failure:
```
Downloading: https://repo1.maven.org/maven2/org/hinerm/packages_test/2/packages_test-2.pom
Downloading: https://maven.pkg.github.com/hinerm/org/hinerm/packages_test/2/packages_test-2.pom
[WARNING] The POM for org.hinerm:packages_test:jar:2 is missing, no dependency information available
Downloading: https://repo1.maven.org/maven2/org/hinerm/packages_test/2/packages_test-2.jar
Downloading: https://maven.pkg.github.com/hinerm/org/hinerm/packages_test/2/packages_test-2.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.290 s
[INFO] Finished at: 2019-09-26T08:57:37-05:00
[INFO] Final Memory: 11M/346M
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal on project package_user: Could not resolve dependencies for project org.hinerm:package_user:jar:2: Could not find artifact org.hinerm:packages_test:jar:2 in central (https://repo1.maven.org/maven2) -> [Help 1]
```
