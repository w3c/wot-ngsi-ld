# Use Case: Thing Description for NGSI-LD Entity as Thing using NGSI-LD API
Summary: The goal of this Use Case is to explore what needs to be done to use a Thing Description to describe an NGSI-LD Entity as a Thing that is accessible using the NGSI-LD API, e.g is stored in an NGSI-LD Context Broker.

## User Stories
(As a STAKEHOLDER/WHO, I want FEATURE/WHAT so that I can PURPOSE/WHY)
* As a Digital Twin Modeler, I want to have a Thing Description for a Digital Twin modelled as an NGSI-LD Entity and accessible through the NGSI-LD API to uniformly handle it with other Digtial Twins for which I have a Thing Description.

## Background
NGSI-LD can be used to model Digital Twins of elements of the physical world in the form of NGSI-LD Entities. This can be conceptual elements like rooms or buildings, but also devices, e.g. sensors and actuators. To enable WoT applications to interact with such Digital Twins, a Thing Description is needed. As the NGSI-LD entity is accessible through the NGSI-LD API, a mapping between the interaction affordances of the W3C WoT interaction Model and corresponding NGSI-LD operations is needed.

## Stakeholder Details
As a Digital Twin Model I want to interact with the world through Things. The interaction is enabled through the interaction affordances specified in a Thing Description. Having a Thing Description for Things represented as NGSI-LD Entities allows the integration of all NGSI-LD Entities. As the NGSI-LD Information Model and the NGSI-LD API provides a uniform way of modelling, a generic approach working for all NGSI-LD Entities can be defined.

## Feature Details
Summary: As NGSI-LD entities have attributes (properties and relationships) - and in the future will have services (actuations), a generic model for corresponding Thing Descriptions can be defined that describes how to read and write NGSI-LD attributes as Thing Properties, how to be notified of changes of NGSI-LD attributes and how to trigger NGSI-LD services as actions. 

Currently, a complete HTTP binding has been defined for the NGSI-LD API. Other bindings are under consideration. Supported content types are `application/json` and `application/ld+json`. In the former case, an HTTP link header providing the JSON-LD @context is required.

Below a first high-level mapping is sketched that needs to be further elaborated on a binding basis:
- **properties**: Thing properties map to NGSI-LD attributes (properties, relationships, etc.) 
- **actions**: Currently NGSI-LD does not directly support actions.
- **events**: NGSI-LD does not have the concept of events, but allows subscriptions to changes of entity attributes. 

### Feature Implementation Alternatives
 1. Describe the existing NGSI-LD HTTP API in a WoT TD, e.g. properties->attributes, links->relationships, etc.
    - **properties**: Generally, the NGSI-LD API handles complete NGSI-LD entities, as these are full JSON-LD objects (with @id and @type), whereas properties are not (no @id) and are always part of their respective entity. Nevertheless, currently patch, replace and delete operations can be directly executed on NGSI-LD attributes, considering a "fragment" representation.         
    - **actions**: Conventions can be used to implement simple actions, i.e. through a subscription to a property, actuation components can be notified of an update to this property and      through this trigger a corresponding actuation. This has been identified as insufficient for more complex cases.
    - **events**: Thing events have the concept of `subscription` and `cancellation`, so a mapping may be feasible. Using `observeproperty`, notifications about changes in NGSI-LD attributes could also be considered. However, currently NGSI-LD does not directly allow subscriptions on the level of attributes, but only on the level of entities (specifying which attribute(s) can trigger the notification). This could possibly be more easily handled in case of `events`.
 2. As 1, but extend the existing NGSI-LD HTTP API to cover concepts not currently defined in NGSI-LD, e.g. actions, events
    - **properties**: Currently patch, replace and delete operations can be directly executed on NGSI-LD attributes, considering a "fragment" representation. Supporting read operations in addition can be considered.
    - **actions**: As the conventions currently used have been identified as insufficient for more complex cases, ETSI ISG CIM is currently working on supporting a "service execution" feature, which ideally should be defined in a way that is compatible to actions in Thing Descriptions.
    - **events**: Using `observeproperty`, notifications about changes in NGSI-LD attributes may be the conceptually better way to handle notifications about changes in attributes. This could be supported by allowing direct subscriptions to NGSI-LD attributes.
 3. Define a new NGSI-LD HTTP API that is easier to describe with a WoT TD and describe that
    - Always possible, but should primarily be considered if 1./2. do not lead to a good solution.
 4. Extend WoT TDs to better fit concepts in the NGSI-LD API
    - Always possible, but should primarily be considered if 1./2. do not lead to a good solution.
 5. Define a profile in WoT that defines precisely how to express NGSI-LD concepts in WoT TDs, e.g. actions
    - The work on 1./2. may lead to such a profile.

## Purpose Details
Summary: Use with WoT Systems to easily include Things represented as NGSI-LD Entities.

NGSI-LD is one approach to model the physical world as entities / digital twins. Making Thing Descrptions for NGSI-LD Entities avaiable, allows the integration into WoT Systems, where all Things are represented as Thing with a Thing Description that allows interactions with the Thing. Due to the uniform approach of NGSI-LD, a generic solution working for all NGSI-LD Entities can be defined.
