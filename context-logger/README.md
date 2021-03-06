> This document contains documentation for the tracee context-logger modules. Click [here](/README.md) to get an overview that TracEE is about.

# context-logger

> __The TracEE context logger subproject helps you to analyze errors in your application by collecting contextual invocation data and writing it to your log files.


The *TracEE* main project helps you to aggregate all your log files. This will technically enable you to track user requests or session throughout the log but does not have an influence on what will be written to the logs.
In case if an error occurs in your application, the context logger subproject collects contextual log information and writes it to your log files.
Like the *TracEE* main project, it integrates with almost no effort - it contains adapters or interceptors for the most popular JavaEE technologies.

* servlet 2.5+
* jax-ws
* jax-rs2
* cdi / ejb
* jms
* AspectJ / Spring-AOP
* Spring

This project is still in an experimental stage and the api may change during further development.

# Getting started

## The contextual information data

So what is this all about?

An example:
- A user is using your JavaEE application. After a short time an exception was thrown in the application server. This could happen due to a lot of reasons (corrupt data, error in software, infrastructure problems).
- If you are monitoring your logfiles, you are going to detect the error on your own, otherwise if you have luck, the user will notify you about the error.
- Now your operations employees or developers come into play and are trying to analyze the cause of the error. This can be a very difficult and time consuming task. Success and speed are directly affected by the information you are able to gather from your log files.

The TracEE context logger subproject will help you to easily provide contextual invocation data, if an error occurs in your application. Therefore it is providing handlers for most JavaEE technologies. Those handler will provide the contextual data in JSON Format, so the output can easily read and processed by automated systems.

It is also possible and very easy to add custom functionality and handlers to the contextual logger.

# Integrating TracEE into your application

The steps to get TracEE contextual logging up and running pretty much depend on your application scenario. The most common use case would be to provide contextual invocation data from a servlet container based frontend or an ejb based backend.

## Integration scenarios

| Framework    | Adapter |
| ----------:  |:------:|
| Servlet      | Use [context-logger-servlet](context-logger-servlet) as a servlet filter. |
| EJB3/CDI/JMS | Use [context-logger-javaee](context-logger-javaee) as an interceptor |
| JAX-RS       | Use [context-logger-servlet](context-logger-servlet) as a servlet filter. |
| JAX-RS2      | TODO |
| JAX-WS       | Use [context-logger-jaxws](context-logger-jaxws) as a message listener. |
| SPRING-AOP   | Use [context-logger-watchdog] (context-logger-watchdog) by offering an aspect triggered by a simple java annotation|
| ASPECTJ      | Use [context-logger-watchdog] (context-logger-watchdog) by offering an aspect triggered by a simple java annotation|



## Modules

TracEE context logger is built highly modular. The modules you need depend on your application and the underlying frameworks and containers.
The following table describes all available TracEE-context-logger modules and their usage scenarios.

| Module                                | Usage |
|--------------------------------------:|:-----:|
| [context-logger-connector-api](context-logger-connector-api)        | Provides an api for developing custom connectors |
| [context-logger-connectors](context-logger-connectors)              | Provides support for writing contextual data to other target as log files (f.e. send error via Http) |
| [context-logger-impl](context-logger-impl)                          | The implementation of the context logger |
| [context-logger-integration-test](context-logger-integration-test)  | Does some integration test for custom data providers |
| [context-logger-javaee](context-logger-javaee)                      | Provides support for EJB / CDI /JMS by offering interceptors |
| [context-logger-jaxws](context-logger-jaxws)                        | Provides support for JAXWS via Message handlers. |
| [context-logger-provider-api](context-logger-provider-api)          | Provides support for Servlets via ServletFilter |
| [context-logger-servlet](context-logger-servlet)                    | Provides support for Servlets via ServletFilter |
| [context-logger-watchdog](context-logger-watchdog)                  | AspectJ / Spring-AOP powered contextual logging |

## Configuration

### Output Profile Selection
The output produced by the TracEE contextual logger can be configured very flexible by allowing you to choose between predefined and custom profiles or by offering you the possibility to overwrite profile settings by system properties.
You can choose between those profiles by setting the **io.tracee.contextlogger.profile** system property in your application server or by adding a **ProfileSelector.properties** file to your webapp. The property file must define the **io.tracee.contextlogger.profile** property. 
Possible values are: 

| Value    | Description |
| --------:|:-----------:|
| BASIC    | Default profile, provides most important and secure contextual information.  |
| ENHANCED | Provides additional contextual information not included in basic profile     |
| FULL     | Outputs almost everything available to the handlers - even security related data, therefore this profile shouldn't be used in production environments  |
| CUSTOM   | Allows you to use a custom profile. Custom profiles are defined by adding a file named **TraceeContextLoggerCustomProfile.properties**. Please copy and customize existing profiles offered by the context-logger-impl project |

You can combine those configurations in any way you want. The settings will be applied in a specific order. Application server wide profile selection will be overwritten by application based profile selection. If no profile selection is configured, basic profile will be used as default.

### TracEE common context information
Please add the following system properties to your application server configuration:

| Property name | Description |
|--------------:|:-----------:|
| io.tracee.contextlogger.tracee-standard-stage| Defines the stage of the system - for example DEV, INT, TEST,...|
| io.tracee.contextlogger.tracee-standard-system| Defines the name of the system|
