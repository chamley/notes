# <u>Fundamentals of Data Engineering</u>

## Foreword

-   Part I, C1 - C4: “Foundation and Building Blocks” 
-   Part II,  C5 - C9: “The Data Engineering Lifecycle in Depth” `meat of book`
-   Part III,  C10 - C11: “Security, Privacy, and the Future of Data Engineering” 
-   Appendices A and B: covering serialization and compression, and cloud net‐ working, respectively


# Section I

### Chapter 1 - Data Engineering Described

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
- Don't get bogged down by the volume. Focus on managing team's throughput through simple solutions and easy deployment.
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


### Chapter 2 - The Data Engineering Lifecycle

#### Overview

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

While non-technical, this section actually seems really important. It's about the human component of data where we have *a clean process to get everyone involved* (**we saw how important this is in early stages of maturity!!!**) 
- Do we have good docs on our datamarts, Eg. If marketing asks questions about customers - how do we define a customer? Do we have tools to generate good docs for us automatically? (ex: DBT `docs` function)
- Way way more stuff .....

#referback
<mark class='orange'>We should definitely refer back here or even write a separate set of notes about this kind of stuff! </mark>

(There is also a bunch of other best practes stuff like [Data Lineage](https://www.kensu.io/blog/a-guide-to-understanding-data-observability-driven-development)  / Data Observability Drive Development that is important to know.)

##### Data Ops

DataOps is a collection of technical practices, workflows, cultural norms, and architectural patterns that enable:
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
Orchestration is the price of decoupling. see later notes

See Chapter 8.

##### Software Engineering

Programming: You better know SQL son.

Core software engineering skills: they are pretty important, as are core software engineering best practices such as code-testing methodologies, such as unit, regression, integration, end-to-end, and smoke. It is important to have a fundamental understanding of how good software is built, the purpose it fulfills, keeping not just low level code but entire codebases / organizations adherent to DRY, KISSES etc .... Don't try to reinvent the wheel unless you work for Ferrari. I will probably keep saying versions of this throughout the whole notes. 


Keeping track of what open source is doing is important. As noted earlier, coding custom solutions has its own special situation that should only be undertaken for clearly identified use cases. 

Streaming: 

```
Streaming data processing is inherently more complicated than batch, and the tools and paradigms are arguably less mature. As streaming data becomes more pervasive in every stage of the data engineering lifecycle, data engineers face interesting soft‐ ware engineering problems.

For instance, data processing tasks such as joins that we take for granted in the batch processing world often become more complicated in real time, requiring more complex software engineering. Engineers must also write code to apply a variety of windowing methods. Windowing allows real-time systems to calculate valuable metrics such as trailing statistics. Engineers have many frameworks to choose from, including various function platforms (OpenFaaS, AWS Lambda, Google Cloud Func‐ tions) for handling individual events or dedicated stream processors (Spark, Beam, Flink, or Pulsar) for analyzing streams to support reporting and real-time actions. 
```

IaC: Terraform is good. (Does Docker fall in here?)

---

Summary: the final word is that data engineering envelops and is enveloped by other fields, each with their own skillsets and best practices that are important to know.


### Chapter 3 - Designing Good Data Architecture

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
Data warehouses tend towards monolithic properties, as do data warehouse solutions like dbt. But we've personally seen how nice this is sometimes.

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
I think deeper understanding of Section II and, more importantly, experience trying to design systems will be helpful before referring back to this chapter. I also think the 'further readings' section at the end of this chapter would be helpful, but I'd really like to get to Section II</mark>
#referback 



### Chapter 4 - Choosing Technologies Across the Data Engineering Lifecycle

The author talks about how to decide which *Tools* to adopt.
Distinction: *Architecture* is the 'what, why, and when' whilst *Tools* are the 'how'.

Reasons for picking the **wrong** tools:
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
*Immutable*: SQL, python, bash, Object Storage (for ex: S3, x86,  C)
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


<mark class= 'red'> we're going to just skip the rest of this chapter. I feel that it is sort of a re hashing of what has been said before but knowledge of section 2 will make it perhaps more interesting on a second read? </mark>

<mark class='orange'>refer back when we know more</mark>
#referback





# Section II

### Chapter 5 - Data Generation in Source Systems

#### Source Systems: Main Ideas


A file is a sequence of bytes, typically stored on a disk. Files store everything.

###### ACID
atomicity, consistency, isolation, durability

###### OLTP vs OLAP
- In contrast to an OLTP system, an online analytical processing (OLAP) system is built to run large analytics queries and **is typically inefficient at handling lookups of individual records.**

###### CDC
CDC (Change data capture) is a method for extracting each change event (insert, update, delete) that occurs in a database. CDC is frequently leveraged to replicate between databases in near real time or create an event stream for downstream processing. Relational data‐ bases often generate an event log stored directly on the database server that can be processed to create a stream
#logs 

###### Logs
Logs are a huge source of data. We saw in our programming experience that there are different types of log levels. In data engineernig we can leverage that.  The log level refers to the conditions required to record a log entry, specifically concerning errors and debugging. Software is often configurable to log every event or to log only errors, for example.
Database logs are extremely useful in data engineering, especially for CDC to generate event streams from database changes.
#logs


###### Insert-Only
The insert-only pattern retains history directly in a table containing data. Rather than updating records, new records get inserted with a timestamp indicating when they were created (Table 5-1). For example, suppose you have a table of customer addresses. Following a CRUD pattern, you would simply update the record if the customer changed their address. With the insert-only pattern, a new address record is inserted with the same customer ID. To read the current customer address by customer ID, you would look up the latest record under that ID.

###### Messages Queues vs Streams

Messages are discrete and singular signals in an event-driven system. For example, an IoT device might send a message with the latest temperature reading to a message queue. This message is then ingested by a service that determines whether the furnace should be turned on or off. This service sends a message to a furnace controller that takes the appropriate action. Once the message is received, and the action is taken, the message is removed from the message queue.
**So I guess the difference is that Message Queues are stateful?**

Streams are append only logs.
As events occur, they are accumulated in an ordered sequence, a timestamp or an ID might order events. (Note that events aren’t always delivered in exact order because of the subtleties of distributed systems.)

Systems that deal with this sort of data usually do both. (Deal with a message queue and create a stream out of it, etc..)

###### Types of Time

*Event time* indicates when an event is generated in a source system, including the timestamp of the original *event* itself. An undetermined time lag will occur upon *event* *creation*, before the event is *ingested* and *processed* downstream. **Always include timestamps for each phase through which an event travels**. **Log events as they occur and at each stage of time—when they’re created, ingested, and processed. Use these timestamp logs to accurately track the movement of your data through your data pipelines.**

In short: if you're logging shit all over the place you're not crazy - its a best practice and data friendly.


#### Source System Practical Details

##### Databases

There's a lot of different types of databases. Generally the dichotomy is made over sql vs nosql however there are many more non-relational databases than relational ones. SQL however is just so ubiquitous that it gets its own slot.

Properties of databases:
- Lookups: how are lookups done? What are the implications? (in SQL it's useful to know the basics about the different types of indexes)
- Query Optimization: is there a query optimizer? how does it work? (knowing the basics helps you work *with it* rather than **against it**)
- Scale: does the db scale? horizontal (more database nodes) or vertical (more compute on a single machine) ?
- Modeling patterns: how do you model your data to take advantage of this database? normalization or OBT (one big table)? see chapter 8
- CRUD: how are CRUD operations performed? each database does things a little differently
- Consistency: Is the database fully consistent, or does it support a relaxed consistency model (e.g., eventual consistency)?

###### Relational

SQL is good

###### Non - Relational

We list some main examples below:


---

Key-value stores:
- Examples: AWS DynamoDB

---


Document stores. (like nested key value stores?)
- Examples: MongoDB, AWS DynamoDB and Azure CosmosDB
- indexes possible
- joins not supported
- Generally not-ACID compliant (see specs of individual db)

---


Wide-column Databases:
- Examples: Apache Cassandra, ScyllaDB, Apache HBase, Google BigTable, and Azure Cosmos DB
-  Optimized for storing massive amounts of data with high transaction rates and extremely low latency
- support petabytes of data, millions of requests per second and sub 10ms latency
		- popular in ecommerce, fintech, ad tech, IoT, and real-time personalization applications
-  support rapid scans of massive amounts of data but do not support complex queries
		- only a single index (the row key) for lookups.
- analysis of the specs of the Wide Column Database is required to set up a
		- suitable configuration
		- design the schema
		- choose an appropriate row key to optimize performance
		- avoid common operational issues.
	- Generally you are forced to extract data and send it to a secondary analytics system to run complex queries to deal with these limitations.
			- This can be accomplished by running large scans for the extraction or employing CDC to capture an event stream.

---

Graph databases.
- good fit when you want to analyze the connectivity between elements

Introduced are unique challenges for data engineers who may be more accustomed to dealing with structured relations, documents, or unstructured data. Engineers must choose whether to do the following:
-   Map source system graph data into one of their existing preferred paradigms   
-   Analyze graph data within the source system itself
-   Adopt graph-specific analytics tools

---

Search Databases

*Text search* involves searching a body of text for keywords or phrases, matching on exact, fuzzy, or semantically similar matches.
*Log analysis* is typically used for anomaly detection, real-time monitoring, security analytics, and operational analytics. 

Queries can be optimized and sped up with the use of indexes.

---
Time Series Database

The schema for a time series typically contains a timestamp and a small set of fields. Because the data is time-dependent, the data is ordered by the timestamp. This makes time-series databases suitable for operational analytics but not great for BI use cases. Joins are not common, though some quasi time-series databases such as Apache Druid support joins. Many time-series databases are available, both as open source and paid options.



##### APIs

Standard stuff: REST, GraphQL

###### Webhooks

Webhooks are a simple event-based data-transmission pattern. The data source can be an application backend, a web page, or a mobile app. When specified events happen in the source system, this triggers a call to an HTTP endpoint hosted by the data consumer. Notice that the connection goes from the source system to the data sink, the opposite of typical APIs. For this reason, webhooks are often called reverse APIs.

The endpoint can do various things with the POST event data, potentially triggering a downstream process or storing the data for future use. For analytics purposes, we’re interested in collecting these events. Engineers commonly use message queues to ingest data at high velocity and volume. We will talk about message queues and event streams later in this chapter.

#### Third-Party Data Sources

Direct third-party data access is commonly done via APIs, through data sharing on a cloud platform, or through data download. APIs often provide deep integration capabilities, allowing customers to pull and push data. For example, many CRMs offer APIs that their users can integrate into their systems and applications. We see a common workflow to get data from a CRM, blend the CRM data through the customer scoring model, and then use reverse ETL to send that data back into CRM for salespeople to contact better-qualified leads.

##### Message Queues and Event-Streaming Platforms

Event-driven architectures are pervasive in software applications and are poised to grow their popularity even further. First, message queues and event-streaming plat‐ forms—critical layers in event-driven architectures—are easier to set up and manage in a cloud environment. Second, the rise of data apps—applications that directly integrate real-time analytics—are growing from strength to strength. Event-driven architectures are ideal in this setting because events can both trigger work in the application and feed near real-time analytics.

###### Message Queues

A message queue is a mechanism to asynchronously send data (usually as small individual messages, in the kilobytes) between discrete systems using *a publish and subscribe model*. Data is published to a message queue and is delivered to one or more subscribers. The subscriber acknowledges receipt of the message, removing it from the queue. **Message queues allow applications and systems to be decoupled from each other.**
#decoupling #architecture 


**There are many different types of message queues.** Important properties that define these different types are *frequency of* *delivery*, *message* *ordering* and *scalability.*

*Message Ordering*:
Order matters. Message queues don't necessarily follow the FIFO of a basic queue when dealing with a large distributed system. 
- Is this ambiguity a problem?
- What are your solutions to it, if it matters? 
- One hardcore solution is simply to ask your cloud provider to enforce FIFO (AWS SQS has an option for this), however are you prepared to accept the throttling that this will incur? #bottleneck 

*Delivery frequency*:
Sometimes a single message might be sent twice. Is this going to screw up your system? Idempotency here will be important!
#idempotent

*Scalability*:
Message queues are generally great at scaling up and down horizontally (add more nodes) however *message ordering* characteristics may change due to this. Ask yourself:
- what are the consequences of massive parallel scaling, should it happen?
#scaledown #scaleup



###### Event-Streaming Platforms

The big difference between messages and streams is that a message queue is primarily used to route messages with certain delivery guarantees. In contrast, an event-streaming platform is used to ingest and process data in an ordered log of records.
When evaluating which event-streaming platform to use we look at 3 key characteristics: *topics*, *stream partitions* and *fault tolerance and resilience*. 

*Topics*
A *producer* **streams** *event*s to a *topic*, **a collection of related events.**  A *topic* can have zero, one, or multiple *producer*s. Then, *subscribers* to the topic are sent the *event*.

*Stream Partitions*
The **streams** mentioned above can be parallelized, like a highway, into stream partitions. You can apply a function to an event to assign it a *partition key*. Events with the same *partition key* will end up in the same partition. Improper setting of *partition keys* means that certain partitions will bottleneck. Example: You are streaming the messages from an app and you apply a function to partition messages by state. Your partitions that take care of NY, CA etc.. will be far more loaded than ones for MO, KY.

*Fault Tolerance and Resilience*
Event-streaming platforms are typically distributed systems, with streams stored on various nodes. If a node goes down, another node replaces it, and the stream is still accessible. This means records aren’t lost; you may choose to delete records, but that’s another story. This fault tolerance and resilience make streaming platforms a good choice when you need a system that can reliably produce, store, and ingest event data.

##### Data Contracts

James Denmore (Data Pipelines Pocket Reference Book):
```
A data contract is a written agreement between the owner of a source system and the team ingesting data from that system for use in a data pipeline. The contract should state what data is being extracted, via what method (full, incremental), how often, as well as who (person, team) are the contacts for both the source system and the ingestion. Data contracts should be stored in a well-known and easy-to-find location such as a GitHub repo or internal documentation site. If possible, format data contracts in a standardized form so they can be integrated into the development process or queried programmatically
```


Since a lot of this stuff may happen upstream from you, written by a software engineer (or potenetially a large team of  engineers) sometimes people adopt SLAs (Service Level Agreements) or Data Contracts. The idea being that you ask certain guarantees from them, and you can build your system adapted to the guarantees they are willing to give you. 




### Chapter 6 - Storage


#### Raw Ingredients of Data Storage

In most data architectures, data frequently passes through magnetic storage, SSDs, and memory as it works its way through the various processing phases of a data pipeline. Data storage and query systems generally follow complex recipes involving distributed systems, numerous services, and multiple hardware storage layers. These systems require the right raw ingredients to function correctly.

##### Magnetic Disk Drive (HDD)

Slower than SSD but very important to remember that they still form the backbone of bulk data storage systems because they are significantly cheaper than SSDs per gigabyte of stored data. 

Important  Properties:
-  Disk capacity scales with areal density (gigabits stored per square inch)
-  transfer speed scales with linear density (bits per inch)

With these  two facts we can see that increasing the size/disk capacity will not scale proportionately with the amount of data you can I/O from the disk.
#bottleneck 

Note:
- current data center drives support maximum data transfer speeds of 200–300 MB/s
- This means that a 30TB disk will take maybe 30 hours to be read in its entirety.



Further notes:
```
A second major limitation is seek time. To access data, the drive must physically relocate the read/write heads to the appropriate track on the disk.

Third, in order to find a particular piece of data on the disk, the disk controller must wait for that data to rotate under the read/write heads. This leads to rotational latency. Typical commercial drives spinning at 7,200 revolutions per minute (RPM) seek time, and rotational latency, leads to over four milliseconds of overall average latency (time to access a selected piece of data).

A fourth limitation is input/output operations per second (IOPS), critical for transactional databases. A magnetic drive ranges from 50 to 500 IOPS.



```

>[!HINT] IOPS is the number of operations possible per second, regardless of the size of those operations

###### Cloud Object Storage

As one might guess from the equation above, at some point it becomes more efficient to parralelize HDDs and distribute the data among them. **Cloud Object Storage is mostly parallelized HDDs!**
As a consequence of this solution, a new #bottleneck  arises: network throughput and CPU.



##### Solid State Drive (SSD)

SSDs can scale:
- data-transfer speeds with storage size
- IOPs with storage size. 

They are the accepted standard for commercial deployments of OLTP systems. SSDs allow relational databases such as PostgreSQL, MySQL, and SQL Server to handle thousands of transactions per second.

Due to costs however they are not primarily used for OLAP, nor for object storage. However **some** OLAP databases leverage SSD caching to support high-performance queries on frequently accessed data. As low-latency OLAP becomes more popular, we expect SSD usage in these systems to follow suit.

##### RAM


Ram offers *significantly higher* transfer speeds and faster retrieval times than SSD storage. DDR5 memory—the latest widely used standard for RAM—offers data retrieval latency on the order of 100 ns, roughly 1,000 times faster than SSD. A typical CPU can support 100 GB/s bandwidth to attached memory and millions of IOPS. (Statistics vary dramatically depending on the number of memory channels and other configuration details.)

Several databases treat RAM as a primary storage layer, allowing ultra-fast read and write performance. In these applications, data engineers must always keep in mind the volatility of RAM. Even if data stored in memory is replicated across a cluster, a power outage that brings down several nodes could cause data loss. Architectures intended to durably store data may use battery backups and automatically dump all data to disk in the event of power loss.

##### Networking and CPU


As we began to talk about in the HDD section, parallelization of storage (and duplication/redundancy of data for safety/integrity purposes across more than one drive) creates an architecture where Networking becomes important.

Furthermore this parallelization makes CPU another factor as it has to take care of orchestrating this distribution:

```
CPUs handle the details of servicing requests, aggregating reads, and distributing writes. Storage becomes a web application with an API, backend service components, and load balancing. Network device performance and network topology are key factors in realizing high performance.
```

**See Appendix B for Cloud Networking**


##### Serialization

Data stored in system memory by software is generally not in a format suitable for storage on disk or transmission over a network. *Serialization* is the process of flattening and packing data into a standard format that a reader will be able to decode. Serialization formats provide a standard of data exchange.

Further Study:
- apache parquet (widely used)
- apache hudi (hybrid serialization)
- apache arrow (in-memory serialization)

Low-level database storage is also a form of serialization. Row-oriented relational databases organize data as rows on disk to support speedy lookups and in-place updates. Columnar databases organize data into column files to optimize for highly efficient compression and support fast scans of large data volumes. Each serialization choice comes with a set of trade-offs, and data engineers tune these choices to optimize performance to requirements.

**See Appendix A**

##### Compression

Compression is the process of making data smaller.

Advantages:
- Reduced Storage space (=> reduced storage cost)
- Reduce Scan Time (10:1 compression => 1/10 of time to scan data)
- Reduced Network latency (10:1 compression => 10x network speed when sending data from one place to another)
#bottleneck 

Disadvantages:
- need to compress/decompress, need a plan for how to do this without creating further bottlenecks elsewhere.

**See Appendix A**

*Summary Table*
![[Screen Shot 2022-10-21 at 4.28.11 PM 1.png]]

#### Data Storage Systems 

Storage systems exist at a level of abstraction above raw ingredients. For example, magnetic disks are a raw storage ingredient, while major cloud object storage platforms and HDFS are storage systems that utilize magnetic disks.

##### Single Machine Versus Distributed Storage

Single Machine storage => big EC2 machine
Distributed Storage => cloud data warehouses, spark, S3

##### Eventual Versus Strong Consistency

Strong consistency => ACID

*Eventual Consistency* is what happens when you end up with a large scale distributed system (basically - parallelized across many machines):
- Consistency is not guaranteed, but database reads and writes are made on a best-effort basis, meaning consistent data is available most of the time.
- The state of the transaction is fuzzy, and it’s uncertain whether the transaction is committed or uncommitted.
- At some point, reading data will return consistent values

**Eventual consistency** allows you to retrieve data quickly without verifying that you have the latest version across all nodes.
**Strong consistency** - the distributed database ensures that writes to any node are first distributed with a consensus and that any reads against the database return consistent values. 

You’ll use strong consistency when you can tolerate higher query latency and require correct data every time you read from the database.

In real life the choice of consistency occurs at 3 levels:
1. The choice of database technology (warehouse, hdfs, spark .....)
2. If an eventual consistency storage is picked - it might have a possibility to be configured further on these consistency guarantees
3. Finally, some database technologies support customization at the *query level* for the consistency guarantees




##### File Storage

The notion of a *file* is somewhat subtle.

>[!Definition] A file is a data entity with specific read, write, and reference characteristics used by software and operating systems.

Properties of a file:
- A file is a finite-length stream of bytes
- Bytes can be appended to the file up to the limits of the host storage system
- Any location in the file can be read/updated.

Talks about how your unix file system works, as an example.(It's just a bunch of pointers  and metadata)

Says  this:
```
Even if you must process files on a server with an attached disk, use object storage for intermediate storage between processing steps. Try to reserve manual, low-level file processing for one-time ingestion steps or the exploratory stages of pipeline development.
```


###### Local Disk Storage

This is the file system you know, the one managed by an OS. 

`ext4` is the file system that ships with linux.


Properties:
- Writes data for easy recovery in the case of a power loss, however unwritten data is lost
- full read after write consistency; reading immedi‐ ately after a write will return the written data.
- sometimes there are locking mechanisms placed on files that are being written to #bottleneck 

Some may support advanced features such as:
	- journaling
	- snap‐ shots
	- redundancy
	- the extension of the filesystem across multiple disks
	- full disk encryption
	- compression

###### Network Attached Storage (NAS)

Network-attached storage (NAS) systems provide a file storage system to clients over a network. 
Considerations:
- what consistency model does the NAS system use

###### Cloud filesystem services (Cloud NAS)
Clouds provide **managed NAS** solutions. *Amazon Elastic File System* *(EFS)* is an example of this. It uses the NFS 4 protocol.



##### Block Storage

```
Fundamentally, block storage is the type of raw storage provided by SSDs and mag‐ netic disks. In the cloud, virtualized block storage is the standard for VMs. These block storage abstractions allow fine control of storage size, scalability, and data durability beyond that offered by raw disks.
```

Transactional database systems generally access disks at a block level to lay out data for optimal performance.

Author talks about RAID

```
RAID stands for redundant array of independent disks.
RAID simultaneously controls multiple disks to improve data durability, enhance perfor‐ mance, and combine capacity from multiple drives.
An array can appear to the operating system as a single block device. Many encoding and parity schemes are available, depending on the desired balance between enhanced effective bandwidth and higher fault tolerance (tolerance for many disk failures).
```


###### Storage Area Network (SAN)

Storage area network (SAN) systems provide virtualized block storage devices over a network, typically from a storage pool. SAN abstraction can allow fine-grained storage scaling and enhance performance, availability, and durability.
Summary: this is some on-prem optimization stuf.

###### Cloud Virtualized Block Storage (Cloud SAN)

**This is AWS EBS** (below equivalent for other clouds probably)

- EBS performance metrics are given in IOPS and throughput
- EBS storage is suitable for applications such as databases, where data durability is a high priority.
- EBS replicates all data to at least two separate host machines, protecting data if a disk fails
- EBS volumes are also highly scalable. At the time of this writing, some EBS volume classes can scale up to 64 TiB, 256,000 IOPS, and 4,000 MiB/s.
- Snapshots of EBS Volumes are stored in S3 and snapshots after the initial full backup are *differential* => only changed blocks are written to S3 to minimize storage costs and backup time. (probably uses similar technology as Docker)

###### Local Instance Volumes

This is the file storage on your EC2 machine. It is cheap, it provides very high IOPS, however if the machine is shut down (intentionally or not) the data is gone. There are none of the services running in the background like EBS that back up your data. 


If you are using AWS EMR on top of EC2 machines you would be using the local instance storage. 
#awsemr Author gives an example:

```
Despite these limitations, locally attached disks are extremely useful. In many cases, we use disks as a local cache and hence don’t need all the advanced virtualization features of a service like EBS. For example, suppose we’re running AWS EMR on EC2 instances. We may be running an ephemeral job that consumes data from S3, stores it temporarily in the distributed filesystem running across the instances, processes the data, and writes the results back to S3. The EMR filesystem builds in replication and redundancy and is serving as a cache rather than permanent storage. The EC2 instance store is a perfectly suitable solution in this case and can enhance performance since data can be read and processed locally without flowing over a network
```


![[Screen Shot 2022-10-22 at 4.40.56 PM.png]]


Summary:
- local storage is good if outages are not a problem as it is fast and cheap
	--> you can always plan a backup to S3




##### Object Storage (big section)

###### Overview

Definition: An object store is a key-value store for *immutable* data objects.

In this context an **Object** is a specialized file-like construct. It could be any type of file—TXT, CSV, JSON, images, videos, or audio.
Examples - AWS S3, Azure Blob Storage, Google Cloud Storage (GCS)

In addition, many cloud data warehouses (and a growing number of databases) utilize object storage as their storage layer, and cloud data lakes generally sit on object stores.

Properties:
- immutable
	=> no need to support locks
	=> no need to support synchronization
- redundancy (backed up across several AZ)
- extremely performant parallel stream writes and reads across many disks
	=> managed on behalf of the engineers - you only need to deal with the data stream rather than communicating with the distributed disks 

Caveats:
- Not good for handling large volumes of atomic/small updates => block storage and transactional databases much more suited for this


###### Object stores for data engineering applications

Cloud object storage is decoupled compute and storage, allowing engineers to process data with ephemeral clusters and scale these clusters up and down on demand. **This is a key factor in making big data available to smaller organizations**
#finops #decoupling #scaleup #scaledown 

**From AWS docs:**
You can send [3,500 PUT/COPY/POST/DELETE or 5,500 GET/HEAD requests](https://docs.aws.amazon.com/AmazonS3/latest/dev/optimizing-performance.html) per second per prefix in an Amazon S3 bucket. There are no limits to the number of prefixes that you can have in your bucket.

Object storage can house any binary data with no constraints on type or structure and frequently plays a role in ML pipelines for raw text, images, video, and audio.


###### Object Consistency and Object Versioning

Up until recently S3 was eventually consistent. 

**Now** **S3 is *strongly consistent*** (also called *read after write*) - not just on *read* but on *list*  (*list after write*). 

---
Before, some architectures placed a strongly consistent layer (eg: a PostGres db) on top of the object storage to get strong consistency from their object layer. (eg. when you write an object, write it's key to the database and a timestamp or hash of a timestamp) 
The price, of course, was added latency.

Many companies seemed to have built their own version of this (eg. Netflix with DynamoDB as the consistency layer) and then open sourced it. 

[Deep Dive on S3 Consistency and its history](https://www.allthingsdistributed.com/2021/04/s3-strong-consistency.html)

---

###### Storage classes and tiers

Basically see [[FoDE#Cloud Economics]] for details on this. There's different cost levels of Object Storage that vary with your network use associated with them.

###### Object store–backed filesystems

You can mount an s3 bucket  as local storage to a machine, effectively getting SMB/NFS-like access to it from a machine (say EC2)

Author: 
Mounting object storage as a local filesystem works well for files that are updated infrequently.

example service: AWS amazon s3 file gateway

#referback <mark class='red'>refer back as needed</mark>



###### Conclusion 
<mark class='green'>AWS S3 was a lot more complicated that we thought it was.</mark>


##### Cache and Memory-Based Storage Systems

- remember: power outages of even a second erase data stored in RAM.

- These ultra-fast cache systems are useful when data engineers need to serve data with ultra-fast retrieval latency.

The author gives two examples:
**Memcached:**
```
Memcached is a key-value store designed for caching database query results, API call responses, and more. Memcached uses simple data structures, supporting either string or integer types. Memcached can deliver results with very low latency while also taking the load off backend systems.
```

**Redis**:
```
Like Memcached, Redis is a key-value store, but it supports somewhat more com‐ plex data types (such as lists or sets). Redis also builds in multiple persistence mechanisms, including snapshotting and journaling. With a typical configuration, Redis writes data roughly every two seconds. Redis is thus suitable for extremely high-performance applications but can tolerate a small amount of data loss.
```

##### The Hadoop Distributed File System (HDFS)

Hadoop is similar to object storage but with a key difference: Hadoop combines compute and storage on the same nodes, where object stores typically have limited support for internal processing.


- The filesystem is managed by the NameNode, which maintains directories, file metadata, and a detailed catalog describing the location of file blocks in the cluster.
- The data is broken down into *blocks* of data less than a few hundred MB in size.

- In a typical configuration, each block of data is replicated to three nodes. This increases both the durability and availability of data. If a disk or node fails, the replication factor for some file blocks will fall below 3. The NameNode will instruct other nodes to replicate these file blocks so that they again reach the correct replication factor. Thus, the probability of losing data is very low, barring a *correlated failure* (e.g., an asteroid hitting the data center).

- Hadoop is not simply a storage system. Hadoop combines compute resources with storage nodes to allow in-place data processing

- HDFS is a key ingredient of many current big data engines, such as Amazon EMR. In fact, Apache Spark is still commonly run on HDFS clusters.

##### Streaming Storage

```
Streaming data has different storage requirements than nonstreaming data. In the case of message queues, stored data is temporal and expected to disappear after a certain duration. However, distributed, scalable streaming frameworks like Apache Kafka now allow extremely long-duration streaming data retention. Kafka supports indefinite data retention by pushing old, infrequently accessed messages down to object storage. Kafka competitors (including Amazon Kinesis, Apache Pulsar, and Google Cloud Pub/Sub) also support long data retention.

Closely related to data retention in these systems is the notion of replay. Replay allows a streaming system to return a range of historical stored data. Replay is the standard data-retrieval mechanism for streaming storage systems. Replay can be used to run batch queries over a time range or to reprocess data in a streaming pipeline. Chapter 7 covers replay in more depth.
```

##### Indexes, Partitioning, and Clustering

In most RDBMSs, indexes are used for primary table keys (allowing unique identification of rows) and foreign keys (allowing joins with other tables). Indexes can also be applied to other columns to serve the needs of specific applications.

###### From Rows  to Columns
```
Columnar serialization allows a database to scan only the columns required for a particular query, sometimes dramatically reducing the amount of data read from the disk. In addition, arranging data by column packs similar values next to each other, yielding high-compression ratios with minimal compression overhead. This allows data to be scanned more quickly from disk and over a network.
Columnar databases perform poorly for transactional use cases—i.e., when we try to look up large numbers of individual rows asynchronously.
```


The author mentions that #denormalization improves performance on column oriented RDBMSs.

<mark class='red'>Hopefully he talks more about this in Chapter 8, else</mark> #referback 

###### Partitions and Clustering

On top of indexes, it is also useful to further improve speed by *partitioning* our data. Partitioning occurs on a field, often a timestamp.

Inside of *partitions* there  are further techniques to improve speed, one it *clustering*.

Clusters allow finer-grained organization of data within partitions. A clustering scheme applied within a columnar database sorts data by one or a few fields, colocat‐ ing similar values. This improves performance for filtering, sorting, and joining these values.


The author talks about how Snowflake has an algorithm that actively analyzes data to  see  which  rows can be clustered together on one or more columns. Snowflake stores in a metadatabase information about each partition, including the number of rows and the range in values for the column(s) it has partitioned on. At each query stage, Snowflake analyzes micro-partitions to determine which ones need to be scanned


#### Data Engineering Storage Abstractions


>[!Abstract] The storage abstraction you require as a data engineer boils down to a few key considerations:
>1. Purpose and use case: You must first identify the purpose of storing the data. What is it used for?
>2. Update patterns: Is the abstraction optimized for bulk updates, streaming inserts, or upserts?
>3. Cost: What are the direct and indirect financial costs? The time to value? The opportu‐ nity costs?
>4. Separate storage and compute. #decoupling 

##### The Data Warehouse

Ex: Redshift, BigQuery

**In practice, cloud data warehouses are often used to organize data into a data lake, a storage area for massive amounts of unprocessed raw data. Cloud data warehouses can handle massive amounts of raw text and complex JSON documents.**
#architecture 

The limitation is that cloud data warehouses cannot handle truly unstructured data, such as images, video, or audio, unlike a true data lake. 


##### The Data Lake

Pure data lakes are rarely used anymore as the MPP RDBMSs offered very useful things: schema management; update, merge and delete capabilities. This is what led to the Data Lakehouse

##### The Data LakeHouse

Ex: Databricks DeltaLake or [this in AWS](https://aws.amazon.com/blogs/big-data/build-a-lake-house-architecture-on-aws/)

```
The data lakehouse is an architecture that combines aspects of the data warehouse and the data lake. As it is generally conceived, the lakehouse stores data in object storage just like a lake. However, the lakehouse adds to this arrangement features designed to streamline data management and create an engineering experience simi‐ lar to a data warehouse. This means robust table and schema support and features for managing incremental updates and deletes. Lakehouses typically also support table history and rollback; this is accomplished by retaining old versions of files and metadata.

A lakehouse system is a metadata and file-management layer deployed with data management and transformation tools.
```


The architecture of the data lakehouse is similar to the architecture used by various commercial *Data Platforms*, including BigQuery and Snowflake. These systems store data in object storage and provide automated metadata management, table history, and update/delete capabilities. The complexities of managing underlying files and storage are fully hidden from the user.

The key advantage of the data lakehouse over proprietary tools is interoperability. It’s much easier to exchange data between tools when stored in an open file format. Reserializing data from a proprietary database format incurs overhead in processing, time, and cost. In a data lakehouse architecture, various tools can connect to the metadata layer and read data directly from object storage.
#decoupling 


##### Data Platforms

Ex: BigQuery (??) , Snowflake

```
Increasingly, vendors are styling their products as data platforms. These vendors have created their ecosystems of interoperable tools with tight integration into the core data storage layer. In evaluating platforms, engineers must ensure that the tools offered meet their needs. Tools not directly provided in the platform can still interoperate, with extra data overhead for data interchange. Platforms also emphasize close integration with object storage for unstructured use cases, as mentioned in our discussion of cloud data warehouses.
```


**At this point, the notion of the data platform frankly has yet to be fully fleshed out. However, the race is on to create a walled garden of data tools, both simplifying the work of data engineering and generating significant vendor lock-in.**
#decoupling #wheelIsInvented 
	


##### Stream-to-Batch Storage Architecture

Data flowing through a topic in the streaming storage system is written out to multiple consumers.

Example of consumers would be:
- AWS Kinesis firehouse which can generate S3 objects based on configurable triggers (e.g., time and batch size)
- BigQuery can ingest streaming data into a streaming buffer. This streaming buffer is automatically reserialized into columnar object storage. 
	 **Note:** The query engine supports seamless querying of both the streaming buffer and the object data to provide users a current, nearly real-time view of the table.


####  Big Ideas and Trends in Storage


##### Data Catalog
A data catalog is a centralized metadata store for all data across an organization. Data catalogs have both organizational and technical use cases.

Data catalogs make metadata easily available to systems. For instance, a data catalog is a key ingredient of the data lakehouse, allowing table discoverability for queries.

Organizationally, data catalogs allow business users, analysts, data scientists, and engineers to search for data to answer questions.

##### Schema

Two major schema patterns exist: *schema on write* and *schema on read.*

*Schema on write* is essentially the traditional data warehouse pattern: a table has an integrated schema; any writes to the table must conform. **To support schema on write, a data lake must integrate a schema metastore.**

*Schema on read*  means we write whatever and determine it when we re-access the data on a *read*. 
**Ideally, schema on read is implemented using file formats that implement built-in schema information, such as Parquet or JSON. CSV files are notorious for schema inconsistency and are not recommended in this setting.**

##### Separation of Compute from Storage

This section was very complicated and long.

#decoupling <mark class='red'> referback when we have more time</mark>


##### Data Storage Lifecycle and Data Retention


Hot/Warm/Cold data.



#### Conclusion

Storage is everywhere and underlays many stages of the data engineering lifecycle. In this chapter, you learned about the raw ingredients, types, abstractions, and big ideas around storage systems. Gain deep knowledge of the inner workings and limitations of the storage systems you’ll use. Know the types of data, activities, and workloads appropriate for your storage.



### Chapter 7 - Ingestion

#### Overview

*Data ingestion* is the process of moving data from one place to another. 


>[!Abstract] Key questions to ask:
>-   What’s the use case for the data I’m ingesting?
>-   Can I reuse this data and avoid ingesting multiple versions of the same dataset?
>-   Where is the data going? What’s the destination?
>-   How often should the data be updated from the source?
>-   What is the expected data volume?
>- What format is the data in? Can downstream storage and transformation accept this format?
>- Is the source data in good shape for immediate downstream use? That is, is the data of good quality? What post-processing is required to serve it? What are data-quality risks (e.g., could bot traffic to a website contaminate the data)?
>- Does the data require in-flight processing for downstream ingestion if the data is from a streaming source?


#### Core Concepts

>[!ABSTRACT] Core concepts that influence architectural decisions:
>-   Bounded versus unbounded
>-   Frequency
>-   Synchronous versus asynchronous 
>-   Serialization and deserialization   
>-   Throughput and scalability
>-   Reliability and durability
>-   Payload
>-   Push versus pull versus poll patterns
>-   Frequency
>-   Synchronous versus asynchronous
>-   Serialization and deserialization
>-   Throughput and scalability
>-   Reliability and durability
>-   Payload
>-   Push versus pull versus poll patterns


##### Bounded Versus Unbounded Data

'I don't believe in continuous distributions, they're simply convenient approximations to discrete ones' - Dan Weiner


The author wishes to make a distinction here. Earlier he talked about how all data starts off as streaming data. Here he talks about how all data is unbounded until it is bounded. He illustrates this by talking about how time is free flowing however we might timestamp it thus creating groups and bounderies. 

Anyway, assuming he is correct he makes a remark:
**Streaming ingestion systems are simply a tool for preserving the unbounded nature of data so that subsequent steps in the lifecycle can also process it continuously.**

##### Frequency

Examples in descending order of frequency:
1. *real-time/streaming* - system might continuously ingest events from IoT sensors and process these within seconds.
2. *Micro-batch* - a CDC system could retrieve new log updates from a source database once a minute.
3. *Batch*  - a business might ship its tax data to an accounting firm once a year.


Important to note: once data reaches a batch process, the batch frequency becomes a bottleneck for all downstream processing.


Author also adds:

```
Streaming systems are the best fit for many data source types. In IoT applications, the typical pattern is for each sensor to write events or measurements to streaming systems as they happen. While this data can be written directly into a database, a streaming ingestion platform such as Amazon Kinesis or Apache Kafka is a better fit for the application. Software applications can adopt similar patterns by writing events to a message queue as they happen rather than waiting for an extraction process to pull events and state information from a backend database. This pattern works exceptionally well for event-driven architectures already exchanging messages through queues. And again, streaming architectures generally coexist with batch processing.
```

##### Synchronous Versus Asynchronous Ingestion

I'm not sure what he was trying to say about synchronous ingestion, he made it seem like it was a bad idea which obviously i dont think it is. idk

On async ingestion:

```
With asynchronous ingestion, dependencies can now operate at the level of individ‐ ual events, much as they would in a software backend built from microservices (Figure 7-6). Individual events become available in storage as soon as they are inges‐ ted individually. Take the example of a web application on AWS that emits events into Amazon Kinesis Data Streams (here acting as a buffer). The stream is read by Apache Beam, which parses and enriches events, and then forwards them to a second Kinesis stream; Kinesis Data Firehose rolls up events and writes objects to Amazon S3.
```

![[Screen Shot 2022-10-24 at 5.49.34 PM.png]]

```
The big idea is that rather than relying on asynchronous processing, where a batch process runs for each stage as the input batch closes and certain time conditions are met.

Each stage of the asynchronous pipeline can process data items as they become available in parallel across the Beam cluster. The processing rate depends on available resources. 

The Kinesis Data Stream acts as the shock absorber, moderating the load so that event rate spikes will not overwhelm downstream processing. 

Events will move through the pipeline quickly when the event rate is low, and any backlog has cleared. 


Note that we could modify the scenario and use a Kinesis Data Stream for storage, eventually extracting events to S3 before they expire out of the stream.
```



##### Serialization and Deserialization

Moving data from source to destination involves serialization and deserialization. As a reminder, serialization means encoding the data from a source and preparing data structures for transmission and intermediate storage stages.

When ingesting data, ensure that your destination can deserialize the data it receives. We’ve seen data ingested from a source but then sitting inert and unusable in the destination because the data cannot be properly deserialized. See the more extensive discussion of serialization in Appendix A.


##### Throughput and Scalability





>[!ram]
>aws athena + s3 = decoupled -- redshift = coupled -- where can we fit this info?
#decoupling #architecture 


> [!RAM]
> 1. Read a section first, then read it againt while taking notes


