# System Design: The Pub-sub Abstraction

Get introduced to the basics of designing a pub-sub system.

> We'll cover the following:
>
> - What is a pub-sub system?
> - Motivation
> - How do we design a pub-sub system?

## What is a pub-sub system?

**Publish-subscribe messaging**, often known as pub-sub messaging, is an asynchronous service-to-service communication method that’s popular in serverless and microservices architectures. Messages can be sent asynchronously to different subsystems of a system using the pub-sub system. All the services subscribed to the pub-sub model receive the message that’s pushed into the system.

For example, when a famous athlete posts on Instagram or shares a tweet, all of their followers are updated. Here, the athlete is the publisher, their post or tweet is the message, and all of their followers are subscribers.

![the pub-sub system](./images/1-1-pub-sub-system.png)

## Motivation

The hardware infrastructure of distributed systems consists of millions of machines. Using a pub-sub system to communicate asynchronously increases scalability. Producers and consumers are disconnected and operate independently, thereby allowing us to scale and develop them separately. The decoupling between components, producers and consumers, allows greater scalability because adding or removing any component doesn’t affect the other components.

## How do we desing a pub-sub system?

We have divided the pub-sub system design into the following lessons:

1. **Introduction:** In this lesson, we learn about the use cases of the pub-sub system, define its requirements, and design the API for it.
2. **Design:** In this lesson, we discuss two designs of the pub-sub system, one with messaging queues and the other with a broker.
