# The Three Virtues of MLOps: Velocity, Validation and Versioning

UC Berkeley Ph.D. candidate Rolando Garcia recently presented the findings of his team’s “[Operationalizing Machine Learning: An Interview Study](https://arxiv.org/pdf/2209.09125.pdf)” research paper in a panel discussion with industry experts and practitioners, hosted by [Union.ai](http://Union.ai) CMO Martin Stein. Guest speakers included machine learning, software engineering and data science professionals from companies such as Lyft, Recogni, Stripe, Woven Planet and Striveworks.

The discussion focused on the role of infrastructure in machine learning production; common MLOps practices; challenges and pain points; and MLOPs tooling advice. The highlight of the conversation was the paper’s finding that “successful MLOps practices center around having high velocity, early validation and maintaining multiple versions for minimal production downtime.”

# A study that explores ML engineers’ process and pains

A team of UC Berkeley scholars headed by Rolando and fellow Ph.D. candidate Shreya Shankar set up semi-structured ethnographic interviews with 18 ML engineers working on chatbots, autonomous vehicles and finance applications, to outline common practices for successful MLOps, discuss pain points and highlight implications of tool design.

Keeping in mind the experimental nature of ML engineering, the paper defines MLOps as a “set of practices that aim to deploy and maintain ML models in production reliably and efficiently.” DevOps is the combination of software developers and operations teams, and MLOps is applying DevOps principles to machine learning.

The interviews found that ML engineers typically perform four routine tasks:

* Data collection and labeling
    
* Model experimentation to improve ML performance
    
* Model evaluation through multi-staged deployment process
    
* ML pipeline monitoring of performance drops in production
    

The interviews also exposed “three variables that govern success for a production ML deployment: Velocity, Validation, and Versioning.” The paper defines these as follows:

* **Velocity**: quick prototyping and iterating on ideas, such as quickly changing the pipeline in response to a bug
    
* **Validation**: early proactive monitoring of pipelines for bugs
    
* **Versioning**: storing and managing multiple versions of production models and datasets to document every change
    

Typical MLOps pain points originate from tensions and synergies among these three Vs. Examples include:

* Mismatches between development and production environments (velocity vs validation)
    
* Data errors (validation vs versioning)
    
* Keeping GPUs warm (high velocity vs versioning)
    

Additional problematic behaviors result from industry-classroom mismatches, where most of the learning happens on the job, and undocumented tribal knowledge that becomes a problem when developers change jobs (and companies) without leaving proper documentation behind.

When analyzing MLOps tools, Rolando et.al noticed that engineers favored tools that increased one of the three Vs — such as experiment tracking tools that increased iteration speed (velocity), and feature stores that helped debug models based on historical version features (versioning).

The key finding of the paper can be summarized as successful MLOps practices designed for *high* velocity, *early* validation, and maintaining *multiple* model versions for minimal production downtime.

# Panel discussion

During the webinar, Rolando compared the results of his interviews with feedback from the panel guest speakers, who all commonly use the ML workflow orchestrator Flyte as their infrastructure tool, but for varying use cases.

The panel members included:

* Varsha Parthasarathy, Software Engineer, Woven Planet
    
* DJ Rich, Senior Data Scientist, Lyft
    
* Brian Tang, Infrastructure Engineer, Stripe
    
* Michael Lujan, Software Engineer, Striveworks
    
* Fabio Gratz, Ph.D., Senior Software Engineer/ML, Recogni
    

Host Martin Stein opens up the discussion with an important question:

## Do we underestimate the role of infrastructure in ML production?

As part of the perception engineering team that works on ground truth perception, **Varsha**’s team uses Flyte for Woven Planet’s DAGs, and believes that: **“Yes, infra is underestimated. … \[I\]nfra and MLOps go hand in hand.”**

Varsha explains that ML engineers focus on iteration and experimentation, which are in turn facilitated by the infrastructure — and that as a startup, it is not always feasible to keep their GPUs warm. To treat precious computational resources efficiently, tools that provide caching will save teams from having to rerun the entire model because they can rerun only the parts that were updated.

Similarly, versioning tools for models and datasets that support abstraction to rerun and retrain models efficiently will eliminate unnecessary steps from the GPU warm time.

For the past three years, **DJ** has been working on a causal forecasting system at Lyft that predicts budgeting and pricing over a three-month time horizon. The forecasting needs to match the experiments, and requires the integration of models from different teams. While not compute-heavy, the infrastructure Lyft uses must combine and synchronize the different inputs.

At Stripe, **Brian** said, **the team breaks apart its ML infrastructure into parts to focus on exploration via training and notebooking; this in turn increases velocity. To increase its ML production, Stripe went with Flyte because it offers that velocity, further reinforcing the role of infrastructure in ML production.**

**Michael** described how Striveworks created an internal MLOps platform, Chariot, which focuses on the lifecycle of a model. It can upload and annotate a dataset, train it based on specific parameters, catalog and serve the model, and detect model drift.

**As a DevOps engineer, Michael notes that Flyte has helped Striveworks "focus on the model development part without thinking about how we are going to productionize this.”** Since Striveworks uses Jupyter notebooks that are not a productionized version of the model, the scheduling portion of the model lifecycle can take place without occupying GPUs. Because Striveworks works with highly sensitive data for government contractors, the entire platform needs to be deployed in one go, a task Flyte also facilitates.

**Fabio** wrapped up by describing his work at Recogni, building custom accelerators for self-driving cars. He works with a software engineering team that provides an MLOps platform for the perception team, which in turn models trips to steer the cars. They “leverage tools like Kubernetes and Flyte just to make experimentation faster and to help (us) keep track of experiments (we’re) running and being able to reproduce them,” Fabio said.

Fabio also described launching an MLOps team for an ML solutions provider that built an internal developer platform. With the platform, he said, engineers could bring up the infrastructure they needed to train, develop or productionize their models in a self-serve fashion. **He stressed the importance of being able to “abstract away the complexity of the tooling that is typically involved … (since) not everybody can become an infrastructure expert, and most engineers don’t want” to manage infrastructure.**

## How deeply are teams invested in infrastructure?

The discussion then turned toward three points from the interview study:

1. People spend a significant amount of time on the less glamorous aspects of ML, like maintaining and monitoring ML pipelines
    
2. ML debugging trauma creates anxiety
    
3. High velocity risks drowning in a sea of versions
    

Martin asked Rolando to compare the responses of the group interviewed for his study with the experiences recounted by the panelists. Were there specific areas Rolando would want to explore further?

Citing the panelists’ investment in infrastructure, Rolando pointed out that in small companies, the engineers are typically responsible for the entire MLOps workflow. When an infrastructure developer leaves, their replacement typically moves to “rip and replace” the entire system, which can cost up to one year of ML improvement and progression.

However, he predicted, larger companies would “eventually converge to an infrastructure that they trusted and had been along for a long time.” Depending on the company culture, that infrastructure could favor high velocity or the simplicity of reduced versions.

Rolando stated that ML engineers typically spend half their time building the infrastructure, and the other half performing the four routine tasks enumerated in his study: data collection/labeling, experimentation, evaluation and monitoring. What if they could avoid building the infrastructure and just use an out-of-the box MLOps solution?

## How is the infrastructure service outsourced to a third party, and how is that incorporated into the workflows?

According to **Michael,** Striveworks’ data scientists and software engineers often tag-team on the whole model life cycle, then pivot toward creating an MLOps platform. It would help data scientists “to not have to worry about this model development life cycle that includes things like versioning and scheduling and actually serving up the model,” he said.

> We needed to really invest in making it simpler for some use cases and also giving control over different options. I don’t want to have to worry about how many resources I’m going to take up.
> 
> Then you have the other end of things where I’m a data scientist and I want to be able to… experiment on these models. Once we figure out this model works, we may just need to retrain that model with new data that’s coming in.

## What was your discovery journey as a data scientist, and what pain did it take for you to settle on an infrastructure solution?

**DJ** said he’d had a hard time shifting to a new tool while using a current one. “Funnily enough I actually resisted Flyte in the first place because I didn’t know anything about it,” he recalled. “We were on Airflow and so we had working models, but we had an evangelist on our side go, ‘You gotta use Flyte,’ and then it was much easier.

> The UI was the thing that caught me by surprise because Airflow was just way too complicated. Flyte just put it task-wise which made it a lot easier. Also, \[Flyte is\] very Python agnostic. … It’s almost like a product because you can just send the link for the job and actually point data scientists and software engineers to the data.

## How did you get your ML engineers and data scientists on board?

According to **Varsha**, Woven Planet’s data-collection method changed at the onset of the COVID-19 pandemic. Before COVID-19, engineers or vehicle specialists would drive to get the data for the engineers, but as that became unfeasible, they turned to a data-driven approach: Teams would select a scene, such as a rainy day or an icy road. The prediction or planning team would supply data that could be curated into information the ML model could understand followed by model training; validation (via regression testing); and finally, analysis.

Woven Planet’s ML engineers were impressed that many of these steps were abstracted with Flyte; together with integrations added on top of Flyte, they just needed to write their part of the code, saving them time and effort.

Another Flyte feature that appealed to the ML engineers: versioning based on internal nomenclature and persistent throughout production, which helped with model artifactory.

According to Varsha, **“The more we invest in infrastructure, the more it’s going to help our engineers get self-driving cars out on the roads pretty soon.”**

## What was your experience with building your operational framework, then filling it with models for fast iteration?

Like DJ at Lyft, **Brian** said Stripe’s move from Airflow DAGs to Flyte provoked some anxiety.

At Stripe, the ML team was responsible for the infrastructure, then the data scientists or ML engineers handled product generation and models. One of the keys to their success was “partnering early on (with data scientists and ML engineers); understanding that they want a better way; and being able to train their models and generate data was definitely the first step. The next one was, ‘Which platform are we going to choose?’ “

Flyte beat other candidates on the short list for its feature set and support for quick iteration. Also, “Jupyter notebooks were pivotal to ‘speaking to users where they start.’ ” **Unlike the ad hoc notebook interfaces other platforms required, Flyte let users iterate on notebooks directly by importing the code base for seamless extraction to a Flyte DAG.**

**Fabio** recalled the days before DAGs organized automated pre-processing, training and evaluation — when MLflow projects required manual orchestration. “Once your note shut down, the pod was gone, the job was gone and nobody ever knew that it happened. ... We realized we weren’t able to reproduce what we ran six months ago because we did not log all hyperparameters.”

To address these limitations, Fabio said, Recogni started to chain different tasks together, then realized they needed to automate the process. Their search for an orchestrator was informed by four key requirements:

* Different tasks for different resources
    
* Different tasks for different images
    
* Distributed training with Pytorch or Tensorflow
    
* Spark integration
    

“We went to data scientists and other blogs on Medium and looked at the top 10 orchestration frameworks for MLOps, and we tried to build a prototype with the tools we found,” Fabio said. “We got pretty frustrated. … \[Then\] we discovered Flyte.

“It does exactly these things,” he said. The Recogni team can specify the resources at the task level instead of the workflow level, register at the top level and run different parts of workflows with different images.

> The separation in the workflows and the tasks is where you actually get the benefit when you focus on tasks. You have granularity and you can actually leverage your resources. But we also know that notebooks are not just like a task — they are potentially a lot more.

“Flyte has boosted our productivity greatly,” Fabio added. Engineers can configure the platform according to Recogni’s best practice so teams can use it as “a kind of self-serve thing … without putting an ML engineer or software engineer on every team.”

## What are your thoughts on notebooks versus a really functional, task-driven approach to machine learning?

As a data scientist, **DJ** said, he believes notebooks are a necessary evil: They are not reliable and no final version of code exists. However, human interaction is important, especially since so many are accustomed to using them. DJ also believes that notebooks “work against you in terms of versioning because notebooks change constantly… GitHub only recently added an integration that allows you to inspect them well… (they are not) very opinionated with respect to state and debugging things and keeping track of errors.”

One of the problems DJ describes is pulling stale data attributed to the use of notebooks and exporting outdated pieces of code. However, “If you want to get that model developer-friendly environment, you sort of have to use notebooks.”

## What is one piece of advice you would give MLOps practitioners? Do you have any specific advice to optimize the three Vs: increase velocity, validate early and version consistently?

**Brian** said the Stripe team struggled with measuring velocity. “We take a very naive way right now, which is looking at the number of experiments run, so having an infrastructure that allows you to iterate faster allows you to add more experiments.” The move to Flyte helped Stripe iterate quickly, he said.

**Varsha**’s advice:

> Version everything, and make sure the version is persistent all the way through. Have your own GPU and your own workstation: When you move to production, things are very different.

She described how Flyte enabled Woven Planet to test models locally on GPU-enabled cloud workstations running an environment identical to the one in production.

**Michael** recommended creating a strong test case “so you’re able to say, ‘This is the golden standard for the models that we’re trying to train,’ and then make sure you’re able to maintain the fastest time to create that model with the least amount of pain for a user.”

He said Striveworks initially focused on creating different components, but not as much on the quality of the model itself.

> We have really strong data scientists who can create their own models, and we can serve them, and they understand the platform. But we’re also working on the other part, where you’re not a data scientist but know how models work, and you need to add permutations. You know your data sets and how this model architecture should work, and you need to test your model once it’s done.

**DJ** said the Lyft team initially built even the simplest pipeline based on input from multiple stakeholders before introducing and optimizing metrics.

> I think we made a mistake. It’s easy to create a pipeline that actually isn’t on the path to building the *final* pipeline.” Instead, “The full infrastructure needs to be represented in that first version of the pipeline. … A piece of advice for that first iteration is to really schematic the whole thing. That design period is incredibly important — we had to rewind after a year into the whole thing and redo a big portion of it.

**Fabio** recalled his efforts three years ago to build a company around machine learning services with a team of five ML engineers. Instead of using Docker or compute engines, they tried to make the parts work together with Python and their own container orchestrator.

> My hint would be, ‘Don’t do that.’ There is an industry standard. Learn Kubernetes. There's a great variety and an awesome amount of tools for this platform. Learn that platform, and a lot of sorrow will go away. … Just use great tools that have developed over the years.

# Final Findings

Rolando wraps up with a few great observations:

“From an academic setting, we have this habit of evaluating things in a vacuum. It's kind of like what DJ said: We build this model-training pipeline, and then we try to find an application that wraps it, and that's a mistake.

“I think product validation is what matters.  If you start thinking about product metrics like revenue and key performance indicators, then that's going to drive you naturally towards collaboration with other stakeholders.

“I also think that in machine learning, once you reach a certain threshold of performance, then if you’re going to iterate it makes more sense to iterate on the application or the software — maybe a heuristic or a filter, rather than the machine learning thing per se.

> So before you start with machine learning, build out the application infrastructure and then see where it belongs in the global scheme of things, and then iterate on the model then.

## References

Watch the full panel discussion on YouTube:

* [Rolando Garcia presents "Operationalizing Machine Learning: An Interview Study" Part 1/2](https://www.youtube.com/watch?v=wWjP6835bSY&list=PLmQd1BBY9MWqxS13cQw3y0-PcWbR6L9ct&index=5&t=827s)
    
* ["Operationalizing Machine Learning: An Interview Study" MLOps Panel Discussion, Part 2/2](https://www.youtube.com/watch?v=kKyLfVZVZ2M&list=PLmQd1BBY9MWqxS13cQw3y0-PcWbR6L9ct&index=4&t=502s)
    

Read the full paper:

* Shreya Shankar, Rolando Garcia, Joseph M. Hellerstein, Aditya G. Parameswaran (2022). *Operationalizing Machine Learning: An Interview Study*. The University of California, Berkeley, CA, USA. Retrieved from [https://arxiv.org/pdf/2209.09125.pdf](https://arxiv.org/pdf/2209.09125.pdf)