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
