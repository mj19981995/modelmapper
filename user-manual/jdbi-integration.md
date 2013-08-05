---
layout: content
menu: user-manual
title: JDBI Integrations
---

# JDBI Integration

ModelMapper supports integration with [JDBI](http://jdbi.org) natively, allowing JDBI results to be mapped to JavaBeans.

## Example Mapping

Consider the following record representing an order:

order_id|customer_id|customer_street_address|customer_address_city
-------|-----------
345|678|123 Main Street|SF

We may need to map this to a more complex object model:

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

Since the source record's fields in this example uses an underscore naming convention, we'll need to configure ModelMapper to tokenize source property names by underscore:

{:.prettyprint .lang-java}
    modelMapper.getConfiguration().setSourceNameTokenizer(NameTokenizers.UNDERSCORE);

With that set, mapping order records to Order objects is simple:

{:.prettyprint .lang-java}
    List<Map<String, Object>> orderRecords = handle.select("SELECT * FROM orders");
    List<Order> orders = modelMapper.map(orderRecords, new TypeToken<List<Order>>(){}.getType());
    Order order = orders.get(0);
	
And we can assert that values are mapped as expected:

{:.prettyprint .lang-java}
    assertEquals(order.getId(), 456);
    assertEquals(order.getCustomer().getId(), 789);
    assertEquals(order.getCustomer().getAddress().getStreet(), "123 Main Street");
    assertEquals(order.getCustomer().getAddress().getCity(), "SF");