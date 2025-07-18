# Users

[Users](https://github.com/Monsters-RPG-game/Users) is service for managing users data like account and profile.

## Tech stack

This service is written in node.js. Core components are:

- RabbitMq
- MongoDb

## Tests

This project has 3 types of tests:

- Unit 
- Db 
- E2e 

All tests are written in jest, which is also used to mock methods.

## Startup diagram

```text
    ┌─────────┐    
    │Start app│    
    └────┬────┘    
  ┌──────▽──────┐  
  │Start mongoDb│  
  └──────┬──────┘  
  ┌──────▽─────┐   
  │Start rabbit│   
  └──────┬─────┘   
┌────────▽────────┐
│Wait for messages│
└─────────────────┘
```

<details>
   <summary>Raw diagram</summary>

```
"Start app"
"Start mongoDb"
"Start rabbit"
"Wait for messages"
```

</details>

