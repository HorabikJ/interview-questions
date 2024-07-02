# Questions

## How does computers work

1. The difference between thread and process
2. Do processes have separate isolated memory designated for them or do they not?
3. How do threads share the memory?

## Databases

1. The difference between `WHERE` and `HAVING`: [link](https://www.shiksha.com/online-courses/articles/difference-between-where-and-having-clause/)
2. The deadlock example on database.
3. Optimistic and pessimistic locking.
4. What `CROS JOIN` is and what is "iloczyn kartezjański".
5. Transaction isolation levels in databases: [link](https://db-diary.blogspot.com/2014/03/transakcje-i-ich-poziomy-izolacji-mysql.html)
6. Table organized by rows and table organized by columns: 
[link-1](https://www.fivetran.com/learn/columnar-database-vs-row-database),
[link-2](https://columnar.docs.hydra.so/organize/data-modeling/row-vs-column-tables)
7. Difference between sub-query aka. inner select and `SELF JOIN`, which one has better performance?
8. How would you measure the cost of a given query?
   * In Postgres we can use `explain` or `explain analyze` command.
9. Isolation levels in databases:
   * `READ UNCOMMITED` 
   * `READ COMMITED`
   * `REPEATABLE READS`
   * `SERIALIZABLE` 

## Multithreading

1. Deadlock and Livelock definition: [link](https://www.baeldung.com/cs/deadlock-livelock-starvation)

## Hibernate

1. What does `merge()` function do and why do not use it.
2. What does it mean that an entity is in a `Detached` state.
3. Good practises when implementing `@..ToMany` relations:
    * initializing Collection in a constructor/at class field level:
    ```java
    @ManyToMany
    private Set<OrderLine> orderLines = new HashSet<>();
    ```
    * implementing helper method to set the relation in bidirectional relation:
    ```java
    public void associateOrderLine(OrderLine orderLine) {
        orderLines.add(orderLine);
        orderLine.setOrderHeader(this);
    }
    ```
   
4. Hibernate `N+1` problem, when may it happen?
    
    In `Order` entity there is a `@ManyToOne` relation with `Customer`:
    ```java
    @Entity
    public class Order {

        @ManyToOne
        private Customer customer;
        
        //...other fields
    }
    ```
    And then we write a queries like below:
    ```java
    //first we fetch the customer
    Customer customer = customerRepository.findByName(CUSTOMER_NAME).get();
    //and then all order headers that this customer have, this is where we have the problem of N+1 queries
    List<Order> orders = orderRepository.findAllByCustomer_Name(customer.getName());
    ```

5. Hibernate `N+1` problem solutions:

    * `FetchType.EAGER`
    Bad practise -> tight coulping, every time we are fetching given entity we are fetching also all entitief from 
    the `@..ToMany` relation.
    * `@EntityGraph` annotation:
    Best solution, much faster than `LEFT JOIN FETCH` from my experiments. 
    ```java
    @EntityGraph(attributePaths = {"orderLines"})
    @Query("SELECT oh FROM OrderHeader oh WHERE oh.customer.name = :customerName")
    List<OrderHeader> findAllByCustomer_Name_withEntityGraph(String customerName);
    ```
    * `LEFT JOIN FETCH` clause:
    ```java
    @Query("SELECT oh FROM OrderHeader oh LEFT JOIN FETCH oh.orderLines WHERE oh.customer.name = :customerName")
    List<OrderHeader> findAllByCustomer_Name_withLeftJoinFetch(String customerName);
    ```

## Kubernetes:

1. Difference between stateless and stateful resources. Example, why `StatefulSet` is stateful and `Deployment` 
   is stateless?
