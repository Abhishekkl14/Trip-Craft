# TripCraft - Multi-AI Agent Travel Planner

An intelligent travel itinerary planning system built using LangGraph and LangChain, powered by the Groq LLM API. This multi-agent system creates personalized day trip itineraries based on user preferences.

Live Demo: https://huggingface.co/spaces/AbhishekBS18/trip-craft
## Features

- **Interactive Input Collection**: Sequential gathering of city and interest information
- **AI-Powered Itinerary Generation**: Uses Llama 3.3 70B model via Groq API
- **Graph-Based Workflow**: Implements a state machine using LangGraph
- **User-Friendly Interface**: Gradio web interface for easy interaction
- **Conversation Tracking**: Maintains message history throughout the planning process

## Architecture

The system uses a multi-agent architecture with three main nodes:

1. **input_city**: Collects the destination city from the user
2. **input_interests**: Gathers user interests (comma-separated)
3. **create_itinerary**: Generates a personalized itinerary using LLM

The workflow follows a linear path: `input_city → input_interests → create_itinerary → END`

## Requirements

```
langchain>=1.0.7
langchain_core>=1.0.5
langchain_groq>=1.0.1
langchain_community>=0.4.1
langgraph>=1.0.3
gradio>=5.49.1
```

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd tripcraft
```

2. Install dependencies:
```bash
pip install langchain langchain_core langchain_groq langchain_community langgraph gradio
```

3. Set up your Groq API key in the code or as an environment variable

## Usage

### Command Line Interface

Run the Jupyter notebook and execute the travel planner function:

```python
user_request = "I want to plan a day trip"
travel_planner(user_request)
```

Follow the prompts to:
1. Enter your destination city
2. Enter your interests (comma-separated)

### Gradio Web Interface

Launch the web interface:

```python
interface.launch()
```

This will start a local web server where you can:
- Enter the city for your day trip
- Specify your interests (e.g., "explore, hike, food")
- Get a personalized itinerary instantly

## Example

**Input:**
- City: Kasaragod
- Interests: to explore, to hike

**Output:**
```
* 8:00 AM: Start the day with a visit to the Bekal Fort
* 10:00 AM: Hike to the nearby Bekal Beach
* 12:30 PM: Head to the Ranipuram Hills for a hike
* 3:30 PM: Visit the Valiyaparamba Backwaters
* 5:30 PM: End the day with a visit to the Chandragiri Fort
```

## System Components

### PlannerState
A TypedDict that maintains the application state:
- `messages`: Conversation history
- `city`: Selected destination
- `interests`: List of user interests
- `itinerary`: Generated travel plan

### LLM Configuration
- **Model**: Llama 3.3 70B Versatile
- **Provider**: Groq
- **Temperature**: 0 (deterministic responses)

### Workflow Visualization
The system generates a Mermaid diagram showing the agent workflow:
- Entry point: input_city
- Sequential flow through nodes
- Exit point: END

## Key Components

### Node Functions

**input_city(state)**
- Collects city information
- Updates state with user input
- Adds message to conversation history

**input_interests(state)**
- Collects comma-separated interests
- Parses and stores as list
- Updates conversation history

**create_itinerary(state)**
- Generates itinerary using LLM
- Formats as bulleted list
- Returns structured day plan

## Configuration

To use your own Groq API key, update the following line:

```python
llm = ChatGroq(
    temperature=0,
    groq_api_key="your-api-key-here",
    model_name="llama-3.3-70b-versatile"
)
```

## Customization

### Modify the Itinerary Prompt
Edit the system message in `itinerary_prompt`:

```python
itinerary_prompt = ChatPromptTemplate.from_messages([
    ("system", "Your custom prompt here..."),
    ("human", "Create an itinerary for my day trip."),
])
```

### Add More Nodes
Extend the workflow by adding new nodes:

```python
workflow.add_node("new_node", new_function)
workflow.add_edge("input_interests", "new_node")
workflow.add_edge("new_node", "create_itinerary")
```

## Limitations

- Requires active internet connection for LLM API calls
- API rate limits apply based on Groq subscription
- Limited to day trip itineraries (can be extended)

## Future Enhancements

- Multi-day trip planning
- Budget considerations
- Weather-based recommendations
- Integration with booking platforms
- Collaborative trip planning
- Save and export itineraries


## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Contact

abhishekbs242@gmail.com

## Acknowledgments

- Built with [LangChain](https://langchain.com/)
- Powered by [Groq](https://groq.com/)
- UI by [Gradio](https://gradio.app/)
- Special thanks to [Chando0185](https://github.com/Chando0185) for the original implementation and inspiration from the [Multiverse of 100+ Data Science Project Series](https://github.com/Chando0185/Multiverse_of_100-_data_science_project_series/blob/main/Travel%20Planner/Travel_Itinerary_Planner_MultiAI_Agent.ipynb)
