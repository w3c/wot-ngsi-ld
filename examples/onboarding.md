# Use Case: Onboarding
Summary: Onboarding of devices described by TDs into NSGI-LD systems.
Support "Southbound" integration e.g. for a FIWARE server ingesting TD from other services.

## User Stories
(As a STAKEHOLDER/WHO, I want FEATURE/WHAT so that I can PURPOSE/WHY)

* As a Digital Twin Modeler, I want to be able to import WoT TDs into an NGSI-LD CIM system so that I can access live data from IoT devices and services.

## Background
WoT Thing Descriptions (TDs) are intended to describe "Things", which are either physical IoT devices are virtual IoT services.
The focus of WoT TDs are on descripting the network-accessible affordances of Things.  While technically optional, most of the 
definitions on the WoT ontologies related to such network affordances.  The WoT TD information model can be used in Linked Data
systems, although TDs themselves are designed to be serialized in JSON-LD in such a way that they can be consumed by non-RDF systems.
Ideally (although not strictly necessary for this use case) it would be possible to re-export ("expose") WoT TDs in such as way that they can be read ("consumed") by other systems
via the NGSI-LD API.

## Stakeholder Details
To do

## Feature Details
Summary: Import WoT TDs into a NGSI-LD CIM System

One use case for the use of WoT TDs with NGSI-LD-based systems is to import (in WoT terminology, "consume") WoT TDs in order to 
obtain information to access external Things.  This can be used to "onboard" a device or service (make it accessible from)
such a system.  Once the information is stored in an NGSI-LD CIM as linked data, it can be accessed by other systems using 
NGSI-LD-defined APIs.

## Feature Implementation Alternatives
The objective could be implemented in (at least) three ways:
1. A WoT TD, being Linked Data, could be imported directly into an NGSI-LD RDF graph.
2. A WoT TD could be translated into NGSI-LD defined entities in the graph.
3. A WoT TD could be used to "annotate" entities in an NGSI-LD graph.

### Option 1: Direct Import
This can only be done if we determine that the WoT and NGSI-LD ontologies are compatible.   For example, we would
have to resolve if the different "type systems" are compatible.  It may also not allow round-tripping, that is, easy
export of WoT TDs if there are downstream consumers that would like to in turn consume these TDs.  However, if possible,
it would make all WoT TD content available to existing NGSI-LD APIs.

Problem: TDs are not designed using NGSI-LD structure and would probably not be compatible.

### Option 2: Translation
If we determine that WoT TD and NGSI-LD ontologies are not compatible, it may be possible to modify the WoT TD content
so it aligns with NGSI-LD conventions.  Like Option 1, this would make all information available to existing NGSI-LD APIs,
but might make round-tripping more difficult: a reverse extraction-and-translation process would be needed.

Problem: non-standard representation 

### Option 3: Annotation
WoT TDs could be imported into the NGSI-LD graph but kept in separate subgraphs with their own sub-contexts, and linked
by a relation to other entities in the NGSI-LD graph.  This may make it possible to access the information expressed in
WoT TDs while still making it easy to re-serialized and "expose" WoT TDs when necessary.

## Purpose Details
Summary: Use with NGSI-LD Systems to connect to External IoT Devices and Services.

* Consider an NGSI-LD graph used as a "digital twin" of some system.
  A WoT TD would provide information about real devices or services that a digital twin could use to access
  "live data" related to some entity in the digital twin's virtual model.
  Ex: access real-time data for a lake modelled in the digital twin.
