---
title: Verify Checksums
layout: default
parent: How-Tos
nav_order: 35
---

# Checksum Report Generation and Verification

## Synopsis

This chapter shows how to accomplish common tasks related to checksums.

## Purpose

Verify the integrity of uploaded or downloaded artifacts. Conversely, detect data corruption.

## Approach

The [checksum-maven-plugin](https://github.com/nicoulaj/checksum-maven-plugin) supports generating reports in CSV and XML formats.

* `checksum:dependencies` generates a checksum report.
* `checksum:check` checks the current files against a previously generated report.
* Possible limitation: It seems it is not possible to retrieve the remote checksums of dependencies and validate against those - at least I couldn't find out how to accomplish this.

### Configuration

```xml
      <plugin>
        <groupId>net.nicoulaj.maven.plugins</groupId>
        <artifactId>checksum-maven-plugin</artifactId>
        <version>1.11</version>
        <configuration>
          <pomSkip>false</pomSkip>
          <!--
          <transitive>true</transitive>
          <failIfNoFiles>true</failIfNoFiles>
          -->
          <!-- By default, dependencies of all types and scopes are processed. -->
          <!--
          <types>
            <type>ttl.bz2</type>
            <type>nt</type>
          </types>
          -->
        </configuration>
        <executions>
          <execution>
            <phase>verify</phase>
            <goals>
              <goal>dependencies</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
```

### Usage

⚠️  `checksum:check` looks for the checksum summary in the base directory rather than the build one - i.e. `${project.basedir}` rather than `${project.build.directory}`.

```
mvn checksum:dependencies

# Pitfall: checksum:check looks for the checksum summary
# in the base directory rather than the build one.
# Therefore we need to copy
cp target/dependencies-checksums.csv .

# Use -X to see which files are being checked.
mvn -X checksum:check
```

