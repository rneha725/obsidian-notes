It happens when we use ORMs like hibernate. Say you have an entity which has a one-to-many/many-to-one dependency on the other table/entity.

When we use ORM to do this:

```java
List<Order> orders = orderRepository.fetchAll();

// Order has dependency on customer has many to one
```

As ORMs by default do lazy loading, when we fire this

```java
for (Order order: orders) order.getCustomer().getName(); //ORMS does the fetch here
```

Now one query becomes N queries because of the child data.

### Resolution
- Make sure you are logging your queries
- Use custom query and use joins
- Use entity graph for eager fetching some of the attributes of a class

----
### Rough

```java
List<Customer> customers = customerRepository().fetchAll()
```

Customer:
```java
@OneToMany
List<Order> orders;
```

This will be problematic:

```java
for(Customer customer: customers) customer.getOrders() // fetches again
```