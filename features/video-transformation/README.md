---
description: URL-based parameters to manipulate videos in real-time.
---

# Video Transformation (Beta release)

## URL Based transformations

Using ImageKit.io, you can perform real-time transformations to deliver perfect videos to the end-users. These transformations can be performed by using the URL-based transformation [parameters](../image-transformations/resize-crop-and-other-transformations.md). 

{% tabs %}
{% tab title="URL structure" %}
```markup
        URL-endpoint        transformation   video path                                    
┌──────────────────────────┐┌────────────┐┌───────────────┐
https://ik.imagekit.io/demo/tr:w-300,h-300/sample-video.mp4
```
{% endtab %}
{% endtabs %}

* **Original video**\
  [https://ik.imagekit.io/demo/sample-video.mp4](https://ik.imagekit.io/demo/sample-video.mp4)
* **Resized 300x300 video**\
  [https://ik.imagekit.io/demo/`tr:w-300,h-300`/sample-video.mp4](https://ik.imagekit.io/demo/tr:w-300,h-300/sample-video.mp4)

These transformation parameters `w-300,h-300` can be added in the URL as path params or as query parameters.

* **As path parameter** [https://ik.imagekit.io/demo/`tr:w-200,h-200`/sample-video.mp4](https://ik.imagekit.io/demo/tr:w-200,h-200/sample-video.mp4)
* **As query parameter** [https://ik.imagekit.io/demo/sample-video.mp4?`tr=w-200,h-200`](https://ik.imagekit.io/demo/sample-video.mp4?tr=w-200,h-200)

## Limitations of the Beta release

* Input video upto `300MB` in size is supported for transformations. This limit can be adjusted based on your pricing plan.
* When you request a new transformation or have turned on video optimization features, if the video is not cached on CDN or our internal caches, ImageKit will transform the video in real-time. However if the video file is being downloaded from your origin takes more than 15 seconds, ImageKit will give a 302 and serve the original content. Within a few seconds, optimized transformations are generated and stored in our caches. From that point onwards, we will serve the actual transformed video.

## Recommendations

Here are a few recommendations you should follow while using video API in a live environment.

### Eagerly generate transformation
Whether a video is optimized or transformed, a new video(s) is generated and saved internally by ImageKit. This process could take a few seconds, depending on the video duration, output format, and transformation parameters. Therefore, you should eagerly generate the transformation before using the video URL in a live environment to ensure it works as expected. It guarantees that all necessary transformations are prepared and ready to be served. This avoids unnecessary data transfer costs on your origin. In the beta version, you will have to manually or programmatically get the resource to see if the `200` status code is received. That means transformation is ready. On the other hand, if a `302` is received, it means transformation is not prepared. In that case, you should wait. In the future, we will support webhook, which you can implement to get updates on asset transformations.

### Check assets in Chrome and Safari
ImageKit prepares a `webm` and `mp4` variant of the video to serve optimized video to your users. In a few cases, for specific input videos, the encoding could permanently fail for one or both formats. In this case, you will always get `302` instead of `200`. Using such a URL in a live environment would result in repeated transformation attempts. This will unnecessarily increase origin data transfer at your end. To ensure that the video is ready in both formats, always fetch the same URL in both Chrome and Safari.
