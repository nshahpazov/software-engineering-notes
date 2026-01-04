
# Prompting and Language Models

## Prompt Engineering

### Tips
- Be specific and clear about what you want. - Avoid vague instructions.
- Provide context if necessary. - Include examples to illustrate your request.
- Use step-by-step instructions for complex tasks. - Experiment with different phrasings to see what works best.
- Specify the desired format of the output. - Use constraints to limit the scope
- Test and iterate on your prompts to improve results.

### Common Prompt Patterns
1. **Instructional Prompts**: "Explain how X works in simple terms."
2. **Comparative Prompts**: "Compare and contrast X and Y."
3. **Creative Prompts**: "Write a short story about X."
4. **Analytical Prompts**: "Analyze the impact of X on Y."
5. **Summarization Prompts**: "Summarize the main points of X
6. **Question-Answering Prompts**: "What are the benefits of X?"
7. **Role-Playing Prompts**: "Act as an expert in X and provide advice on Y."
8. **Data Extraction Prompts**: "Extract key information from the following text: X."
9. **Formatting Prompts**: "Format the following text as a bulleted list: X."

### Agents.md file
Agents are autonomous or semi-autonomous entities that can perform tasks, make decisions, and interact with their environment. In the context of AI and language models, agents can be designed to carry out specific functions, such as answering questions, providing recommendations, or automating workflows.


### Approach when building things

- Write a file with objective, constraints, resources, requirements and evaluation criteria.
- Make the LLM generate a plan of baby steps. Ask him to ask questions if something is unclear. Make things simple and explicit.
- Iterate over the steps, making sure each is done correctly before moving to the next.
- After all steps are done, make the LLM review the entire output against the original requirements.