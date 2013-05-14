---
layout: user-manual
title: Extensions
---

# Extensions

ModelMapper's [API](/user-manual/api-overview/) and [SPI](/user-manual/spi-overview/) allow for simple the creation of extensions. The core extensions provided along with the library are described below.

### Spring

The Spring extension allows for the provisioning of destination objects to be delegated to a Spring BeanFactory during the mapping process. To use the extension, first obtain a Provider for a Spring BeanFactory:

{:.prettyprint .lang-java}
	Provider<?> springProvider = SpringIntegration.fromSpring(beanFactory);

Then configure the Provider for to be used globally for a ModelMapper:

{:.prettyprint .lang-java}
	modelMapper.getConfiguration().setProvider(springProvider);

Or set the Provider to be used for a specific TypeMap:

{:.prettyprint .lang-java}
	typeMap.setProvider(springProvider);

The provider can also be used for individual mappings:

{:.prettyprint .lang-java}
	with(springProvider).map().someSetter(source.someGetter());

### Guice

The Guice extension allows for the provisioning of destination objects to be delegated to a Guice Injector during the mapping process. To use the extension, first obtain a Provider for a Guice injector:

{:.prettyprint .lang-java}
	Provider<?> guiceProvider = GuiceIntegration.fromGuice(injector);

Then configure the Provider for to be used globally for a ModelMapper:

{:.prettyprint .lang-java}
	modelMapper.getConfiguration().setProvider(guiceProvider);

Or set the Provider to be used for a specific TypeMap:

{:.prettyprint .lang-java}
	typeMap.setProvider(guiceProvider);

The provider can also be used for individual mappings:

{:.prettyprint .lang-java}
	with(guiceProvider).map().someSetter(source.someGetter());