---
layout: content
menu: user-manual
title: Integrations
---

# Integrations

ModelMapper's [API](/user-manual/api-overview/) and [SPI](/user-manual/spi-overview/) allow for simple the integration with 3rd party libraries. Several 3rd party integrations are available.

### Provider Integrations

[Providers](/user-manual/providers) allow you to provide you own instance of destination objects prior to mapping. ModelMapper has several 3rd party integrations that allow for external libraries to provide destination objects:

* [Spring](/user-manual/spring-integration)
* [Guice](/user-manual/guice-integration)
* [Dagger](/user-manual/dagger-integration)

### Value Reader Integrations

Value Readers allow you to read and map values from different types of source object, aside from typical JavaBeans. ModelMapper has several 3rd party integrations that allow for the mapping of values from various types of source objects:

* [jOOQ](/user-manual/jooq-integration)
* [Jackson](/user-manual/jackson-integration)
* [Gson](/user-manual/gson-integration)