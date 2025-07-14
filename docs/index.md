# Welcome to Monsters documentation

This project is open source game API written in node.js. This project is written in SOA microservices architecture.

Current work name of this project is `Monsters` and this might change in the future.

This website will try to explain services mechanics, how they work, how you can start them on your own and how you can contribute.

## What is this 

`Monsters` is open-source backend for games, written in node.js. Project is divided into "microservices", which allows this project to better scale to users requirements. Most users are moving on the map ? Add more resources to Maps. Users spend whole day messaging each other ? Scale [Messages](./services/messages.md) horizontally. This way of writing code allows you, game owner/hobbyist/dev to better handle usage of your servers.

## Services

Each service is unique and manages part of your game. You can learn more about each service, on its sub-page, which you can chose on the left. Lets break them down.

- [Gateway](./services/gateway.md) is main service included in this project. Its job is to transfer user"s requests to each microservice. It also handles traffic between services, caching, authorization and real-time actions.
- [Users](./services/users.md) controls user's profiles and other user related actions.
- [Messages](./services/messages.md) as the name applies, manages messages. There are 2 types of messages, which service controls. Live-chat messages and classic messages.
- [Head](./services/head) is project, which includes configuration files for each of previously mentioned services. With it, you can deploy whole application in minutes!

## Base setup for development

If you would like to work on this application, you will need:

- MongoDB
- RabbitMQ
- Redis

Easiest way to install them is to use docker. You can find scripts to start these in [Head](./services/head.md) service

In addition to these external services, each application needs configuration files. You can either manually set up each service with help of `Readme.md` files in each one, or simply use scripts from [Head](./services/head.md). 

