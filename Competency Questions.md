# Competencey questions for the SBEO

The given SPARQL are _examples_ that may be reinterpreted and reused for applications.

1. ### User model-related competency questions:

2. ### Building model-related competency questions:

    a. #### Spatial information.
        i. Which building blocks are the part of which specific building?   

        ```
        SELECT ?buildingBlock ?specificBuilding
        WHERE {
            ?buildingBlock rdf:type ?building ;
                           sbeo:partOf ?specificBuilding .
            ?building rdfs:subClassOf* seas:Building . 
        }
        ```
       
        ii. What is the length and width of all corridors (excluding corridor segments)? 

         ```
        SELECT ?corridor ?length ?width 
        WHERE {
            ?corridor rdf:type seas:Corridor ;
                      sbeo:length ?length ; 
                      sbeo:width ?width . 
        }
         ```

        iii. How many points of interests are located on each floor of the building?   
        ```
        SELECT ?floor (COUNT (distinct ?poi) AS ?counter) 
        WHERE {
            ?poi rdf:type sbeo:PointOfInterest ;
                 sbeo:locatedIn ?space . 
            ?space sbeo:locatedIn ?floor . 
            ?floor rdf:type seas:BuildingStorey .
        }
        GROUP BY ?floor 
        ```

        iv. Which other spaces are adjacent to the kitchen?   
        ```
        SELECT ?adjacentSpace
        WHERE {
            ?kitchen rdf:type seas:Kitchen ;
                     sbeo:adjacentTo ?adjacentSpace . 
            ?adjacentSpace rdf:type ?space . 
            ?space rdfs:subClassOf* sbeo:Space .
        }
        ```

        v. What is the current occupancy of all corridors?
        ```
        SELECT ?corridor ?value
        WHERE {
            ?corridor rdf:type seas:Corridor ;
                      sbeo:currentOccupancy ?value . 
        }
        ```

        vi. Which spaces are excluded for which person?  
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


    b. #### Devices and components of the indoor environment.
        i. What are the fire incident protection devices located at the same floor where a person is located? 
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

            ?allTypePerson rdfs:subClassOf* foaf:Person . 
            ?allTypeSpace1 rdfs:subClassOf* sbeo:Space .
            ?allTypeSpace2 rdfs:subClassOf* sbeo:Space .
            ?allTypeIncidentProtection  rdfs:subClassOf* sbeo:IncidentProtectionDevice .
        }
        ```

        ii. Which sensors are installed in each office?
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

        iii. Who is using a hand-held device and which one? 
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

        iv. What type of sensors are installed in the building?
        ```
        SELECT DISTINCT (?allTypeSensor AS ?sensorType)
        WHERE {
            ?sensor rdf:type ?allTypeSensor .
            ?allTypeSensor rdfs:subClassOf* sbeo:Sensor . 
        }
        ```




    c. #### Route graph.

        i. What are the types of routes in terms of from graph-based representation?
        ```
        SELECT ?route ?graphBasedtype 
        WHERE {
            ?route rdf:type ?allTypeRoute ;
                   sbeo:routeType ?graphBasedtype . 
            ?allTypeRoute rdfs:subClassOf* sbeo:Route .
        }
        ```

        ii. What is the travel time of each route?
        ```
        SELECT ?route ?time 
        WHERE {
            ?route rdf:type ?allTypeRoute ;
                   sbeo:travelTime ?time . 
            ?allTypeRoute rdfs:subClassOf* sbeo:Route .
        }
        ```

        iii. How many nodes and edges are generated from the layout of the building?
        ```
        SELECT  (COUNT (DISTINCT ?edge) AS ?edgeCount) (COUNT (DISTINCT ?node) AS ?nodeCount) 
        WHERE {
            ?edge rdf:type ?allTypePassage .              
            ?allTypePassage rdfs:subClassOf* sbeo:Passage . 

            ?node rdf:type ?allTypeRoutePoint .              
            ?allTypeRoutePoint rdfs:subClassOf* sbeo:RoutePoint . 
        }
        ```


  

