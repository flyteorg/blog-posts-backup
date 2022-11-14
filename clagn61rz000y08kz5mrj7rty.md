# Flyte Monthly

This blog post is an edited, public copy of Flyte Monthly’s November 2022 newsletter issue. To subscribe, visit getrevue.co/profile/flyte. 

Welcome to our newsletter, and thank you for subscribing.

Each month, we’ll keep you up to date with Flyte news, the latest releases and features, tutorials and workshops, events, and maybe a top meme or tweet.

# What's Up?
## KubeCon + CloudNativeCon North America 2022
Union.ai represented Flyte at KubeCon + CloudNativeCon North America 2022 in Detroit. In addition to the physical booth on site, we also had a [virtual booth](https://bit.ly/3Cy6iz5) that’s still live through Nov 21.
![Virtual Booth](https://cdn.hashnode.com/res/hashnode/image/upload/v1668118114031/Mt3Zg2V0h.jpg align="left")
Flyte maintainers [Yee Hing Tong](https://www.linkedin.com/in/yeehingtong/), [Katrina Rogan](https://www.linkedin.com/in/katrina-rogan-64660120/) and [Daniel Rammer](https://www.linkedin.com/in/daniel-rammer-b1ab4249/) hosted the Flyte booth during the event, while [Eduardo Apolinario](https://www.linkedin.com/in/eduardo-apolinario-7b222035/), [Samhita Alla](https://www.linkedin.com/in/samhita-alla/) and [Shivay Lamba](https://www.linkedin.com/in/shivaylamba/) demoed Flyte and the all-new Flyte Sandbox during the virtual booth live office hours. Read up on the press release ([Orchestration Takes Center Stage at KubeCon](https://www.benzinga.com/pressreleases/22/10/n29374723/orchestration-takes-center-stage-at-kubecon)), and try out the Flyte Sandbox [here](http://go.union.ai/8dOxsLys).

## GITEX Global Dubai 2022
[Haytham Abuelfutuh](https://www.linkedin.com/in/haythamafutuh/?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) and [Niels Bantilan](https://www.linkedin.com/in/nbantilan/?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) of Union.ai attended GITEX Global Dubai 2022, where they conducted presentations on Flyte, UnionML and Pandera.
![Haytham and Niels giving the presentation "Unveiling Flyte" (Source: AiEverything)](https://cdn.hashnode.com/res/hashnode/image/upload/v1668190015636/3PMH16epW.jpg align="left")
Recordings of the talk will be published in the coming months. Until then, you can find the presentation materials [here](https://go.union.ai/Y9DYKxsC).

# Flyte's Latest...
## … Release 🚀
Look for a **Flyte v1.3.0** release coming out in January 2023. Details on what that release will contain is in the section below, but keep an eye on our [GitHub](http://go.union.ai/jQkZcUUR) page for the official release.

## Have suggestions for Flyte?
We’d love to hear from you during our next Quarterly Planning Meeting in early January. We’ll discuss the future of Flyte’s development, community involvement, and we’ll take feature requests for upcoming releases!

Mark the event in your calendar [here](https://evt.to/auussgugw?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter).

The first quarterly planning meeting was in October, when [Eduardo Apolinario](https://www.linkedin.com/in/eduardo-apolinario-7b222035/?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) covered the Flyte Roadmap for v1.3.0, and other features for the release in early 2023. The full rundown is in the recording below.

%[https://youtu.be/5rbsTJ6JQeo]

You can also raise suggestions in our Slack, specifically in channels #ask-the-community and #feature-discussions, or directly let Eduardo Apolinario know!

## … Success Story 🤝
**Lacuna.ai**

Lacuna.ai is a transportation technology company that creates products to help its customers optimize traffic movement for cities, airports and other complex networks of moving vehicles.

At the most recent Flyte Community Sync, Machine Learning Engineer [Wen Shu Tang](https://www.linkedin.com/in/wenshutang?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) spoke about the conception of Lacuna.ai and the problems it faced.

When evaluating workflow frameworks for Lacuna.ai, Wen Shu said he needed to balance data overhead and ease of adoption. Flyte’s clean abstractions and active community helped him choose Flyte. Here’s how Wen Shu convinced his team of data scientists that Flyte is critical for their workflows:
> It was a difficult case I had to make to the team because these things I’m talking about — operational efficiency, or lower infrastructural overhead, or having good abstractions — they are all very abstract concepts. For somebody who comes from an academic or pure data-science domain, it’s very hard to grasp. So I very much had to play the role of advocacy and try to evangelize it. At the end of the day, it’s not something that’s tangible. So I think being able to start the process of converting some of our work into something tangible, into something you can visualize, that’s another advantage of Flyte that I really like.

Hear Wen Shu’s full journey in guiding Lacuna.ai’s adoption of Flyte below.

%[https://www.youtube.com/watch?v=2-3nOahd3IY]

# This Is How We Do It... ⚙️
## Flyte Roadmap
In the video below, [Eduardo Apolinario](https://www.linkedin.com/in/eduardo-apolinario-7b222035/?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) details Flyte’s recent v1.2.0 release, which includes changes such as extended workflow execution names, and a large performance improvement by offloading FlyteWorkflow CRD. In addition, Eduardo highlights the timeline for v1.3.0, and the highly requested features in store for that update: human-in-the-loop tasks, and out-of-core back-end plugins!

%[https://youtu.be/nIFaeRj72QM?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter]

## Flyte Map Tasks
A Flyte blog post by [Vijay Saravana Jaishanker](https://hashnode.com/@vijaysaravana?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) of Woven Planet describes how Flyte map tasks offer a simpler user experience than Spark workflows. Vijay goes into detail about Spark’s features and use cases, and where Flyte’s workflows are able to relieve some of the user pain points in areas where Spark’s more advanced features aren’t required. Examples and further comparison can be read in the article here:

%[https://go.union.ai/mwqtkpLl]

# In The News 📣
## Union-Hosted Flyte Sandbox
Union.ai has announced that it is hosting a Flyte Sandbox! Essentially a trial and walkthrough, Union.ai’s Flyte Sandbox lets users test Flyte features in a self-contained environment within your browser. Check out [this blog post](http://go.union.ai/V6wwuFKP) to learn how to get started!
## Hacktoberfest Recap
Hacktoberfest 2022 is over, and the Flyte community finished strong! We’re pleased to report that we had 23 contributors, 42 issues and 50 PRs between Flyte and UnionML.

**Contributions**

[Ryan Nazareth](https://github.com/ryankarlos?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) is the top contributor and he’ll be soon receiving an O’Reilly individual premium annual subscription worth $500! His contributions include:
- [Plugin for supporting Vaex DataFrame type](https://github.com/flyteorg/flytekit/pull/1230?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter)
- [TypeTransformer for TensorFlow TFRecord](https://github.com/flyteorg/flytekit/pull/1240?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter)
- [Fixing mypy errors for incompatible types](https://github.com/flyteorg/flytekit/pull/1245?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter)
- [Tutorial on running an NLP workflow with Flyte](https://github.com/flyteorg/flytesnacks/pull/911?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter)

Other noteworthy Hacktoberfest contributions:
- TensorFlow [tensor](https://github.com/flyteorg/flytekit/pull/1269/?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) and [model](https://github.com/flyteorg/flytekit/pull/1241?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) types by [@VPraharsha03](https://github.com/VPraharsha03?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) and [@techytushar](https://github.com/techytushar?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter)
- [GitPod environment setup](https://github.com/unionai-oss/unionml/pull/187?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) in UnionML by [@MrKrishnaAgarwal](https://github.com/MrKrishnaAgarwal?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter)
- [Listing differences on conflicting definition when registering](https://github.com/flyteorg/flyte/issues/2522?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) by [@jerempy](https://github.com/jerempy?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter)
- [Using passthrough workflowstore to perform CRD terminations / manipulations](https://github.com/flyteorg/flytepropeller/pull/496?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) by [@daniel-shuy](https://github.com/daniel-shuy?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter)
- [Dockerizing call to “go generate” in the flyteidl makefile](https://github.com/flyteorg/flyteidl/pull/338?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) by [@daniel-shuy](https://github.com/daniel-shuy?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter)

![Union and Flyte at Hacktoberfest 2022](https://cdn.hashnode.com/res/hashnode/image/upload/v1668202458173/D4mbWhAOQ.webp align="left")

# Hear Us Speak 🎤
## Upcoming…
[Andrew Dye](https://www.linkedin.com/in/andrewwdye/?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) was recently a guest on the MLOps Community Podcast, and we’re thrilled for the release of that recording! Keep an eye out for it [here](https://anchor.fm/mlops/episodes/MLOps-Coffee-Sessions-12-Flyte-an-open-source-tool-for-scalable--extensible---and-portable-workflows-eksa5k?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) in the upcoming weeks.

If you would like to see Flyte at any of your favorite meetups, let us know! Contact Tyler Su in the [Flyte Slack](https://slack.flyte.org/?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) for details.

# Coming Up... 📅
On Nov. 15 at 9 a.m. PT, we’ll hold our biweekly Community Sync! We’ll feature Calvin Leather from Embark Veterinary, in addition to our usual agenda of community updates.

Mark the event on your calendar [here](http://go.union.ai/amFY0VAm?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter).

# Where To Find Us 🔎

**Slack**

Join us on [Slack](https://slack.flyte.org/?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) to ask a question, learn from a thread, follow announcements or just say hi!

**Twitter**

Follow us on Twitter at [@flyteorg](https://twitter.com/flyteorg?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) for everything Flyte.

**Zoom**

- Weekly on Wednesdays” office hours: three 30-minute sessions at [7:00 a.m. PT](https://www.addevent.com/event/zF10349020/?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter), [1:30 p.m. PT](https://app.addevent.com/event/fW13717944/?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) and [9:00 p.m. PT](https://www.addevent.com/event/dQ10349168/?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter)
- Community Sync: Join us twice a month on [Tuesdays at 9 a.m. PT](https://www.addevent.com/event/rT7842495?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) for presentations and open discussions

**GitHub**

Post a question or comment on [GitHub discussions](https://github.com/flyteorg/flyte/discussions/categories/q-a?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter), open an issue, or show us your work!

# Thank You Contributors! 🙏
Flyte would like to recognize ✨Contributors of the Month✨ of October, for their valuable work and contributions:

- **Ailin Yu**, [@niliayu](https://github.com/niliayu?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) implemented [container-level configuration on default PodTemplate](https://github.com/flyteorg/flyteplugins/pull/280?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) to improve the configurability of Flyte

- **Nicholas LoFaso**, [@nicklofaso](https://github.com/nicklofaso?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) leads the Flyte + MethaneSAT integration. Check out [Nick’s talk on how MethaneSAT’s transforming satellite data with Flyte](https://www.youtube.com/watch?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter&v=dCa4Ou7H41E)

- **Justin Tyberg**, [@jtyberg](https://github.com/jtyberg?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) helped us [discover a security vulnerability](https://github.com/flyteorg/flyteadmin/security/advisories/GHSA-67x4-qr35-qvrm?utm_campaign=Flyte%20Monthly&utm_medium=email&utm_source=Revue%20newsletter) on the OAuth authorization server (fixed in flyteadmin 1.1.44)
