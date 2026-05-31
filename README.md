# Preface

## Microservices

- An approach to **distributed systems** that promote the use of finely grained services **that can be changed, deployed, and released INDEPENDENTLY**.
  - **Note:** As a distributed system, they bring a host of **complexity**.
- Provide us with a huge number of **options** for building out systems, giving us a lot of **flexibility**.
- **Trade off** between flexibility and complexity.

## Why Sam Newman Wrote This Book

- Microservices have become, for many, the **default architectural choice**. This is something that is **hard to justify**.

## Navigating This Book

- The **main body** of the book is broken into three separate parts:
  1. Foundation
  2. Implementation
     - Chapter 5 - Technologies used to implement **inter-microservice communication**.
     - Chapter 6 - Comparison of **sagas** and **distributed transactions** and discusses their usefulness in **modeling business processes** involving **multiple** microservices.
     - Chapter 9 - Challenges of testing microservices, including the issues caused by **end-to-end tests**.
     - Chapter 11 - Microservice architectures can **increase the surface area of attack** but also give us more opportunity to **defend in depth**.
     - Chapter 13 - **Four aspects of scaling** and show how they can be used in combination.
  3. People

# Part I. Foundation

# Chapter 1. What Are Microservices?

## Microservices at a Glance

- Modeled around **business domain**.
- **A type of service-oriented architecture (SOA)**, albeit one that is **opinionated about how service boundaries should be drawn**, and one in which **independent deployability** is key.
- Hosts business functionality on one or more **network endpoints**.
- Microservice architectures **AVOID** the use of **shared databases**.
- A microservice exposing its functionality over a **REST API** and a **topic**:
  ![figure-1-1-a-microservice-exposing-its-functionality-over-a-rest-api-and-a-topic](images/figure-1-1-a-microservice-exposing-its-functionality-over-a-rest-api-and-a-topic.png)
- Microservices embrace the concept of **information hiding** (similar to the **encapsulation** in OOP), meaning hiding as much information as possible inside a component.
  - **Hexagonal Architecture** pattern - The importance of keeping the internal implementation separate from its external interfaces, with the idea that you might want to interact with the same functionality over different types of interfaces.
- \*Having a **stable service boundaries** that don't change when the internal implementation changes results in systems that have **looser coupling** and **stronger cohesion**.

### Are SOA and Microservices Different Things?

#### SOA

- **Multiple services collaborate** to provide **a certain end set** of capabilities.
- **Lack of good consensus** on how to do SOA well.
- **Problems** of SOA are:
  - Vendor middleware
  - A lack of guidance about service granularity
  - Wrong guidance on picking places to split your system (Unstable service boundaries)
- Oftentimes, services are **coupled to a database** and had to **deploy everything together**. So, it's **not microservices**.

#### Microservices

- Think of it as being a **specific approach for SOA**.

## Key Concepts of Microservices

### \*Independent Deployability

- Can make a change to a microservice, deploy it, and release that change to our users, without having to deploy any other microservices.
- **Simple idea** that is nonetheless **complex in execution**. (Simple is not easy)
- **\*Key takeaway:** Embrace the concept of independent deployability.
- To achieve:
  - Need to make sure our microservices are **loosely coupled**.
  - Need explicit, well-defined, and **stable contracts** between services.
  - **Note:** There are so many other things you have to get right that in turn have their own benefits.

### Modeled Around a Business Domain

- **Domain-driven design (DDD)** is a technique to define **service boundaries**.
- When **service boundaries** are **not well-defined**...
  - **Rolling out a feature** that requires changes to **more than one microservice** is **expensive**. You need to coordinate the work across each service (and potentially across separate teams) and carefully **manage the order** in which the new versions of these services are deployed.
  - **Layered architecture** - Each service boundary based on related **technical functionality**.
    - Figure 1-3. Making a change across all three tiers is more involved

    ![figure-1-3-making-a-change-across-all-three-tiers-is-more-involved](images/figure-1-3-making-a-change-across-all-three-tiers-is-more-involved.png)

- With microservices, we **prioritize high cohesion of business functionality** over high cohesion of technical functionality.

### Owning Their Own State

- To make independent deployability a reality, we need to ensure that we **limit backward-incompatible changes** to our microservices.
- **Important:** **Sharing databases** is one of the **WORST** things you can do if you're trying to achieve independent deployability.
- **Objective:** To reduce the effort needed to change business-related functionality.

### Size

- Chris Richardson opinion - The goal of microservices is to have "as small an interface as possible."
- But, the concept of size is **highly contextual**.
- Not to worry about size. Rather **focus on**:
  1. How many microservices can you handle? As you have more services, the complexity of your system will increase.
     - Adopt **incremental migration** to microservice architecture.
  2. How do you define **service boundaries** (**loose coupling**, **high cohesion**)?

### Flexibility

- Have a **cost**.
- **Four aspects** of flexibility:
  1. Organizational
  2. Technical
  3. Scale
  4. Robustness
- Think of adopting microservices as less like flipping a switch, and more like turning a dial.

### Alignment of Architecture and Organization

- **Conway's law** - Organizations which design systems are constrained to produce designs which are copies of the communication structures of these organizations.
- Group people in **poly-skilled teams** to reduce handoffs and silos.
- **Three tiers** - An architecture that has **high cohesion of related technology** but **low cohesion of business functionality**.
- To make it easier to make changes, instead we **need to change how we group code, choosing cohesion of business functionality** rather than technology.
- A **dedicated team** that has **full-end-to-end responsibilities** for making changes.
  - Figure 1-4. The UI is broken apart and is owned by a team that also manages the server side functionality that supports the UI

    ![figure-1-4-the-ui-is-broken-apart-and-is-owned-by-a-team-that-also-manages-the-server-side-functionality-that-supports-the-ui](images/figure-1-4-the-ui-is-broken-apart-and-is-owned-by-a-team-that-also-manages-the-server-side-functionality-that-supports-the-ui.png)

## The Monolith

- To understand whether **microservices** are **worth considering**.
- Referring to **a unit of deployment**. When all functionality in a system must be deployed together.
- Variations of monolith:
  - Single-process monolith
  - Modular monolith
  - Distributed monolith

### The Single-Process Monolith

- You may have two or more monoliths that are tightly coupled to one another, potentially with some vendor software in the mix.
- Even as the organization grows, the monolith **can potentially grow with it** (modular monolith).

### The Modular Monolith (A Middle Ground)

- A **subset** of the single-process monolith.
- Single process consists of **separate modules**.
- If the **module boundaries** are well defined, it can allow for a high degree of **parallel work**, while **avoiding the challenges** of the more **distributed microservice** architecture by having a much **simpler deployment topology**.
- **Challenge** - **Database** tends to **lack the decomposition** we find in the code-level, leading to **significant challenges** if you want to **pull apart** the monolith in the future.
  - **Solution** - Having the database decomposed along the same lines as the modules.
  - Figure 1-8. A modular monolith with a decomposed database:

    ![figure-1-8-a-modular-monolith-with-a-decomposed-database](images/figure-1-8-a-modular-monolith-with-a-decomposed-database.png)

### The Distributed Monolith

- **Analogy** - The failure of a computer you **didn't even know existed** (e.g. other microservices) can render your own computer unusable.
- Has **all the disadvantages** of a distributed system, and the disadvantages of a single-process monolith, without having enough of the upsides of either.
- Not enough focus was placed on concepts like **information hiding** and **cohesion of business functionality**.
- Cause changes **to ripple across** service boundaries.

### Monolith and Delivery Contention

- When **more people** are working on the same codebase, we face challenges of **confused line of ownership**.
- **Microservice architecture** does give you more **concrete boundaries** around which ownership lines can be drawn in a system.

### Advantages of Single-Process or Modular Monolith

- Simpler/Simplify...
  - Deployment topology
  - Developer workflows
  - Monitoring
  - Troubleshooting
  - End-to-end testing
  - Code reuse
- To **reuse code** within a **distributed system**, we need to decide whether we want to:
  1. Copy code
  2. Break out libraries
  3. Push the **shared** functionality into a **service**
- Monolith is the sensible default choice as an architectural style.

## Enabling Technology

- **Understanding the tools** that are available to help you get the most out of this architecture is going to be a **key part** of making any implementation of microservices a success.

### \*Log Aggregation and Distributed Tracing

- Treat **log aggregation** system as a **PREREQUISITE** for adopting a microservice architecture.
- To make these log aggregation tools even more useful by implementing **correlation IDs**, in which a single ID is used **for a related set of service calls**.
- As your system **grows in complexity**, it becomes **essential to consider tools** that allow you to better explore what your system is doing, providing the ability to **analyze traces** across multiple services, detect bottlenecks, and ask questions of your system.

| Tool                                | Product                                                                                                                                                                                                                                                                                                                                   |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \*Log aggregation                   | - **Simple logging services** provided by **public cloud vendors**, such as [Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/fundamentals/overview) (might be good enough to get started).<br />- [Falcon LogScale](https://www.crowdstrike.com/platform/next-gen-siem/falcon-logscale/) (formerly known as _Humio_) |
| Distributed tracing (OpenTelemetry) | - [Jaeger](https://www.jaegertracing.io/)<br />- [Lightstep](https://docs.lightstep.com/), [Honeycomb](https://www.honeycomb.io/) (**new generation** of tools)                                                                                                                                                                           |

### Containers and Kubernetes

- Normal virtualization techniques (VM) can be quite **heavy**.
- **Containers** are...
  - Lightweight
  - Faster spin-up times
  - Cost effective
- If you do end up adopting Kubernetes, ensure that **someone else** is running the Kubernetes cluster for you.
- Running your own Kubernetes cluster can be a **significant amount of work**!

### Streaming

| Tool              | Product                                                                                                                                                                                      |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Data streaming    | - [Apache Kafka](https://kafka.apache.org/) (**the de facto choice** for streaming data)<br />- [Debezium](https://debezium.io/) (help stream data from **existing datasources** over Kafka) |
| Stream-processing | - [Apache Flink](https://flink.apache.org/)                                                                                                                                                  |

### Public Cloud and Serverless

- As your microservice architecture grows, **more work** will be pushed into the **operational space**.
- **Serverless** - Hide the underlying machines, allowing you **to work at a higher level of abstraction**.
- Examples of **serverless products**:
  - Message brokers
  - Storage solutions
  - Databases
  - Function as a Service (FaaS) - Provide a **nice abstraction** around the **deployment of code**.

## Advantages of Microservices

### Technology Heterogeneity

- Can decide to **use different technologies** inside each one.
- Able to more quickly adopt technologies.

### Robustness

- As long as a **failure doesn't cascade**, you can isolate the problem.
- To make microservices more robust, we need to understand the **new sources of failure** that distributed systems have to deal with (e.g. **network failure**).

### Scaling

- By splitting out **core parts** of a monolith system, we can solve its scaling problem.
- By using a public cloud provider, we can even scale **on demand**.

### Ease of Deployment (For New Changes)

- Allows us to get our code deployed more quickly.
- Making fast rollback easy to achieve.

### Organization Alignment

- Smaller teams working on smaller codebases tend to be more productive.

### Composability

- Open up opportunities for **reuse** of functionality.
- Allow for our functionality to be consumed in different ways for different purposes.
- **Microservices** - **Opening up seams** in our system that are addressable by outside parties.
- **Monolith** - Often have **one coarse-grained seam** that can be used from the outside.

## Microservice Pain Points

### Developer Experience

- More **resource-intensive runtimes** like the JVM can limit the number of microservices that can be run on a single developer machine.
- Developing in the cloud - **Feedback cycles** can suffer greatly.

### Technology Overload

- Need to spend a lot of time understanding issues around...
  - Data consistency
  - Latency
  - Service modeling

### Reporting

- Figure 1-12. reporting carried out directly on the database of a **monolith**

  ![figure-1-12-reporting-carried-out-directly-on-the-database-of-a-monolith](images/figure-1-12-reporting-carried-out-directly-on-the-database-of-a-monolith.png)

- More modern approaches to reporting, such as using **streaming** to allow for real-time reporting on large volumes of data.
- Publish data from your microservices into central reporting databases.

### Monitoring and Troubleshooting

- **Monolith** - Failure mode is somewhat **binary**.

### Security

- More vulnerable to being observed **in transit**.

### Testing

- Scope of end-to-end tests becomes **very large**.
- Need to be prepared for the **false negatives** that occur when environmental issues, such as **service instances dying** or **network timeouts of failed deployments**, cause tests to fail.
- New forms of testing:
  - Contract-driven testing
  - Testing in production
- Progressive delivery techniques:
  - Parallel runs
  - Canary releases

### Latency

- Needs to be serialized, transmitted, and deserialized over networks.
- Can be difficult to measure the exact impact on latency of operations at the **design** or **coding phase**.
- To **measure** the end-to-end latency, you can use **distributed tracing** tools.

### Data Consistency

- The use of distributed transactions proves to be **highly problematic** in coordinating state changes.
- Using concepts like **sagas** and **eventual consistency**.

## Should I Use Microservices?

- Need to assess your own **problem space**, skills, and technology landscape and understand **what you are trying to achieve**.

### Whom They Might Not Work For

- **Brand-new products:**
  - **Reason** - The domain that you are working with is typically undergoing **significant change** as you **iterate on the fundamentals**, result in more changes being made to **service boundaries**.
  - Should **wait until** enough of the **domain model has stabilized** before looking to define service boundaries.
- **Startup:**
  - **Reason** - Smaller team has **limited resources** to handle the complexity of the microservice architecture, such as deployment and service management ("microservice tax").
  - It's much easier to move to microservices **later**, after you **understand where the constraints are** in your architecture and what **pain points** are.
- Software that will be deployed and managed by their **customers**.

### Where They Work Well

- To allow for **more developers** to work on the same system.
  - The single biggest reason.
  - To reduce **delivery contention**.
- **Software as a Service (SaaS)** applications
  - Expected to operate 24-7, which creates **challenges** when it comes to **rolling out changes**.
- Applications that leverage various technologies
  - **Technology-agnostic** nature of microservices ensures that you can get the most out of **cloud platforms**.
  - **Note:** **FaaS** platform is not a deployment mechanism that would be suitable in all cases.

## Summary

- Why many people are choosing microservice architectures:
  - Flexibility in choosing technology, handling robustness and scaling, organizing teams.
- Still, the choice must be justified by the problems you are trying to solve.

# Chapter 2. How to Model Microservices

- **Decomposition** comes in different forms.
- Primary technique - Domain-Driven Design.

## What Makes a Good Microservice Boundary

- In essence, microservices are **just another form of MODULAR decomposition**.
- **Three key concepts/techniques** to working out what makes for a good microservice boundary:
  1. Information hiding
  2. Cohesion
  3. Coupling

### \*Information Hiding

- The **most effective** way to define module boundaries.
- Benefits of modules:
  - Improved development time
  - Comprehensibility
    - Can be **looked at** in **isolation** and **understood** in isolation.
  - Flexibility
    - Can be changed independently.
- Microservices is just another form of modular architecture.
- **BUT**, having modules **doesn't result in** you actually achieving these outcomes.
- By keeping the number of interfaces **small**, it is easier to ensure that we can change one module without impacting others.

### Cohesion

- \***The code that changes together, stays together.**
- Ease of making changes in business functionality.
  - Functionality grouped in such a way that we **can make changes in as few places as possible**.
- Making changes in lots of **different places** is **slower**, and deploying lots of services at once is **risky**.

### Coupling

- **Loosely coupled** - A change to one service should not require a change to another.
- A loosely coupled service **knows as little as it needs to** about the services with which it collaborates.
- **Chatty communication** can lead to tight coupling.

### The Interplay of Coupling and Cohesion

- \*A structure is **stable** if **cohesion is STRONG** and **coupling is LOW**.
- **Cohesion** - The relationship between things inside a boundary (a microservice).
- **Coupling** - The relationship between things across.
- **Note:** The world isn't static - Sometimes parts of your system may be going through so much change that stability might be impossible.

## Types of Coupling

- **NOT** all coupling is bad.
- In fact, some coupling will be **unavoidable**.
- **What we want** - **Reduce** how much coupling we have.
- Microservices are a style of **modular** architecture.
  - Can use a lot of these original **concepts**.
- Figure 2-1. **The different types of coupling**, from loose (low) to tight (high)

  ![figure-2-1-the-different-types-of-coupling-from-loose-low-to-tight-high](images/figure-2-1-the-different-types-of-coupling-from-loose-low-to-tight-high.png)

### Domain Coupling

- One microservice needs to interact with another microservice.
- Figure 2-2. An example of domain coupling, where Order Processor needs to make use of the functionality provided by other microservices

  ![figure-2-2-an-example-of-domain-coupling,-where-order-processor-needs-to-make-use-of-the-functionality-provided-by-other-microservices](images/figure-2-2-an-example-of-domain-coupling,-where-order-processor-needs-to-make-use-of-the-functionality-provided-by-other-microservices.png)

- This type of interaction is largely **unavoidable**.
- **BUT**, we still want to keep this to a **minimum**.
- \*When a **single** microservice depending on **multiple downstream** services, it **might imply** a microservice that is **doing too much** (too much logic has been **centralized**).
- Domain coupling can become problematic as more **complex sets of data** are sent between services.
- **Note:** Share only what you absolutely have to, and send only the absolute **minimum amount of data**.

### Pass-Through Coupling

- One microservice passes data to another microservice purely because the data is **needed by** some other microservice **further downstream**.
- \*One of the **most problematic** forms of **implementation coupling**.
  - Figure 2-4. **Pass-through coupling**, in which data is passed to a microservice purely because another downstream service needs it
    - ❌ Changes to _Shipping Manifest_ require a **lockstep rollout** of all three microservices (cascade effect).

    ![figure-2-4-pass-through-coupling,-in-which-data-is-passed-to-a-microservice-purely-because-another-downstream-service-needs-it](images/figure-2-4-pass-through-coupling,-in-which-data-is-passed-to-a-microservice-purely-because-another-downstream-service-needs-it.png)

- **First solution** - Bypass the intermediary.
  - Figure 2-5. One way to work around pass-through coupling involves communicating directly with the downstream service

    ![figure-2-5-one-way-to-work-around-pass-through-coupling-involves-communicating-directly-with-the-downstream-service](images/figure-2-5-one-way-to-work-around-pass-through-coupling-involves-communicating-directly-with-the-downstream-service.png)

  - _Order Processor_ speaks directly to _Shipping_.
  - **Tradeoff**
    - Increase domain coupling.
    - Might still be fine, as domain coupling is a **looser form** of coupling.

- **Second solution** - Hide the requirement for a _Shipping Manifest_.
  - Figure 2-6. Hiding the need for a _Shipping Manifest_ from the Order Processor

    ![figure-2-6-hiding-the-need-for-a-shipping-manifest-from-the-order-processor](images/figure-2-6-hiding-the-need-for-a-shipping-manifest-from-the-order-processor.png)

### Common Coupling

- Two or more microservices make use of a **common set of data**.
  - Shared database, memory or filesystem.
- Changes to the structure of the data can impact multiple microservices at once.
- Still acceptable, but **not recommended**:
  - Figure 2-7. Multiple services accessing shared static reference data related to countries from the same database

    ![figure-2-7-multiple-services-accessing-shared-static-reference-data-related-to-countries-from-the-same-database](images/figure-2-7-multiple-services-accessing-shared-static-reference-data-related-to-countries-from-the-same-database.png)

  - Since **static reference data** doesn't tend to change and is **read-only**, this is still fine.

- Becomes more problematic:
  - If the **structure** of the common data changes more frequently.
  - If multiple microservices are reading and **writing to the same data**.

- **Problem:**
  - Figure 2-8. An example of common coupling in which both Order Processor and Warehouse are updating the same order record

    ![figure-2-8-an-example-of-common-coupling-in-which-both-order-processor-and-warehouse-are-updating-the-same-order-record](images/figure-2-8-an-example-of-common-coupling-in-which-both-order-processor-and-warehouse-are-updating-the-same-order-record.png)

  - Both microservices **share responsibilities** for managing **different aspects of the life cycle** of an order.
  - One way to ensure that the state is changed in a correct order would be to create a **finite state machine**, ensuring **invalid state transitions are prohibited**.
  - Figure 2-9. An overview of the allowable state transitions for an order in MusicCorp

    ![figure-2-9-an-overview-of-the-allowable-state-transitions-for-an-order-in-musiccorp](images/figure-2-9-an-overview-of-the-allowable-state-transitions-for-an-order-in-musiccorp.png)

- **Solution** - Ensure that a **single microservice** manages the order state.
  - Figure 2-10. Both Order Processor and Warehouse can request that changes be made to an order, but the **Order microservice decides which requests are acceptable**

    ![figure-2-10-both-order-processor-and-warehouse-can-request-that-changes-be-made-to-an-order,-but-the-order-microservice-decides-which-requests-are-acceptable](images/figure-2-10-both-order-processor-and-warehouse-can-request-that-changes-be-made-to-an-order,-but-the-order-microservice-decides-which-requests-are-acceptable.png)

- **Alternative:**
  - Implement the Order service **more than a wrapper** around database CRUD operations.
  - CRUD wrapper is **a sign of weak cohesion and tighter coupling**, as logic that should be in that service is instead **spread elsewhere**.
- Potential sources of resource contention (overload shared resource).
- Common coupling is **sometimes** OK, but **often it's not**.
- Often indicate a lack of cohesion.
- One of the **least desirable** forms of coupling.

### Content Coupling (Worst)

- An upstream service **reaches into the internals** of a downstream service and **changes its internal state**.
- E.g. An external service accessing another microservice's database and changing it directly.
- The lines of **ownership become less clear**, and it becomes more difficult for developers to change a system.
- Results in very **poor data integrity**.
- Figure 2-11. An example of content coupling in which the Warehouse is directly accessing the internal data of the Order service

  ![figure-2-11-an-example-of-content-coupling-in-which-the-warehouse-is-directly-accessing-the-internal-data-of-the-order-service](images/figure-2-11-an-example-of-content-coupling-in-which-the-warehouse-is-directly-accessing-the-internal-data-of-the-order-service.png)

- Also have the issue that the **internal data structure is exposed**.

## Just Enough Domain-Driven Design

- Some core concepts of DDD:
  - Ubiquitous language
  - Aggregate
  - Bounded context

### Ubiquitous Language

- Common problem - When rich domain language is map to generic code concepts (too abstract).

### Aggregate

- Typically have a **life cycle**, and can be implemented as a **state machine**.
- Ensure that the code that handles the **state transitions** of an aggregate are **grouped together**, along with the **state itself** (saga state).
- \*If an outside party requests a **state transition** in an aggregate, the aggregate can say no to **protect business invariants**.
- Assuming **Finance microservice**'s database stores the **vanilla customer ID** into the `CustID` column.
  - **Problem** - Relationship between the `CustID` column and the remote customer is **entirely implicit**.
  - **Solution 1** - Store a **URI** to make the relationship explicit.
  - Figure 2-13. An example of how a relationship between two aggregates in different microservices can be implemented

    ![figure-2-13-an-example-of-how-a-relationship-between-two-aggregates-in-different-microservices-can-be-implemented](images/figure-2-13-an-example-of-how-a-relationship-between-two-aggregates-in-different-microservices-can-be-implemented.png)

  - **Solution 2** - Pseudo-URI scheme (if you aren't building a REST system)
    - E.g. soundcloud:tracks:123

- Factors that determines how we define aggregates, to **reshape aggregates** over time:
  - For performance reasons.
  - For ease of implementation.

### Bounded Context

- E.g. At MusicCorp:
  - **Core domain** - Warehouse
  - **Supporting domain** - Finance
- Bounded contexts hide implementation detail.
- Internal concerns should be hidden from the outside word.

#### Hidden models

- Figure 2-14. A shared model between the finance department and the warehouse
  - To work out the valuation of the company, finance employees need information about the stock.
  - May be beneficial to name the internal and external models differently to avoid confusion.

  ![figure-2-14-a-shared-model-between-the-finance-department-and-the-warehouse](images/figure-2-14-a-shared-model-between-the-finance-department-and-the-warehouse.png)

- Figure 2-15. A model that is shared can decide to hide information that should not be shared externally

  ![figure-2-15-a-model-that-is-shared-can-decide-to-hide-information-that-should-not-be-shared-externally](images/figure-2-15-a-model-that-is-shared-can-decide-to-hide-information-that-should-not-be-shared-externally.png)

#### Shared models

- E.g. **Customer** can have **different meanings** in the **different bounded contexts**.
  - Finance - "Customer"
  - Warehouse - "Recipient"
- May need to link both **local concepts** to a **global customer**, to look up common, shared information.
  - Figure 2-13. An example of how a relationship between two aggregates in different microservices can be implemented

    ![figure-2-13-an-example-of-how-a-relationship-between-two-aggregates-in-different-microservices-can-be-implemented](images/figure-2-13-an-example-of-how-a-relationship-between-two-aggregates-in-different-microservices-can-be-implemented.png)

### Mapping Aggregates and Bounded Contexts to Microservices

- Aggregate is a self-contained **state machine** that **focuses on a single domain concept**.
- \***Service boundaries:**
  - **Coarse-grained** - One microservice **per bounded context**. **(Preferred)**
  - **Fine-grained** - One microservice **per aggregate**.
- \***Note:** One microservice can manage one or more aggregates, but we don't want one aggregate to be managed by more than one microservice.

#### \*Turtles all the way down

- At the start, you identify some coarse-grained bounded contexts. **BUT**, these bounded contexts **can contain further bounded contexts**.
- By implementing one microservice per coarse-grained context, we can hide future service decomposition decisions.
- Decision to decompose a service into smaller parts is arguably an **implementation decision**.
- Figure 2-16. The Warehouse service internally has been split into Inventory and Shipping microservices
  - Another form of **information hiding**.
  - **Nested approach** - Chunk up the architecture to **simplify testing**.

  ![figure-2-16-the-warehouse-service-internally-has-been-split-into-inventory-and-shipping-microservices](images/figure-2-16-the-warehouse-service-internally-has-been-split-into-inventory-and-shipping-microservices.png)

### Event Storming

- A collaborative brain-storming exercise designed to help **surface a domain model**.
- Brings together **technical** and **nontechnical** stakeholders.
- Can be used to build:
  - Event-driven system
  - Request/response-oriented system

#### Logistics (How event storming should be run)

- You want **representatives** of all parts of the domain that you plan to model: **users**, **subject matter experts**, **product owners**.
- Walls to be used for capturing information.

#### The process

- Identify the **DOMAIN EVENTS**.
  - Things that happen (facts that you care about).
  - E.g. "Order Placed".
- Identify the **COMMANDS** that cause these events to happen.
  - Identity the key **human actors**.
- \***Note:** Not to let any current implementation warp the perception of what the domain is.
- With events and commands captured, **AGGREGATES** come next.
  - **Events** can also highlight what the potential aggregates might be.
  - E.g. **"Order Placed"**
    - **"Order"** (noun) could be a potential **aggregate**.
    - **"Placed"** describes something that can happen, so this could be part of the **life cycle** of the aggregate.
  - Commands and events are clustered around the aggregate.
  - Events from one aggregate might trigger behavior in another.
- Group aggregates into **BOUNDED CONTEXTS**.
  - Bounded contexts most commonly follow a company's **organizational structure**.

## Alternatives to Business Domain Boundaries

- DDD is **NOT the only technique** for finding microservice boundaries.

### Volatility

- Identify the parts of your system going through **more frequent change** and then extract that functionality into their own services.
- If your intention is to **SCALE** your application, then **volatility-based decomposition is NOT the right technique**.
- **Bimodal IT** (Mindset Behind):
  - Break systems down into two categories, based on **how fast or slow** they need to go.
    1. "Mode 1" (aka Systems of Record)
    2. "Mode 2" (aka Systems of Innovation)
  - **Cons:**
    - Imply a very **fixed view** (oversimplification).
    - Fall apart when parts of systems that didn't need to change much in the past suddenly do.
    - Often becomes a way for people to dump stuff that is hard to change into a nice neat box.
    - Avoids the fact that quite often changes in functionality require changes in "Systems of Record" (Mode 1) to allow for changes in "Systems of Innovation" (Mode 2).
  - **Pros:**
    - Useful if the **main driver** is about **fast time to market**.

### Data

- **Use case** - To limit which services handle **personally identifiable information (PII)**.
- E.g. A payment company handles **credit card data**, and needs to comply with **Payment Card Industry (PCI) standards** (company's system and processes needed to be **audited**).
- So, we limit what has to be in this red zone.
- Segregation of data is often driven by a variety of **privacy** and **security concerns**.
- Figure 2-17. PaymentCo, which segregates processes based on its use of credit card information to limit the scope of PCI requirements

  ![figure-2-17-paymentco-which-segregates-processes-based-on-its-use-of-credit-card-information-to-limit-the-scope-of-pci-requirements](images/figure-2-17-paymentco-which-segregates-processes-based-on-its-use-of-credit-card-information-to-limit-the-scope-of-pci-requirements.png)

### Technology

- Often a **less than ideal** architecture.

### Organizational

- **Conway's law** - How you organize yourself ends up driving your systems architecture.
- Figure 2-19. A service boundary split across technical seams

  ![figure-2-19-a-service-boundary-split-across-technical-seams](images/figure-2-19-a-service-boundary-split-across-technical-seams.png)

## Mixing Models and Exceptions

- Follow the guidelines of **information hiding**, **coupling** and **cohesion**.
- Organizational and **domain-driven** service boundaries are great **starting point**.

## Summary

- **Understanding of our domain** can be vital tool in helping us find these **seams**.

# Chapter 3. Splitting the Monolith

## Have a Goal

- Migrating to a microservices architecture **only if** you can't find any easier way to move toward your **end goal**.
- Don't confuse **activity** (create microservices) with **outcome**.
- Track your progress against that end goal and **change course** as necessary.

## Incremental Migration

- **Limit the impact** of getting something wrong.
- Breaking things into **smaller pieces** to identify **quick wins**.
- Start somewhere small.
- **Warning:** You won't appreciate the true horror, pain, and suffering that the microservice architecture can bring until you are running in production.

## The Monolith Is Rarely the Enemy

- Monolith architecture **isn't inherently bad**.
- It is common for the existing monolith architecture to **remain** after a shift towards microservices.
  - E.g. by **removing the 10%** of functionality that is currently bottlenecked, leaving the **remaining 90%** in the monolith system.
- Focus on **delivering improvements**, while also, **knowing when to stop**.
- Situations where **the decommission of the monolith** might be a hard requirement:
  - Existing monolith is based on **dead or dying technology**.
  - **Infrastructure** that needs to be **retired**.
  - **Third-party system** that you want to **ditch**.

### The Dangers of Premature Decomposition

- **Danger** in creating microservices when you have an unclear understanding of the domain.
- You need a **deep understanding of the domain** to determine the correct boundaries.
- Having an existing codebase you want to decompose into microservices is **much easier**.

## What to Split First?

- Depends on **WHY** you think microservices are a good idea.
  - **To scale** - Functionality that currently **constrains the system's ability** to handle load.
  - **To improve time to market** - Decompose the system based on **volatility**.
  - To isolate **critical** or **high risk** functionality.
- Decision is based on a balance between two forces - **how easy** versus the **benefit**
  - Lean toward the **"easy" end** of the spectrum - **Low-hanging fruit** that **has some impact** on achieving our **end goal**.

## Decomposition by Layer (Horizontal)

- **Three tiers** of a web-based system
  1. User interface
  2. Backend application code
  3. Data
- **Mapping** from a **microservice** to a **user interface** is often **NOT 1:1**.
- **Extracting user interface** related to the microservice could be considered a **separate step**.
  - Caution about ignoring the user interface part.
  - Sometimes benefits can come from decomposition of the UI. So make sure it doesn't lag too much.
- **\*Backend code** and **storage** (database) are both **vital to be in scope** when extracting a microservice.
- Let's consider extracting functionality related to managing a **customer's wishlist**.
  ![figure-3-2-the-wishlist-code-and-data-in-the-existing-monolithic-application](images/figure-3-2-the-wishlist-code-and-data-in-the-existing-monolithic-application.png)

### Code First (Common)

![figure-3-3-moving-the-wishlist-code-into-a-new-microservice-first-leaving-the-data-in-the-monolithic-database](images/figure-3-3-moving-the-wishlist-code-into-a-new-microservice-first-leaving-the-data-in-the-monolithic-database.png)

- \*Remember, we **HAVEN'T COMPLETED** the decomposition **until** we've also moved out the related data.
- Tends to deliver more **short-term benefit**.
- **Benefit** - If we found that it was **impossible to extract** the application code cleanly, we could **abort any future work** (e.g. detangle the database).
- **Caveat** - If, however, the application code is cleanly extracted but **extracting the data proves to be impossible**, we could be **in trouble**.
- \*So, **sketch** out how both application code and data will be extracted **before you start**.

### Data First (Rare)

![figure-3-4-the-tables-associated-with-the-wishlist-functionality-are-extracted-first](images/figure-3-4-the-tables-associated-with-the-wishlist-functionality-are-extracted-first.png)

- Can be useful when you are **unsure** whether the data can be separated cleanly.
- **Short-term benefit** - De-risking the full extraction of the microservice.
- It forces you **to deal upfront with issues** like **loss of enforced data integrity** or **lack of transactional operations** across both sets of data (ACID transactions).

## Useful Decompositional Patterns

### \*Strangler Fig Pattern (Frequently used)

![alt text](images/figure-3-5-extra-strangler-fig-pattern.png)

- **Intercept calls** to the existing monolithic application.
  - How to intercept? It depends on the protocol (e.g. HTTP, IPC, etc) used to access the functionality.

  ![figure-3-5-an-overview-of-the-strangler-fig-pattern](images/figure-3-5-an-overview-of-the-strangler-fig-pattern.png)

### Parallel Run

- Useful for migrating **critical** functionality.
- Running both side by side and comparing the results.

### Feature Toggle

- To switch between **two different implementations**.
- Has good general applicability, not exclusive to microservice migration.
- Leave the existing functionality in place during the transition so we can switch between versions.
- Example of using it with HTTP proxy - Implement the feature toggle in the proxy layer for a simple control.

## Data Decomposition Concerns (Issues)

### Performance

- Databases are good at **joining data** across different tables.
- When we split databases apart, we end up having to **move join operations** from the data tier up into the microservices (application tier).
- Generate "Bestseller report" in **Monolith**:
  ![figure-3-6-a-join-operation-in-the-monolithic-database](images/figure-3-6-a-join-operation-in-the-monolithic-database.png)
- Generate "Bestseller report" in **Microservice**:
  - Finance microservice needs to **fetch** data from Catalog microservice.

  ![figure-3-7-replacing-a-database-join-operation-with-service-calls](images/figure-3-7-replacing-a-database-join-operation-with-service-calls.png)

- Ways to mitigate the performance issue:
  - Lookup the data in **bulk**.
  - Caching the required data **locally**.

### Data Integrity

- With tables being in the same database, we can define a **foreign key relationship**.
- With foreign key constraints, we **won't be able to delete records** if they were **referenced**.
- With separated databases, we no longer have enforcement of the integrity of our data model.
- **\*Workarounds:**
  - Use **soft delete**.
  - **Copy the reference data** into the transaction table. **BUT** need to resolve how to **synchronize changes**.

### Transactions

- **Lose** the safety of the **ACID transactions**.
- New sources of complexity in managing state changes across multiple microservices.

### Tooling

- Why changing databases is difficult:
  - **Limited tools** for us to make changes easily.
  - Database has **state**, as opposed to code, which is fundamentally stateless.

### Reporting Database

- As part of extracting **microservices**, we break apart our databases, as we want to **hide access to our internal data storage**.
- By hiding access to our databases, we can create **stable interfaces**, which make **independent deployability** possible.
- But for reporting, we need to access data from more than one microservice.
- So, we create a **dedicated database** for reporting that is designed for **external access**.
- It is the **responsibility of the microservice** to push data from internal storage to the externally accessible reporting database.
  ![figure-3-8-an-overview-of-the-reporting-database-pattern](images/figure-3-8-an-overview-of-the-reporting-database-pattern.png)
- **\*Two key points:**
  1. **Information hiding** - Expose only the bare minimum of data. As this is **not a direct mapping**, we use a **different schema** design or **different type of database technology**.
  2. Reporting database integration should be treated as **API contract** - Microservice maintainer to ensure the API contract is maintained even if the microservice changes its internal implementation detail.

# Chapter 4. Microservice Communication Styles

## From In-Process to Inter-Process

### Performance

| In-process                                                                                 | Inter-process                                                                                                         |
| ------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------- |
| The **compiler** and **runtime** can perform many **optimizations**.                       | The **overhead** of a call is **significant** compared to an in-process call.                                         |
| An API can be more **fine-grained**. E.g. making **1,000 calls** is usually not a concern. | An API that works in-process may **not make sense** across processes.                                                 |
| Passing a **reference** does not require extra **memory allocation** for the call.         | We need to be mindful of **payload size** between processes.                                                          |
| Data can be passed directly within the same process.                                       | We may need more efficient **serialization**, or pass a **file location reference** instead.                          |
| The call stays within the same **process boundary**.                                       | Do not hide **network calls** behind abstractions; developers should know when a call crosses a **process boundary**. |

### Changing API Interfaces

| In-process                                                                                       | Inter-process                                                                                                                       |
| ------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------- |
| The code **implementing** the interface and the code **calling** it are in the same **process**. | The interface is used across **process boundaries**, often by separate **service consumers**.                                       |
| A change can usually be released in one **atomic** step.                                         | A **backward-incompatible** change may require **lockstep deployment** with consumers, or a **phased rollout** of the new contract. |

### Error Handling

- **In-process** - Errors either are **expected and easy to handle**, or are catastrophic and can be propagated up the call stack. In short, they are **deterministic**.
- **Inter-process** - We are vulnerable to errors outside our control, such as **network timeouts**, **network disconnections**, or processes get killed due to **out of memory**.
- **Five types** of failure mode in **inter-process** communication
  1. **_Crash failure_**
  2. **_Omission failure_** - Didn't get a response or expecting a downstream microservice to be firing messages, and it just stops.
  3. **_Timing failure_** - Something happened **too late** or **too early**.
  4. **_Response failure_** - Information is missing in the response.
  5. **_Arbitrary failure_** - Unable to determine if the failure has occurred.
- Many of inter-process errors are **often transient** in nature.
- \*It is important to provide a **richer set of error semantics** so clients can take the appropriate action. E.g.
  - **404 Not Found** - Should stop retrying.
  - **503 Service Unavailable** - Issue may be temporary, so retrying could succeed.

## Styles of Microservice Communication

![figure-4-1-different-styles-of-inter-microservice-communication-along-with-example-implementing-technologies](images/figure-4-1-different-styles-of-inter-microservice-communication-along-with-example-implementing-technologies.png)

- **_Synchronous blocking_** - Block operation **waiting for the response**.
- **_Asynchronous nonblocking_** - **Emit a call** and **continue processing** regardless whether the call is received.
- **_Request-response_** - Send a request and **expect** to receive a response.
- **_Event-driven_** - **Emit events**. Microservice emitting the event is **unaware** of which microservices, if any, consume the events.
- **_Common data_** - Collaborate via some **shared data source**.

---

- **Deciding factors / requirements / constraints:**
  - Reliable communication
  - Acceptable latency
  - The ability to scale
  - Security-related aspects
- **Start with** deciding whether a **request-response** or an **event-driven** is more appropriate for the given situation.
- For **request-response**, it supports both **synchronous** and **asynchronous** implementations, so we have a **second choice to make**.
- For **event-driven**, it is limited to **asynchronous**.

## Pattern: Synchronous Blocking (Request-response)

![figure-4-2-order-processor-sends-a-synchronous-call-to-the-loyalty-microservice-blocks-and-waits-for-a-response](images/figure-4-2-order-processor-sends-a-synchronous-call-to-the-loyalty-microservice-blocks-and-waits-for-a-response.png)

- **Use when** the **result of the call** is needed for some **further operation**, or to make sure the call worked (**critical operation**).

### Advantages

- **Simple** and **familiar** (e.g. running a SQL query, making an HTTP request)

### Disadvantages

- Having **temporal coupling**.
- When the call **fails** (service unavailable), the client has to perform **compensating action**, such as immediate retry, buffering the call to retry later, or giving up.
- Coupling is **two-way** - If the downstream service wants to send a response back, but the upstream instance has died, the response will get lost.
- The use of synchronous calls can make a system **vulnerable to cascading issues** caused by **downstream outages**.

### Where to Use It

- Simple microservice architectures.
- **Problem:** When you start having **more chains of calls**.
  - An issue in any of the four involved microservices could cause the whole operation to fail.
  - Can cause significant **_resource contention_** due to opened network connections.

  ![figure-4-3-checking-for-potentially-fraudulent-behavior-as-part-of-order-processing-flow](images/figure-4-3-checking-for-potentially-fraudulent-behavior-as-part-of-order-processing-flow.png)

- **Solution:** Take the "Fraud Detection" out of the main flow, and have it **run in the background**.
  - This is something that could be **checked earlier** in the payment process.
  - One fewer dependency to worry.

  ![figure-4-4-moving-fraud-detection-to-a-background-process-can-reduce-concerns-around-the-length-of-the-call-chain](images/figure-4-4-moving-fraud-detection-to-a-background-process-can-reduce-concerns-around-the-length-of-the-call-chain.png)

## Pattern: Asynchronous Nonblocking

- **Three** most common styles/types:
  1. **_Communications through common data_** - **Shared database** or filesystem.
  2. **_Request-response_**
  3. **_Event-driven interaction_** - **Broadcast** an event (something that has happened). Interested services (aka consumers) can **listen** for the events and **react** accordingly.

### Advantages

- Microservices that receive the call **do not need to be reachable at the same time** the call is made.
- **Avoid temporal coupling**.
- Useful if the functionality **takes a long time to process**.
- **Example:**
  - A form of **asynchronous request-response** communication.

  ![figure-4-5-the-order-processor-kicks-off-the-process-to-package-and-ship-an-order-which-is-done-in-an-asynchronous-fashion](images/figure-4-5-the-order-processor-kicks-off-the-process-to-package-and-ship-an-order-which-is-done-in-an-asynchronous-fashion.png)

### Disadvantages

- More **complex** and many **choices** (a bewildering list of technology).
- Many **edge cases** to handle.

### Where to Use It

- Have to consider which **type** of asynchronous communication to use, as each type has its own **trade-offs**. All depends on specific **use cases**.
- Obvious candidates - **long-running processes**, **long call chains**

## Pattern: Communication Through Common Data

![figure-4-6-one-microservice-writes-out-a-file-that-other-microservices-make-use-of](images/figure-4-6-one-microservice-writes-out-a-file-that-other-microservices-make-use-of.png)

- One microservice **puts data into a defined location** (e.g. dropping a file in a location), and another microservice then makes use of the data.

### Implementation

- Need some sort of **persistent store**, such as file system.
- Periodically scan a file system, note the presence of a new file, and react to it accordingly.
- **Downstream microservice** needs its own mechanism **to identify when new data is available**, such as polling.
- Two **examples** of this pattern are the **data lake** and the **data warehouse**.
  - They exist at **opposite ends** of the spectrum regarding **coupling**.
  - **Data lake** (low coupling) - Sources upload **raw data**, and downstream consumers are expected to know how to process the information.
  - **Data warehouse** (high coupling) - **Structured** data store. Microservices pushing data to the data warehouse need to know the structure. If the structure changes in a backward-incompatible way, then these producers will need to be updated.
  - Assumption: The flow of information is in a **single direction**.
- **Problematic implementation** - The use of a **shared database** in which multiple microservices **both read and write**.

### Advantages

- Can be implemented very simply using common technology.
- Enables **interoperability**

### Disadvantages

- Unlikely to be useful in **low-latency** situations.
- If you are using this pattern for very **large volumes** of data, it's less likely that low latency is high on your list of requirements.
- To process large volumes of data in **more "real time"**, use **streaming technology** like Kafka.

### Where to Use It

- Enable interoperability between processes that have **restrictions on the use of technology**, such as integrating with an **existing system**.
- To share large volumes of data.

## Pattern: Request-Response Communication

### Implementation: Synchronous versus Asynchronous

- **Synchronous** - Microservice sending the response **doesn't really need to know about the microservice that sent the request**. It's just **sending stuff back over an inbound connection**.
- **Asynchronous** example:
  - Request is sent as a **message** over a message broker.
  - Handler (Warehouse) **sends the response back to a queue** that the requester (Order Processor) is reading from.
  - Handler (Warehouse) **needs to know where to route the response**, such as send the response back **over another queue**.

  ![figure-4-10-using-queues-to-send-stock-reservation-requests](images/figure-4-10-using-queues-to-send-stock-reservation-requests.png)

- **Asynchronous**:
  - Handler that receives the request **needs either to know implicitly where to route the response**, or **to be told** where the response should go.
  - With a queue, multiple requests could be **buffered up**.
  - Requester might need to **relate** the response to the original request, since the response may **not come back to the same instance**.
  - Requester has to **store any state associated with the original request** into a database.
- **All forms** of request-response interaction are likely going to require some form of **timeout handling**.

### Where to Use It

- For any situation in which the **result** of the request is **needed before further processing**.
- In situations where a microservice wants to know if a **call didn't work** so that it can carry out some sort of **compensating action**, like a retry.

## Pattern: Event-Driven Communication

- Rather **asking**, **emits events** instead.
- An event is a **statement** about something that has **occurred**.
- Microservice emitting the event has **no knowledge** of the intent of other microservices to use the event, and it may **not even be aware** that other microservices **exist**.
- **Example:** The process of packaging up an order
  - Producer (Warehouse) **broadcasts events**.

  ![figure-4-11-the-warehouse-emits-events-that-some-downstream-microservices-subscribe-to](images/figure-4-11-the-warehouse-emits-events-that-some-downstream-microservices-subscribe-to.png)

- Event-driven interactions are much more **loosely coupled**.
- Having the **inversion of responsibility** when compared to request-response calls.
- With request-response, the microservice sending the request **knows what should be done**.
- Events and Messages - Message is the **medium**. Event is the **payload** (fact).

### Implementation

- Use message broker.
- \*Keep your **middleware dumb**, and keep the **smarts in the endpoints**.

### What's in an Event?

#### Just an ID

![figure-4-13-the-notifications-microservice-needs-to-request-further-details-from-the-customer-microservice-that-are-not-included-in-the-event](images/figure-4-13-the-notifications-microservice-needs-to-request-further-details-from-the-customer-microservice-that-are-not-included-in-the-event.png)

- **Downsides:**
  - Notifications microservice now has to know about the Customer microservice, adding additional **domain coupling**.
  - In a situation with a **large number of receiving microservices**, the producer might get a **large number of requests**.

#### Fully detailed events (Preferred)

![figure-4-14-an-event-with-more-information-in-it-can-allow-receiving-microservices-to-act-without-requiring-further-calls-to-the-source-of-the-event](images/figure-4-14-an-event-with-more-information-in-it-can-allow-receiving-microservices-to-act-without-requiring-further-calls-to-the-source-of-the-event.png)

- Put everything into an event that you would be happy otherwise sharing via an API.
- Make consumers more **self-sufficient**.
- Consumers **might never need to know** the producer exists.
- Events with more information can double as a **historical record** of what happened to a given entity.
- Could help in building an **auditing system** or **event sourcing**.
- **Downsides:**
  - Data associated with an event could be **large**. When you **start to worry** about the size of your events, use a **hybrid approach**.
  - **Concerns:** To **limit** the scope of which microservices can see what kind of **sensitive data**, such as PII and payment card details. A way to **solve** this could be to send **two different types of events**.
- Once we put data into an **event**, it becomes part of our **contract** with the outside world.
- **Information hiding** is still an important concept in event-driven collaboration.

### Where to Use It

- Information wants to be **broadcast**.
- To **invert intent** - Letting downstream microservices work this out for themselves.
- Focus on **loose coupling** more than other factors.
- **Caution:** Comes with new sources of **complexity**.
- Author's **default** microservice communication style (opinionated).
- Far more teams replacing request-response interactions with event-driven interactions than the reverse.

## Proceed with Caution

- **Event-driven architectures** do lead to an increase in **complexity**.
- **Not just** the complexity required to manage publishing and subscribing to messages, but also complexity in **other problems**.
- Considerations for handling **async request-response**:
  - Does the response come back to the same node that initiated the request?
  - If so, what happens if that node is down?
  - If not, do I need to store information somewhere?
- Considerations for handling **synchronous blocking call**:
  - If you get a **timeout**, did this happen because the **request got lost** and the downstream party didn't receive it? or did the request get through, but the **response got lost**?
  - Implement **idempotency** to handle this.
- Other concerns:
  - **Poison / Malformed message** handling
  - **Retry strategy** - Maximum retry, exponential backoff, etc.
  - **Dead letter management** - To view and replay messages.
- Ensure you have good **monitoring** in place.
- Use **correlation IDs**.
