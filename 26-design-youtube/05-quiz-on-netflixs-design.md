# The Reality Is More Complicated

Learn how YouTube can use different techniques to effectively deliver content to the end user.

> We'll cover the following:
>
> - Introduction
> - Encode
> - Deploy
>   - Recommendations
> - Deliver
>   - Adaptive streaming
> - Potential follow-up questions

## Introduction

Now that we’ve understood the design well, let’s see how YouTube can optimize the usage of storage and network demands while maintaining good quality of experience (QoE) for the end user.

When talking about providing effective service to end users, the following three steps are important:

- **Encode:** The raw videos uploaded to YouTube have significant storage requirements. It’s possible to use various encoding schemes to reduce the size of these raw video files. Apart from compression, the choice of encoding scheme will also depend on the types of end devices used to stream the video content. Since multiple devices could be used to stream the same video, we may have to encode the same video using different encoding schemes resulting in one raw video file being converted into multiple files each encoded differently.  
   This strategy will result in a good user-perceived experience because of two reasons: users will save bandwidth because the video file will be encoded and compressed to some limit, and the encoded video file will be appropriate for the client for a smooth playback experience.
- **Deploy:** For low latency, content must be intelligently deployed so that it is closer to a large number of end users. Not only will this reduce latency, but it will also put less burden on the networks as well as YouTube’s core servers.
- **Deliver:** Delivering to the client requires knowledge about the client or device used for playing the video. This knowledge will help in adapting to the client and network conditions. Therefore, we’ll enable ourselves to serve content efficiently.

Let’s understand each phase in detail now.

## Encode

Until now, we’ve considered encoding one video with different encoding schemes. However, if we encode videos on a per-shot basis, we’ll divide the video into smaller time frames and encode them individually. We can divide videos into shorter time frames and refer to them as segments. Each segment will be encoded using multiple encoding schemes to generate different files called chunks. The choice of encoding scheme for a segment will be based on the detail within the segment to get optimized quality with lesser storage requirements. Eventually, each shot will be encoded into multiple chunk sizes depending on the segment’s content and encoding scheme used. As we divide the raw video into segments, we’ll see its advantages during the deployment and delivery phase.

Let’s understand how the per-segment encoding will work. For any video with dynamic colors and high depth, we’ll encode it differently from a video with fewer colors. This means that a not-so-dynamic segment will be encoded such that it’s compressed more to save additional storage space. Eventually, we’ll have to transfer small
