# Gateway

[Gateway](https://github.com/Monsters-RPG-game/Gateway) is main service included in this project. Its job is to transfer users requests to each microservice. It also handles data caching, authorization and real-time notifications.


## Tech stack

This service is written in node.js. Core components are:

- Express
- RabbitMq
- Redis
- Ws

## Dependencies

Gateway tries to connect to every other service. Besides that, it connects to:

- MongoDB
- RabbitMq
- Redis

## Documentation

This application uses jsdoc for documenting methods in code. Alongside jsdoc, you can access swagger docs on route /docs. Swagger configs are created with package swagger-jsdoc.

## Tests

This project has 4 types of tests:

- Unit 
- Db 
- E2e 
- Integration

All tests are written in jest, which is also used to mock methods. E2e tests are using superTest to send requests on started server. Websocket tests are made using MocSocket. Integration tests are lacking and do require [Head](https://github.com/Monsters-RPG-game/Head) service to call them. Head includes all required scripts, to auto start each integration test.

## Startup diagram

```text
┌──────────────┐                                                                                      
│Run migrations│                                                                                      
└───────┬──────┘                                                                                      
   ┌────▽────┐                                                                                        
   │Start app│                                                                                        
   └────┬────┘                                                                                        
 ┌──────▽──────┐                                                                                      
 │Start mongoDb│                                                                                      
 └──────┬──────┘                                                                                      
  ┌─────▽─────┐                                                                                       
  │Start redis│                                                                                       
  └─────┬─────┘                                                                                       
 ┌──────▽─────┐                                                                                       
 │Start rabbit│                                                                                       
 └──────┬─────┘                                                                                       
 ┌──────▽──────┐                                                                                      
 │Start express│                                                                                      
 └──────┬──────┘                                                                                      
┌───────▽───────┐                                                                                     
│Start websocket│                                                                                     
└───────┬───────┘                                                                                     
  ______▽_______        ┌──────────────┐                                                              
 ╱              ╲       │Validate other│                                                              
╱ Rabbit started ╲______│services      │                                                              
╲                ╱yes   └───────┬──────┘                                                              
 ╲______________╱         ______▽______                                        ┌─────────────────────┐
        │no              ╱             ╲                                       │Set timeout to 5     │
┌───────▽───────┐       ╱ Is other      ╲______________________________________│seconds to revalidate│
│Wait for rabbit│       ╲ service alive ╱yes                                   └──────────┬──────────┘
│to start       │        ╲_____________╱                                               ┌──▽─┐         
└───────────────┘               │no                                                    │Redo│         
                      ┌─────────▽─────────┐                                            └────┘         
                      │Prevent sending    │                                                           
                      │messages to service│                                                           
                      └─────────┬─────────┘                                                           
                       _________▽_________     ┌──────────────────────────────┐                       
                      ╱                   ╲    │Mark service as dead and      │                       
                     ╱ Did service fail to ╲___│prevent sending messages to it│                       
                     ╲ resurrect 10 times  ╱yes└───────────────┬──────────────┘                       
                      ╲___________________╱         ┌──────────▽──────────┐                           
                                │no                 │Wait until service   │                           
                        ┌───────▽──────┐            │gets restarted by k8s│                           
                        │Wait 5 seconds│            └─────────────────────┘                           
                        │and retry     │                                                              
                        └──────────────┘                                                              
```

<details>
   <summary>Raw diagram</summary>

```
"Run migrations"
"Start app"
"Start mongoDb"
"Start redis"
"Start rabbit"
"Start express"
"Start websocket"

if ("Rabbit started") {
  "Validate other services"
  if ("Is other service alive") { 
    "Set timeout to 5 seconds to revalidate"
    "Redo"
  } else {
    "Prevent sending messages to service"
    if ("Did service fail to resurrect 10 times") {
      "Mark service as dead and prevent sending messages to it"
      "Wait until service gets restarted by k8s"
    } else {
      "Wait 5 seconds and retry"
    }
  }
} else {
  "Wait for rabbit to start"
}
```

</details>

