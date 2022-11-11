# Flyte Monthly

This blog post is an edited, public copy of Flyte Monthly’s October 2022 newsletter issue. To subscribe, visit https://www.getrevue.co/profile/flyte.  

# What’s Up?

Flyte returned to Hacktoberfest — but not alone. This year, [UnionML](https://www.union.ai/unionml), an open-source Python framework built on top of Flyte, joined the fun!

![Hacktoberfest2022.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1667924637361/iL6VC6gFW.jpg align="left")

Making contributions is super simple! For one month, Hacktoberfest contributors can choose any “hacktoberfest”-labeled issue on GitHub, create pull requests linked to the issue, and fill out this [form](https://tally.so/r/nWO7qQ) to claim their swag. Details were featured in the [Meet Flyte and UnionML at Hacktoberfest 2022](https://blog.flyte.org/meet-flyte-and-unionml-at-hacktoberfest-2022) blog as well as this recording from a Flyte Community Sync event - [Flyte at Hacktoberfest 2022](https://www.youtube.com/watch?v=iQ_GW1vCIkA).

# Customer Success

## Infinome Biosciences

[Infinome Bio](https://infinomebio.com/) is a biotech startup working toward a sustainable bio-enabled future, using advancements in gene editing and lean bioengineering to unlock the potential of synthetic biology for highly scalable, safer, affordable and accessible solutions.

Data Scientist [Krishna Yerramsetty](https://www.linkedin.com/in/krishnayerramsetty/) talked to us about Infinome’s journey with Flyte, highlighting some challenges that the industry faced, how Flyte offers features that drastically cut processing time, and why Flyte was the right fit for them. According to Krishna:

> “Biology is always the bottleneck. It’s hard, so we want to make life easier for data scientists. Even if that means sometimes custom pipelines need to be built, or we need to spend a lot of time trying to make a workflow file format easily readable by biologists, that’s definitely what we want to do. We don’t want to take shortcuts for data science and make life easy for biology, and that’s where I think Flyte really really shines for us.”

Catch the full story in the video [How Infinome is Using Flyte - Krishna Yerramsetty](https://www.youtube.com/watch?v=GsJK3_-SRwo).

# Tech News

## Flyte’s Latest Releases

[Flyte v1.2.0](https://github.com/flyteorg/flyte/releases/tag/v1.2.0) was released on Oct. 3!

The most notable feature of this release, Flyte’s Ray integration, was thanks to valuable contributions from Spotify — one of Flyte’s earliest customers — who identified a pain point, talked to us, and worked with us on feature requests and PRs that resulted in Flyte improvements with significant performance gains.

The Ray integration supports large nested and dynamic workflows so they can be shared across teams or implemented as part of even bigger workflows.
It can now reduce the size of Kubernetes resources for large Flyte workflows, by having FlyteAdmin write parts of the CRD to storage before it creates the Flyte workflow resource on Kubernetes. FlytePropeller then reads the CRD parts when handling the Flyte workflow resource. Learn more on the Ray integration in the blog [Ray and Flyte: Distributed Computing and Orchestration](https://blog.flyte.org/ray-and-flyte) by [Kevin Su](https://www.linkedin.com/in/pingsutw/).

In addition to Ray support, v1.2.0 includes:
- Perf improvements
- Two Flytekit plugins: DBT plugin and hugging face
- Improvements to CLIs, including changes to PyFlyte Run

## This is How We Do it

### Flyte Deployment Journey

Follow Union.ai Software Engineer [Eduardo Apolinario](https://www.linkedin.com/in/eduardo-apolinario-7b222035/) as he takes you through performance improvements in Flyte v1.2.0. 

The video [Flyte Roadmap — Deployment Journey](https://www.youtube.com/watch?v=ikOAwVLjHUM), streamlines Flyte’s Getting Started, and a typical deployment journey, from a self-contained Flyte environment, to cloud deployment; production-ready; and finally, operating core infra.

### Flyte Cheat Sheet

Introducing the all-new Flyte Cheat Sheet!

![FlyteCheatSheet.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1667925198657/Ma1oOYT2P.jpg align="left")

**What’s in it?**
- Code snippets of crucial Flyte concepts
- Latest fundamental Flyte features tied to Python code
- Flytekit syntax and features

**Who is it for?**
- Data and ML engineers

**Why use it?  **
- To reduce the time invested in Flytekit coding

For more information and a download link, check out the [Flyte Cheat Sheet](https://blog.flyte.org/flyte-cheat-sheet) blog by Union.ai Tech Evangelist [Samhita Alla](https://www.linkedin.com/in/samhita-alla/).

# Community News

## Tweet of the Week

The [Twitter thread](https://twitter.com/Ubunta/status/1573323620197212160) on rebuilding Data/ML stacks using Flyte and other tools was going strong for almost a week, earning over 3.1K engagements! Check it out!

![TwitterThreadUbunta.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1667925355172/-O0uREt3v.jpg align="left")

## Talks / MeetUps / Community Events

### ML Hangout

![MLHangout.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667935881122/XowktkLC2.png align="left")

Flyte and Ray developers recently came together for an in-person hangout to discuss all things Kubernetes.

Anyscale Engineering Manager [Richard Liaw](https://www.linkedin.com/in/richardliaw/) presented the technology behind [Ray](https://www.anyscale.com/ray-open-source), while Union.ai Software Engineer [Eduardo Apolinario](https://www.linkedin.com/in/eduardo-apolinario-7b222035/) took a deep dive into the [Flyte-Ray integration](https://blog.flyte.org/ray-and-flyte).

Keep an eye out for the next one!

### Open Source Summit Europe

![OSSummitEurope.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1667925958308/spIFHSCge.jpg align="left")

Merantix Momentum MLOps Lead [Dr. Fabio Grätz](https://www.linkedin.com/in/fabiomgraetz/) presented at the Open Source Summit in Europe “**Building Robust ML Production Systems Using OSS Tools for Continuous Delivery for ML (CD4ML)**.” The talk discussed how Merantix Momentum:

- Lives a DevOps and MLOps culture
- Introduces software best practices to ML, and 
- Builds robust ML production systems using vendor-agnostic OSS tools such as Flyte, Mlfow, Squirrel, Hydra and Seldon.

Watch the [recording](https://www.youtube.com/watch?v=LXaLuaOUzLs) on the [Linux Foundation YouTube](https://www.youtube.com/c/LinuxfoundationOrg) channel.

### Global DevSlam

![GlobalDevslam.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1667927790915/H1ZSY_RaB.jpg align="left")

Flyte Co-founder [Haytham Abuelfutuh](https://www.linkedin.com/in/haythamafutuh/) and Union.ai ML Engineer [Niels Bantilan](https://www.linkedin.com/in/nbantilan/) introduced Flyte and two Flyte Ecosystem projects, UnionML and Pandera, in these talks at the [GITEX](https://www.gitex.com/) conference in Dubai:

- Unveiling Flyte: The ML workflow behind Lyft, Spotify, Freenome and others
- UnionML: an MLOps framework for building and deploying ML microservices
- Deep dive: Introducing the Pandera project to make data-processing pipelines more readable and robust

### KubeCon + CloudNativeCon North America 2022

![KubeConNA22_WebGraphics_snackable2.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1667935954276/2OfabqHaS.jpg align="left")

Flyte will be participating at KubeCon, sponsored by Union.ai. The physical booth will feature demos and videos, and a chance to connect with Flyte developers and maintainers. 

The virtual booth will feature videos and live office hours, including a demo of the latest Union-Hosted Flyte Sandbox. Try it out at [sandbox.union.ai](https://signin.hosted.unionai.cloud/). 

![VirtualBooth.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1667936001623/hBFsDa_Bj.jpg align="left")

### Flyte Quarterly Planning Meeting

To respond directly to the needs of our Flyte users, we are opening up the planning process to the community.

Join Flyte maintainers in the next Flyte Quarterly Planning Meeting in early January 2023. Take part in improving the planning process, increasing community involvement, and surfacing high-level feature requests to help drive the roadmap going forward. 

In the meantime, send us suggestions on [Slack](https://slack.flyte.org) in the **#ask-the-community** and **#feature-discussions** channels, or ping @Eduardo Apolinario (eapolinario) directly.

## Publications

### [Orchestration for Data, ML & Infrastructure](https://www.union.ai/blog-post/orchestration-for-data-machine-learning-and-infrastructure)

Zwift Senior Data & ML Engineer [Sarah Floris](https://www.linkedin.com/in/sarah-floris/) and Union.ai Software Engineer [Samhita Alla](https://www.linkedin.com/in/samhita-alla/) put their heads together to formulate this long-awaited guide to data, machine learning and infrastructure-orchestration techniques. Make an informed decision about which one works best for your use case.

## Thank You, Contributors

Flyte would like to recognize ✨Contributors of the Month✨ of September, for their valuable work and contributions:

- **Zev Isert** [@zevisert](https://github.com/zevisert), for working on a UnionML [PR](https://github.com/unionai-oss/unionml/pull/147) that checks and warns about dirty Git index during deployment.
- **Arief Rahmansyah** [@ariefrahmansyah](https://github.com/ariefrahmansyah) & Eduardo Apolinario [@eapolinario](https://github.com/eapolinario), for adding a Flytekit plugin for DBT (available in the upcoming Flytekit release).

And many thanks to the whole [Community of Contributors](https://github.com/flyteorg/flyte/blob/master/README.md)!
Every PR counts!

# Join Us!

Be part of the Flyte Community by joining any or all of the following:
- [Slack](https://slack.flyte.org): Ask a question, learn from a thread, follow announcements, or just say hi!
- Twitter: at [@flyteorg](https://twitter.com/flyteorg) for everything Flyte.
- Zoom: 
>- “Weekly on Wednesdays” office hours: three 30-minute sessions at [7:00 a.m. PT](https://www.addevent.com/event/zF10349020/), [1:30 p.m. PT](https://app.addevent.com/event/fW13717944/) and [9:00 p.m. PT](https://www.addevent.com/event/dQ10349168/)
> - Community Sync: Join us twice a month on [Tuesdays at 9 a.m. PT](https://www.addevent.com/event/rT7842495) for presentations and open discussions.
- [GitHub](https://github.com/flyteorg): Post a question or comment on [GitHub discussions](https://github.com/flyteorg/flyte/discussions/categories/q-a), open an issue or show us your work!







