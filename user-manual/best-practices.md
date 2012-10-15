---
layout: user-manual
title: Best Practices
---

# Best Practices

#### Reuse ModelMapper instances where possible

Each [ModelMapper](http://modelmapper.org/javadoc/org/modelmapper/ModelMapper.html) instance contains a set of [TypeMap](http://modelmapper.org/javadoc/org/modelmapper/TypeMap.html) instances, each of which contain the mapping of properties between a specific source and destination type. While the cost associated with creating a `TypeMap` is incurred only once for a pair of types, this applies only to the specific `ModelMapper` instance that created the `TypeMap`.

Unless you have use cases where you need different mappings between the same types, then it's best to _re-use the same `ModelMapper` instance_. If you use a dependency injection container, you can accomplish this by configuring `ModelMapper` as a singleton.

#### Validate mappings via tests

Always write tests to ensure that mapping between two types works expected. ModelMapper's built-in [validation](/user-manual/validation/) can help with this.