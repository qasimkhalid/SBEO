# SBEO : Smart Building Evacuation Ontology

Smart Building Evacuation Ontology (SBEO) is an ontology that couples the information about any building with its occupants such that it can be used in many useful ways. For example, indoor localization of people, detection of any hazard, a recommendation of shopping or stadium seating routes, a recommendation of safe and feasible emergency evacuation routes, or all together. 

The core SBEO uses the three major concepts: topology of the building, situation awareness in the building, and the information about the physical characteristics and preferences of each person located in the building. 

Following are some general concepts in SBEO: Person, BuildingSpace, BuildingSpaceConnection, Incident, Device, SpaceCharacteristic, Route and Distance. As the reusing the vocabulary is the essence of linked data and semantic web, therefore in SBEO, we have also reused three external vocabularies, i.e. Person from FOAF(http://xmlns.com/foaf/spec/), and both BuildingSpace and BuildingSpaceConnection from SEAS Building Ontology (https://ci.mines-stetienne.fr/seas/BuildingOntology-1.0). On the other hand, we have introduced some classes in SBEO which are the subclasses of these existing vocabularies.


![](Figures/SBEO_Class_Diagram_updated.png)
