# Maintainability

Learn about maintainability, how to measure it, and its relationship with reliability.

> We'll cover the following:
>
> - What is maintainability?
> - Measuring maintainability
>   > - Maintainability and reliability

## What is maintainability?

Besides building a system, one of the main tasks afterward is keeping the system up and running by finding and fixing bugs, adding new functionalities, keeping the system's platform updated, and ensuring smooth system operations.  
 One of the saliant features to define such requirements of an exemplary system design is **maintainability.**

We can further divide the concept of maintainability into three underlying aspects:

1.  **Operability:** This is the ease with which we can ensure the system's smooth operational running under normal circumstance and achieve normal conditions under a fault.
2.  **Lucidity:** This refers to the simplicity of the code. The simpler the code base, the easier it is to understand and maintain it, and vice versa.
3.  **Modifiability:** This is the capability of the system to integrate modified, new, and unforeseen features without any hassle.

## Measuring maintainability

Maintainability, M, is the **probablity that the service will restore its functions within a specified time of fault occurrence.**  
 M measures how conveniently and swiftly the service regains its normal operating conditions.

For example, suppose a component has a defined maintainability value of 95% for half an hour. In that case, the probablity of restoring the component to its fully active form in half an hour is 0.95.

> NOTE: Maintainability gives us insight into the system's capability to undergo repairs and modifications while it's operational.

We use (mean time to repair) MTTR as the metric to measure it.

MTTR = (Total Maintenance Time) / (Total Number of Repairs)

In other words, **MTTR is the average amount of time required to repair and restore a filled component.** Our goal is to have as low a value of MTTR as possible.

#### Maintainability and reliability

Maintainability can be defined more clearly in close relation to reliability.  
 The only difference between them is the variable of interest.

**Maintainability refers to time-to-repair,** whereas reliability refers to both **time-to-repair** and the **time-to-failure**.

> Combining maintainability and reliability analysis can help us achieve availability, downtime, and uptime insights.
