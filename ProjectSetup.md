Released versions of Google maven projects are now available in the [central maven repository](http://repo1.maven.org/maven2/) so it is not necessary to reference a repository directly in your pom.xml.

SNAPSHOT versions of Google maven projects may be obtained from Google's snapshot repository. Add the following snippet to your pom.xml to reference that repository.

```
<repository>
  <id>google-maven-snapshot-repository</id>
  <name>Google Maven Snapshot Repository</name>
  <url>https://oss.sonatype.org/content/repositories/google-snapshots/</url>
  <snapshots>
    <enabled>true</enabled>
  </snapshots>
</repository>
```