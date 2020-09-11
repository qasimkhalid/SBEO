# Competencey questions for the SBEO

The given SPARQL are _examples_ that may be reinterpreted and reused for applications.

1. ### Spatial Information-related competency questions:
    1. Which building blocks are the part of which specific building?   

    ```
    SELECT ?buildingBlock ?specificBuilding
    WHERE {
        ?buildingBlock rdf:type ?building .
        ?building rdfs:subClassOf* seas:Building .
        ?buildingBlock sbeo:partOf ?specificBuilding . 
    }
    ```
    2. What is the length and width of all corridors (excluding corridor segments)?  
 
     ```
    SELECT ?corridor ?length ?width 
    WHERE {
        ?corridor rdf:type seas:Corridor .
        ?corridor sbeo:length ?length . 
        ?corridor sbeo:width ?width . 
    }
     ```
    
    3. How many points of interests are located on each floor of the building?   
    ```
    SELECT ?floor (COUNT (distinct ?poi) AS ?counter) 
    WHERE {
        ?poi rdf:type sbeo:PointOfInterest .
        ?poi sbeo:locatedIn ?space . 
        ?space sbeo:locatedIn ?floor . 
        ?floor rdf:type seas:BuildingStorey .
    }
    GROUP BY ?floor 
    ```
    
    4. Which other spaces are adjacent to the kitchen?   
    ```
    SELECT ?adjacentSpace
    WHERE {
        ?kitchen a seas:Kitchen .
        ?kitchen sbeo:adjacentTo ?adjacentSpace . 
        ?adjacentSpace rdf:type ?space . 
        ?space rdfs:subClassOf* sbeo:Space .
    }
    ```

    5. What is the current occupancy of all corridors?  
    ```
    SELECT ?corridor ?value
    WHERE {
        ?corridor a seas:Corridor .
        ?corridor sbeo:currentOccupancy ?value . 
    }
    ```

    6. Which spaces are excluded for which person?   
    ```
    SELECT ?space ?person
    WHERE {
        ?person rdf:type ?allType . 
        ?allType rdfs:subClassOf* foaf:Person . 
        ?space rdf:type ?allTypeSpace ;
               sbeo:excludedFor ?person . 

        ?allTypeSpace rdfs:subClassOf* sbeo:Space .
    }
    ```


2. ### Devices and components of the indoor environment-related competency questions:

    What are the fire incident protection devices located at the same floor where a person is located?  
    ```
    SELECT DISTINCT ?device ?person
    WHERE {
        ?device rdf:type ?allTypeIncidentProtection ;
                       sbeo:locatedIn ?poi .
        ?poi rdf:type sbeo:PointOfInterest ; 
                 sbeo:locatedIn ?space2 . 
        ?space1 rdf:type ?allTypeSpace1 ; 
                         sbeo:locatedIn ?floor .

        ?person rdf:type ?allTypePerson ;
                    sbeo:locatedIn ?space1 . 
        ?space2 rdf:type ?allTypeSpace2 ;
                         sbeo:locatedIn ?floor . 

        ?floor rdf:type seas:BuildingStorey .

        ?allTypeIncidentProtection  rdfs:subClassOf* sbeo:IncidentProtectionDevice .
        ?allTypePerson rdfs:subClassOf* foaf:Person . 
        ?allTypeSpace1 rdfs:subClassOf* sbeo:Space .
        ?allTypeSpace2 rdfs:subClassOf* sbeo:Space .
    }
    ```

    Which sensors are installed in each office?   
    ```
    SELECT ?office ?sensor
    WHERE {
        ?sensor rdf:type ?allTypeSensor ;
                        sbeo:installedIn ?office . 
        ?allTypeSensor rdfs:subClassOf* sbeo:Sensor . 

        ?office rdf:type seas:Office . 
    }
    ORDER BY ?office 
    ```

    Who is using a hand-held device and which one?  
    ```
    SELECT ?person ?device (?allTypeDevice AS ?deviceType)
    WHERE {
        ?person rdf:type ?allTypePerson ;
                        sbeo:uses ?device . 
        ?allTypePerson rdfs:subClassOf* foaf:Person . 

        ?device rdf:type ?allTypeDevice . 
        ?allTypeDevice rdfs:subClassOf* sbeo:HandheldDevice. 
    }
    ORDER BY ?person
    ```

    What type of sensors are installed in the building?   
    ```
    SELECT DISTINCT (?allTypeSensor AS ?sensorType)
    WHERE {
        ?sensor rdf:type ?allTypeSensor .
        ?allTypeSensor rdfs:subClassOf* sbeo:Sensor . 
    }
    ```




3. ### Route graph-related competency questions:

    What are the types of routes in terms of from graph-based representation?   
    ```
    ```

    What is the travel time of each route?   
    ```
    ```

    How many nodes and edges are generated from the layout of the building?   
    ```
    ```




4. ### Users’ characteristics and preferences-related questions.

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





5. ### Building situation awareness-related competency questions:

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



6. ### Users situation awareness-related competency questions:

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





7.  ### Emergency evacuation-related competency questions:

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

