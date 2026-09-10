# 470x Less Energy, Same Ten-Minute Video

**Status:** Published to anth.us.

**Published as:** `posts/desert-ant-little-brains.mdx` @ `ed349aa` in AnthusAI/anthus-site-content main
(originally staged on branch `posts/desert-ant-little-brains` @ `c66b883`)
**Cover:** Desert Ant official OG image (`images/posts/desert-ant-little-brains.png`)
**Source:** https://desertant.com/blog/introducing-desert-ant-labs/

Title and lead were picked from three options each (Ryan reviewed) before publish:
title "470x Less Energy, Same Ten-Minute Video", numbers-first lead paragraph.
Checked with Limatus against Anth.us's style profile before publish: one advisory
finding (a claim sourced from Desert Ant's own linked announcement), nothing
blocking.

## Published MDX

```mdx
---
title: "470x Less Energy, Same Ten-Minute Video"
slug: "desert-ant-little-brains"
date: "2026-09-10"
authors:
  - author: <a href="/ryan">Ryan Porter</a>
tags:
  - posts
excerpt: |
  A 284MB on-device model does a video-editing job 470x cheaper on energy than Claude Sonnet — one of eighteen small models a European lab just shipped to stop the frontier-API burn on always-on work.
preview_image: "../images/posts/desert-ant-little-brains.png"
images:
  - "../images/posts/desert-ant-little-brains.png"
state: published
---

import BlogImage from "../../components/blog-image"

<p class="center-full-image">
  <BlogImage
    className="center-small-image"
    images={props.pageContext.frontmatter.images}
    name="desert-ant-little-brains.png"
    alt="Desert Ant Labs launch banner: on-device intelligence for every product"
  />
</p>

A 284MB model just turned a ten-minute video into a dozen shorts in five seconds — ten times faster and, by [Desert Ant Labs](https://desertant.com/blog/introducing-desert-ant-labs/)'s own numbers, four hundred seventy times less energy than sending the same job to Claude Sonnet. It's one of about eighteen small, specialized on-device models the company just shipped, built for the always-on work that never needed a frontier model in the first place.

The stack runs through one Swift / Kotlin / JavaScript SDK — twelve of those models stable, six in beta — with a free tier up to 100k monthly active devices and no token meter. They also cite NVIDIA researchers estimating that forty to seventy percent of the calls in three agent systems could go to a small specialized model instead of a large one.

That is the same thrift family as [The Year Coding Became a Commodity](/blog/ai-coding-cost-collapse-2026/), pointed at inference rather than coding model swaps: a little brain for the always-on work, and the big brain only when the job actually needs it.
```
