# agent_builder_class5_hw1_os

# What I Learned

## 1. Pipeline vs Agent

Pipelines are useful for simple tasks operating on a single database. However, they cannot easily fetch or combine data from multiple sources.

Agents, with the help of tools, can overcome this limitation. Agents operate in a ReAct-style loop, where they reason, choose tools, observe outputs, and continue iterating until they reach an answer.

---

## 2. When Does Each Win?

### Pipeline

A pipeline with:

- domain knowledge,
- few-shot examples,
- and a good description

works well on a single structured database.

### Agent

An agent with:

- domain knowledge,
- few-shot examples,
- descriptions,
- and external tools

has the capability to fetch and combine data from different sources.

### Example Where the Agent Helped

The agent successfully answered questions requiring multiple tools, such as:

> "What is the vehicle name and height of the rocket which has highest payload?"

It first queried the launch database and then used another tool to fetch vehicle specifications. A simple pipeline would struggle to coordinate across multiple tools dynamically.

### Example Where the Pipeline Would Be Safer

For aggregation-heavy SQL queries, the pipeline would likely be safer because it follows more deterministic logic.

Example:

> "What was the total payload for the first successful SpaceX flight?"

The agent generated an incorrect SQL query and ignored important constraints.

---

## 3. Where Did the Agent Surprise Me?

The agent behaved unexpectedly on the following question:

> "What was the total payload for the first successful SpaceX flight?"

When asked to perform aggregation, the agent failed to generate the correct SQL query.

Issues observed:

- It incorrectly used `SUM(payload_kg)` across all successful launches.
- It ignored the "SpaceX" entity constraint completely.
- It did not return the actual payload for the first successful launch.

I expected the agent to correctly interpret both the aggregation requirement and the SpaceX constraint.

This highlighted the importance of:

- prompt engineering,
- better tool descriptions,
- retry logic,
- and stronger model reasoning.

---

## 4. What Did I Change From the Workbook Defaults?

I did not make major changes to the default notebook configuration.

I intentionally preserved the default setup because I wanted to study the agent’s natural failure modes without adding extra safeguards.

This helped reveal where the system breaks under ambiguity or complex reasoning.

As observed:

> We often do not know when or why an agent will fail.

---

## 5. What Would I Do Differently Next Time?

Next time, I would improve:

- prompt design,
- tool descriptions,
- model selection,
- and retry/error-handling logic.

I would also add better instructions for SQL aggregation and entity filtering to reduce reasoning mistakes.
