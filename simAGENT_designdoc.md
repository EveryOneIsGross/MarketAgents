## perSIM module for MarketAgents

# Economic Simulation and perSIM Behavior Overview

## Program Structure

This program is an economic simulation module centered around a single perSIM (simulated person) living in a modest home. 
Uses Python and leverages Pydantic for data validation and serialization. The main components of the program are:

1. **Simulation**: The core class that manages the entire simulation.
2. **Sim**: Represents the simulated person with various needs and attributes.
3. **House**: Represents the Sim's living space.
4. **Market**: Simulates a economic market.
5. **Item**: Represents objects in the house that the perSim can interact with.

## perSIM Behavior and Mechanics

### Needs System

The perSim has eight fundamental needs:

1. Hunger
2. Hygiene
3. Bladder
4. Energy
5. Social
6. Fun
7. Environment
8. Comfort

Each need is represented by a value from 0 (completely unsatisfied) to 10 (fully satisfied). These needs decay over time and drive the perSim's behavior.

### Mood

The Sim's mood is determined by the overall state of their needs:
- Happy: When needs are generally well-satisfied
- Satisfied: When needs are moderately met
- Stressed: When multiple needs are low

### Decision Making

The perSim makes decisions based on its current needs, mood, and environment. The decision-making process involves:

1. Assessing the lowest need
2. Identifying items in the house that can address that need
3. Moving towards and interacting with the chosen item

### Economic Aspects

- The perSim has a limited amount of money
- Items in the house have purchase and operating costs
- The perSim can work to earn money
- There's a market system where prices and quantities fluctuate

### Interactions and Activities

- The perSim can interact with various items in the house (e.g., bed, fridge, shower)
- Each interaction affects one or more needs
- Some activities (like work) can generate income but may decrease certain needs

### Automatic Behaviors

- Sleep: The perSim will automatically seek rest when energy is very low
- Purchasing: The perSim may automatically buy items if a need is critically low and they have sufficient funds

### Environmental Influences

- The layout of the house affects the Sim's movement and access to items
- The presence or absence of certain items impacts the Sim's ability to satisfy specific needs

## Causes and Effects

1. **Need Decay**
   - Cause: Time passing
   - Effect: Gradual decrease in need satisfaction, influencing mood and decisions

2. **Item Interaction**
   - Cause: perSim uses an item (e.g., fridge, bed)
   - Effect: Increase in related need(s), possible decrease in others (e.g., energy)

3. **Work**
   - Cause: perSim chooses to work
   - Effect: Increases money, decreases energy, may affect other needs

4. **Market Fluctuations**
   - Cause: perSim's purchases and time passing
   - Effect: Changes in item prices and availability

5. **Mood Changes**
   - Cause: Overall state of needs
   - Effect: Influences decision-making priorities

6. **Environmental Constraints**
   - Cause: House layout and available items
   - Effect: Limits or enables certain behaviors and need satisfaction

7. **Economic Decisions**
   - Cause: Limited money and varying item costs
   - Effect: Choices between immediate need satisfaction and long-term planning

8. **Automatic Purchases**
   - Cause: Critical need level and sufficient funds
   - Effect: Immediate money decrease, potential for better need management

9. **Sleep Behavior**
   - Cause: Very low energy
   - Effect: Forced rest, rapid energy recovery, possible neglect of other needs

```mermaid
graph TD
    A[Initialize PerSIM and HOUSE] --> B[Update PerSIM Stats]
    B --> E{Choose Activity}
    E --> |Work| F[Produce Goods]
    E --> |Social| G[Enter Chatroom]
    E --> |Use Item| H[Interact with Item]
    E --> |Sleep| I[Rest and Recover]
    F --> J[Update Inventory]
    G --> K[Social Interaction]
    G --> L[Trade Goods]
    H --> M[Satisfy Needs]
    I --> N[Restore Energy]
    J --> O[Update Memory]
    K --> O
    L --> O
    M --> O
    N --> O
    O --> P[Make Decisions]
    P --> B
```

Agent relative to the market.

Agent Memory:
1. personal_trade_history: A list of trades the agent has participated in.
2. commodity_price_history: Historical prices for each commodity the agent has interacted with.
3. past_decisions: A record of decisions made, including context and outcomes.
4. strategy_performance: Metrics on how well different strategies have performed.
5. social_interactions: A record of interactions with other agents.
6. market_beliefs: The agent's beliefs about different commodities in the market.
7. learning_experiences: Specific scenarios where the agent has learned something.
8. utility_history: A record of the agent's utility over time.
9. endowment_history: How the agent's endowment has changed over time.
10. message_history: A record of messages sent and received.

Environment Memory:
1. global_price_history: Historical prices for all commodities in the market.
2. all_trades: A record of all trades that have occurred in the market.
3. market_trends: Overall trends for each commodity.
4. agent_behaviors: Summary of behaviors for each agent in the market.
5. institution_changes: Record of changes to market institutions and their impacts.
6. global_supply_demand: Supply and demand information for each commodity.
7. economic_indicators: Various economic indicators relevant to the market.


```mermaid
classDiagram
    class AgentMemory {
        +personal_trade_history: List[Trade]
        +commodity_price_history: Dict[Commodity, List[float]]
        +past_decisions: List[Decision]
        +strategy_performance: Dict[Strategy, PerformanceMetrics]
        +social_interactions: Dict[Agent, List[Interaction]]
        +market_beliefs: Dict[Commodity, MarketBelief]
        +learning_experiences: List[LearningExperience]
        +utility_history: List[float]
        +endowment_history: List[Dict[Commodity, float]]
        +message_history: List[ACLMessage]
    }

    class Decision {
        +timestamp: datetime
        +action_taken: str
        +context: Dict
        +outcome: str
    }

    class PerformanceMetrics {
        +profit: float
        +utility_gained: float
        +success_rate: float
    }

    class Interaction {
        +timestamp: datetime
        +agent: Agent
        +interaction_type: str
        +outcome: str
    }

    class MarketBelief {
        +expected_price: float
        +price_volatility: float
        +supply_estimate: float
        +demand_estimate: float
        +trend: str
    }

    class LearningExperience {
        +timestamp: datetime
        +scenario: str
        +action_taken: str
        +outcome: str
        +lessons_learned: List[str]
    }

    class EnvironmentMemory {
        +global_price_history: Dict[Commodity, List[float]]
        +all_trades: List[Trade]
        +market_trends: Dict[Commodity, Trend]
        +agent_behaviors: Dict[Agent, BehaviorSummary]
        +institution_changes: List[InstitutionChange]
        +global_supply_demand: Dict[Commodity, SupplyDemand]
        +economic_indicators: Dict[str, float]
    }

    class Trend {
        +direction: str
        +strength: float
        +duration: int
    }

    class BehaviorSummary {
        +trading_frequency: float
        +average_trade_volume: float
        +price_sensitivity: float
        +strategy_changes: int
    }

    class InstitutionChange {
        +timestamp: datetime
        +change_type: str
        +old_value: Any
        +new_value: Any
        +impact_assessment: str
    }

    class SupplyDemand {
        +supply_curve: Function
        +demand_curve: Function
        +equilibrium_price: float
        +equilibrium_quantity: float
    }

    AgentMemory -- Decision
    AgentMemory -- PerformanceMetrics
    AgentMemory -- Interaction
    AgentMemory -- MarketBelief
    AgentMemory -- LearningExperience
    EnvironmentMemory -- Trend
    EnvironmentMemory -- BehaviorSummary
    EnvironmentMemory -- InstitutionChange
    EnvironmentMemory -- SupplyDemand
```
