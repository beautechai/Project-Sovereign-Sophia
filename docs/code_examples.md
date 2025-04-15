# Code Examples and Templates

This document provides starter code examples and templates that can be used as a foundation for implementing aspects of Project Sovereign Sophia. These examples are meant to inspire and guide, rather than provide production-ready solutions.

## Knowledge Graph Example

```python
# Simple Knowledge Graph Implementation
import networkx as nx
import matplotlib.pyplot as plt

class PersonalKnowledgeGraph:
    def __init__(self):
        self.graph = nx.DiGraph()
        
    def add_node(self, node_id, **attributes):
        """Add a node to the knowledge graph with attributes."""
        self.graph.add_node(node_id, **attributes)
        
    def add_edge(self, source, target, **attributes):
        """Add a directed edge between two nodes with attributes."""
        self.graph.add_edge(source, target, **attributes)
        
    def get_related_nodes(self, node_id, relation_type=None):
        """Get nodes related to the specified node."""
        if relation_type:
            return [target for source, target, attr in self.graph.edges(node_id, data=True) 
                   if attr.get('type') == relation_type]
        return list(self.graph.neighbors(node_id))
    
    def visualize(self, figsize=(12, 8)):
        """Visualize the knowledge graph."""
        plt.figure(figsize=figsize)
        pos = nx.spring_layout(self.graph)
        nx.draw(self.graph, pos, with_labels=True, node_color='lightblue', 
                node_size=1500, arrowsize=20, font_size=10)
        edge_labels = {(u, v): d.get('type', '') for u, v, d in self.graph.edges(data=True)}
        nx.draw_networkx_edge_labels(self.graph, pos, edge_labels=edge_labels)
        plt.title("Personal Knowledge Graph")
        plt.axis('off')
        plt.show()

# Example usage
if __name__ == "__main__":
    # Create a simple personal knowledge graph
    pkg = PersonalKnowledgeGraph()
    
    # Add concept nodes
    pkg.add_node("AI", type="concept", description="Artificial Intelligence")
    pkg.add_node("Philosophy", type="concept", description="Study of fundamental questions")
    pkg.add_node("Sovereignty", type="concept", description="Self-governance and autonomy")
    
    # Add resource nodes
    pkg.add_node("Manifesto", type="document", path="/docs/manifesto.md")
    pkg.add_node("Technical Framework", type="document", path="/docs/technical_framework.md")
    
    # Add connections
    pkg.add_edge("AI", "Sovereignty", type="enables")
    pkg.add_edge("Philosophy", "Sovereignty", type="informs")
    pkg.add_edge("Manifesto", "AI", type="references")
    pkg.add_edge("Manifesto", "Philosophy", type="references")
    pkg.add_edge("Technical Framework", "AI", type="implements")
    
    # Visualize the graph
    pkg.visualize()
```

## Simple Agent Template

```python
# Simple Agent Template
import datetime
import json
import os

class Agent:
    def __init__(self, name, mission, values=None):
        self.name = name
        self.mission = mission
        self.values = values or []
        self.memory = []
        self.created_at = datetime.datetime.now()
        
    def perceive(self, input_data):
        """Process input data and add to memory."""
        timestamp = datetime.datetime.now()
        perception = {
            "timestamp": timestamp.isoformat(),
            "type": "perception",
            "content": input_data
        }
        self.memory.append(perception)
        return perception
        
    def reflect(self, perception):
        """Reflect on the perception based on mission and values."""
        # This is where you would implement more sophisticated reflection
        # For now, we'll just create a simple reflection
        reflection = {
            "timestamp": datetime.datetime.now().isoformat(),
            "type": "reflection",
            "based_on": perception["timestamp"],
            "content": f"Reflecting on input related to mission: {self.mission}"
        }
        self.memory.append(reflection)
        return reflection
        
    def respond(self, reflection):
        """Generate a response based on reflection."""
        # This is where you would implement more sophisticated response generation
        response = {
            "timestamp": datetime.datetime.now().isoformat(),
            "type": "response",
            "based_on": reflection["timestamp"],
            "content": f"Response aligned with mission: {self.mission} and values: {', '.join(self.values)}"
        }
        self.memory.append(response)
        return response
        
    def archive(self):
        """Archive the agent's memory to a file."""
        filename = f"{self.name.lower().replace(' ', '_')}_memory_{datetime.datetime.now().strftime('%Y%m%d_%H%M%S')}.json"
        with open(filename, 'w') as f:
            json.dump({
                "agent": {
                    "name": self.name,
                    "mission": self.mission,
                    "values": self.values,
                    "created_at": self.created_at.isoformat()
                },
                "memory": self.memory
            }, f, indent=2)
        return filename

# Example usage
if __name__ == "__main__":
    # Create a simple agent
    focus_agent = Agent(
        name="Focus Assistant",
        mission="Help maintain focus and alignment with core values",
        values=["Presence", "Intentionality", "Growth"]
    )
    
    # Run a simple agent loop
    input_data = "I'm feeling scattered today and having trouble focusing on my priorities."
    perception = focus_agent.perceive(input_data)
    reflection = focus_agent.reflect(perception)
    response = focus_agent.respond(reflection)
    
    print(f"Input: {input_data}")
    print(f"Response: {response['content']}")
    
    # Archive the interaction
    archive_file = focus_agent.archive()
    print(f"Interaction archived to {archive_file}")
```

## Agentic Loop Implementation

```python
# Agentic Loop Implementation
import time
import datetime
import json
import os

class AgenticLoop:
    def __init__(self, name, description=None):
        self.name = name
        self.description = description
        self.steps = []
        self.history = []
        
    def add_step(self, step_name, step_function):
        """Add a step to the agentic loop."""
        self.steps.append({
            "name": step_name,
            "function": step_function
        })
        
    def execute(self, initial_input=None):
        """Execute the full agentic loop."""
        current_data = initial_input
        execution_record = {
            "timestamp": datetime.datetime.now().isoformat(),
            "loop_name": self.name,
            "steps": []
        }
        
        for step in self.steps:
            start_time = time.time()
            try:
                current_data = step["function"](current_data)
                status = "success"
                error = None
            except Exception as e:
                status = "error"
                error = str(e)
            
            execution_record["steps"].append({
                "step_name": step["name"],
                "status": status,
                "error": error,
                "duration": time.time() - start_time
            })
            
            if status == "error":
                break
                
        self.history.append(execution_record)
        return current_data, execution_record
        
    def save_history(self, filename=None):
        """Save the execution history to a file."""
        if filename is None:
            filename = f"{self.name.lower().replace(' ', '_')}_history_{datetime.datetime.now().strftime('%Y%m%d_%H%M%S')}.json"
        
        with open(filename, 'w') as f:
            json.dump({
                "loop": {
                    "name": self.name,
                    "description": self.description,
                    "step_count": len(self.steps),
                    "step_names": [step["name"] for step in self.steps]
                },
                "history": self.history
            }, f, indent=2)
        
        return filename

# Example usage
if __name__ == "__main__":
    # Define step functions
    def journal_input(data):
        # In a real implementation, this might come from a user interface
        return "Today I need to focus on completing my project proposal."
    
    def analyze_state(journal_entry):
        # In a real implementation, this might use NLP or other analysis
        return {
            "journal_entry": journal_entry,
            "detected_focus": "project proposal",
            "sentiment": "neutral",
            "urgency": "high"
        }
    
    def suggest_focus(analysis):
        # In a real implementation, this might use more sophisticated logic
        return {
            "analysis": analysis,
            "suggestions": [
                "Block 2 hours of uninterrupted time for the project proposal",
                "Gather all necessary resources before starting",
                "Set clear completion criteria for today's session"
            ]
        }
    
    def generate_schedule(suggestions):
        # In a real implementation, this might integrate with calendar
        return {
            "suggestions": suggestions,
            "schedule": [
                {"time": "9:00 AM - 11:00 AM", "activity": "Work on project proposal"},
                {"time": "11:00 AM - 11:30 AM", "activity": "Break and review progress"},
                {"time": "11:30 AM - 12:30 PM", "activity": "Complete and review proposal"}
            ]
        }
    
    # Create and configure the agentic loop
    daily_focus_loop = AgenticLoop(
        name="Daily Focus Planning",
        description="A loop to help plan daily focus based on journal input"
    )
    
    daily_focus_loop.add_step("Journal Input", journal_input)
    daily_focus_loop.add_step("Analyze State", analyze_state)
    daily_focus_loop.add_step("Suggest Focus", suggest_focus)
    daily_focus_loop.add_step("Generate Schedule", generate_schedule)
    
    # Execute the loop
    result, execution_record = daily_focus_loop.execute()
    
    # Display the result
    print("Daily Focus Planning Result:")
    print("\nSchedule:")
    for item in result["schedule"]:
        print(f"{item['time']}: {item['activity']}")
    
    print("\nFocus Suggestions:")
    for suggestion in result["suggestions"]["suggestions"]:
        print(f"- {suggestion}")
    
    # Save the history
    history_file = daily_focus_loop.save_history()
    print(f"\nExecution history saved to {history_file}")
```

These examples provide a starting point for implementing aspects of Project Sovereign Sophia. They can be modified and expanded to suit your specific needs and technical capabilities.
