Google maven projects are now deployed to [the central maven repository](http://repo1.maven.org/maven2/) run by Sonatype (see [the Sonatype blog](http://www.sonatype.com/people/2010/04/uploading-artifacts-to-the-central-maven-repository-diy/) for a good overview). You can search the Central repo at http://oss.sonatype.org/.

To get your Google maven project into Central, follow the instructions here:

[Sonatype OSS Maven Repository Usage Guide](https://docs.sonatype.org/display/Repository/Sonatype+OSS+Maven+Repository+Usage+Guide).

After you request an account on the JIRA as instructed in the document, Sonatype will ask a Googler to verify your request. You will then be able to deploy snapshots and releases by following the guide. Google projects should deploy to the following repo URLs:

https://oss.sonatype.org/service/local/staging/deploy/maven2/ (release staging, no SNAPSHOTs)

https://oss.sonatype.org/content/repositories/google-snapshots/ (for SNAPSHOTs, use this instead of the SNAPSHOT repo URL in the Sonatype usage guide)

## If your project builds with maven ##

Life is much easier if your project builds with maven. Simply use "mvn deploy" and/or "mvn release:perform" as instructed in the Sonatype usage guide to deploy to the staging repo.

## If your project doesn't build with maven ##

If your project doesn't build with maven, you will have to manually create the required javadoc and source jars. To create a source jar, you can use "jar cvf artifactId-version-sources.jar src/". To create a javadoc jar, you can use an ant task, the javadoc command, or even Project | Generate Javadoc... in Eclipse.

In place of maven deploy / release, you will have to deploy each artifact individually using deploy-file like this:

```
  mvn gpg:sign-and-deploy-file \
  -Dfile=$curFile \
  -Durl=$mavenRepoUrl \
  -DrepositoryId=$mavenRepoId \
  -DgroupId=$groupId \
  -DartifactId=$artifactId \
  -Dversion=$mavenVersion \
  -Dpackaging=jar \
  -DpomFile = $pomFile \
  -DuniqueVersion=false \
  -Dclassifier=$classifier \
  -Dgpg.passphrase="$gpgPassphrase"
```

where $classifier is "sources", "javadoc" or "" (for regular jars). The local filename specified with -Dfile is unimportant as the filename in the repository is created from the artifactId, version, and classifier.

Once you have deployed all project artifacts, close and promote the staging repository as described in the Sonatype Usage Guide.