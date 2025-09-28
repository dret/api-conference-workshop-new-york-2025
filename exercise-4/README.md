# API Conference New York 2025: Exercise 4 (Design and Linting)

This exercise focuses on *API Design* and *API Linting*. There are three main angles you can take, and the choice is up to you:

- Focus more on the *API Design* aspect and think about what kind of API design practices you would want to see. This can be driven by certain design practices that are already in place, but mostly informally, or it could be that currently things are a bit too chaotic and there probably should be some practices in place.

- Focus on the *API Linting* aspect but for an individual *API Design*. This could be something that simply helps people to design better APIs if they want to follow practices. What are things that are allowed by OpenAPI but that in some cases may lead to suboptimal API designs?

- Focus on the *API Linting* aspect but for an entire *API Landscapes*. In this case, the main goal is provide some guardrails so that API designs can be quickly validated in an automated way. What would you put into API guidelines for your API landscape?

For this exercise you can focus more on listing rules you would like to make part of guidelines, or you can try to express some of these as Spectral rules. If you go the latter way, here are two ways to start using Spectral:


## Install Spectral

Spectral is an open source project (developed by Stoplight which is now part of SmartBear), and [installing spectral](https://docs.stoplight.io/docs/spectral/b8391e051b7d8-installation) is relatively straightforward if you have Node.js installed on your computer.

After installing spectral the easiest way to use it is through CLI as shown in the introduction slides. For trying out simple examples, all you need is an OpenAPI document to lint, and a ruleset that you will use to lint it (which as shown can be very minimal as a starting point).


## Use Web-based Spectral

[Scalar](https://scalar.com/) has a [web-based editor](https://editor.scalar.com/), and if you go to the "Diagnostic Rules" tab in the editing window, this is where you can paste in Spectral rules. This way you can easily test Spectral rules without having to install any software on your machine.
