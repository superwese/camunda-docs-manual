---

title: "Update from 7.4 to 7.5"
weight: 80

menu:
  main:
    name: "7.4 to 7.5"
    identifier: "migration-guide-74"
    parent: "migration-guide-minor"
    pre: "Update from `7.4.x` to `7.5.0`."

---

This document guides you through the update from Camunda BPM `7.4.x` to `7.5.0`. It covers these use cases:

1. For administrators and developers: [Database Updates]({{< relref "#database-updates" >}})
2. For administrators and developers: [Full Distribution Update]({{< relref "#full-distribution" >}})
3. For administrators and developers: [Application with Embedded Process Engine Update]({{< relref "#application-with-embedded-process-engine" >}})

This guide covers mandatory migration steps as well as optional considerations for initial configuration of new functionality included in Camunda BPM 7.5.

Noteworthy new Features and Changes in 7.5:

{{< note title="TODO" class="info" >}}
  Add Features and Changes!
{{< /note >}}

{{< note title="TODO" class="info" >}}
  Add warning if no rolling upgrades!
{{< /note >}}

# Database Updates

Every Camunda installation requires a database schema upgrade.

## Procedure

1. Check for [available database patch scripts]({{< relref "update/patch-level.md#database-patches" >}}) for your database that are within the bounds of your upgrade path.
 Locate the scripts at `$DISTRIBUTION_PATH/sql/upgrade` in the pre-packaged distribution (where `$DISTRIBUTION_PATH` is the path of an unpacked distribution) or in the [Camunda Nexus](https://app.camunda.com/nexus/content/groups/public/org/camunda/bpm/distro/camunda-sql-scripts/).
 We highly recommend to execute these patches before upgrading. Execute them in ascending order by version number.
 The naming pattern is `$DATABASENAME_engine_7.4_patch_?.sql`.

2. Execute the corresponding upgrade scripts named

    * `$DATABASENAME_engine_7.4_to_7.5.sql`

    The scripts update the database from one minor version to the next one and change the underlying database structure, so make sure to backup your database in case there are any failures during the upgrade process.

3. We highly recommend to also check for any existing patch scripts for your database that are within the bounds of the new minor version you are upgrading to. Execute them in ascending order by version number. _Attention_: This step is only relevant when you are using an enterprise version of the Camunda BPM platform, e.g., `7.5.X` where `X > 0`. The procedure is the same as in step 1, only for the new minor version.

## Special Considerations

### MariaDB

Since 7.5.0 there are separate SQL scripts for MariaDB. If you use MariaDB and update from a version < 7.5.0, you have to execute the script `mariadb_engine_7.4_to_7.5.sql`.

Additionally, you have to adjust the [database configuration]({{< relref "user-guide/process-engine/database.md#database-configuration" >}}) of your process engine configuration and set the property `databaseType` to `mariadb`.

# Full Distribution

This section is applicable if you installed the [Full Distribution]({{< relref "introduction/downloading-camunda.md#full-distribution" >}}) with a **shared process engine**.

The following steps are required:

1. Upgrade Camunda Libraries and Applications inside the application server
2. Migrate custom Process Applications

Before starting, make sure that you have downloaded the Camunda BPM 7.5 distribution for the application server you use. It contains the SQL scripts and libraries required for upgrade. This guide assumes you have unpacked the distribution to a path named `$DISTRIBUTION_PATH`.

## Camunda Libraries and Applications

Please choose the application server you are working with from the following list:

* [Apache Tomcat]({{< relref "update/minor/74-to-75/tomcat.md" >}})
* [JBoss AS/Wildfly]({{< relref "update/minor/74-to-75/jboss.md" >}})
* [Glassfish]({{< relref "update/minor/74-to-75/glassfish.md" >}})
* [IBM WebSphere]({{< relref "update/minor/74-to-75/was.md" >}})
* [Oracle WebLogic]({{< relref "update/minor/74-to-75/wls.md" >}})

{{< note title="TODO" class="info" >}}
  Add sections for application servers.
{{< /note >}}

## Custom Process Applications

For every process application, the Camunda dependencies should be updated to the new version. Which dependencies you have is application- and server-specific. Typically, the dependencies consist of any of the following:

* `camunda-engine-spring`
* `camunda-engine-cdi`
* `camunda-ejb-client`
* ...

There are no new mandatory dependencies for process applications.

# Application with Embedded Process Engine

This section is applicable if you have a custom application with an **embedded process engine**.

Upgrade the dependencies declared in your application's `pom.xml` file to the new version. Which dependencies you have is application-specific. Typically, the dependencies consist of any of the following:

* `camunda-engine`
* `camunda-bpmn-model`
* `camunda-engine-spring`
* `camunda-engine-cdi`
* ...

There are no new mandatory dependencies. That means, upgrading the version should suffice to migrate a process application in terms of dependencies.

{{< note title="TODO" class="info" >}}
  Add new mandatory or transitive dependencies if necessary!
{{< /note >}}

## Special Considerations

This section describes changes in the engine's default behavior. While the changes are reasonable, your implementation may rely on the previous default behavior. Thus, the previous behavior can be restored by explicitly setting a configuration option. Accordingly, this section applies to any embedded process engine but is not required for a successful upgrade.

{{< note title="TODO" class="info" >}}
  Add engine changes!
{{< /note >}}

### Incident Handler

The interface of an [Incident Handler]({{< relref "user-guide/process-engine/incidents.md" >}}) has changed. Instead of a long parameter list, the methods pass a context object which bundles all required informations, like process definition id, execution id and tenant id. Since the existing methods have been overridden, custom implementations of an incident handler have to be adjusted.

### Correlation Handler

A new method has been added to the interface of a {{< javadocref page="?org/camunda/bpm/engine/impl/runtime/CorrelationHandler.html" text="Correlation Handler" >}}. The new method `correlateStartMessage()` allows to explicit trigger a message start event of a process definition. If the default implementation is replaced by a custom one then it have to be adjusted.

