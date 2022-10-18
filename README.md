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

# Objectives

There are 3 main objectives in our case,

- Finding out the vulnarable point/location of Covid 19 infection.
- Tracking covid 19 positive cases.
- Analysing the susceptibility of a person getting infected.

# Inside `NEO4J`: Analyzing the data ... 
