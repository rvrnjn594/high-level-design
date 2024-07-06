# Introduction to Modern System Design

Get an overview of the topics we'll cover in this course.

> We'll cover the following:
>
> - What is system design?
>   > - Modern system design using building blocks
>   > - About this course
>   > - Who should take this course?
>   > - Prerequisites for this course

## What is system design?

System design is the process of **defining components and their integration, APIs, and data models to build large-scale systems** _that meet a specified set of functional and non-functional requirements._

System design uses the **concepts of computer networking, parallel computing, and distributed systems to craft systems that scale well and are performant.**

Distributed systems scale well by nature. However, distributed systems are inherently complex.

> The discipline of system design helps us tame this complexity and get the work done.
> ![system design](./images/system%20design.png)

System design aims to build **systems that are reliable, effective, and maintainable,** among other characteristics.

- Reliable systems _handle faults, failures, and errors._
- Effective systems _meet all users needs and business requirements._
- Maintainable systems _are flexible and easy to scale up or down._
  The ability to add new features also comes under the umbrella of maintainability.

## Modern system design using building blocks

There are 16 building blocks that are crucial in designing modern systems.
![building block of system design](./images/building%20block.png)

> This serves two purposes:
>
> - allows to discuss all the building blocks in detail and discuss their interesting mini-design problems.
> - second, concentrate on problem-specific aspects, mention the building block we'll use, and how we'll use it. This helps remove duplicate discussion of commonly-occurring design elements.

## About this course

> This course is about designing systems that scale with increasing users, remain available even under different faults, and meet functional goals with good performance.
>
> Real-world system building is an iterative process where we start with a reasonably good design, measure how it performs, and improve the design in the next iteration.
>
> The focus of this course is to immerse ourselves into carefully-selected system design endeavors to enable ourselves to tackle any novel design problem, be it in a system design interview or a task at the office.
>
> This course aims to teach concepts instead of givin out boilerplate designs.

Some gaps that this course aims to fill are listed below:

- **A fresh look at system design:** Systems design is as much an art as it is a science, and attacking a design problem from the first principles gives a fresh feel to it.
- **Going deep and broad:** Tackle some traditional problems, but with added in-depth discussions on them.  
  Give proper rationale for why we use some components despite their tradeoffs. Example: why use a particular database, a caching system, or a load balancing technique in a design.
- Address some new design problems as well that touch not only scalability but also availability, maintainability, consistency, and fault-tolerance.  
  (Collectively, traditional and new design problems cover all aspects of modern system design activity.)
- Real systems are complex and, often need to make assumptions to properly scope a problem. (Problems are covered in more detail to properly grasp the real-world systems.)
- **Iterative process:** We often start with something simple, but when bottlenecks arise in one or more of the system's parts, a new design becomes necessary. We make one design, identify bottlenecks and improve on it.
  > Working under time constraints might not permit iterations on the design. However, two iterations are recommended - first, do best to come up with a design (that takes about 80 percent of our time), and a second iteration for improvements.
  >
  > Another choice is to change things as we figure out new insights.  
  > (Inevitably, we discover new details as we spend more time working with a problem.)
- **Iteractive learning:** provide ample oppurtunities to get experience with system design.

## Who should take this course?

System design is for any software engineer who wants to advance in their career.

- **Interview preperation:** Have an elaborate guide on preperation for a system design interview in the second chapter of this course for learners who are interested.
- **Software developers:** System design is primarily for backend developers who aim to become principle engineers or solution architects. It's because these engineers handle active user data.
  > Once the data is submitted to back-end systems by the frontend, effective handling of data makes the overall application successful.
  >
  > Fullstack or frontend developers may also want to learn system design to improve their work.
  >
  > At the same time, support engineers (also called SREs) who work on-call in the production environment have to deal with all sorts of problems daily. Therefore, system design concepts enable SREs to effectively find the root causes of complex problems.
- **Product/project managers:** A big challenge in product/project management is to build systems that scale well and perform effectively over time.  
  It is imperitive for product/project managers to understand system design concepts to lead the design and development of successful applications.
- **System design learners:** System design is an interesting subject, and people in tech domains can greatly benefit from learning system design.  
  Helps a learner understand how giant tech companies design and build successful applications from scratch and improve them over time.

## Prerequisites for this course

Fundamentals concepts of a distrinuted system.

System design borrows many great concepts from distributed systems.

Apart from [distributed systems](https://www.educative.io/courses/distributed-systems-practitioners), some basic concepts on [computer networking](https://www.educative.io/courses/grokking-computer-networking) and [operating systems](https://www.educative.io/courses/operating-systems-virtualization-concurrency-persistence) are also helpful before taking this course.
