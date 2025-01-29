# Natural-Language-Interaction
create an AI agent capable of interacting with users in natural language (English) to effectively gather requirements. We want this developed using Langgraph. It should be conversational and this agent bot should essentially gather requirements in the form of answers to a set of questions. It is likely that users might not have answers to all the questions.

Want to develop a multi-agent system where this is the first agent. This agent should store the extracted answer as well

The ideal candidate will have experience in natural language processing and AI development, with a focus on user-friendly and efficient interactions. Your role will involve designing the conversation flow, implementing Agentic AI , and ensuring a seamless user experience.
-----
To create an AI agent that interacts with users in natural language (English) and gathers requirements in a conversational way, you can use Langgraph. Langgraph allows the design of structured dialogue flows for AI agents, while leveraging Natural Language Processing (NLP) to create conversational interactions.

In this case, the agent will gather requirements in the form of answers to a set of predefined questions. Since users may not always have answers to all questions, the agent should handle such cases gracefully (e.g., by asking follow-up questions or skipping questions).

We will build a multi-agent system, with this agent serving as the first part of the conversation, responsible for collecting answers.

Here’s a breakdown of how to implement this AI agent with Langgraph:
Key Steps:

    Set up Langgraph.
    Design the conversation flow.
    Store and extract user answers.
    Handle missing answers and continue the conversation.

Step-by-Step Code Implementation
1. Install Langgraph and Dependencies

Before starting the development, you need to install Langgraph.

pip install langgraph

Langgraph will facilitate the design of the conversational agent by creating structured dialogues with natural language processing (NLP) capabilities.
2. Define the Conversation Flow

Let’s define the conversational flow where the agent will ask a series of questions to gather user requirements. We will define the questions and handle cases where the user doesn't have an answer.

from langgraph import Agent, Memory, Intent, Statement, Action
import random

# Define a class for the conversation agent
class RequirementGatheringAgent(Agent):
    def __init__(self):
        # Initialize the agent with its memory
        self.memory = Memory()
        self.create_conversation()

    def create_conversation(self):
        # Define the questions the agent will ask
        self.questions = [
            "What is your project about?",
            "What is the target audience for this project?",
            "What are the main goals of this project?",
            "Do you have any specific requirements for the design or features?",
            "When would you like the project to be completed?"
        ]
        
        # Define the actions when each question is asked
        self.intents = []
        for i, question in enumerate(self.questions):
            intent = Intent(f"question_{i+1}", Statement(question), Action(self.ask_question, question, i))
            self.intents.append(intent)

        # Register intents with the agent
        for intent in self.intents:
            self.memory.add_intent(intent)
    
    def ask_question(self, question, index):
        print(f"Agent: {question}")
        
        # Wait for user input (simulate conversational process)
        user_answer = input("You: ").strip()
        
        # If the user does not answer, skip the question
        if not user_answer:
            user_answer = "No answer provided."

        # Store the answer in memory
        self.memory.set(f"answer_{index+1}", user_answer)

        # Return a confirmation or next question
        if index + 1 < len(self.questions):
            return f"Thank you for the answer. Let’s move to the next question."
        else:
            return f"Thank you for your responses. The requirements have been gathered."

    def run(self):
        # Start the conversation by asking the first question
        self.ask_question(self.questions[0], 0)


# Create the agent instance
agent = RequirementGatheringAgent()

# Start the conversation
agent.run()

Breakdown of the Code:

    Agent Initialization: We define the RequirementGatheringAgent class, which inherits from Agent in Langgraph. Inside the class, we initialize the memory (Memory) and define the conversation flow.
    Questions: We define a list of predefined questions that the agent will ask. These questions are stored in self.questions.
    Conversation Flow: We iterate through the questions and create intents (Intent) using Langgraph. Each intent corresponds to a specific question. When a question is asked, the agent executes the ask_question method.
    Handle User Input: The ask_question method listens for user input, stores the answer in memory (with self.memory.set), and moves to the next question.
    Missing Answers: If the user provides no answer, we handle it by storing "No answer provided." and move on to the next question. This ensures that the agent doesn’t get stuck if a user doesn’t answer a question.
    Conversation Flow Continuation: After each question, the agent moves on to the next one until all questions are asked.

3. Multi-Agent System Integration

In a multi-agent system, you can create other agents that interact with this initial requirement-gathering agent. You can expand this setup by defining other agents that handle tasks like analyzing answers or helping with specific requirements once gathered.

For instance, you can have an agent that processes the answers (e.g., organizes them, provides recommendations), or an agent that assists with specific technical tasks after gathering the requirements.

Here is how you might expand the system:

class RequirementAnalysisAgent(Agent):
    def __init__(self, memory):
        self.memory = memory

    def analyze_requirements(self):
        # Extract the answers from the memory
        answers = {
            "Project Description": self.memory.get("answer_1"),
            "Target Audience": self.memory.get("answer_2"),
            "Main Goals": self.memory.get("answer_3"),
            "Design Requirements": self.memory.get("answer_4"),
            "Completion Time": self.memory.get("answer_5")
        }
        
        # Process the answers and analyze them (e.g., categorize or provide insights)
        print("\n--- Analyzing the gathered requirements ---")
        for key, value in answers.items():
            print(f"{key}: {value}")

# Create the analysis agent, passing the memory from the first agent
analysis_agent = RequirementAnalysisAgent(agent.memory)
analysis_agent.analyze_requirements()

In this example, once the RequirementGatheringAgent has gathered all the answers, the RequirementAnalysisAgent is responsible for processing them. The data is shared via the agent’s memory, allowing it to pass relevant information to other agents in the system.
4. Enhancing the Conversational Experience

To improve the conversational experience, you can incorporate features like:

    Contextual Follow-ups: If a user gives an incomplete answer, the agent can follow up with questions for clarification.
    Personalization: The agent can remember past conversations or adapt its tone based on user preferences.
    Multilingual Support: You can use translation APIs to extend support for multiple languages, making the agent more accessible.

5. Storing and Retrieving Data

The answers from the user are stored in Langgraph Memory and can be retrieved later. Here is how you can access stored answers:

# Retrieve a specific answer from memory
answer = agent.memory.get('answer_1')
print(f"The user's answer to the first question: {answer}")

Conclusion

This example demonstrates how to create a Requirement Gathering Agent using Langgraph. The agent is conversational, asks a series of questions, handles missing answers, and stores user input efficiently. This could be expanded into a multi-agent system where different agents interact with the gathered data.

You can improve this further by adding more sophisticated conversation handling, integrating with other services, or personalizing the agent's behavior based on the answers provided by the user.
