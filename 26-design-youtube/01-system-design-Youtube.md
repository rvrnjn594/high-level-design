# Sysytem Design: YouTube

Let's understand the basics details of design the YouTube service.

> We'll cover the following:
>
> - What is YouTube?
> - YouTube's popularity
> - YouTube's growth
> - How will we design YouTube?

## What is YouTube?

YouTube is a poular video streaming service where users upload, stream, search, comment, share, and like or dislike videos.  
 Large businesses as well as individuals maintain channels where they host their videos.

YouTube allows free hosting of video content to be shared with users globally. YouTube is considered a primary source of entertainment, especially among young people, and as of 2022, it is listed as the second-most viewed website after Google by Wikipedia.

![depictin of a content creater uploading a video to YouTube that is streamed by many viewers](./images/1-1-depiction-of-a-content-creater-uploading-a-video.png)

## YouTube's popularity

Some of the reasons why YouTube has gained popularity over the years include the follwing reasons:

- **Simplicity:** YouTube has a simple yet powerful interface.
- **Rich content:** Its simple interface and free hosting has attracted a large number of content creators.
- **Continuous improvement:** The development team of YouTube has worked relentlessly to meet scalability requirements over time.  
   Additionally, Google acquired YouTube in 2007, which has added to its credibility.
- **Source of income:** YouTube coined a partnership program where it allowed content creators to earn money with their viral content.

Apart from the above reasons, YouTube also partnered up with well-established market giants like CNN and NBC to set a stronger foothold.

## YouTube's growth

Let's look at some interesting statistics about YouTube and its popularity.

Since its inception in February 2005, the number of YouTube users has multiplied. As of now, there are more than 2.5 billion monthly active users of YouTube.
Let's take a lool at the chart below to see how YouTube users have increased in the last decade.

![increase in monthly active users of YouTube per year](./images/1-2-increase-in-monthly-active-users-of-YouTube-per-year.png)

YouTube is the second most-streamed website after Netflix.  
A total of 694,000 hours of video content is streamed on YouTube per minute.

> Let's evaluate your understanding of the building blocks required in the design of YouTube. Based on the functional and non-funtional requirements given below, **_identify three key building blocks needed to curate the design of YouTube_**.
>
> **Functional requirements:**
>
> - Stream videos
> - Upload videos
> - Search videos according to titles
> - Like and dislike videos
> - Add comments to videos
>
> **Non-functional requirements:**
>
> - High availbility
> - Scalability
> - Good performance
>
> You should focus on the building blocks required for back-end storage and efficient data transmission to end users.
>
> ---
>
> The three key building blocks required to design a system like YouTube are:
>
> 1. **Databases:** Essential for storting metadata of videos, thumbnails, comments, and user information.
> 2. **Blob storage:** Important for storing the actual video files on the platform.
> 3. **A CDN (Content Delivery Network):** Crucial for efficiently delivering content to end users, which helps in reducing latency and the load on the original servers.

Let's understand how to design YouTube in the next lessons.

## How will we design YouTube in the next lessons.

We've divided the design of YouTube into five lessons:

1. **Requirements:**  
   This is where we **identify the functional and non-functional requirements.** We also **estimate the resources required to serve** millions of users each day.  
    This lesson answers questions like how much **storage space YouTube will need to store 500 hours of video content uploaded** to YouTube per minute.
2. **Design:**  
   In this lesson, we explain **how we'll design the YouTube service.** We also briefly **explain the API design and database schemas.**  
    Lastly, we will also briefly go over **how YouTube's search works**.
3. **Evaluation:**  
   This lesson explains how YouTube is able to fulfill all the requirements through the proposed design.  
   It also looks at **how scaling in the future can affect** the system and **what solutions are required** to deal with scaling problems.
4. **Reality is more complicated:**  
   During this lesson, we'll **explore different techniques that YouTube employs to deliver content effectively** to the client and avoid network congestions.
5. **Quiz:**  
   We reinforce major concepts we learned designing YouTube by considering how we could design Netflix's system.

Our discussion on the usage of various building blocks in the design will be limited since we've already explored them in detail in the building block chapter.
