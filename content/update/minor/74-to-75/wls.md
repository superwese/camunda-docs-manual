---

title: "Update an Oracle Weblogic Installation from 7.4 to 7.5"

menu:
  main:
    name: "Weblogic"
    identifier: "migration-guide-74-weblogic"
    parent: "migration-guide-74"

---

The following steps describe how to upgrade the Camunda artifacts on an Oracle WebLogic application server in a shared process engine setting. For the entire procedure, refer to the [upgrade guide][upgrade-guide]. If not already done, make sure to download the [Camunda BPM 7.5 Oracle WebLogic distribution](https://app.camunda.com/nexus/content/groups/internal/org/camunda/bpm/weblogic/camunda-bpm-weblogic/).

The upgrade procedure takes the following steps:

1. Uninstall the Camunda Applications and Archives
2. Replace Camunda Core Libraries
3. Replace Optional Camunda Dependencies
4. Maintain the BPM Platform Configuration
5. Maintain Process Engine Configuration
6. Maintain Process Applications
7. Install the Camunda Archive
8. Install the Web Applications

In each of the following steps, the identifiers `$*_VERSION` refer to the current version and the new versions of the artifacts.

# 1. Uninstall the Camunda Applications and Archives

First, uninstall the Camunda web applications, namely the Camunda REST API (artifact name like `camunda-engine-rest`) and the Camunda applications Cockpit, Tasklist and Admin (artifact name like `camunda-webapp`).

Uninstall the Camunda EAR. Its name should be `camunda-oracle-weblogic-ear-$PLATFORM_VERSION.ear`.

# 2. Replace Camunda Core Libraries

After shutting down the server, replace the following libraries in `$WLS_DOMAIN_HOME/lib` with their equivalents from `$WLS_DISTRIBUTION/modules/lib`:

* `camunda-engine-$PLATFORM_VERSION.jar`
* `camunda-bpmn-model-$PLATFORM_VERSION.jar`
* `camunda-cmmn-model-$PLATFORM_VERSION.jar`
* `camunda-xml-model-$PLATFORM_VERSION.jar`

Add or replace (if already present) the following libraries:

* `camunda-engine-dmn-$PLATFORM_VERSION.jar`
* `camunda-engine-feel-api-$PLATFORM_VERSION.jar`
* `camunda-engine-feel-juel-$PLATFORM_VERSION.jar`
* `camunda-dmn-model-$PLATFORM_VERSION.jar`
* `camunda-commons-logging-$COMMONS_VERSION.jar`
* `camunda-commons-typed-values-$COMMONS_VERSION.jar`
* `camunda-commons-utils-$COMMONS_VERSION.jar`

# 3. Replace Optional Camunda Dependencies

In addition to the core libraries, there may be optional artifacts in `$WLS_DOMAIN_HOME/lib` for LDAP integration, Camunda Spin, and Groovy scripting. If you use any of these extensions, the following upgrade steps apply:

## LDAP integration

Copy the following libraries from `$WLS_DISTRIBUTION/modules/lib` to the folder `$WLS_DOMAIN_HOME/lib`, if present:

* `camunda-identity-ldap-$PLATFORM_VERSION.jar`

## Camunda Spin

Copy the following libraries from `$WLS_DISTRIBUTION/modules/lib` to the folder `$WLS_DOMAIN_HOME/lib`, if present:

* `camunda-spin-core-$SPIN_VERSION.jar`

## Groovy Scripting

Copy the following libraries from `$WLS_DISTRIBUTION/modules/lib` to the folder `$WLS_DOMAIN_HOME/lib`, if present:

* `groovy-all-$GROOVY_VERSION.jar`

# 4. Maintain the BPM Platform Configuration

If you have previously replaced the default BPM platform configuration by a custom configuration following any of the ways outlined in the [deployment descriptor reference][configuration-location], it may be necessary to restore this configuration. This can be done by repeating the configuration replacement steps for the upgraded platform.

# 5. Maintain Process Engine Configuration

This section describes changes in the engine’s default behavior. While the change is reasonable, your implementation may rely on the previous default behavior. Thus, the previous behavior can be restored for shared process engines by explicitly setting a configuration option.

<< Add necessary process engine configuration changes as subsections here >>

# 6. Maintain Process Applications

This section describes changes in behavior of API methods that your process applications may rely on.

<< Add necessary application changes as subsections here >>

## Incident Handler

The interface of an [Incident Handler]({{< relref "user-guide/process-engine/incidents.md" >}}) has changed. Instead of a long parameter list, the methods pass a context object which bundles all required informations, like process definition id, execution id and tenant id. Since the existing methods have been overridden, custom implementations of an incident handler have to be adjusted.

## Correlation Handler

A new method has been added to the interface of a {{< javadocref page="?org/camunda/bpm/engine/impl/runtime/CorrelationHandler.html" text="Correlation Handler" >}}. The new method `correlateStartMessage()` allows to explicit trigger a message start event of a process definition. If the default implementation is replaced by a custom one then it have to be adjusted.

# 7. Install the Camunda Archive

Install the Camunda EAR, i.e., the file `$WLS_DISTRIBUTION/modules/camunda-oracle-weblogic-ear-$PLATFORM_VERSION.ear`.

# 8. Install the Web Applications

## REST API

Deploy the web application `$WLS_DISTRIBUTION/webapps/camunda-engine-rest-$PLATFORM_VERSION-wls.war` to your Oracle WebLogic instance.

## Cockpit, Tasklist, and Admin

Deploy the web application `$WLS_DISTRIBUTION/webapps/camunda-webapp-ee-wls-$PLATFORM_VERSION.war` to your Oracle WebLogic instance.

[configuration-location]: {{< relref "reference/deployment-descriptors/descriptors/bpm-platform-xml.md" >}}
[upgrade-guide]: {{< relref "update/minor/74-to-75/index.md" >}}
