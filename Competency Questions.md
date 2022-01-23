# Competencey questions for the SBEO

The given SPARQL are _examples_ that may be reinterpreted and reused for applications.

1. ### User model-related competency questions:


   * ***Users’ characteristics.***


        1. Who is not capable of running?
		
        2. How many families are located in the building?\\
        ```
        SELECT (COUNT (DISTINCT ?family) AS ?familyCount)
        WHERE {
            ?family rdf:type sbeo:Family ;
                    sbeo:locatedIn ?space .

            ?space rdf:type ?allTypeBuilding . 
            ?allTypeBuilding rdfs:subClassOf* seas:Building .
        }
        ```

        3. Who has a bad quality of hearing ability (in the building)?
		
        4. What are the types of people concerning their physical characteristics?? 
        ```
        SELECT ?person (?allTypePerson AS ?type)
        WHERE {
            ?person rdf:type ?allTypePerson . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .
        }
        ```

   * ***Users’ preferences.***

      	
	5. What are route preferences (for emergency evacuation, e.g., simplest path, shortest path) of each person?
       ```   
       SELECT  ?person ?rp
       WHERE {
           ?person rdf:type ?allTypePerson ;
                   sbeo:routePreference ?rp .
           ?allTypePerson rdfs:subClassOf* foaf:Person  .  
       }
       ```
	   	  
      	6. What are the notification preferences (in terms of description, e.g., audio, textual) of each person??
       ```
       SELECT  ?person ?np
       WHERE {
           ?person rdf:type ?allTypePerson ;
                   sbeo:notificationPreference ?np .
           ?allTypePerson rdfs:subClassOf* foaf:Person  .  
       }
       ```


2. ### Building model-related competency questions:


    * ***Spatial information.***
       

  	7. What is the relative occupancy ratio of all corridors?
		```
        SELECT ?corridor ?value
        WHERE {
            ?corridor rdf:type seas:Corridor ;
                      sbeo:currentOccupancy ?value . 
        }
        ```
		
	8. How many points of interest are located on each floor of the building?   
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
        
        9. Which other spaces are adjacent to a specific space (e.g., kitchen) in the building?   
        ```
        SELECT ?adjacentSpace
        WHERE {
            ?kitchen rdf:type seas:Kitchen ;
                     sbeo:adjacentTo ?adjacentSpace . 
            ?adjacentSpace rdf:type ?space . 
            ?space rdfs:subClassOf* sbeo:Space .
        }
        ```
		
        * 10. Which space (e.g., a specific building block) is a sub-part of which space (e.g., building)?   

        ```
        SELECT ?buildingBlock ?specificBuilding
        WHERE {
            ?buildingBlock rdf:type ?building ;
                           sbeo:partOf ?specificBuilding .
            ?building rdfs:subClassOf* seas:Building . 
        }
        ```
        
        * 11. What is the area of all corridors (it can be of any shape, such as, rectangular, circular, trapezoidal or triangular)? 

         ```
        SELECT ?corridor ?length ?width 
        WHERE {
            ?corridor rdf:type seas:Corridor ;
                      sbeo:length ?length ; 
                      sbeo:width ?width . 
        }
         ```



        * 12. Which spaces are excluded (due to any reason such as limited access on account of a mobility impairment or privacy policy of spaces, e.g., hotels) for which person? 
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


 * ***Route graph***.


		* 13. How many nodes and edges are there in the graph-based representation of building?
        ```
        SELECT  (COUNT (DISTINCT ?edge) AS ?edgeCount) (COUNT (DISTINCT ?node) AS ?nodeCount) 
        WHERE {
            ?edge rdf:type ?allTypePassage .              
            ?allTypePassage rdfs:subClassOf* sbeo:Passage . 

            ?node rdf:type ?allTypeRoutePoint .              
            ?allTypeRoutePoint rdfs:subClassOf* sbeo:RoutePoint . 
        }
        ```

        * 14. What is the type of each route in terms of its graph-based representation (e.g., Shortest Path or Simplest Path)?
        ```
        SELECT ?route ?graphBasedtype 
        WHERE {
            ?route rdf:type ?allTypeRoute ;
                   sbeo:routeType ?graphBasedtype . 
            ?allTypeRoute rdfs:subClassOf* sbeo:Route .
        }
        ```


        * 15. What is the travel time of all exit routes for each person (the starting and ending points of each exit route is considered as origin and destination respectively)?
        ```
        SELECT ?route ?time 
        WHERE {
            ?route rdf:type ?allTypeRoute ;
                   sbeo:travelTime ?time . 
            ?allTypeRoute rdfs:subClassOf* sbeo:Route .
        }
        ```


    * ***Devices***
 

        * 16. Who is using a hand-held device and of what type?
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
		
		* 17. Which sensors are installed in each space of a specific type (e.g., office)? 
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
        *  18. How many fire protection devices are installed on the same floor where a specific person is located?  
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
 
   
3. ### Context model-related competency questions:       


    * ***Building***.
	
	
		* 19. Which activities (e.g, visit, evacuation, shopping) are being done in the building? 
        ```
        SELECT ?activity
        WHERE {
            ?activity rdf:type ?allTypeActivity .
            ?allTypeActivity rdfs:subClassOf* sbeo:Activity .
        }
        ```
		
        * 20. What is the availability status (i.e., Available or Unavailable) of each space? 
        ```
        SELECT ?space ?status
        WHERE {
            ?space rdf:type ?allTypeSpace ;
                   sbeo:hasAvailabilityStatus ?status . 
            ?allTypeSpace rdfs:subClassOf* sbeo:Space .
        }
        ```
		
		
    * ***User***.
	
	
        * 21. Where is each person located in the building?
        ```
        SELECT ?person ?space
        WHERE {
            ?person rdf:type ?allTypePerson ;
                    sbeo:locatedIn ?space . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .
        }
        ```
		
		*  22. What is the role of each member within any group? 
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
		
		* 23. How many times a person has deviated from the provided path?   
        ```
        SELECT ?person ?deviation
        WHERE { 
            ?person rdf:type ?allTypePerson ;
                    sbeo:hasXTimesDeviated ?deviation . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .
        }
        ```
		 
        * 24. What is the fitness status (i.e, Exhausted, Fit, or Injured) of each person?
        ```
        SELECT ?person ?fitStatus
        WHERE { 
            ?person rdf:type ?allTypePerson ;
                    sbeo:hasFitnessStatus ?fitStatus . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .
        }
        ```
		
        *  25. Which route is assigned to whom of which group (refers to a number of people that are classified together, e.g., a family)?
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

		* 26. What is the motion state of each person (refers to the movement of a person, e.g., walking, standing, running, rolling, or scooting)?
        ```
        SELECT ?person ?state
        WHERE { 
            ?person rdf:type ?allTypePerson ;
                    sbeo:hasMotionState ?state . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .
        }   
        ```

        *  27. What is the navigational state of each person (refers to the state while following a path to check either a person is following the provided path or deviating from it)?
        ```
        SELECT ?person ?state
        WHERE { 
            ?person rdf:type ?allTypePerson ;
                    sbeo:hasNavigationalState ?state . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .
        }
        ```

        
    * ***Emergency evacuation***.


        * 28. Is there an incident in the building?
        ```
        SELECT ?incident
        WHERE {
            ?incident rdf:type ?allTypeIncident .
            ?allTypeIncident rdfs:subClassOf* sbeo:Incident .
        }
        ```

        * 29. At what time an incident occurred?
        ```
        SELECT ?incident ?startedTime
        WHERE {
            ?incident rdf:type ?allTypeIncident ;
                      sbeo:startedAtTime ?startedTime . 
            ?allTypeIncident rdfs:subClassOf* sbeo:Incident .
        }
        ```

        * 30. What is the availability status of the spaces that are a part of emergency evacuation routes?
        ```
        SELECT ?route ?avStatus
            WHERE { 
            ?route rdf:type sbeo:EmergencyEvacuationRoute ;
                   sbeo:hasAvailabilityStatus ?avStatus .
        }
        ```

        * 31. How many groups are still in the process of evacuating the building ?
        ```
        SELECT ?group 
        WHERE { 
            ?group rdf:type ?allTypeGroup ;
                   sbeo:hasActivityStatus sbeo:Evacuating . 
            ?allTypeGroup rdfs:subClassOf* sbeo:Group .
        }
        ```

        * 32. What is the impact of activities on persons having mild quality of seeing ability?
        
		* 33. What is the severity of the incidents for mobility-impaired persons (of all types)?
        
		* 34. What are the intensities (refers to the magnitude or strength) of the events occurred?
		
		* 35. Who has evacuated the building successfully (refers to the activity status of a person he who completes his/her provided exit route)?
        ```
        SELECT ?person 
        WHERE { 
            ?person rdf:type ?allTypePerson ;
                    sbeo:hasActivityStatus sbeo:Evacuated . 
            ?allTypePerson rdfs:subClassOf* foaf:Person .
        }
        ```

        
