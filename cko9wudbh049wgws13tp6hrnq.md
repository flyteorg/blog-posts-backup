## Flyte Joins LF AI & Data

By: Anand Swaminathan

*This [article](https://eng.lyft.com/flyte-joins-lf-ai-data-48c9b4b60eec) was originally published on the Lyft Engineering blog on March 19th, 2021.*

A little over a year ago on January 7th, 2020 Lyft [open-sourced and announced](https://eng.lyft.com/introducing-flyte-cloud-native-machine-learning-and-data-processing-platform-fb2bb3046a59) Flyte — its core platform for orchestrating Machine Learning and Data Processing Jobs. Today we are elated to share that [Flyte](https://flyte.org/) is joining the Linux Foundation AI & Data ([LF AI & Data](https://lfaidata.foundation/)), as its 25th hosted project. 2020 has been a tough and long year for many organizations around the world, but at the same time, it has been a year in which Flyte made its slow and steady march towards maturity. In this post, we will go over the history of Flyte and its journey to the foundation and how we believe it will continue to progress.

# Road to Open source
Flyte was born in late 2016 out of the need to deliver models for a core product at Lyft — Estimated Time of Arrival (ETA). This product had solved problems unique to Lyft’s ETA needs…or so we thought:

- Large amounts of historical data had to be used to train a set of ensemble models.
- The output artifacts could be as complex as a generated map or a trained model.
- Backtesting a model and validating the hypothesis was a requirement.
- Once in production, retraining the model frequently was required.
- Multiple models were developed, tested, and deployed simultaneously. Our largest cause for the slowdown was often the need to procure, manage and build new infrastructure, which may not be useful if the model did not work out.

As we developed a solution for ETA, we quickly realized that other internal teams were interested in using a similar platform. This led to a new set of challenges and goals from the maintainers’ point of view like:

- Agile management and development of the platform.
- Ability to deploy upgrades and changes to the platform without affecting the users.
- Ability to monitor the platform and provide insights for the operators to troubleshoot.
- Clearly separating cases in which the platform failed vs the user failed.
- Providing a single hosted experience for the entire company — which meant built-in multi-tenancy.
- Providing elastic scalability, which would grow as the demand for the platform grew.

In late 2017, we launched our v1 of Flyte at Lyft, using a cloud-available scheduler (AWS Step Functions). This enabled us to make a dramatic impact on the organization and adoption grew exponentially. At the same time, we started talking with other companies and realized that orchestration was a common problem. We also recognized that building such a platform would benefit greatly from a large vibrant community.

As the usage of the initial version grew within Lyft, we started noticing problems with scale and usability. AWS Step Functions made it hard for us to add new features natively and we identified the need to build a container native scheduling engine. The learnings from our previous iterations resulted in Flyte. The team has been continuously and incrementally developing Flyte over the last few years and the solution was scalable and reliable enough to serve the needs of large organizations. As we felt that the product had matured, we open-sourced it in early 2020 and began our journey into building an ambitious open-source project.

# Building the community
Since the beginning of our open source journey, [Spotify](https://www.spotify.com/) has been a foundational partner for Flyte. Their involvement meant Flyte had to be portable across clouds right out of the box. Thus Flyte is designed to work across clouds and has been battle-tested on AWS and GCP. As the events of the year 2020 unfolded, the Flyte core team was focused on ensuring that the total cost of running Flyte at Lyft was low and the open-source onboarding experience suffered.

By mid-2020, Flyte was powering more than 1 million pipelines at Lyft, across the ETA, Pricing, Mapping, Driver Engagement, Growth, and Map generation teams, and growing. Around this time, we received contributions from the open-source community that dramatically improved Flyte. [Freenome](https://www.freenome.com/), a biotech company pioneering work in early cancer detection improved our “Getting Started” user experience. Spotify built [JAVA/Scala SDK for Flyte](https://github.com/spotify/flytekit-java) which validated the true polyglot power of Flyte’s declarative, specification based model.

In the first year of open-sourcing, we realized that one of the crucial components of Flyte was the python SDK — [flytekit](https://github.com/flyteorg/flytekit). Foundations of flytekit were built in early 2017 and since then the user interface remained almost consistent, with minor modifications, to ensure backward compatibility for existing users. This interface was built pre-python-3 and was cumbersome to extend. By the end of 2020, the core team completed a major revamp of flytekit — which introduced an innovative typing engine based on python3 type hints and made it possible to express complex ideas succinctly. [Flytekit](https://docs.flyte.org/projects/flytekit/en/latest/) now works completely locally without needing a Flyte backend for most development scenarios and makes it possible to customize the DSL for your use cases. We encourage you to checkout flytekit and the tutorial on how to use, extend and leverage it in your projects.

# Contribution & Onwards journey
Flyte started with the vision of truly unifying systems, projects, and workflows with an extremely intuitive and approachable design language. From the onset, we realized that Flyte was an ambitious and enormous project that could only work with a vibrant, thriving, and helpful community. We are very lucky to have amazing partners who are responsive and contribute back to Flyte.

Going forward, we want Flyte to become the conduit for collaborating across myriad open-source projects. To achieve this it is essential that Flyte be perceived as a truly open platform so that we can break the silos and truly help our users. This is sorely missing in the current open-source landscape. To make this happen, Lyft decided to contribute Flyte to the Linux Foundation (AI & Data chapter). This we feel is a giant step forwards towards our goal of simplifying the life of all the hardworking ML and Data Engineers.

With the solid foundation in place, we will accelerate the feature set and integrations in Flyte. This includes multiple projects that make Flyte incredibly useful for organizations, both small and large. Some of these projects include:

**Event Egress** — Flyte now supports egressing events to your desired pub-sub channel. Platform builders and users can subscribe to these events and react to them.

**Flytekit for builders** — Flytekit provides the necessary tools to write customized DSLs, which can provide a more tailored experience for certain use-cases.

**Reactive pipelines** — Flyte workflows can react to external data and events generated in other workflows.

**Performance and scalability improvements** — Ability to run concurrent workflows with thousands of nodes, scalable to multiple clusters with minimal deviation from ideal performance.
Improved UI and UX — Improved interface to visualize, track and compare experiments, executions and artifacts.

**Gitops using flytectl** — Most user-facing Flyte entities are completely versioned, but certain core entities, like projects, domains, etc. are not versioned. The Flyte community has been working on flytectl, which intends to be a simple CLI for interacting with Flyte and provide a gitOps interface for managing non-versioned administration entities.

**Integrations and more integrations** — ML Ops observability tools, DataFrame scalers (Vaex, etc), Modern compute frameworks (Flink, Ray), customized and specialized type extensions (TFData, etc), data correctness and quality plugins (Pandera, Great expectations, etc), interaction with SQL engines and more.

Contributing Flyte to LF AI & Data is just the first step in the journey. We invite all of you to checkout Flyte at [https://flyte.org](https://flyte.org), drop us a line or bring in new ideas for collaboration.

Cover Image Credits: Photo by [Pawel Czerwinski](https://unsplash.com/@pawel_czerwinski) on [Unsplash](https://unsplash.com/photos/hNrd99q5peI).


