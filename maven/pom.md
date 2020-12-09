# Introduction
  The POM 4.0.0 XSD and descriptor reference documentation.
  <https://maven.apache.org/pom.html>

# Quick Overview
  This is a listing of the elements directly under the POM's project element. Notice that `modelVersion` contains 4.0.0. That is currently the only supported POM version, and is always required.
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <!-- The Basics -->
  <groupId>...</groupId>
  <artifactId>...</artifactId>
  <version>...</version>
  <packaging>...</packaging>
  <dependencies>...</dependencies>
  <parent>...</parent>
  <dependencyManagement>...</dependencyManagement>
  <modules>...</modules>
  <properties>...</properties>

  <!-- Build Settings -->
  <build>...</build>
  <reporting>...</reporting>

  <!-- More Project Information -->
  <name>...</name>
  <description>...</description>
  <url>...</url>
  <inceptionYear>...</inceptionYear>
  <licenses>...</licenses>
  <organization>...</organization>
  <developers>...</developers>
  <contributors>...</contributors>

  <!-- Environment Settings -->
  <issueManagement>...</issueManagement>
  <ciManagement>...</ciManagement>
  <mailingLists>...</mailingLists>
  <scm>...</scm>
  <prerequisites>...</prerequisites>
  <repositories>...</repositories>
  <pluginRepositories>...</pluginRepositories>
  <distributionManagement>...</distributionManagement>
  <profiles>...</profiles>
</project>
```

# The Basics
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.codehaus.mojo</groupId>
  <artifactId>my-project</artifactId>
  <version>1.0</version>
</project>
```

## packaging
when no packaging is declared, Maven assumes the packaging is the default : `jar`. The valid types are Plexus role-hints(read more on Plexus for a explanation of roles and role-hints) of the component role `org.apache.maven.lifecycle.mapping.lifecycleMapping`. The current core packaging values are `pom`, `jar`, `maven-plugin`,`ejb`,`war`,`ear`,`rar`.

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <packaging>war</packaging>
  ...
</project>
```

# POM Relationships
One powerful aspect of maven is its handing of project relationships: this includes dependencies (and transitive dependencies), inheritance,  and aggregation(multi-module projects)
## Dependencies
```
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <type>jar</type>
      <scope>test</scope>
      <optional>true</optional>
    </dependency>
    ...
</dependencies>
```
There are times, unfortunately, when a project cannot be download from the central maven repository.the are three methods for dealing with this scenario.
1. Install the dependency locally using the install plugin.
```
mvn install: install-file -Dfile=jar_file_location.jar -DgroupId=some.group -DartifactId=some.artifactId -Dversion=some.version  -Dpackaging=jar
```
2. create your own repository and deploy it here `deploy:deploy-file`.
3. set the dependency scope to `system` and define a  `systemPath`

## The Super POM
Similar to the inheritance of objects oriented programming,POMs that extend a parent POM inherit certain values from that parent.All project models inherit from a base Super POM , The sinppet below is the Super POM from Maven 3.5.4

## Dependency Management
Besides inheriting certain top-level element, parents have elements to configure values from child POMS and transitive dependencies. One of those elements is `dependencyManagement`

## Aggregation (or Multi-Module)
A project with modules is known as a multi-module, or aggregator project, Modules are projects that this pom lists, and are executed as a group.
```
<modules>
    <module>my-project</module>
    <module>another-project</module>
    <module>third-project/pom-example.xml</module>
  </modules>
```
 ## Properties
 Properties are the last required piece to understand POM basics. Maven properties are value placeholder, like perperties in Ant ,Their values are accessible anywhere within a POM by using the notation ${x}, where x is the property.
```
<properties>
  <maven.compiler.source>1.7</maven.compiler.source>
  <maven.compiler.target>1.7</maven.compiler.target>
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
</properties>
```
# Build Settings
beyond the basics of the POM given above, there are two elements that must be understood before claiming basic competency of the POM. There are the `build` element and the `reporting` element.
## build
Note: There different `build` elements may be denoted `project build` and `profile build`
```
<!-- "Project Build" contains more elements than just the BaseBuild set -->
  <build>...</build>

  <profiles>
    <profile>
      <!-- "Profile Build" contains a subset of "Project Build"s elements -->
      <build>...</build>
    </profile>
  </profiles>
```
The `BaseBUild` is exactly as it sounds: the base set of elements between the two build elements in the POM.
```
<build>
  <defaultGoal>install</defaultGoal>
  <directory>${basedir}/target</directory>
  <finalName>${artifactId}-${version}</finalName>
  <filters>
    <filter>filters/filter1.properties</filter>
  </filters>
  ...
</build>
```
## Resources
Another feature of build elements is specifying where resources exist within your project.Resources are not (usually) code. they are not compiled, but are items meant to be bundled within you project
or used for  various other reasons.such as code generation.
```
<build>
  ...
  <resources>
    <resource>
      <targetPath>META-INF/plexus</targetPath>
      <filtering>false</filtering>
      <directory>${basedir}/src/main/plexus</directory>
      <includes>
        <include>configuration.xml</include>
      </includes>
      <excludes>
        <exclude>**/*.properties</exclude>
      </excludes>
    </resource>
  </resources>
  <testResources>
    ...
  </testResources>
  ...
</build>
```
## repository
The repository is one of the most powerful features of the maven community.By default maven searches the central repository at <https://repo.maven.apache.org/maven2/>
```
<repositories>
  <repository>
    <releases>
      <enabled>false</enabled>
      <updatePolicy>always</updatePolicy>
      <checksumPolicy>warn</checksumPolicy>
    </releases>
    <snapshots>
      <enabled>true</enabled>
      <updatePolicy>never</updatePolicy>
      <checksumPolicy>fail</checksumPolicy>
    </snapshots>
    <name>Nexus Snapshots</name>
    <id>snapshots-repo</id>
    <url>https://oss.sonatype.org/content/repositories/snapshots</url>
    <layout>default</layout>
  </repository>
</repositories>
```
## Plugin repositories
Maven plugins are themselves a special type of artifact, Because of this. plugin repositories may be separated from other repository.
```
<pluginRepositories>
  ...
</pluginRepositories>
```

# Distribution Management
It management the distribution of the artifact and supporting files generated throughout the build process. Strating with the last elements first:
```
<distributionManagement>
    ...
    <downloadUrl>http://mojo.codehaus.org/my-project</downloadUrl>
    <status>deployed</status>
</distributionManagement>
```
## repository
```
<distributionManagement>
  <repository>
    <uniqueVersion>false</uniqueVersion>
    <id>corp1</id>
    <name>Corporate Repository</name>
    <url>scp://repo/maven2</url>
    <layout>default</layout>
  </repository>
  <snapshotRepository>
    <uniqueVersion>true</uniqueVersion>
    <id>propSnap</id>
    <name>Propellors Snapshots</name>
    <url>sftp://propellers.net/maven</url>
    <layout>legacy</layout>
  </snapshotRepository>
  ...
</distributionManagement>
```
## Relocation
```
<distributionManagement>
  ...
  <relocation>
    <groupId>org.apache</groupId>
    <artifactId>my-project</artifactId>
    <version>1.0</version>
    <message>We have moved the Project under Apache</message>
  </relocation>
  ...
</distributionManagement>
```
## Profiles
A new feature of the POM 4.0 is the ability of a project to change setting depending on the environment where it is being built.
```
<profiles>
  <profile>
    <id>test</id>
    <activation>...</activation>
    <build>...</build>
    <modules>...</modules>
    <repositories>...</repositories>
    <pluginRepositories>...</pluginRepositories>
    <dependencies>...</dependencies>
    <reporting>...</reporting>
    <dependencyManagement>...</dependencyManagement>
    <distributionManagement>...</distributionManagement>
  </profile>
</profiles>
```
