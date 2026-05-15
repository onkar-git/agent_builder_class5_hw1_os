# agent_builder_class5_hw1_os
"What I learned" 

1. Pipeline vs agent: 

Pipeline is great when we do the simple tasks i=on single database. However, it can not fetch data from the multiple sources. 
Agents with the help of tools can overcome above problem. It will work in the react loop.  

2. when does each win? Give one concrete example from your own runs where the agent did something the pipeline could not, and one example where the pipeline would have been the safer choice.

Pipleline with domain knowledge + few shots + description: will be good on simgle database
Agent with domain knowledge + few shots + description and tools will have capbility to fectch data from different sources

3. Where did the agent surprise you? Either something it got right that you did not expect, or something it got wrong in an interesting way.

when we asked about: 

What was the total payload for the first successful SpaceX flight?

when we ask about agent to perform aggregation it did not provide correct SQL Query. it completely ignored SpaceA and it did not provide actual ans. I was expecting it to do that.

That means we need to do : Prompt tweaks, tool descriptions, model choice, retry logic, anything.

4. What did you change from the workbook defaults, and why? Prompt tweaks, tool descriptions, model choice, retry logic, anything.
    No I havent made any achnges I rather want to preserve default notbook to have faiulre mode study. We literally dont know when it will fail.

5. What is one thing you would do differently next time?
    Prompt tweaks, improved tool descriptions, model choice.
