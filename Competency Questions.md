# Competencey questions for the SBEO

The given SPARQL are _examples_ that may be reinterpreted and reused for applications.

1. ### User model-related competency questions:

   * ***Users’ characteristics.***

        * How many families are located in the office building?
    ```
    SELECT (COUNT (DISTINCT ?family) AS ?familyCount)
    WHERE {
        ?family rdf:type sbeo:Family ;
                sbeo:locatedIn ?space .

        ?space rdf:type ?allTypeBuilding . 
        ?allTypeBuilding rdfs:subClassOf* seas:Building .
    }
    ```

      * What are types of people with respect to their physical characteristics? 
    ```
    SELECT ?person (?allTypePerson AS ?type)
    WHERE {
        ?person rdf:type ?allTypePerson . 
        ?allTypePerson rdfs:subClassOf* foaf:Person .
    }
    ```

   * ***Users’ preferences.***

      * What are the notification preferences of each person?
       ```
       SELECT  ?person ?np
       WHERE {
           ?person rdf:type ?allTypePerson ;
                   sbeo:notificationPreference ?np .
           ?allTypePerson rdfs:subClassOf* foaf:Person  .  
       }
       ```

      * What are route preferences of each person?
       ```   
       SELECT  ?person ?rp
       WHERE {
           ?person rdf:type ?allTypePerson ;
                   sbeo:routePreference ?rp .
           ?allTypePerson rdfs:subClassOf* foaf:Person  .  
       }
       ```

2. ### Building model-related competency questions:


    * ***Spatial information.***
        * Which building blocks are the part of which specific building?   

        ```
        SELECT ?buildingBlock ?specificBuilding
        WHERE {
            ?buildingBlock rdf:type ?building ;
                           sbeo:partOf ?specificBuilding .
            ?building rdfs:subClassOf* seas:Building . 
        }
        ```
        
        * What is the length and width of all corridors (excluding corridor segments)? 

         ```
        SELECT ?corridor ?length ?width 
        WHERE {
            ?corridor rdf:type seas:Corridor ;
                      sbeo:length ?length ; 
                      sbeo:width ?width . 
        }
         ```


        * How many points of interests are located on each floor of the building?   
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


        * Which other spaces are adjacent to the kitchen?   
        ```
        SELECT ?adjacentSpace
        WHERE {
            ?kitchen rdf:type seas:Kitchen ;
                     sbeo:adjacentTo ?adjacentSpace . 
            ?adjacentSpace rdf:type ?space . 
            ?space rdfs:subClassOf* sbeo:Space .
        }
        ```


        * What is the current occupancy of all corridors?
        ```
        SELECT ?corridor ?value
        WHERE {
            ?corridor rdf:type seas:Corridor ;
                      sbeo:currentOccupancy ?value . 
        }
        ```


        * Which spaces are excluded for which person?  
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


    * ***Devices and components of the indoor environment.***

        * What are the fire incident protection devices located at the same floor where a person is located? 
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


        * Which sensors are installed in each office?
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
        

        * Who is using a hand-held device and which one? 
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

        * What type of sensors are installed in the building?
        ```
        SELECT DISTINCT (?allTypeSensor AS ?sensorType)
        WHERE {
            ?sensor rdf:type ?allTypeSensor .
            ?allTypeSensor rdfs:subClassOf* sbeo:Sensor . 
        }
        ```

    * ***Route graph***.


        * What are the types of routes in terms of from graph-based representation?
        ```
        SELECT ?route ?graphBasedtype 
        WHERE {
            ?route rdf:type ?allTypeRoute ;
                   sbeo:routeType ?graphBasedtype . 
            ?allTypeRoute rdfs:subClassOf* sbeo:Route .
        }
        ```


        * What is the travel time of each route?
        ```
        SELECT ?route ?time 
        WHERE {
            ?route rdf:type ?allTypeRoute ;
                   sbeo:travelTime ?time . 
            ?allTypeRoute rdfs:subClassOf* sbeo:Route .
        }
        ```


        * How many nodes and edges are generated from the layout of the building?
        ```
        SELECT  (COUNT (DISTINCT ?edge) AS ?edgeCount) (COUNT (DISTINCT ?node) AS ?nodeCount) 
        WHERE {
            ?edge rdf:type ?allTypePassage .              
            ?allTypePassage rdfs:subClassOf* sbeo:Passage . 

            ?node rdf:type ?allTypeRoutePoint .              
            ?allTypeRoutePoint rdfs:subClassOf* sbeo:RoutePoint . 
        }
        ```

3. ### Context model-related competency questions:       

    * ***Building situation awareness***.
        * Finding out any incident occurred in the building?
        ```
        SELECT ?incident
        WHERE {
            ?incident rdf:type ?allTypeIncident .
            ?allTypeIncident rdfs:subClassOf* sbeo:Incident .
        }
        ```

         * Finding out all the activities being done in an indoor environment?  
        ```
        SELECT ?activity
        WHERE {
            ?activity rdf:type ?allTypeActivity .
            ?allTypeActivity rdfs:subClassOf* sbeo:Activity .
        }
        ```

        *  At what time any incident occurred?
        ```
        SELECT ?incident ?startedTime
        WHERE {
            ?incident rdf:type ?allTypeIncident ;
                      sbeo:startedAtTime ?startedTime . 
            ?allTypeIncident rdfs:subClassOf* sbeo:Incident .
        }
        ```

        * What is the availability status of each space?
        ```
        SELECT ?space ?status
        WHERE {
            ?space rdf:type ?allTypeSpace ;
                   sbeo:hasAvailabilityStatus ?status . 
            ?allTypeSpace rdfs:subClassOf* sbeo:Space .
        }
        ```



    * ***Users situation awareness***.
        * Where is each person located in the building?
        ```
        SELECT ?person ?space
        WHERE {
            ?person rdf:type ?allTypePerson ;
                    sbeo:locatedIn ?space . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .
        }
        ```

        *  Which route is assigned to each person of each group (e.g., a family)?
        ```
        SELECT DISTINCT ?group ?person ?route
        WHERE {
            ?group rdf:type ?allTypeGroup ;
                   sbeo:hasMember ?person .  
            ?allTypeGroup rdfs:subClassOf* sbeo:Group . 

            ?person rdf:type ?allTypePerson ;
                    sbeo:assignedRoute ?route . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .

            ?route rdf:type ?allTypeRoute . 
            ?allTypeRoute rdfs:subClassOf* sbeo:Route .
        }
        ```

        *  What are the navigational states of each person?
        ```
        SELECT ?person ?state
        WHERE { 
            ?person rdf:type ?allTypePerson ;
                    sbeo:hasNavigationalState ?state . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .
        }
        ```

        * What are the motion states of each person?
        ```
        SELECT ?person ?state
        WHERE { 
            ?person rdf:type ?allTypePerson ;
                    sbeo:hasMotionState ?state . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .
        }   
        ```

        * How many times a person has deviated from one's provided path?   
        ```
        SELECT ?person ?deviation
        WHERE { 
            ?person rdf:type ?allTypePerson ;
                    sbeo:hasXTimesDeviated ?deviation . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .
        }
        ```

        * What is the fitness status of each person?
        ```
        SELECT ?person ?fitStatus
        WHERE { 
            ?person rdf:type ?allTypePerson ;
                    sbeo:hasFitnessStatus ?fitStatus . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .
        }
        ```
    
        * What is the role of each member within any group?
        ```
        SELECT DISTINCT ?person ?role ?group
        WHERE {
            ?person rdf:type ?allTypePerson ;
                    sbeo:hasRole ?role .  
            ?allTypePerson rdfs:subClassOf* foaf:Person .

            ?group rdf:type ?allTypeGroup ;
                   sbeo:hasMember ?person .  
            ?allTypeGroup rdfs:subClassOf* sbeo:Group .
        }
        ```





    * ***Emergency evacuation***.

        * What is the availability status of all emergency evacuation routes?
        ```
        SELECT ?route ?avStatus
            WHERE { 
            ?route rdf:type sbeo:EmergencyEvacuationRoute ;
                   sbeo:hasAvailabilityStatus ?avStatus .
        }
        ```

        * How many emergency evacuation groups are located in building?
        ```
        SELECT (COUNT (DISTINCT ?group) AS ?emergencyEvacGroups)
        WHERE { 
            ?group rdf:type sbeo:EmergencyEvacuationGroup .
        }
        ```

        * Who has evacuated the building successfully?
        ```
        SELECT ?person 
        WHERE { 
            ?person rdf:type ?allTypePerson ;
                    sbeo:hasActivityStatus sbeo:Evacuated . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .
        }
        ```
    
        * How many groups are still in the process of evacuating the building?
        ```
        SELECT ?group 
        WHERE { 
            ?group rdf:type ?allTypeGroup ;
                   sbeo:hasActivityStatus sbeo:Evacuating . 
            ?allTypeGroup rdfs:subClassOf* sbeo:Group .
        }
        ```


        
