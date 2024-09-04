---
layout: project
title: "SCNGame"
brief: "Game project"
year: 2024
tags:
    - libgdx
    - gamedev
    - java
---

## Bleep Bloop

Probably like every developer in existence, I've decided to start a game dev project! 

However, there's a bit of a catch. I'm not really making this game for the sake of making a game, more so the other things that come along with it. Not only that, I'm not using a game engine. I'm using [LibGDX](https://libgdx.com/) as a framework, as well as bolting on a lot of my own stuff when needed.

There's probably a lot wrong, but this is my first real excursion into software (if you could consider it that?) architecture. Going through this project (in progress) has been teaching me MANY things:

- software architecture
- graphics programming (ended up dipping my toes into GLSL)
- the pains of text rendering
- automating builds with GitHub actions
- making gradle builds

## Downloads

Download the latest pre-release [here](https://github.com/silas-hw/SCNGame/releases/download/v0.0.3/desktop-v0.0.3.jar). Make sure to save it to its own folder, because it will create a 'saves' folder when you run it to store save files.

Or, if you're feeling extra daring, you can download bleeding edge builds from these [GitHub action workflows](https://github.com/silas-hw/SCNGame/actions/workflows/canary-build.yml). You'll have to scroll down to the 'artifacts' on a workflow run and donwload the zip file, which contains the built jar.

Oh yeah, there's currently no properly packaged releases. You'll have to install Java 17+ in order to get it running... but it's installed on over 3 billion machines right? So you probably already have it.

## Updates?

This is just the project page, showcasing the project off as a whole. I will probably end up posting separate dev-blogs ranting about specific things I'm working on at some point.
