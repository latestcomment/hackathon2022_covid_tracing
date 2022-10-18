# hackathon2022_covid_tracing

This REPO contains analysis for covid tracing in Indonesia from February to August 2022 using Graph Analytics.

`Spoiler Alert` 

**Neo4j** would be our primary weapon :wink:.

# The Data Itself

The data is from Metadata Peduli Lindungi Apps publicly shared by HACKATHON 2022 Team.

In this case, not all data is used. Only top 15 of the most number check-in in sub-category.

For analysis purposes, we added a new column (dummy data) to the original data about Outlet name, so we could find the exact (more specific) location of people check-in.

# Data Model

The data model is created in Neo4j Labs: [arrows.app](https://arrows.app/)

<p align="center">
  <img src="https://user-images.githubusercontent.com/98151352/196348325-68dffc58-0436-494e-a242-519cacc318a2.png" />
</p>

In our data model, it shows that we have Person attend an Event or we can say that he/she does an activity with check-in/check-out, covid status and user color status information.
Then as we **Traverse** more hops, we have informations about which outlet and which city he/she does the activity in.

The physical model would look like picture below

<p align="center">
  <img src="https://user-images.githubusercontent.com/98151352/196351657-559d44c2-3578-4e7f-a6df-bc92005b111e.png" />
</p>

# Our Objectives

There are 3 main objectives in our case,

- Finding out the vulnarable point/location of Covid 19 infection.
- Tracking covid 19 positive cases.
- Analysing the susceptibility of a person getting infected.

# Here comes the Assumptions

There are several things we needed to assumed,

We assumed

- check-in data meant the person who check-in would be going inside or did activity in those locations.
- 

# Inside `NEO4J`: Analyzing the data ...

After loading the data to Neo4j according to our data model,
we inferred our graph with new relationships, to accoumplish our objectives.

- `:PROX_MEET` Relationship

It connects Event nodes to other Event nodes based on the check-in data at a particular time window.
In our case, we assumed the interval of check-in time of 2 people that would be meeting is under **15 minutes**.
This relationship is representing that a person would be meeting another person.

- `:MOVE` Relationship

It connects Event Node to other Event Node of a specific person (Connect Event Nodes of the one person).
This relationship means the sequence of a person's activity based on check-in time.

<p align="center">
  <img src="https://user-images.githubusercontent.com/98151352/196360381-9a33bf0a-08dd-4151-af57-19920a8e00c4.png" />
</p>

## Objective: <br> Finding out the vulnarable point/location of Covid 19 infection


## Objective: <br> Tracking covid19 positive cases

