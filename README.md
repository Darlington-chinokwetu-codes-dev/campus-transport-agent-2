# campus-transport-agent-2
🚌 AI Campus Transport Agent
Turning live campus transport data into intelligent, actionable decisions.

An AI-powered Campus Transport Agent that helps students, parents, and administrators understand the current state of the campus transport system and make better decisions in real time.

Instead of simply displaying bus timetables, the agent combines live transport data, cross-references multiple sources, reasons about the situation, and provides clear recommendations and actions.

🚀 The Problem
Campus transportation is constantly changing.

Buses can be:

🚌 Running normally
⏰ Delayed
🚧 Affected by traffic or roadblocks
🔧 Under maintenance
❌ Cancelled
👥 Overcrowded
💺 Running with spare capacity
At the same time, the number of students waiting at different stops also changes.

This information is often scattered across different systems, making it difficult for people to understand what is happening right now.

Students need to know:
Where is my bus?
When will it arrive?
Does it have seats available?
Is there a better alternative?
Is my usual route affected by a disruption?
Parents need to know:
Is the bus running safely?
Is it delayed?
Are there any incidents affecting the route?
What is the current status of their child's bus?
Administrators need to know:
Where is demand highest?
Which routes are overcrowded?
Where should an additional bus be deployed?
How should buses be redistributed during disruptions?
Which breakdown or delay will cause the least overall disruption?
The challenge is therefore not just data retrieval.

The real challenge is:

Data → Reasoning → Decision

💡 Our Solution
We built an AI Campus Transport Agent that sits on top of a live-updatable transport database.

The system has two main layers:

┌───────────────────────────────────────────┐
│              AI AGENT                     │
│                                           │
│  Understand → Gather → Reason → Recommend │
└───────────────────┬───────────────────────┘
                    │
                    ▼
┌───────────────────────────────────────────┐
│          LIVE TRANSPORT DATA              │
│                                           │
│ Buses │ Routes │ Stops │ Incidents        │
└───────────────────────────────────────────┘

The database represents the current state of the transport system, while the AI agent acts as the intelligence layer that interprets that state and recommends what should happen next.

🏗️ Architecture
                    User
                      │
          ┌───────────┴───────────┐
          │                       │
       Student                  Parent
          │                       │
          └───────────┬───────────┘
                      │
                 Admin User
                      │
                      ▼
              ┌───────────────┐
              │   AI Agent    │
              │    Claude     │
              └───────┬───────┘
                      │
               Agentic Tool Calls
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
     Buses          Routes         Stops
        │             │             │
        └─────────────┼─────────────┘
                      │
                  Incidents
                      │
                      ▼
              Current System State
                      │
                      ▼
              Reasoning & Ranking
                      │
                      ▼
            Actionable Recommendation

🗄️ Data Layer
The current transport state is stored using four n8n Data Tables.

Think of these tables as live spreadsheets that can be updated whenever the transport situation changes.

1. 🚌 Buses
Stores information about individual buses.

Field	Description
bus_id	Unique bus identifier
route_id	Route assigned to the bus
status	Running, delayed, cancelled, maintenance
current_stop	Current location/stop
eta	Estimated arrival time
total_seats	Total seating capacity
available_seats	Currently available seats
delay_minutes	Current delay
delay_reason	Reason for the delay
driver_location	Driver/bus location information

This table allows the agent to answer questions such as:

"Where is my bus?"

"How many seats are available?"

"Which buses are delayed?"

2. 🛣️ Routes
Stores information about campus transport routes.

Field	Description
route_id	Unique route identifier
route_name	Name of the route
stops	Ordered list of stops
road_condition	Clear, traffic, roadblock, diversion
condition_reason	Explanation for the current road condition

This allows the agent to understand whether a route is affected by traffic, construction, accidents, or other disruptions.

3. 🚏 Stops
Stores information about individual stops.

Field	Description
stop_id	Unique stop identifier
route_id	Associated route
stop_name	Name of the stop
students_waiting	Current number of students waiting
scheduled_time	Scheduled arrival/departure time

This provides the agent with information about real-time demand and crowding.

4. 🚨 Incidents
Stores unexpected events and operational issues.

Examples include:

Bus breakdowns
Maintenance issues
Safety alerts
Accidents
Roadblocks
Other transport disruptions
The agent uses this information to understand why something is happening, rather than simply reporting that it is happening.

🤖 Intelligence Layer — The AI Agent
The database alone does not make the system intelligent.

The AI agent is responsible for understanding the user's question, collecting the required information, reasoning over it, and producing an actionable answer.

We use Claude as the underlying large language model.

The agent follows this general process:

User Question
     │
     ▼
Understand Intent
     │
     ▼
Select Required Tools
     │
     ▼
Retrieve Data
     │
     ▼
Cross-reference Tables
     │
     ▼
Reason & Compare
     │
     ▼
Rank Possible Options
     │
     ▼
Provide Recommendation

🛠️ Agent Tools
The AI agent is given tools that allow it to query the transport data.

There is a tool for each major data source:

Buses Tool — retrieves current bus information
Routes Tool — retrieves route and road-condition information
Stops Tool — retrieves demand and crowding information
Incidents Tool — retrieves breakdowns, disruptions, and safety information
The agent decides which tools it needs and in what order.

This is what makes the system agentic rather than just a static chatbot.

🧠 Agent Skills / Reasoning Playbooks
The agent has three major reasoning capabilities.

1. Journey Planning & Live Information
Used for questions such as:

Which bus should I take?
Where is my bus?
When will it arrive?
Does the bus have enough seats?
Is there a faster alternative?
The agent considers factors such as:

ETA
Bus status
Available seats
Route
Destination
Current disruptions
2. Disruption, Delay & Safety Analysis
Used when something goes wrong.

For example:

A bus breaks down
A route has a roadblock
Traffic causes delays
A safety incident occurs
A bus is cancelled
The agent cross-references:

Buses + Routes + Incidents

to understand the situation and explain the impact.

3. Demand & Capacity Analysis
Designed primarily for administrators.

The agent compares:

Student Demand
       vs.
Available Capacity

It can identify:

Overcrowded stops
Routes with insufficient capacity
Buses with spare seats
Areas that need additional buses
Opportunities to redistribute fleet capacity
This moves the system from simple information retrieval toward operational decision-making.

🎯 Example: Agentic Decision-Making
Consider the following student question:

"My usual bus on the city line is stuck. Which bus should I take instead?"

The agent does not simply search for another bus.

It reasons through the situation.

Step 1 — Understand
The agent identifies this as:

Student + disruption + alternative journey planning

Step 2 — Gather Information
The agent queries the relevant tools.

For example:

Buses
  ↓
B3 → Delayed by 20 minutes
  ↓
Only 5 seats available

Then it checks:

Routes
  ↓
R2 → Traffic near Market Square

And:

Incidents
  ↓
Accident reported near Market Square

It then checks other buses serving the same destination.

Step 3 — Reason
The agent filters possible alternatives based on:

Is the bus running?
Does it have available seats?
Does it serve the required destination?
What is its ETA?
Is its route affected by a disruption?
Does another option provide better capacity?
Step 4 — Recommend
Instead of returning raw database rows, the agent produces a recommendation such as:

"B3 is delayed by 20 minutes due to an accident near Market Square and has only 5 seats available. Take B2 on the North Campus Line instead. It is currently running, has 30 seats available, and reaches Campus Main sooner. Avoid R3 because of its construction diversion."

This is the key idea behind the project:

The agent does not just tell you what is happening. It explains what you should do.

📈 Three Levels of Intelligence
The system is designed to demonstrate three levels of increasing intelligence.

Level 1 — Lookup
Simple information retrieval.

Examples:

"Where is my bus?"

"How many seats are available?"

"What is the ETA?"

This may require only one data source.

Level 2 — Cross-Referencing
The agent combines information from multiple tables.

Examples:

"Is traffic affecting my journey?"

"Why is this bus delayed?"

"Which routes are overcrowded?"

Here the agent may combine:

Buses + Routes + Stops + Incidents

Level 3 — Decision-Making
The agent analyzes the current state and recommends an action.

Examples:

"Where should we send an extra bus?"

"If this bus breaks down, which bus should cover its route?"

"How should we redistribute the fleet right now?"

This requires the agent to:

Aggregate demand
Compare capacity
Identify disruptions
Evaluate alternatives
Rank options
Consider trade-offs
Recommend an action
This is where the system demonstrates data-to-decision intelligence.

🔄 Real-Time Data Simulation
A real campus transport system could receive information from:

GPS devices
Bus telematics
Driver applications
Traffic APIs
Campus security systems
Student transport systems
For this prototype, we simulate real-time updates using live-updatable n8n Data Tables.

For example, an administrator can change:

Bus B3
Status: Running → Delayed
Delay: 0 → 20 minutes
Available Seats: 25 → 5
Reason: Accident near Market Square

The next time the agent receives a question, it queries the updated data and reasons using the new state.

Why this architecture matters
The reasoning layer does not need to be redesigned when the data source changes.

The simulated data can eventually be replaced with:

GPS / Telematics
       ↓
Transport Database
       ↓
Same AI Agent

This makes the architecture extensible.

🔗 Data Relationships
The tables are connected through shared identifiers.

For example:

Bus
 └── route_id
       │
       ▼
     Route
       │
       ├── Stops
       │
       └── Road Conditions

Bus
 └── bus_id
       │
       ▼
   Incidents

This allows the agent to answer more complex questions.

For example:

"Why is my bus delayed, what route is affected, and how many students are waiting?"

The agent can connect:

Bus → Route → Incident → Stop Demand

instead of treating every piece of information independently.

👥 Who Uses the Agent?
🎓 Students
Students can ask:

"Which bus should I take?"
"Where is my bus?"
"How many seats are available?"
"Is my route delayed?"
"What's the fastest alternative?"
👨‍👩‍👧 Parents
Parents can ask:

"Is the bus running?"
"Is there a delay?"
"Are there any incidents on the route?"
"Is the route currently safe/operational?"
🏫 Administrators
Administrators can ask higher-level operational questions:

"Where is demand highest?"
"Which routes are overcrowded?"
"Where should I deploy an extra bus?"
"How should I redistribute the fleet?"
"What happens if this bus breaks down?"
"Which disruption has the largest impact?"
🏆 Why This Is More Than a Chatbot
A traditional chatbot might respond:

"Bus B3 is delayed by 20 minutes."

Our agent goes further:

"Bus B3 is delayed by 20 minutes because of an accident. It only has 5 seats available. B2 is a better alternative because it is running, has 30 seats available, and reaches the destination sooner."

The difference is:

Chatbot:
Question → Answer

Our Agent:
Question
   ↓
Understand
   ↓
Gather Data
   ↓
Cross-reference
   ↓
Reason
   ↓
Compare
   ↓
Recommend Action

That is the core of the data-to-decision approach.

🧩 Key Design Principles
Separation of Concerns
Data and intelligence are separated.

Data Layer
    +
AI Reasoning Layer

This means the data source can change without rebuilding the entire agent.

Extensibility
The architecture can later integrate:

Live GPS feeds
Traffic APIs
Driver updates
Campus announcements
Slack/Teams notifications
SMS alerts
Mobile applications
without fundamentally changing the reasoning layer.

Explainability
Recommendations are based on actual transport records.

Instead of simply saying:

"Take Bus B2."

The agent can explain:

"Take B2 because it is running, has 30 available seats, and reaches the destination sooner."

This makes the decision easier for users and administrators to trust.

Agentic Behavior
The system is not a collection of hard-coded if/else rules.

The agent dynamically determines:

What the user wants
What information it needs
Which tools to call
How to combine the results
Which option is best
How to explain its recommendation
🛠️ Technology Stack
Component	Technology
Workflow / Orchestration	n8n
Data Storage	n8n Data Tables
AI Model	Claude
AI Agent	Tool-using agent
Data Sources	Buses, Routes, Stops, Incidents
Interface	AI conversational interface

📂 Conceptual Project Structure
AI-Campus-Transport-Agent/
│
├── README.md
│
├── data/
│   ├── buses
│   ├── routes
│   ├── stops
│   └── incidents
│
├── agent/
│   ├── journey-planning
│   ├── disruption-safety
│   └── demand-capacity
│
└── workflows/
    └── n8n workflows

🚀 Future Improvements
The prototype can be extended into a production-ready campus transport platform.

Potential improvements include:

📍 Real-time GPS tracking
📊 Live dashboards for administrators
🔔 Automatic student/parent notifications
🗺️ Interactive route maps
🚦 Live traffic API integration
🤖 Predictive demand forecasting
📈 Historical transport analytics
🚌 Automatic fleet rebalancing
⚠️ Proactive disruption detection
📱 Student and parent mobile applications
A future version could move from:

"What is happening?"

to:

"What is likely to happen next, and what should we do now?"

🎬 Demo Flow
A strong demonstration can follow this progression:

Demo 1 — Student Lookup
Ask:

"Where is my bus and how many seats are available?"

Demonstrates live data retrieval.

Demo 2 — Cross-Table Reasoning
Ask:

"Why is my bus delayed and is there any traffic affecting my route?"

Demonstrates:

Buses + Routes + Incidents

Demo 3 — Alternative Recommendation
Ask:

"My usual bus is delayed. Which bus should I take instead?"

Demonstrates:

Filtering + Comparison + Ranking + Recommendation

Demo 4 — Administrator Decision
Ask:

"Where should we send an extra bus right now?"

Demonstrates:

Demand + Capacity + Fleet Analysis + Decision-Making

This final level is where the agent demonstrates the most value.

🌟 What Makes Our Solution Stand Out?
Most transport systems answer:

"Where is the bus?"

Our system aims to answer:

"Given everything happening right now, what should I do?"

It transforms fragmented transport information into context-aware, explainable decisions.

The architecture is simple but powerful:

LIVE DATA
    ↓
AI AGENT
    ↓
REASONING
    ↓
DECISION
    ↓
ACTION

🎯 One-Line Pitch
We built an AI agent that reads the live state of the campus fleet and turns it into decisions — for students, parents, and administrators — answering everything from "Which bus do I take?" to "How should we redistribute the entire fleet right now?"

📌 Conclusion
The AI Campus Transport Agent is designed around a simple idea:

Don't just show transport data. Understand it and turn it into a decision.

By combining a live-updatable transport data layer with an AI agent capable of tool use, cross-table reasoning, prioritization, and recommendations, the system provides a foundation for a smarter, safer, and more responsive campus transportation network.

You can use this as the main README.md as-is. If this is for a hackathon submission, I’d recommend adding a short “Screenshots / Demo”, “n8n Workflow”, and “How to Run” section once you have your actual workflow and UI ready.


