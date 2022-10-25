# hackathon2022_covid_tracing

This REPO contains analysis for covid tracing in Indonesia from February to August 2022 using Graph Analytics.

**Please kindly refer to our writing in this [Link](https://drive.google.com/file/d/1FRx6nZ2wraRm7hmW1RZmYA1v5c_KnSHv/view?usp=sharing) for more detail
and explore more in our [Dashboard](http://dashboard.dintegrasi.com/)**

`Spoiler Alert` 

**Neo4j** would be our primary weapon :wink:.

# The Data Itself

The data is from Metadata Peduli Lindungi Apps publicly shared by HACKATHON 2022 Team.

In this case, not all data is used. Only top 15 of the most number check-in in sub-category are used.

For analysis purposes, we added a new column (dummy data) to the original data about Place name, so we could find the exact (more specific) location of people check-in. And for visualization purposes we create random coordinate for each place.

# Data Model

The data model is created in Neo4j Labs: [arrows.app](https://arrows.app/)

<p align="center">
  <img src="https://user-images.githubusercontent.com/98151352/197609684-08ce3afe-20cf-449a-ac64-b815e8c98606.png" />
</p>

In our data model, it shows that we have Person attend an Event or we can say that he/she does an activity with check-in/check-out, covid status and user color status information.
Then as we **Traverse** more hops, we have informations about which outlet and which city he/she does the activity in.

The physical model would look like picture below

<p align="center">
  <img src="https://user-images.githubusercontent.com/98151352/197609511-2e8b64fd-d824-4a9e-9464-b315b265f236.png" />
</p>

# Our Objectives

There are 3 main objectives in our case,

- Finding out the vulnarable point/location of Covid 19 infection.
- Tracing Covid 19 positive cases.
- Analyzing the susceptibility of a person getting infected.

# Here comes the assumptions

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

It connects Event Node to other Event Node of a specific person (Connects Event Nodes of one person).
This relationship means the sequence of a person's activity based on check-in time.

<p align="center">
  <img src="https://user-images.githubusercontent.com/98151352/196360381-9a33bf0a-08dd-4151-af57-19920a8e00c4.png" />
</p>

## Objective: <br> Finding out the vulnarable point/location of Covid 19 infection

Take a look at the people who have black `user_color_status`. With our assumption, we computed the vulnerability score of the places those people went. The score (in perc, %) is calculated by comparison of the total watchlist (black) check-in and total of all check-in per day at particular place.

<p align="center">
  <img src="https://user-images.githubusercontent.com/98151352/197607559-c280e020-0e84-4952-9f2b-aad4781a784f.png" />
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/98151352/197608173-f14602e8-4880-4dd4-be41-b7ef093cbdbc.png" />
</p>


## Objective: <br> Tracking covid19 positive cases

This case comes after testing process (PCR or any test method). We need to find out who people that the patient interacted with in are. 

The poeple interaction network would be like image below,
<p align="center">
  <img src="https://user-images.githubusercontent.com/98151352/196590342-e592320e-bb21-479f-bffc-0b7d2a376552.png" />
</p>

the red Person node is our starting point. We will traverse to its Event node, through PROX_MEET relationship to others Event node then go to another Person node (the yellow ones) which is our target point. The target points represent the people who the patient interacted with in some period of time.

For this case, we didn't have positive tested data. So, at some point, we altered the covid status data of Person to a Positive value.

The scenario is that the person is tested positive, then we look at its interaction network.

Let's say a Person with nik `e7cb7fda6bb6b252a604c65a10a59193d68c26cb5004d1a06be3ea4105337c5b` is tested positive on 18 February 2022.

Image below shows the Event or activity he did when he had positive covid status

<p align="center">
  <img src="https://user-images.githubusercontent.com/98151352/196601916-9ddb9906-026c-47e5-9f1e-d01b9f3b3309.png" />
</p>

We look at its interaction network from 3 days before he's tested to 7 days after he's tested.

<p align="center">
  <img src="https://user-images.githubusercontent.com/98151352/196627974-d8882914-5354-4c72-88c9-32e28809cbe0.png" />
</p>

The yellow box indicates the target point which means people that interact with patient (red node).

## Objective: <br> Analyzing the susceptibility of a person getting infected

Susceptibility score parameters:

- Vulnerability score of places that the person visited
- `user_color_score` of person
- Frequency of person meeting another people with black `user_color_status`

The susceptibility score is attached to each event,

<p align="center">
  <img src="https://user-images.githubusercontent.com/98151352/197688330-14352635-202b-4f90-864e-dce900cbbea5.png" />
</p>

