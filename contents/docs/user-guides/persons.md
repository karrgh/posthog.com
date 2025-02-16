---
title: Persons
sidebar: Docs
showTitle: true
---

PostHog tracks user behaviour, whether or not the user is logged in and identifiable.

A short video on Persons can be found [here](https://youtu.be/8_SsZW1v56Q);

<BorderWrapper>
    <Quote
        imageSource="/images/customers/andy.jpeg"
        size="md"
        name="Andy Su"
        title="Founder and CEO, Pry"
        quote={`We look into things such as how valuable customers who come to us via ads are compared to those who are organic. We then use that information to make decisions about our advertising strategy.”`}
    />
</BorderWrapper>

## Demo video

<iframe width="560" height="315" src="https://www.youtube.com/embed/GtSSxmOdyk4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Viewing user list

Click on 'Persons' on the left-hand navigation. This will open a submenu containing 'All Users', like so:

![Persons page](../../images/features/people/people-page.png)

<br />

If you have added properties to your users, these will also appear here. 

For instance, if you have set an email, we will display that instead of the randomized Distinct ID:

![Person with email](../../images/features/people/user-email.png)

Clicking the '+' button next to the user will show all of that user's properties. 

## User history

Clicking on an individual person brings up their entire event history, as well as their properties:

![Person page](../../images/features/people/person-page.png)

You can go even deeper by inspecting each event individually. Clicking on the event will bring up all the event properties. 

## Deleting person data

You can also delete data on a person with ease. This can be done if you have created data by yourself for testing purposes or if a user asks you to do so.

When in the person history you can select 'Delete all data on this person'. This will delete all information on that user permanently.
