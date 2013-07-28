---
layout: content
menu: user-manual
title: Gson Integration
---

# Gson Integration

ModelMapper's Gson integration allows you to map a Gson [JsonElement](http://google-gson.googlecode.com/svn/tags/1.2.3/docs/javadocs/com/google/gson/JsonElement.html) to a JavaBean.

### Setup

To get started, add the `modelmapper-gson` Maven dependency to your project:

{:.prettyprint .lang-xml}
	<dependency>
	  <groupId>org.modelmapper</groupId>
	  <artifactId>modelmapper-gson</artifactId>
	  <version>{{ site.version }}</version>
	</dependency>
	
Next, configure ModelMapper to support the JsonElementValueReader, which allows for values to be read and mapped from a Gson [JsonElement](http://google-gson.googlecode.com/svn/tags/1.2.3/docs/javadocs/com/google/gson/JsonElement.html):

{:.prettyprint .lang-java}
    modelMapper.getConfiguration().addValueReader(new JsonElementValueReader());
	
### Example Usage

Consider the following JSON representing an order:

{:.prettyprint .lang-json}
    {
      "id": 456,
      "customer": {
        "id": 789,
        "street_address": "123 Main Street", 
        "address_city": "SF"
      }
    }

We may need to map this to a different object model:

{:.prettyprint .lang-java}
	// Assume getters and setters are present

    public class Order {
      private int id;
      private Customer customer;
    }

    public class Customer {
	  private Address address;
    }

    public class Address {
      private String street;
	  private String ciity;
    }
    
Since the order JSON in this example uses an underscore naming convention, we'll need to configure ModelMapper to tokenize source property names by underscore:

{:.prettyprint .lang-java}
    modelMapper.getConfiguration().setSourceNameTokenizer(NameTokenizers.UNDERSCORE);

With that set, mapping a JsonElement for the order JSON to an Order object is simple:

{:.prettyprint .lang-java}
    JsonElement orderElement = new JsonParser().parse(orderJson);
	Order order = modelMapper.map(orderElement, Order.class);
	
And we can assert that values are mapped as expected:

{:.prettyprint .lang-java}
    assertEquals(order.getId(), 456);
    assertEquals(order.getCustomer().getId(), 789);
    assertEquals(order.getCustomer().getAddress().getStreet(), "123 Main Street");
    assertEquals(order.getCustomer().getAddress().getCity(), "SF");
    
### Things to Note

ModelMapper maintains a [TypeMap](http://modelmapper.org/javadoc/org/modelmapper/TypeMap.html) for each source and destination type, containing the mappings bewteen the two types. For "generic" types such as JsonElement this can be problematic since the structure of a JsonElement can vary. In order to distinguish structurally different JsonElements that map to the same destination type, we can provide a _type map name_ to ModelMapper.

Continuing with the example above, let's map another order JSON, this one with a different structure, to the same Order class:

{:.prettyprint .lang-json}
    {
      "id": 222,
      "customer_id": 333,
      "customer_street_address": "444 Main Street",
      "customer_address_city": "LA"
    }
    
Mapping this JSON to an order is simple, but we'll need to provide a _type map name_ to distinguish this JsonElement to Order mapping from the previous unnamed mapping:

{:.prettyprint .lang-java}
    JsonElement orderElement = new JsonParser().parse(flatOrderJson);
	Order order = modelMapper.map(orderElement, Order.class, "flat");
	
Here we've used the name "flat" to represent the structure of the JsonElement that we're mapping to an Order.