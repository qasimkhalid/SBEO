# Competencey questions for the SBEO

The given SPARQL are _examples_ that may be reinterpreted and reused for applications.

1. Spatial Information-related competency questions.

	
Which building blocks are the part of which specific building?   
```
SELECT ?buildingBlock ?specificBuilding
WHERE {
    ?buildingBlock rdf:type ?building .
    ?building rdfs:subClassOf* seas:Building .
    ?buildingBlock sbeo:partOf ?specificBuilding . 
}
```
    
What is the length and width of all corridors (excluding corridor segments)?  
    ```
    ```
    
How many points of interests are located on each floor of the building?   
    ```
    ```
    
Which other spaces are adjacent to the kitchen?   
    ```
    ```
    
What is the current occupancy of all corridors?  
    ```
    ```
    
Which spaces are excluded for which person?   
    ```
    ```
    
	
	
	
	
2. Devices and components of the indoor environment-related competency questions.
	
What are the fire incident protection devices located at the same floor where a person is located?  
    ```
    ```
    
Which sensors are installed in each office?   
    ```
    ```
    
Who is using a hand-held device and which one?  
    ```
    ```
    
What type of sensors are installed in the building?   
    ```
    ```
    
	
	

3. Route graph-related competency questions.

What are the types of routes in terms of from graph-based representation?   
    ```
    ```
    
What is the travel time of each route?   
    ```
    ```
    
How many nodes and edges are generated from the layout of the building?   
    ```
    ```
    
	


4. Users’ characteristics and preferences-related questions.

What are the notification preferences of each person?   
    ```
    ```
    
What are route preferences of each person?   
    ```
    ```
    
How many families are located in the office building?  
    ```
    ```
    
How people are classified with respect to their physical characteristics?   
    ```
    ```
    
What is the role of each member within any group?   
    ```
    ```
    
	



5. Building situation awareness-related competency questions.

Finding out any incident occurred in the building?   
    ```
    ```
    
Finding out all the activities being done in an indoor environment?   
    ```
    ```
    
At what time any incident occurred?   
    ```
    ```
    
What is the availability status of each space?   
    ```
    ```
    


6. Users situation awareness-related competency questions.

Where is each person located in the building?   
    ```
    ```
    
Which route is assigned to each person of each group (e.g., a family)?  
    ```
    ```
    
What are the navigational states of each person?  
    ```
    ```
    
What are the motion states of each person?  
    ```
    ```
    
How many times a person has deviated from one's provided path?   
    ```
    ```
    
What is the fitness status of each person?   
    ```
    ```
    
	



7. Emergency evacuation-related competency questions.

What is the availability status of all emergency evacuation routes?   
    ```
    ```
    
How many emergency evacuation groups are located in building?   
    ```
    ```
    
Who has evacuated the building successfully?   
    ```
    ```
    
How many groups are still in the process of evacuating the building?   
    ```
    ```
    
