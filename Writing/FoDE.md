# <u>Fundamentals of Data Engineering</u>

## Foreword

-   Part I, C1 - C4: “Foundation and Building Blocks” 
-   Part II,  C5 - C9: “The Data Engineering Lifecycle in Depth” `meat of book`
-   Part III,  C10 - C11: “Security, Privacy, and the Future of Data Engineering” 
-   Appendices A and B: covering serialization and compression, and cloud net‐ working, respectively


## Chapter 1

#### Data Maturity and the Data Engineer

Data maturity refers to the data and is largly irrespective of the maturity of the company. It is useful to understand the stages of Data Management Maturity and **where the data engineer can bring value at each respective stage.**

##### Stage 1: Starting with data

At low data maturity, being  data-driven is more of a motivational aspiration than anything. ML generally not useful. **Here the key thing to focus is small wins that will deliver an immediate, material competitive advantage to key stakeholders across the the company.**

Data Engineer should focus on:
- Getting buy in, from stakeholders that are key to the company, including management, to build the core data architecture that will support a competitive advantage for these stakeholders. 
	- Small wins of delivering bits of value will give you time while you architect the foundations.
	- Small wins also help map what bigger wins may look like in future, however dont get tunnel visioned here as architecture may suffer, see pitfalls below.
- Identify and audit data that will support key initiatives.
- Play the data scientist until one gets hired, however ML is generally **not a win** at this stage, and thus its a **hot air** type of situation. (eg: we sound cool saying "we do ML", but better dont talk about it too much)


Pitfalls:
- Not delivering on those small wins will have people wondering why you're there in the first place. Talking to everyone in the company all the time is how you figure out what these small wins are. Don't be in a situation we're higher ups would rather have an extra software engineer rather than a pure data engineer.
- 
- delivering small wins fast generally comes at the price of solid architecture, **have a plan to (reduce/transition out of) this tech debt you will incur**. 

	<mark class='green'>My personal thoughts on this are that since businesses change so quickly the best initial architecture is one that is decoupled or easy to decouple. Data is there to serve a business objective. A lot of these Data "thought leaders" aren't talking about this. I feel it's more important to architect for a smooth change in context than it is to try to anticipate what that change is going to be and then architect for that. </mark>
#reversable #decoupling 

- Never 're-invent the wheel' at this stage. Use frameworks to do all the heavy lifting, this will help speed, however be mindful of the tech debt point above. 
	- DO create custom solutions however when it will create a competitive advantage. Again, talking to everyone is the key here.

##### Stage 2: Scaling with data

When the data scaling happens hopefully there has been the solid bedrock developed in stage 1 to build sophisticated data architecture. While building it, it will be important to:
- adopt formal data practices
- focus on robustness and scale
- Adopt DevOps and DataOps practices (??? whats this concretely)
- build systems that support ML
- Continue to not 're-invent the wheel', let modern tools do the heavy lifting
	- continue to create custom solutions ONLY if it will create a well understood advantage (key company stakeholders determine what this is). **crucial**

Pitfalls to avoid:
-  now that data has proven its worth it is tempting to over-index on it and adopt bleeding edge technologies. Do not do this. Technology  decisions should be driven by value.
- Don't get bogged down by the volum. Focus on managing team's throughput through simple solutions and easy deployment.
- given success in stage 1, your status will have risen in the organization, **avoid complacency**. Use newfound trust to demand further insight from teams across organization. Teach teams basics of data fluency to get smarter input from them for future work/ data products. Build good I/O between you and the rest of the organization.

##### Stage 3 Leading with data

Keep the ship together. We're not here yet by a long shot nor would we want to be in the next 5 years. Refer back later. 

Potentially lots of legacy tech (eg: Hive) Funny legacy quote:
```
Legacy is condescending engineer-speak for something that actually makes money.
```




---

Then for the rest of the chapter is then a section about the different data roles in a company and how the data engineer relates to them (data architect, software engineer, ML engineer, research engineer, data analyst, data scientist, SRE, DevOps engineers)

And also how DEs relate to product managers, project managers, C-suit, etc. Nothing too interesting. But interesting for career moves down the line. Refer back later.


## Chapter 2

#### The Data Engineering Lifecycles

Here we briefly look at what the author calls the data engineering lifecycle. It is not an 'idée fixe' of how things should look however it divides up areas of work within the pipeline in a somewhat useful way, and we look at the questions that are key to answering in each area.

1. Generation
2. Storage
3. Ingestion
4. Transformation
5. Serving Data

##### Generation: Source Systems

A well designed source system is absolutely fundamental. Errors here (and there will be some) will ripple downstream to everything.

###### Key engineering questions:

>[!Key engineering questions]
>- What are the essential characteristics of the data source? Is it an application, a swarm of IoT device ..?
>- How is data persisted in the source system? Long term, temporary and quickly deleted?
>- At what reate is data generated? How many events per second? How many gigabytes per hour?
>- What level of consistency can data engineers expect from the output data? how often do data inconsistencies occur - nulls where they aren't expected, lousy formattting? etc.
>- How of do erros occur?
>- Will the data contain  duplicates?
>- Will some data values arrive late, possibly much later than other messages produced simultaneously?
>- What is the schema of the ingested data? Will data engineers need to  join across several tables or even several systems to get a complete picture of the data?
>- If there are schema changes (for example, new column) how is this dealt with and communicated to downstream stakeholders?
>- How frequently should data be pulled from the source system?
>- For stateful systems (eg. a databse tracking customer account information) is data provided as periodic snapshots or update events from[ change data capture](https://en.wikipedia.org/wiki/Change_data_capture) (CDC) What's the logic of how changes are performed and how are these tracked in the source database?
>- Who /what is the data provider that will transmit the data  for downstream consumption?
>- **Will  reading from a data source impact its performance?**
>- Does the source system have upstream data dependencies? Understand these fully.
>- Are data-quality checks  in place for missing data and also **late data**

##### Storage

Probably most important area. There will often be several storage solutions implemented.  The way the data is stored impacts how it can be used. There is a notion of *data temperatures* where we can refer to *hot data* as data that is accessed very often (up to several times per second) and all the way to *cold data* that has been archived and put away (S3 Glacier use cases for example)

>[!Key Engineering Questions]
>- Is this storage solution compatible with the architecture’s required write and read speeds?
>- Will storage create a bottleneck for downstream processes?
>- Do you understand how this storage technology works? Are you utilizing the storage system optimally or committing unnatural acts? For instance, are you applying a high rate of random access updates in an object storage system? (This is an antipattern with significant performance overhead.)
>- Will this storage system handle anticipated future scale? You should consider all capacity limits on the storage system: total available storage, read operation rate, write volume, etc.
>- Will downstream users and processes be able to retrieve data in the required [service-level agreement (SLA)](https://databand.ai/blog/what-is-a-data-sla/#:~:text=What%20is%20a%20data%20SLA,that%20the%20commitment%20is%20public.)?
>- Are you capturing metadata about schema evolution, data flows, data lineage, and so forth? Metadata has a significant impact on the utility of data. Meta‐ data represents an investment in the future, dramatically enhancing discoverabil‐ ity and institutional knowledge to streamline future projects and architecture changes.
>-   Is the storage system schema-agnostic (object storage)? Flexible schema (Cassandra)? Enforced schema (a cloud data warehouse)?
>- -How are you tracking master data, golden records data quality, and data lineage for data governance?
>-  How are you handling regulatory compliance and data sovereignty? For example, can you store your data in certain geographical locations but not others?

##### Ingestion

The ingestion systems (along with the source system) will be the main bottleneck of the data engineering lifecycle. This ingestion systems are super critical.

> [!Key Engineering Questions]
> -   What are the use cases for the data I’m ingesting? Can I reuse this data rather than create multiple versions of the same dataset?
>-   Are the systems generating and ingesting this data reliably, and is the data available when I need it?
> -   What is the data destination after ingestion?
>-   How frequently will I need to access the data?
>-   In what volume will the data typically arrive?
>-   What format is the data in? Can my downstream storage and transformation systems handle this format?
>-   Is the source data in good shape for immediate downstream use? If so, for how long, and what may cause it to be unusable?
>-   If the data is from a streaming source, does it need to be transformed before reaching its destination? Would an in-flight transformation be appropriate, where the data is transformed within the stream itself?

There is then the introduction of two concepts: 

###### Batch vs Streaming systems:

If you think about it, almost all data is streaming data at first. So often is seems the choice is: at which stage to we want to break it into batches. Also important to note that batches can be very frequent / small in size (micro batching). Also note that batches can be divided in  *time* (batch what is available every x units of time) and/or in *space* (batch after x amount of units are available)

<mark class='orange'>more on this section, we should come back and make more notes once we're deeper into the section 2 material </mark>
#referback 

###### Push vs Pull systems:

Author says it's a fine line between the two. Clearly CDC is an important concept. 
<mark class='orange'>We should refer back later here because this seems to touch on important concepts, if not being one itself?`</mark>
#referback

##### Transformation

Recall that since almost all data starts off as *streaming*  you can often transform the data *in flight* for  certain aspects of a transformation.

There is no  meaningful *key  engineering questions* here to be honest,  one suggested key question i  thought was useful was asking:
- "Is the transformation  as simple and  self-isolated as possible?"

This is too deep a topic therefore not much is  covered here by the author in this primer. Most of the business value is determined by which/when transformations are applied therefore **Chapter 8  will  be an important chapter.** 

##### Serving Data

top 3 use cases, non exhaustive: analytics, ML, reverse ETL

###### Analytics
Bizness Analytics gud. BOBODDY.
Operational Analytics gud.
Embedded Analytics gud.

###### ML
DEs may be in charge of managing the Spark cluster that the DS uses. High sync between the DE and the DS / ML engineer is crucial on many points including:
- orchestration
- supporting metadata and cataloging systems  (track data history, lineage, and others)
- building the analytics pipeline
- just a lot of stuff, too much to talk about here.

>[!Important considerations]
>-   Is the data of sufficient quality to perform reliable feature engineering? Quality requirements and assessments are developed in close collaboration with teams consuming the data.
>-    Is the data discoverable? Can data scientists and ML engineers easily find valuable data?
>-   Where are the technical and organizational boundaries between data engineering and ML engineering? This organizational question has significant architectural implications.
>-   Does the dataset properly represent ground truth? Is it unfairly biased?

Just recall that generally the first phase of data maturity in a company will in most use cases have analytics as the bread and butter so a solid foundation there is a more pressing consideration.

###### Reverse ETL

I'm really not sure what this is. The Author:

```
The gist is that transformed data will need to be returned to source systems in some manner, ideally with the correct lineage and business process associated with the source system.`

Example: Marketing analysts might calculate bids in Microsoft Excel by using the data in their data warehouse, and then upload these bids to Google Ads. This process was often entirely manual and primitive.
```


#### Major Undercurrents Across The Data Engineering Lifecycles

Security, Data Management, DataOps, Data Architecture, Orchestration, Software Engineering

##### Security

Knowledge of user and identity access management (IAM) roles, policies, groups, network security, password policies, and encryption are good places to start. 
#referback
<mark class='orange'>Refer back to this later.</mark>

##### Data Management

While non-technical, this section actually seems really important. It's about the human component of data where we have a clean process to get everyone involved (**we saw how important this is in early stages of maturity!!!**) 
- Do we have good docs on our datamarts, Eg. If marketing asks questions about customers - how do we define a customer? Do we have tools to generate good docs for us automatically? (ex: DBT `docs` function)
- Way way more stuff .....

#referback
<mark class='orange'>We should definitely refer back here or even write a separate set of notes about this kind of stuff! </mark>

(There is also a bunch of other best practes stuff like [Data Lineage](https://www.kensu.io/blog/a-guide-to-understanding-data-observability-driven-development)  / Data Observability Drive Development that is important to know.)

##### Data Ops

DataOps is a collection of technical practices, workflows, cultural norms, and architec‐ tural patterns that enable:
-   Rapid innovation and experimentation delivering new insights to customers with increasing velocity
-   Extremely high data quality and very low error rates
-   Collaboration across complex arrays of people, technology, and environments
-   Clear measurement, monitoring, and transparency of results

DataOps is can be started on Day 1 or added in little by little. *Observability and Monitering* are good  places to  start to get a window into the performance of a system, then *Automation* and *Incident Response*. 

###### Automation
Using Airflow is a good example of a basic DataOps practice. Automated deployment of dags in Airflow via dag testing, checking for new Python dependencies in new dags,  etc.. are all examples that build on top of this. 

###### Observability and Monitering
Observability, monitoring, logging, alerting, and tracing are all critical to getting ahead of any problems along the data engineering lifecycle. We recommend you incorporate SPC to understand whether events being monitored are out of line and which incidents are worth responding to.
Example SPC for us: 
	- Has a fighter fought more fights this year than our `total amount of events` field, calculated somewhere, would indicate. 

Even once data is months old or years, it may still be used for analysis. Horror stories of *bad data* can still occur when the CEO or analysts are looking at past data. **All data being served needs some form of DataOps process attached to it.**

###### Incident Response

I think the key takeaway from this section is that through observability and monitering the data team is the first to find out when there are issues and can begin to fix them. Ideally downstream consumers of data (eg: analysts in their dashboards) will never find out about data issues. But if they report one and hear that the "data team is already on it" it will prevent a breaking of trust.

>[!Failure]
><mark class='red'>Someone downstream raising a red flag and saying something like "this data doesn't look right" and being right is THE WORST POSSIBLE THING that can happen to a data engineering team. It wreaks havoc on trust that is difficult or impossible to earn back. </mark>
>

Werner Vogels, CTO of Amazon Web Services, is famous for saying, “Everything breaks all the time.” Keep this and the above warning simultaneously in mind.

----

Author admits that:
```
The general practices we describe for DataOps are aspirational, and we suggest companies try to adopt them to the fullest extent possible, given the tools and knowledge available today.
```


##### Data Architecture

See chapter 3

##### Orchestration

"Ya like dags?" - Mickey

We must point out that orchestration is strictly a batch concept. The streaming alternative to orchestrated task DAGs is the streaming DAG. Streaming DAGs remain challenging to build and maintain.
See Chapter 8.

##### Software Engineering

Programming: You better know SQL son.

Core software engineering skills: they are pretty important, as are core software engineering best practices such as code-testing methodologies, such as unit, regression, integration, end-to-end, and smoke. It is important to have a fundamental understanding of how good software is built, the purpose it fulfills, keeping not just low level code but entire codebases / organizations adherent to DRY, KISSES etc .... Don't try to reinvent the wheel unless you work for Ferrari. I will probably keep saying versions of this throughout the whole notes. **This wheel habit is  something I personally struggle with a lot, on all levels of my engineering skills and in life in general.**  


Keeping track of what open source is doing is important. As noted earlier, coding custom solutions has its own special situation that should only be undertaken for clearly identified use cases. 

Streaming: 

```
Streaming data processing is inherently more complicated than batch, and the tools and paradigms are arguably less mature. As streaming data becomes more pervasive in every stage of the data engineering lifecycle, data engineers face interesting soft‐ ware engineering problems.

For instance, data processing tasks such as joins that we take for granted in the batch processing world often become more complicated in real time, requiring more complex software engineering. Engineers must also write code to apply a variety of windowing methods. Windowing allows real-time systems to calculate valuable metrics such as trailing statistics. Engineers have many frameworks to choose from, including various function platforms (OpenFaaS, AWS Lambda, Google Cloud Func‐ tions) for handling individual events or dedicated stream processors (Spark, Beam, Flink, or Pulsar) for analyzing streams to support reporting and real-time actions. 
```

IaC: EMR is good, Databricks is better. Terraform is good. (Does Docker fall in here?)

---

Summary: the final word is that data engineering envelops and is enveloped by other fields, each with their own skillsets and best practices that are important to know.


## Chapter 3

#### Overview

Data Architecture is a subset of Enterprise Architecture.

Enterprise Architecture Definition: 
```
Enterprise architecture is the design of systems to support change in the enterprise, achieved by flexible and reversible decisions reached through careful evaluation of trade-offs.
```

Some architecture decisions are *reversible*, some are not.

Data Architecture definition:
```
Data architecture is the design of systems to support the evolving data needs of an enterprise, achieved by flexible and reversible decisions reached through a careful evaluation of trade-offs.
```



The author emphasizes between *operational architecture* - what needs to be done - and *technical architecture* - how it will be done - as the two components of a data architecture.

#### Principles of Data Architecture

Both AWS and GCP have manifestos on how they think cloud architecture should look like. It's very high level stuff. These principle manifestos seem both important but also kind of pompous. The author goes through some principles he think are important, but in the  context of data. Some of these there's a lot to say, some less.

1. Choose common components wisely.
	- cloud makes this easy, the shared object layer is great (S3)
2. Plan for failure
	- seemed like a good topic but not sure what he was trying to say with his acronyms
3. Architect for scalability
	- things should ideally be able to #scaledown  down as well as up.

<mark class= 'blue'>4. Architecture is leadership.</mark>
```
Returning to the notion of technical leadership, Martin Fowler describes a specific archetype of an ideal software architect, well embodied in his colleague Dave Rice:9

"In many ways, the most important activity of Architectus Oryzus is to mentor the development team, to raise their level so they can take on more complex issues. Improving the development team’s ability gives an architect much greater leverage than being the sole decision-maker and thus running the risk of being an architectural bottleneck."

An ideal data architect manifests similar characteristics. They possess the technical skills of a data engineer but no longer practice data engineering day to day; they mentor current data engineers, make careful technology choices in consultation with their organization, and disseminate expertise through training and leadership. They train engineers in best practices and bring the company’s engineering resources together to pursue common goals in both technology and business.

As a data engineer, you should practice architecture leadership and seek mentorship from architects. Eventually, you may well occupy the architect role yourself.
```

Data Architect seems like one of many cool exits from being a data engineer into management.

<mark class= 'blue'> 5. Always be architecting. </mark>
```
an architect’s job is to develop deep knowledge of the baseline architecture (current state), develop a target architecture, and map out a sequencing plan to determine priorities and the order of architecture changes.

We add that modern architecture should not be command-and-control or waterfall but collaborative and agile. The data architect maintains a target architecture and sequencing plans that change over time. The target architecture becomes a moving target, adjusted in response to business and technology changes internally and world‐ wide. The sequencing plan determines immediate priorities for delivery.
```

<mark class= 'blue'>6. Build loosely coupled systems.</mark> #decoupling #reversable
This is an important concept to keep in mind when architecting at all levels of abstraction.
The Bezos API mandate (2002):

```
1. All teams will henceforth expose their data and functionality through service interfaces.
2. Teams must communicate with each other through these interfaces.
3. There will be no other form of interprocess communication allowed: no direct linking, no direct reads of another team’s data store, no shared-memory model, no back-doors whatsoever. The only communication allowed is via service inter‐ face calls over the network.
4. It doesn’t matter what technology they use. HTTP, Corba, Pubsub, custom proto‐ cols—doesn’t matter.
5. All service interfaces, without exception, must be designed from the ground up to be externalizable. That is to say, the team must plan and design to be able to expose the interface to developers in the outside world. No exceptions.
```

Loosely coupled architecture description:

```
For software architecture, a loosely coupled system has the following properties:

1. Systems are broken into many small components.
2. These systems interface with other services through abstraction layers, such as a messaging bus or an API. These abstraction layers hide and protect internal details of the service, such as a database backend or internal classes and method calls.
3. As a consequence of property 2, internal changes to a system component don’t require changes in other parts. Details of code updates are hidden behind stable APIs. Each piece can evolve and improve separately.
4. As a consequence of property 3, there is no waterfall, global release cycle for the whole system. Instead, each component is updated separately as changes and improvements are made.
```

<mark class= 'blue'>7. Make reversible decisions. </mark>
Loosely coupled systems allow for reversable decisions in many cases. Reversable decisions are good.

8. Prioritize security. 


<mark class= 'blue'>9. Embrace FinOps.</mark>
#finops
Basically ask yourself if certain architectures are less expensive than others, all else being held equal. One point i liked was how security was noted as a part of FinOps.Attackers can create massive costs in a breach (bitcoin mining, ddos, ...)




----

##### More on Decoupling (micro-service vs monolithic architectures)
#decoupling #reversable 

>[!Hint]
 >A lot of the design patterns of software are important, see: Software Architecture: The Hard Parts by Neal Ford. *In the data context it seems that monolithic architectures are unavoidable in many contexts however if you can get a decoupling, remember that a prize of decoupling is reversable decisions!* So its always worth considering.

---

>[!Illustration of Decoupling in the context of Data] 
>Data pipelines might consume data from many sources and store it all in a data warehouse. Data warehouses are inherently monolithic architectures. A decoupling suggestion would be to see if its possible to triage pipelines into contexts and have each context have its on warehouse. 


Also a reference to *Event Driven Architectures*, which is possible in a decoupled architecture.**Important Material here. see chapter 5 for more?**


#### Data Architecture Types


##### The Data Warehouse

This is the classic. Advances have been the introduction of MPP (massive parrallel processing) that has made warehouses faster (ex: AWS Redshift). Often the data sources are ingested into a staging area (which might be an object storage layer or something else)and an ETL pipeline will move it to one big centralized data warehouse, after which the data is served to datamarts. Datamarts are clean versions of the data that are organized for their specific use cases (ex: you might have separate data marts for the marketing team, the data science team, the business team) for clarity, security, regulatory or other reasons.

Data source > Staging area > ETL process > Warehouse > Datamarts

Currently the data warehouse is seeing very important developments to the extent that it may require a new name altogether.

##### The Data Lake

Basically dump all your stuff into an object storage (like S3). The benefits are
- no schema constraints
- virtually limitless and cheap storage
- decoupling of storage and compute allowed for flexibility (remember advantages of decoupling)

Downsides:
- lack of enforced schema means if you are not careful the *lake* can slowly derail  into a *swamp* of unrecognizable data. Results a  silo with 'local experts/owners'. Democratized access to data difficult and we outlined why this is a problem here: [[FoDE#Chapter 1#Stage 1 Starting with data]]
- processing can also be a disadvantage as no easy way to performs SQL joins.
- engineering talent in this domain is harder to find, so can put small companies in difficult positions if their lead engineers leave.

##### The Data LakeHouse

Basically trying to grab the best of ACID compliant, schema on write etc.. + object storage. Section was a bit hand wavy. Generally i think pure data lakes/warehouses are rare nowadays and the LakeHouse concept is the one that makes sense? Idk if i got this part.


##### The Modern Data Stack

I seriously don't understand what he's trying to say.

##### The Lambda Architecture

A streaming architecture that involved sources being sent to a 'stream processing' (kafka is here somewhere) and then to a NoSQL database for fast querying by some analytical process? Not sure.

<mark class='orange'>refer back when we know more</mark>
#referback

##### The Kappa Architecture

The response to the imperfections of the Lambda Architecture. Proposed by Jay Kreps. Didn't really get what he was trying to say.

<mark class='orange'>refer back when we know more</mark>
#referback 

##### The Dataflow Model and Unified Batch and Streaming

Read up on the Google Dataflow Model and Apache Beam. Look at Flink, look at spark streaming. 

<mark class='orange'>refer back when we know more</mark>
#referback 

##### IoT Architectures
#IoT #edge 
Any device capable of collecting data from its environment is an IoT device.
Device examples:
- consumer products: smartwatch, doorbell camera, smart thermostat, smart Bulbs ...
- more advanced hardware:  GPS tracker to record vehicle rotations, a FSD system in a car, AI powered camera to detect faults in an assembly line, a raspberry pi programmed to brew your coffee
- super advanced stuff: NASA stuff, satellites, corporate/govt spyware, military ..

Often IoT swarms interface to a *gateway* which then relays to the internet (ex: your Phillips Hue)

**Devices or gateways can do one or potentially all of these:**
- send back data through the internet
- perform a compute locally (called *edge computing*)
- hold an AI model locally to apply (called *edge AI*)

>[!Important]
>IoT data flow can be affected in ways that traditional data streams (eg: consumer web products) wouldn't be. EG: a car might get caught in a storm, someone might beat your doorbell camera down with a baseball bat. Data Engineers should make architectural decisions around these type of things.

IoT architectures are especially context dependent. Generally knowledge of streaming is important. The author doesn't add much more but encourages further reading and keeping up to date with current events as the field is nascent.

##### The Data Mesh

###### Dehghani Article - [link here](https://martinfowler.com/articles/data-monolith-to-mesh.html)

<mark class='orange'>No clue what this lady is on about. refer back when we know more</mark>
#referback 

###### Second Dehghani Article - [link here](https://martinfowler.com/articles/data-mesh-principles.html)

<mark class='orange'>No clue what this lady is on about. refer back when we know more</mark>
#referback 


###### Dehghani Book

<mark class='orange'>This lady has a full on book on this stuff</mark>
#referback 


Conclusion: refer back, this looks kinda cool. Not sure how real it is though.

##### Other Architectures

The author concludes with:
```
Data architectures have countless other variations, such as data fabric, data hub, #scaledown  architecture, metadata-first architecture, event-driven architecture, live data stack (Chapter 11), and many more.
```


-----

<mark class='green'>
I think deeper understanding of Section II and, more importantly, experiece trying to design systems will be helpful before referring back to this chapter. I also think the 'further readings' section at the end of this chapter would be helpful, but I'd really like to get to Section II</mark>
#referback 



## Chapter 4

The author talks about how to decide which *Tools* to adopt.
Distinction: *Architecture* is the 'what, why, and when' whilst *Tools* are the 'how'.

Reasons for picking the **wrong** architecture:
- shiny object syndrome (me haz snoflaek, wbu?)
- RDD (resume-driven development, I thought this was funny)

Author asserts:
```
In practice, this prioritization of technology often means they cobble together a kind of Dr. Seuss fantasy machine rather than a true data architecture.`
```

I thought that was funny too. We missed a technical dive on the "Dr. Seuss fantasy machine architecture" in chapter 3, hope to see this in future editions.
The rest of the chapter then covers variables that affects which tools to pick.

#### Team Size and Capabilities

Larger Teams can afford specialists with custom solutions that are high maintenace to (ideally) gain an edge or develop new products altogether. Smaller teams (usually teams of 1) cannot do this. See [[#Chapter 1#Stage 1 Starting with data]]

Smaller teams should go with tools/languages that are commonly familiar to the job market and are familiar to the founders.

#### Speed to Market

Great is the enemy of good. Always ask: All else being equal - will adopting this technology help or hinder us to **deliver value early and often**? ([[#Chapter 1#Stage 1 Starting with data]])

#### Interoperability
```

Always be aware of how simple it will be to connect your various technologies across the data engineering lifecycle. As mentioned in other chapters, we suggest designing for modularity and giving yourself the ability to easily swap out technologies as new practices and alternatives become available.
```
#reversable #decoupling 

#### Cost Optimization and Business Value

`what happened to the money Lebowski?`

Basic advice, I liked how he talked about technologies becoming obsolete and vendor lock-in. These are super important constraints and the CTO better know what he's doing.
#finops 

#### Today Versus the Future: Immutable Versus Transitory Technologies

The author links a hilarious video entitled **Silicon Valley - Making the World a Better Place**

Examples
*Immutable*: SQL, bash, Object Storage (for ex: S3, x86,  C)
*Transitory*: js frameworks, Hive

Mass Adoption != Immutable. Ex: Hive allowed analysts and engineers to query massive datasets without coding massive MapReduce jobs, however despite its explosion in adoption it has been quickly  abandoned for better tools that have been built since.
Author gives a rule of thumb that every 2 years a re-assessment should be made of where the data world is going and how that fits into the company vision.

#### Location

Co-lo vs on-prem vs cloud.
Skipping the first 2.

##### Cloud

Search "larry ellison cloud rant" for a comprehensive introduction to the topic.

The author talks about the benefits of cloud, managed services etc.. these seem pretty straightforward. Important to note that using the Cloud enables #serverless architecture which helps to #scaledown. **Some architectures are not possible without the cloud!**

>[!Warning]
>Using the cloud means *adopting cloud practices*. If you use the same architecture in the cloud as you do on-prem or co-lo this is an anti-pattern and will incure large costs! (bad dev experience, high financial costs, fighting with the cloud provider, etc..)


###### Cloud Economics
#finops #architecture

**This is one of my favorite bits in the book so far**

```
To understand how to use cloud services efficiently through cloud native architecture, you need to know how clouds make money. This is an extremely complex concept and one on which cloud providers offer little transparency. Consider this sidebar a starting point for your research, discovery, and process development.
```


```

Cloud Services and Credit Default Swaps

Let’s go on a little tangent about credit default swaps. Don’t worry, this will make sense in a bit. Recall that credit default swaps rose to infamy after the 2007 global financial crisis. A credit default swap was a mechanism for selling different tiers of risk attached to an asset (e.g., a mortgage). It is not our intention to present this idea in any detail, but rather to offer an analogy wherein many cloud services are similar to financial derivatives; cloud providers not only slice hardware assets into small pieces through virtualization, but also sell these pieces with varying technical characteristics and risks attached. While providers are extremely tight-lipped about details of their internal systems, there are massive opportunities for optimization and scaling by understanding cloud pricing and exchanging notes with other users.

Look at the example of archival cloud storage. At the time of this writing, GCP openly admits that its archival class storage runs on the same clusters as standard cloud storage, yet the price per gigabyte per month of archival storage is roughly 1/17 that of standard storage. How is this possible?

Here’s our educated guess. When purchasing cloud storage, each disk in a storage cluster has three assets that cloud providers and consumers use. First, it has a certain storage capacity—say, 10 TB. Second, it supports a certain number of input/output operations (IOPs) per second—say, 100. Third, disks support a certain maximum bandwidth, the maximum read speed for optimally organized files. A magnetic drive might be capable of reading at 200 MB/s.

Any of these limits (IOPs, storage capacity, bandwidth) is a potential bottleneck for a cloud provider. For instance, the cloud provider might have a disk storing 3 TB of data but hitting maximum IOPs. An alternative to leaving the remaining 7 TB empty is to sell the empty space without selling IOPs. Or, more specifically, sell cheap storage space and expensive IOPs to discourage reads.

Much like traders of financial derivatives, cloud vendors also deal in risk. In the case of archival storage, vendors are selling a type of insurance, but one that pays out for the insurer rather than the policy buyer in the event of a catastrophe. While data storage costs per month are extremely cheap, I risk paying a high price if I ever need to retrieve data. But this is a price that I will happily pay in a true emergency.

Similar considerations apply to nearly any cloud service. While on-premises servers are essentially sold as commodity hardware, the cost model in the cloud is more subtle. Rather than just charging for CPU cores, memory, and features, cloud vendors monetize characteristics such as durability, reliability, longevity, and predictability; a variety of compute platforms discount their offerings for workloads that are ephem‐ eral or can be arbitrarily interrupted when capacity is needed elsewhere.

Cloud ≠ On Premises

This heading may seem like a silly tautology, but the belief that cloud services are just like familiar on-premises servers is a widespread cognitive error that plagues cloud migrations and leads to horrifying bills. This demonstrates a broader issue in tech that we refer to as the curse of familiarity. Many new technology products are intentionally designed to look like something familiar to facilitate ease of use and accelerate adoption. But, any new technology product has subtleties and wrinkles that users must learn to identify, accommodate, and optimize.

Moving on-premises servers one by one to VMs in the cloud—known as simple lift and shift—is a perfectly reasonable strategy for the initial phase of cloud migration, especially when a company is facing some kind of financial cliff, such as the need to sign a significant new lease or hardware contract if existing hardware is not shut down. However, companies that leave their cloud assets in this initial state are in for a rude shock. On a direct comparison basis, long-running servers in the cloud are significantly more expensive than their on-premises counterparts.

The key to finding value in the cloud is understanding and optimizing the cloud pricing model. Rather than deploying a set of long-running servers capable of han‐ dling full peak load, use autoscaling to allow workloads to scale down to minimal infrastructure when loads are light and up to massive clusters during peak times. To realize discounts through more ephemeral, less durable workloads, use reserved or spot instances, or use serverless functions in place of servers.

We often think of this optimization as leading to lower costs, but we should also strive to increase business value by exploiting the dynamic nature of the cloud.3 Data engineers can create new value in the cloud by accomplishing things that were impossible in their on-premises environment. For example, it is possible to quickly spin up massive compute clusters to run complex transformations at scales that were unaffordable for on-premises hardware.

Data Gravity

In addition to basic errors such as following on-premises operational practices in the cloud, data engineers need to watch out for other aspects of cloud pricing and incentives that frequently catch users unawares.

Vendors want to lock you into their offerings. Getting data onto the platform is cheap or free on most cloud platforms, but getting data out can be extremely expensive. Be aware of data egress fees and their long-term impacts on your business before getting blindsided by a large bill. Data gravity is real: once data lands in a cloud, the cost to extract it and migrate processes can be very high.
```


[Corey Quin Article](https://www.lastweekinaws.com/blog/the-key-to-unlock-the-aws-billing-puzzle-is-architecture/)

Cloud Billing == Cloud architecture. They're the same thing.
**Example:**
```
Imagine if you will, the three-tieriest of [three-tier architecture](https://en.wikipedia.org/wiki/Multitier_architecture#Three-tier_architecture): a pile of web servers that talk to a pile of application servers that talk to a pile of database servers. I can do all kinds of things to optimize the AWS bill in such an environment by asking the right set of questions and making architectural adjustments:

-   What’s the data transfer look like between AZs and tiers?
-   Are the instances themselves the proper size?
-   Is there an RDS story that improves the overall economics of the database?
-   Do the web servers autoscale?
-   Can any of these tiers benefit from Spot instances?
-   If not, do Savings Plans or Reserved Instances make sense?

All of those questions are fundamental derivatives of the architecture itself.
```

Quin further illuminates:
```
Early on (when the newsletter first started and I was still bright-eyed with undeserved optimism about cloud billing), I naively thought that tracking AWS feature and service enhancements with financial relevance would be simple: look for price reductions and the occasional feature announcement. In practice, that assumption didn’t survive the first round of customer discussions because, as mentioned above, **cost is architecture**. It’s the rare AWS announcement that doesn’t have architecture repercussions.

For example, if you didn’t use SQS when building your application because it couldn’t handle your throughput needs or it was too expensive, [that changed a couple of weeks ago](https://twitter.com/zackkanter/status/1399698492981981187). SQS is to the point where it’s now effectively unlimited throughput at a cost that’s just 3.6% of what it was at launch. The economics have shifted, but it’s not obvious from Amazon’s [somewhat dry enhancement announcement](https://aws.amazon.com/about-aws/whats-new/2021/05/amazon-sqs-now-supports-a-high-throughput-mode-for-fifo-queues/) that there’s any financial impact at all.
```

Potential further reading: 
- Cloud FinOps by J.R. Storment, Mike Fuller

##### Hybrid Cloud

Have only *some* of your infrastructure in the cloud. This has potentially some great benefits. I think most big tech companies work this way. Wish i knew more about this.

<mark class='orange'>refer back when we know more</mark>
#referback


##### Multi - Cloud

Have your infrastructure *distributed among more than 1 cloud provider*. Many services (Databricks, Airflow) are available in many clouds so there is a growing trend towards flexibility in the space.

Benefits:
- Get the best performant tools of each world.
- Can hire certain types of engineers easier (eg: many more aws engineers than other, therefore you can optimize certain pieces of the architecture for that.) Hiring engineers is hard !!!

Disadvantages:
- Data egress costs
- network bottlenecks
- harder to manage.

Author makes note of the **cloud of clouds** trend that is happening and recommends the reader to keep tabs on it. (WUPHF.com for the cloud)

##### Blockchain and the Edge

```
Though not widely used now, it’s worth briefly mentioning a new trend that might become popular over the next decade: decentralized computing. Whereas today’s applications mainly run on premises and in the cloud, the rise of blockchain, Web 3.0, and edge computing may invert this paradigm. For the moment, decentralized platforms have proven extremely popular but have not had a significant impact in the data space; even so, keeping an eye on these platforms is worthwhile as you assess technology decisions.
```
#blockchain #edge





#### Open Source Software

**Good primer on open source, I recommend reading all of it.**

```
Open source software (OSS) is a software distribution model in which software, and the underlying codebase, is made available for general use, typically under specific licensing terms. Often OSS is created and maintained by a distributed team of collaborators. OSS is free to use, change, and distribute most of the time, but with specific caveats. For example, many licenses require that the source code of open source–derived software be included when the software is distributed.


The motivations for creating and maintaining OSS vary. Sometimes OSS is organic, springing from the mind of an individual or a small team that creates a novel solution and chooses to release it into the wild for public use. Other times, a company may make a specific tool or technology available to the public under an OSS license.
```

**OSS has two main flavors: community managed and commercial OSS.**

##### Community-managed OSS


```


OSS projects succeed with a strong community and vibrant user base. Community- managed OSS is a prevalent path for OSS projects. The community opens up high rates of innovations and contributions from developers worldwide with popular OSS projects.

The following are factors to consider with a community-managed OSS project:

Mindshare

Avoid adopting OSS projects that don’t have traction and popularity. Look at the number of GitHub stars, forks, and commit volume and recency. Another thing to pay attention to is community activity on related chat groups and forums. Does the project have a strong sense of community? A strong community creates a virtuous cycle of strong adoption. It also means that you’ll have an easier time getting technical assistance and finding talent qualified to work with the framework.

Maturity

How long has the project been around, how active is it today, and how usable are people finding it in production? A project’s maturity indicates that people find it useful and are willing to incorporate it into their production workflows.

Troubleshooting

How will you have to handle problems if they arise? Are you on your own to troubleshoot issues, or can the community help you solve your problem?

Project management

Look at Git issues and the way they’re addressed. Are they addressed quickly? If so, what’s the process to submit an issue and get it resolved?

Team

Is a company sponsoring the OSS project? Who are the core contributors?

Developer relations and community management

What is the project doing to encourage uptake and adoption? Is there a vibrant chat community (e.g., in Slack) that provides encouragement and support?

Contributing

Does the project encourage and accept pull requests? What are the process and timelines for pull requests to be accepted and included in main codebase?

Roadmap

Is there a project roadmap? If so, is it clear and transparent?

Self-hosting and maintenance

Do you have the resources to host and maintain the OSS solution? If so, what’s the TCO and TOCO versus buying a managed service from the OSS vendor?

Giving back to the community

If you like the project and are actively using it, consider investing in it. You can contribute to the codebase, help fix issues, and give advice in the community forums and chats. If the project allows donations, consider making one. Many OSS projects are essentially community-service projects, and the maintainers often have full-time jobs in addition to helping with the OSS project. Sadly, it’s often a labor of love that doesn’t afford the maintainer a living wage. If you can afford to donate, please do so.

```

##### Commercial OSS (COSS)

Eg. Databricks (Spark), Confluent (Kafka), DBT Labs (dbt)  ......

```

Sometimes OSS has some drawbacks. Namely, you have to host and maintain the solution in your environment. This may be trivial or extremely complicated and cumbersome, depending on the OSS application. Commercial vendors try to solve this management headache by hosting and managing the OSS solution for you, typi‐ cally as a cloud SaaS offering. Examples of such vendors include Databricks (Spark), Confluent (Kafka), DBT Labs (dbt), and there are many, many others.

This model is called commercial OSS (COSS). Typically, a vendor will offer the “core” of the OSS for free while charging for enhancements, curated code distributions, or fully managed services.

A vendor is often affiliated with the community OSS project. As an OSS project becomes more popular, the maintainers may create a separate business for a managed version of the OSS. This typically becomes a cloud SaaS platform built around a managed version of the open source code. This is a widespread trend: an OSS project becomes popular, an affiliated company raises truckloads of venture capital (VC) money to commercialize the OSS project, and the company scales as a fast-moving rocket ship.

At this point, the data engineer has two options. You can continue using the community-managed OSS version, which you need to continue maintaining on your own (updates, server/container maintenance, pull requests for bug fixes, etc.). Or, you can pay the vendor and let it take care of the administrative management of the COSS product.


The following are factors to consider with a commercial OSS project:


Value

Is the vendor offering a better value than if you managed the OSS technology yourself? Some vendors will add many bells and whistles to their managed offerings that aren’t available in the community OSS version. Are these additions compelling to you?


Delivery model

How do you access the service? Is the product available via download, API, or web/mobile UI? Be sure you can easily access the initial version and subsequent releases.


Support

Support cannot be understated, and it’s often opaque to the buyer. What is the support model for the product, and is there an extra cost for support? Frequently, vendors will sell support for an additional fee. Be sure you clearly understand the costs of obtaining support. Also, understand what is covered in support, and what is not covered. Anything that’s not covered by support will be your responsibility to own and manage.


Releases and bug fixes

Is the vendor transparent about the release schedule, improvements, and bug fixes? Are these updates easily available to you?


Sales cycle and pricing

Often a vendor will offer on-demand pricing, especially for a SaaS product, and offer you a discount if you commit to an extended agreement. Be sure to understand the trade-offs of paying as you go versus paying up front. Is it worth paying a lump sum, or is your money better spent elsewhere?


Company finances

Is the company viable? If the company has raised VC funds, you can check their funding on sites like Crunchbase. How much runway does the company have, and will it still be in business in a couple of years?


Logos versus revenue

Is the company focused on growing the number of customers (logos), or is it trying to grow revenue? You may be surprised by the number of companies primarily concerned with growing their customer count, GitHub stars, or Slack channel membership without the revenue to establish sound finances.


Community support

Is the company truly supporting the community version of the OSS project? How much is the company contributing to the community OSS codebase? Controver‐ sies have arisen with certain vendors co-opting OSS projects and subsequently providing little value back to the community. How likely will the product remain viable as a community-supported open source if the company shuts down?

Note also that clouds offer their own managed open source products. If a cloud vendor sees traction with a particular product or project, expect that vendor to offer its version. This can range from simple examples (open source Linux offered on VMs) to extremely complex managed services (fully managed Kafka). The motivation for these offerings is simple: clouds make their money through consumption. More offerings in a cloud ecosystem mean a greater chance of “stickiness” and increased customer spending.

```



#### Monolith vs Modular
#decoupling #reversable #architecture 

##### The Monolith Pattern
Interesting point that **the price of modular is having to add/manage yet another service - orchestration**.

##### The Distributed Monolith Pattern
<mark class='orange'>refer back when we know more</mark>
#referback





> [!RAM]
> 1. Read a section first, then read it againt while taking notes

