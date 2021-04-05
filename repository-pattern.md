### Repository Pattern

In Domain Driven Design, it’s important to emphasize that we should only define one repository for each aggregate root.

- To achieve the goal of the aggregate root to maintain transactional consistency between all the objects within the aggregate, you should never create a repository for each table in the database.

- In Domain Driven Design, it’s recommended that we define and place the repository interfaces in the domain model layer, so the application layer, such as Web API micorservice, does not depend directly on the infrastructure layer where we’ve implemented the actual repository classes.

By doing this and using Dependency Injection in the controllers of our Web API, we can implement mock repositories that return fake data instead of data from the database. This decoupled approach allows us to create and run unit tests that focus the logic of our application without requiring connectivity to the database.

> A repository performs the tasks of an intermediary between the domain model layers and data mapping, acting in a similar way to a set of domain objects in memory. Client objects declaratively build queries and send them to the repositories for answers. Conceptually, a repository encapsulates a set of objects stored in the database and operations that can be performed on them, providing a way that is closer to the persistence layer. Repositories, also, support the purpose of separating, clearly and in one direction, the dependency between the work domain and the data allocation or mapping. - Patterns of Enterprise Application Architecture, Martin Fowler

### DAO Pattern

A typical DAO implementation has the following components:

- A DAO factory class.
- A DAO interface.
- A concrete class that implements the DAO interface.
- Data transfer object (sometimes called Value Objects)

DAO's are related to Persistence layer, not domain.

#### Dao Actors

**BusinessObject**

The BusinessObject represents the data client. It is the object that requires access to the data source to obtain and store data. A BusinessObject may be implemented as a session bean, entity bean, or some other Java object, in addition to a servlet or helper bean that accesses the data source.

**DataAccessObject**

The DataAccessObject is the primary object of this pattern. The DataAccessObject abstracts the underlying data access implementation for the BusinessObject to enable transparent access to the data source. The BusinessObject also delegates data load and store operations to the DataAccessObject.

**DataSource**

This represents a data source implementation. A data source could be a database such as an RDBMS, OODBMS, XML repository, flat file system, and so forth. A data source can also be another system (legacy/mainframe), service (B2B service or credit card bureau), or some kind of repository (LDAP).

**TransferObject**

This represents a Transfer Object used as a data carrier. The DataAccessObject may use a Transfer Object to return data to the client. The DataAccessObject may also receive the data from the client in a Transfer Object to update the data in the data source.

#### Data Transfer Object

- Data Transfer Object is only a class that contains data with getter/setter methods.
- It contains the **serialization** and **deserialization** mechanism.
- DTO doesn’t contain the business logic.

1. Layered achitecture pattern

Normally, we will use DTOs between multiple processes, or systems. So, in this architecture, we will use it in some places.

- Receive data from Client to Server

DTO will be used in Spring’s RestController. It will be used to map request to DTO.

- Send data from Server to Client

Layered architectural pattern will use database-centric approach. It means that our business logic will lean toward entities. Then, some developers can use an entity with APIs to expose data to client. But it is a bad idea because:

- Tightly coupling between our persistent model and APIs. It’s difficult to maintain when new requirements always change, then data that Client wants will be also modified.

- We can expose what Client does not want, and can event leak the important information.

To know more about the problem exposing entities in API, we can see the following link.

To avoid encountering this problem, we will use DTO to hide all the core data to the world.

2. Domain Driven design
   The way to use DTO in Domain Driven design is as almost same as the Layered architecture pattern.

- Receive data from Client
- To send data from Server to Client, we need to convert Domain Model to DTO, not using entities.
- DDD uses Bounded Context concept to isolate subdomains. So to communicate between other Bounded Contexts, we will use DTOs to loose coupling between subdomains.

#### When to Use Dto

In Domain-driven design, we need to use DTO to communicate between bounded contexts, and return data to Client.

In Restful system, we usually utilize DTO pattern to transfer data between client and server.

#### Benefits of Dto

- Encapsulate the serialization mechanism for transferring data over the wire. By encapsulating the serialization like this, the DTOs keep this logic out of the rest of the code and also provide a clear point to change serialization should you wish.

- We only pass the necessary data from Server to Client without full fields from Domain Model, or Entity. So, the less data passed through network, the less bandwidth that is taken. And we doesn’t expose too much data to the end user, it prevent the leak other important information.

- Decoupling between APIs with our persistent model - entities in Layered architectural pattern, or APIs with Domain Model in Domain Driven design, …

To remove boilerplate code for mapping data to get DTOs, using ModelMapper or MapStruct libraries.

#### See also

- [Clean Architecture](https://ducmanhphan.github.io/2020-08-08-Clean-architecture/)
- [clean-architecture-example](https://github.com/mattia-battiston/clean-architecture-example/)
- [Catalog of Patterns of Enterprise Application Architecture](https://martinfowler.com/eaaCatalog/index.html)
- [P of EAA: Data Transfer Object](https://martinfowler.com/eaaCatalog/dataTransferObject.html)
- [P of EAA: Repository](https://martinfowler.com/eaaCatalog/repository.html)
- [P of EAA: Service Layer](https://martinfowler.com/eaaCatalog/serviceLayer.html)
- [Java DAO vs Repository Patterns](https://www.baeldung.com/java-dao-vs-repository)
- [The DAO Pattern in Java](https://www.baeldung.com/java-dao-pattern)
- [Repository Pattern](https://ducmanhphan.github.io/2019-04-28-Repository-pattern/)
- [DAO Pattern](https://ducmanhphan.github.io/2019-02-15-DAO-pattern-in-java/)
- [DTO](https://ducmanhphan.github.io/2020-12-10-data-transfer-object/)
- [Core J2EE Patterns - Data Access Object](https://www.oracle.com/java/technologies/dataaccessobject.html)
- [The difference between Repository pattern and DAO pattern](https://ducmanhphan.github.io/2019-04-28-Repository-pattern/#the-difference-between-repository-pattern-and-dao-pattern)
- [자바 직렬화, 그것이 알고싶다. 훑어보기편](https://woowabros.github.io/experience/2017/10/17/java-serialize.html)
- [자바 직렬화, 그것이 알고싶다. 실무편](https://woowabros.github.io/experience/2017/10/17/java-serialize2.html)
