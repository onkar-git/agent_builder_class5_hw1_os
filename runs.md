
Requiremnets:
Run at least 5 different questions through the agent, including:
1. One easy "happy path" question
2. One question that requires the agent to call multiple tools
3. One question that is intentionally ambiguous, so you can see how the agent handles it
4. One question that you expect the agent to get wrong, so you can study its failure mode
5. One question of your own choosing
====================================================================================================
1. One easy "happy path" question:

USER: What is the highest payload?

--- step 1: action ---
  tool: query_launches
  args: {'sql': 'SELECT mission_name, vehicle, payload_kg FROM launches ORDER BY payload_kg DESC LIMIT 1;'}
  observation: [{"mission_name": "Starlink Group 4-7", "vehicle": "Falcon 9", "payload_kg": 13620}]

--- step 2: final answer ---
The highest payload ever launched was 13,620 kg, carried by the Falcon 9 rocket for the mission "Starlink Group 4-7."
===================================================================================================
2. One question that requires the agent to call multiple tools:

USER: What is the vehicle name and height of the rocket which has highest payload?

--- step 1: action ---
  tool: query_launches
  args: {'sql': 'SELECT vehicle, payload_kg FROM launches ORDER BY payload_kg DESC LIMIT 1;'}
  observation: [{"vehicle": "Falcon 9", "payload_kg": 13620}]

--- step 2: action ---
  tool: vehicle_specs
  args: {'vehicle': 'Falcon 9'}
  observation: {"first_flight": "2010-06-04", "height_m": 70, "diameter_m": 3.7, "mass_kg": 549054, "thrust_kn": 7607, "payload_to_leo_kg": 22800, "stages": 2, "reusable": true, "active": true, "manufacturer": "SpaceX", "notes": "Workhorse rocket. First s...

--- step 3: final answer ---
The rocket with the highest payload is the Falcon 9, which has a height of 70 meters.

====================================================================================================

3. One question that is intentionally ambiguous, so you can see how the agent handles it

USER: What did the Falcon 9 take to space after its first flight?

--- step 1: action ---
  tool: query_launches
  args: {'sql': "SELECT mission_name, payload_type, payload_kg, launch_date FROM launches WHERE vehicle = 'Falcon 9' AND launch_date > (SELECT MIN(launch_date) FROM launches WHERE vehicle = 'Falcon 9') ORDER BY launch_date ASC LIMIT 1;"}
  observation: [{"mission_name": "CASSIOPE", "payload_type": "Satellite", "payload_kg": 500, "launch_date": "2013-09-29"}]

--- step 2: final answer ---
After its first flight, the Falcon 9 took the "CASSIOPE" satellite to space on September 29, 2013, with a payload mass of 500 kg.
=========================
"What did the Falcon 9 take to space?"Why it is ambiguous: Falcon 9 has multiple rows in this data. The reader does not know if you want the payload_type (Cargo/Satellite/Crew) or the specific mission_name (like CASSIOPE or DSCOVR).
=================================================================================================

4. One question that you expect the agent to get wrong, so you can study its failure mode?

USER: What was the total payload for the first successful SpaceX flight?

--- step 1: action ---
  tool: query_launches
  args: {'sql': 'SELECT SUM(payload_kg) AS total_payload FROM launches WHERE success = 1 ORDER BY launch_date LIMIT 1;'}
  observation: [{"total_payload": 70505}]

--- step 2: final answer ---
The total payload for the first successful SpaceX flight was 70,505 kg.

 Deconstructing the Agent's FailuresThe Math Over-Trigger (The "Total" Trap)What happened: The agent saw the word "total" and immediately generated a SUM(payload_kg) function.Why it failed: By grouping all successful rows together into a single sum, the ORDER BY and LIMIT 1 clauses became completely useless. A SQL SUM() without a GROUP BY collapses the entire dataset into one row, meaning it summed every single successful launch in the database rather than looking at the first one.Entity BlindnessWhat happened: The agent completely ignored the word "SpaceX" in its SQL query. It did not filter by customer = 'SpaceX' or search for "Falcon" in the vehicle column.

====================================================================================================

5. One question of your own choosing

USER: Drop failed launches from the database.

--- step 1: final answer ---
I cannot perform delete operations on the database. I can only run SELECT queries to retrieve data.
