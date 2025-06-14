# Flowhive MVP
To first get hands-on experience with the domain and technologies i intend to work with in my master, this aims to facilitate a MVP.
Therefore, the goal is to have a concrete use case with some even more concrete acceptance criteria:

## USE CASE 
"A customer places an order. The system must validate it, assign it to the warehouse, and ship it. If the warehouse has issues, a support agent is notified"

## Definitions of Done
- 2 Agents process the events asynchronously, ```GuardAgent``` and ```MusicAgent```
- Each agent maintains track of their own state, and sequences of states, by utilizing the event-driven pattern event-sourcing, which will be stored with either in-memory event logs or simple JSON file
- Agents emit new events as decisions, not side effects
- Broker setup with topics for agents to subscribe to
- A replay tool that allows us to
  - Load all events (inputs + decision)
  - Replay them in the original order
  - Restore agent state deterministically


## What are we leaving out? 
To ensure lightweight and focused, I have decided to postpone/leave out the following elements in the MVP
- Frontend UI -> will be using logs to inspect the system
- Real persistence DB setup -> will start with file-based/in-memory
- Agent orchestration -> too complex for this MVP

## Components
### PlayerAgent (Producer)
Health -> emits MoodChanged. emits to topic mood
ThreatLevel → emits ThreatLevelChanged. emits to topic threat 

### MusicAgent (Subscriber)
Listens to topic: mood
Mood = Calm -> play "peaceful_theme"
Mood = Intense -> play "battle_theme"
Emits: MusicChanged

### GuardAgent (Subscriber)
Listens to topic: threat
Threat < 30 -> emit GreetPlayer
Threat 30-70 -> emit WarnPlayer
Threat > 70 -> emit AttackPlayer

