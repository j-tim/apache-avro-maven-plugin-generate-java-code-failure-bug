# Avro Maven plugin 1.12.0 bug: Failed to generate Java classes from multiple .avsc files containing same record

Simple Repository to reproduce a (possible) bug in the Apache Avro Maven plugin.
Simple Repository to reproduce a (possible) bug in the Apache Avro Maven plugin.
Failed to generate Java classes from multiple .avsc files containing same record

> Caused by: org.apache.avro.SchemaParseException: Can't redefine: com.example.avro.Address

Java version: 21
Affected Avro Maven plugin version: 1.12.0 

## Description

This issue looks like [AVRO-3940](https://issues.apache.org/jira/browse/AVRO-3940) (already resolved)
After upgrading from version `1.11.4` to version `1.12.0` of the plugin I run into the same error: `Caused by: org.apache.avro.SchemaParseException: Can't redefine: com.example.avro.Address` when generating Java classes from two .avsc files that both contain an identical `record` instead of an `enum`.

Both Avro schemas define the `Address` record: 

* [Company Schema](src/main/resources/avro/Company.avsc)
* [Employee Schema](src/main/resources/avro/Employee.avsc)


## Steps to reproduce

1. Clone this repository: https://github.com/j-tim/apache-avro-maven-plugin-generate-java-code-failure-bug
2. Build the Maven project

```bash
./mvnw clean package -X
```

3. Observe the `org.apache.avro.SchemaParseException` in the output

## Expected

The code generator should accept the same record.

## Result

```log
Caused by: org.apache.avro.SchemaParseException: Can't redefine: com.example.avro.Address
    at org.apache.avro.ParseContext.put (ParseContext.java:219)
    at org.apache.avro.Schema.parseRecord (Schema.java:1865)
    at org.apache.avro.Schema.parse (Schema.java:1835)
    at org.apache.avro.Schema.parseField (Schema.java:1891)
    at org.apache.avro.Schema.parseRecord (Schema.java:1872)
    at org.apache.avro.Schema.parse (Schema.java:1835)
    at org.apache.avro.Schema$Parser.parse (Schema.java:1538)
    at org.apache.avro.Schema$Parser.parseInternal (Schema.java:1523)
    at org.apache.avro.JsonSchemaParser.parse (JsonSchemaParser.java:86)
    at org.apache.avro.JsonSchemaParser.parse (JsonSchemaParser.java:77)
    at org.apache.avro.SchemaParser.parse (SchemaParser.java:245)
    at org.apache.avro.SchemaParser.parse (SchemaParser.java:137)
    at org.apache.avro.SchemaParser.parse (SchemaParser.java:103)
    at org.apache.avro.SchemaParser.parse (SchemaParser.java:88)
    at org.apache.avro.mojo.SchemaMojo.doCompile (SchemaMojo.java:82)
    at org.apache.avro.mojo.AbstractAvroMojo.compileFiles (AbstractAvroMojo.java:327)
    at org.apache.avro.mojo.AbstractAvroMojo.execute (AbstractAvroMojo.java:262)
    at org.apache.maven.plugin.DefaultBuildPluginManager.executeMojo (DefaultBuildPluginManager.java:126)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute2 (MojoExecutor.java:328)
    at org.apache.maven.lifecycle.internal.MojoExecutor.doExecute (MojoExecutor.java:316)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:212)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:174)
    at org.apache.maven.lifecycle.internal.MojoExecutor.access$000 (MojoExecutor.java:75)
    at org.apache.maven.lifecycle.internal.MojoExecutor$1.run (MojoExecutor.java:162)
    at org.apache.maven.plugin.DefaultMojosExecutionStrategy.execute (DefaultMojosExecutionStrategy.java:39)
    at org.apache.maven.lifecycle.internal.MojoExecutor.execute (MojoExecutor.java:159)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:105)
    at org.apache.maven.lifecycle.internal.LifecycleModuleBuilder.buildProject (LifecycleModuleBuilder.java:73)
    at org.apache.maven.lifecycle.internal.builder.singlethreaded.SingleThreadedBuilder.build (SingleThreadedBuilder.java:53)
    at org.apache.maven.lifecycle.internal.LifecycleStarter.execute (LifecycleStarter.java:118)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:261)
    at org.apache.maven.DefaultMaven.doExecute (DefaultMaven.java:173)
    at org.apache.maven.DefaultMaven.execute (DefaultMaven.java:101)
    at org.apache.maven.cli.MavenCli.execute (MavenCli.java:906)
    at org.apache.maven.cli.MavenCli.doMain (MavenCli.java:283)
    at org.apache.maven.cli.MavenCli.main (MavenCli.java:206)
    at jdk.internal.reflect.DirectMethodHandleAccessor.invoke (DirectMethodHandleAccessor.java:103)
    at java.lang.reflect.Method.invoke (Method.java:580)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced (Launcher.java:255)
    at org.codehaus.plexus.classworlds.launcher.Launcher.launch (Launcher.java:201)
    at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode (Launcher.java:361)
    at org.codehaus.plexus.classworlds.launcher.Launcher.main (Launcher.java:314)
```

