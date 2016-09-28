# FOREWORD

The Open Command and Control Forum (OpenC2 or the Forum) supports the cyber defense community of interest. The Open Command and Control Forum promotes the global development and adoption of the OpenC2 language and reference material.

This Forum serves developers, users, and the entire cybersecurity ecosystem by providing a set of shared resources to expand the use of standardized command and control for cyber defense activities, to enable technology vendors building orchestration and cyber response technologies, and to assist developers in producing response technologies that can be readily used in coordinated responses. The goal of the Forum is to present its findings and artifacts to recognized standards bodies for the open standardization of the command and control language.

This document represents the outcome of collaboration between technology vendors, government agencies, and academia on the topic of command and control for cyber defensive measures. We gratefully acknowledge their contributions to the definition of the OpenC2 language. As we exercise the language in reference implementations, we expect to continue to refine the language to ensure its suitability to support machine-to-machine command and control communications in response to cyber threats in cyber-relevant time.

Visit openc2.org for other on-line resources.

# Introduction

Cyberattacks are increasingly more sophisticated, less expensive to execute, dynamic, and automated. Current cyber defense products are typically integrated in a unique or proprietary manner and statically configured. As a result, upgrading or otherwise modifying tightly integrated, proprietary cyber defense’s functional blocks is resource intensive; cannot be realized within a cyber-relevant timeframe; and the upgrades may degrade the overall performance of the system.

Future cyber defenses against current and pending attacks require the integration of new or upgraded functional capabilities, the coordination of responses across domains, synchronization of response mechanisms, and deployment of automated actions in cyber relevant time.

Standardization of the lexicons and languages used in the interfaces and protocols necessary for machine-to-machine command and control communications in cyber relevant time will enable cyber defense system flexibility, interoperability, and responsiveness in cyber relevant time.

## Purpose

The purpose of the Open Command and Control (OpenC2) Language Description Document is to define a language and lexicon at a level of abstraction that will enable the coordination and execution of command and control of cyber defense components between domains and within a domain. It is expected that the OpenC2 language can be profiled (e.g., applicable commands, applicable values) by community groups for specific uses like Software Defined Networking.

## Scope

The scope of this document is to create a set of terms that define the actions, the target of the actions, and the entities that execute the actions. The document also defines an extensible syntax to accommodate attributes that further specify the targets, components, and modify actions to support a wide range of operational environments.

Future OpenC2 efforts will further refine the controlled vocabulary and define implementation approaches to facilitate interoperable machine to machine communications. These efforts will support the development and promulgation of reference implementations to demonstrate the use and flexibility of the OpenC2 language and promote the incorporation of OpenC2 in cyber defense solutions.

The definition of a language such as OpenC2 is necessary but insufficient to enable future cyber defenses. OpenC2 commands can be carried within any number of constructs (e.g., STIX, workflows, playbooks). In addition, OpenC2 is designed to be flexible, agnostic of external protocols that provide services such as transport, authentication, key management and other services. Cyber defense implementations will still need to rely on other protocols and security services.

## Intended Audience

This OpenC2 Language Description Document is intended for organizations investigating the implementation of automated pre-approved cyber defensive measures as well as academia and industry partners involved with the development and integration of security orchestration, network components or services, endpoint security applications, and security services for cyber defenses.

## Document Overview

[Section 1, Introduction](#introduction), describes the impetus for the OpenC2 language and lays out the purpose, scope, and intended audience of the document.

[Section 2, Background](#background), describes the design principles for the language and how the language can be contextualized for different operating environments.

[Section 3, OpenC2 Language](#openc2-language), describes the abstract syntax and the basic building blocks of the language. It also further specifies the vocabulary for actions, targets, actuators, and modifiers.

Section 4, Example OpenC2 Usage, provides examples of OpenC2 command constructs. For each action, the supported targets, actuators, and action specific modifiers are identified and example usages are provided.

Section 5, Example OpenC2 Use Case, depicts an example use case for mitigating an evil domain. The use case shows the OpenC2 commands that could be used to mitigate the attacks or vulnerabilities and where they could be applied.

Appendix A, Example OpenC2 commands, contains example OpenC2 commands organized in tables by OpenC2 action. These example commands were based on use cases provided by government agencies, critical infrastructure, industry (e.g., security orchestrator, actuator, and sensor) and academia.

# Background
## Design Principles

OpenC2 can be implemented in a variety of systems to perform the secure delivery and management of command and control messages in a context-specific way. OpenC2 commands are vendor neutral and message fabric agnostic, thus can be incorporated in different architectures and environments (i.e. connection oriented, connectionless, pub-sub, hub and spoke, etc.).

OpenC2 was designed to have a concise set of core actions that are extensible through attributes and modifiers to the language to provide context specific details. Conciseness ensures minimal overhead to meet possible latency and overhead constraints while extensions enable greater utility and flexibility.

There is an underlying assumption that issuing OpenC2 commands are event-driven and that an action is warranted. OpenC2 was designed to focus on the actions that are to be executed in order to thwart an attack, mitigate some vulnerability or otherwise address a threat. The exchange of indicators, rationale for the decision to act, authentication and/or information sharing are beyond the scope of OpenC2 and left to other standards such as STIX.

The actual performance and efficacy of OpenC2 will be implementation-specific and will require the incorporation of other technologies. The OpenC2 design principles include the following:

-   Support cyber relevant response time for coordination and response actions.
-   Be infrastructure, architecture, and vendor agnostic.
-   Support multiple levels of abstraction, necessary to permit the contextualization of commands for a wide variety of operating environments.
-   Permit commands to be invoked that are either tasking/response actions or notifications.

    -   Tasking/response actions result in a state change.

    -   Notifications require supporting analytics/decision processes.

-   Provide an extensible syntax to accommodate different types of actions, targets, and actuators (e.g., sensor, endpoint, network device, human) and specific targets and actuators.
-   Ensure the OpenC2 command is independent of a message construct that provides transport, identifies priority/ quality of service, and supports security attributes.

Traditional command and control implementations utilize complete, self-standing constructs. OpenC2 decouples the actions from the targets of the actions and from the recipients of the commands. An OpenC2 command is not complete until an action is paired with a target, providing the command context for the action. This enables the OpenC2 language to be more concise, yet still support the entire C2 space. This characteristic of OpenC2 also permits a more flexible and extensible approach to accommodate future technologies and varying network environments.

## OpenC2 and Deployment Environments

OpenC2 is defined at a level of abstraction such that an inter-domain
tasking or coordination effort can be described without requiring in
depth knowledge of the recipient network’s components, but through the
use of specifiers and modifiers, enough detail can be appended to carry
out specific tasks on particular devices to support intra-domain command
and control.

This level of abstraction permits end to end applicability of OpenC2. As
depicted in Figure 2‑1, an OpenC2 command is sent to enable coordination
or send a high level tasking from the peer or upper tier enclave. An
OpenC2 command received by an enclave will trigger events within the
enclave to annotate the command with context specific information so
that specific devices within the enclave can respond appropriately. This
allows the enclave to take advantage of this context-specific knowledge
to interpret OpenC2 commands (e.g., inventory of actuators controlled by
the enclave, the local security policy, the communication paths and
protocols available, and the command structure of the enclave).

Each domain or enclave contextualizes an OpenC2 action for the specific
sensors and actuators within its environment so it can further specify
the command to reflect the implementations of which it is capable.
Context-specific modifiers provide an ability to further specify the
action while enabling the set of actions to remain tightly constrained.
This minimizes the overhead, permits further contextualization of the
OpenC2 commands for specific environments, and thereby enables
flexibility and extensibility.

![](media/image2.emf){width="6.5in" height="3.6417410323709536in"}

<span id="_Ref444589552" class="anchor"><span id="_Toc437493505"
class="anchor"><span id="_Toc444668331" class="anchor"><span
id="_Toc459021501" class="anchor"><span id="_Toc459584884"
class="anchor"></span></span></span></span></span>Figure ‑. OpenC2
Deployment Environments

For example, an organization may have executed a series of actions to
protect against a particular attack that was signaled by an external
indicator (such as a STIX message). In order to elicit a consistent
response across an organization (whether hierarchical or peer to peer),
a complex course of action can be constructed and shared. The use of
standardized and unambiguous OpenC2 commands to communicate a responsive
action between enterprises/enclaves will be more precise and more
quickly actionable than a set of recommended steps within a text
document (e.g., flash), which must be parsed, analyzed, interpreted, and
executed. Standardizing OpenC2 commands helps to ensure a more uniform
response at enterprises/enclaves that reflects enterprise-wide level
decisions.

<span id="_Toc442171147" class="anchor"><span id="_Toc442171254" class="anchor"><span id="_Toc442185949" class="anchor"><span id="_Toc442186130" class="anchor"><span id="_Toc442215786" class="anchor"><span id="_Toc442171148" class="anchor"><span id="_Toc442171255" class="anchor"><span id="_Toc442185950" class="anchor"><span id="_Toc442186131" class="anchor"><span id="_Toc442215787" class="anchor"><span id="_Toc444511032" class="anchor"><span id="_Toc444512134" class="anchor"><span id="_Toc444258420" class="anchor"><span id="_Toc444611152" class="anchor"><span id="_Ref444668890" class="anchor"><span id="_Toc459584780" class="anchor"></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span>OpenC2 Language
===============================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================

<span id="_Toc431301970" class="anchor"><span id="_Toc444258421" class="anchor"><span id="_Toc444611153" class="anchor"><span id="_Toc459584781" class="anchor"></span></span></span></span>Overview
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

The OpenC2 language is designed at a level of abstraction high enough
such that it enables persistence as technologies advance and is
implementation agnostic, but detailed enough so that the need for
specifiers and modifiers is limited.

<span id="_Toc444258422" class="anchor"><span id="_Toc444611154" class="anchor"><span id="_Toc459584782" class="anchor"></span></span></span>Abstract Syntax
------------------------------------------------------------------------------------------------------------------------------------------------------------

<span id="_Toc439802251" class="anchor"><span id="_Toc439802620"
class="anchor"><span id="_Toc439802700" class="anchor"><span
id="_Toc439802781" class="anchor"><span id="_Toc439802862"
class="anchor"><span id="_Toc439802942" class="anchor"><span
id="_Toc439806062" class="anchor"><span id="_Toc442108301"
class="anchor"><span id="_Toc442108407" class="anchor"><span
id="_Toc437330633" class="anchor"><span id="_Toc437330725"
class="anchor"><span id="_Toc437335533" class="anchor"><span
id="_Toc437335626" class="anchor"><span id="_Toc437350546"
class="anchor"><span id="_Toc437350746" class="anchor"><span
id="_Toc437351693" class="anchor"><span id="_Toc437351783"
class="anchor"><span id="_Toc437352129" class="anchor"><span
id="_Toc442171152"
class="anchor"></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span>Conceptually,
an OpenC2 command has the following form:

(

ACTION (

type = &lt;ACTION\_TYPE&gt;

),

TARGET (

type = &lt;TARGET\_TYPE&gt;,

&lt;target-specifier&gt;

),

ACTUATOR (

type = &lt;ACTUATOR\_TYPE&gt;,

&lt;actuator-specifier&gt;

),

MODIFIERS (

&lt;list-of-modifiers&gt;

)

)

Fields denoted with angle brackets (“&lt;&gt;“) are replaced with the
appropriate details. Some of the fields are considered optional. The
table below describes these fields and whether they are required,
optional or ignored in certain situations. Actual implementation
approaches will leverage pre-existing conventions and notations such as
XML, JSON, or Type-Length-Value delimitation.

The following table contains the description of the fields that can be
contained in an OpenC2 command.

<span id="_Toc459584886" class="anchor"></span>Table ‑. OpenC2 Command
Field Descriptions

  -------------------------------------------------------------------------------------------------------------------------------------------------
  Field                                   Description
  --------------------------------------- ---------------------------------------------------------------------------------------------------------
  ACTION                                  Required.
                                          
                                          The task or activity to be performed (i.e., the ‘verb’).

  type                                    Required.
                                          
                                          The ACTION type is the name of the action.

  TARGET                                  Required for actions, not applicable for responses.
                                          
                                          The object of the action. The ACTION is performed on the TARGET.

  type                                    Required.
                                          
                                          The TARGET type will be defined within the context of a namespace.

  target-specifier                        Optional.
                                          
                                          The specifier further describes a specific target, a list of targets, or a class of targets.

  ACTUATOR                                Optional.
                                          
                                          The subject of the action. The ACTUATOR executes the ACTION on the TARGET.

  type                                    Required.
                                          
                                          The ACTUATOR type will be defined within the context of a namespace.

  actuator-specifier                      Optional.
                                          
                                          The specifier further describes a specific actuator, a list of actuators, or a class of actuators.

  MODIFIERS (&lt;list-of-modifiers&gt;)   Optional.
                                          
                                          Provide additional information about the action such as date/time, periodicity, duration, and location.
  -------------------------------------------------------------------------------------------------------------------------------------------------

There are cases where an ACTION and TARGET are sufficient to complete
the command, especially in the case of inter-domain commands where the
method or approach to complete or execute the action can be determined
within the receiving domain/enclave.

The majority of commands within an enclave will have an ACTION, TARGET
and ACTUATOR. Inclusion of the ACTUATOR provides additional context for
the command as a whole and enables efficiency.

Specifiers for TARGETs and ACTUATORs are optional and can be used to
provide context specific information that could be used to reflect the
local environment, policies, and operational conditions within an
enterprise/enclave. Specifiers can call out a specific target/actuator,
a list of targets/actuators, or a class of targets/actuators.

Modifiers to the ACTION are optional and are used to provide effect
based context to the ACTION. Modifiers are further discussed in Section
3.2.5.

Table 3‑2 illustrates the use of specifiers and modifiers to extend the
range of OpenC2 commands to cover the higher level ‘strategic’ commands
to the unambiguous enclave-specific use case. This provides greater
flexibility to the language and allows the OpenC2 actions to be further
contextualized for the mission environment. The table below provides
some examples of the different levels of specificity achievable in an
OpenC2 command.

<span id="_Ref442249049" class="anchor"><span id="_Toc444260049"
class="anchor"><span id="_Toc444611351" class="anchor"><span
id="_Toc459021502" class="anchor"><span id="_Ref459022418"
class="anchor"><span id="_Toc459584887"
class="anchor"></span></span></span></span></span></span>Table ‑. OpenC2
Syntax Flexibility Examples

  Description                                                                                                                                                                                                    Action   Target                                 Actuator                Modifier
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -------- -------------------------------------- ----------------------- -------------------
                                                                                                                                                                                                                          Target-Specifier                       Actuator-Specifier      
  Block traffic to/from specific IP address \[effects-based, no actuator specified\]; suitable for inter-domain coordination                                                                                     DENY     Network Connection                                             
                                                                                                                                                                                                                          Source and/or Destination IP Address                           
  Block traffic at all network devices \[specify actuator class\]; suitable for inter-domain coordination or as a command to an orchestration engine which further contextualizes to the enclave’s environment   DENY     Network Connection                     Network (any devices)   
                                                                                                                                                                                                                          Source and/or Destination IP Address                           
  Block traffic at network routers \[specify type of network device actuator\]; suitable within an enclave                                                                                                       DENY     Network Connection                     Network.router          
                                                                                                                                                                                                                          Source and/or Destination IP Address   (optional)              
  Block traffic at specific network router; \[specify identity of network router\]; suitable within an enclave                                                                                                   DENY     Network Connection                     Network.router          
                                                                                                                                                                                                                          Source and/or Destination IP Address   Router identity         
  Block access to bad external IP by null routing; \[specify method of performing action\]; suitable within an enclave                                                                                           DENY     Network Connection                     Network.router          Method= blackhole
                                                                                                                                                                                                                          Source and/or Destination IP Address   (optional)              

<span id="_Toc442185955" class="anchor"><span id="_Toc442186136"
class="anchor"><span id="_Toc442215792" class="anchor"><span
id="_Toc442185956" class="anchor"><span id="_Toc442186137"
class="anchor"><span id="_Toc442215793" class="anchor"><span
id="_Toc442185957" class="anchor"><span id="_Toc442186138"
class="anchor"><span id="_Toc442215794" class="anchor"><span
id="_Toc442185958" class="anchor"><span id="_Toc442186139"
class="anchor"><span id="_Toc442215795" class="anchor"><span
id="_Toc442185959" class="anchor"><span id="_Toc442186140"
class="anchor"><span id="_Toc442215796" class="anchor"><span
id="_Toc442185960" class="anchor"><span id="_Toc442186141"
class="anchor"><span id="_Toc442215797" class="anchor"><span
id="_Toc442185961" class="anchor"><span id="_Toc442186142"
class="anchor"><span id="_Toc442215798" class="anchor"><span
id="_Toc442185962" class="anchor"><span id="_Toc442186143"
class="anchor"><span id="_Toc442215799" class="anchor"><span
id="_Toc442185963" class="anchor"><span id="_Toc442186144"
class="anchor"><span id="_Toc442215800" class="anchor"><span
id="_Toc442185964" class="anchor"><span id="_Toc442186145"
class="anchor"><span id="_Toc442215801" class="anchor"><span
id="_Toc444258423"
class="anchor"></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span>

### <span id="_Toc444611155" class="anchor"><span id="_Toc459584783" class="anchor"></span></span>Action

All OpenC2 commands start with an ACTION which indicates the type of
command to perform such as gather and convey information, control
activities and devices, and control permissions and access. The range of
options and potential impact on the information system associated with a
particular ACTION is a function of the ACTUATOR. For cases that involve
multiple options for an ACTION, modifiers are used.

Refer to Section 3.3 for the list of ACTIONs and their definitions and
usage.

### <span id="_Toc444258424" class="anchor"><span id="_Toc444611156" class="anchor"><span id="_Toc459584784" class="anchor"></span></span></span>Target

The TARGET is the object of the ACTION (or alternatively, the ACTION is
performed on the TARGET). Targets include objects such as network
connections, URLs, hashes, IP addresses, files, processes, and domains.

There will be only one TARGET type per OpenC2 command. By design, OpenC2
will support any data model, but for illustrative purposes OpenC2
TARGETs will reference CybOX objects to the greatest extent practical in
this document. Data models will be identified by a namespace before the
TARGET type name. Section 3.4 contains a compiled list and definitions
of the TARGETs that support the OpenC2 language.

### <span id="_Toc442108306" class="anchor"><span id="_Toc442108412" class="anchor"><span id="_Toc442108515" class="anchor"><span id="_Toc442171156" class="anchor"><span id="_Toc442171261" class="anchor"><span id="_Toc442185967" class="anchor"><span id="_Toc442186148" class="anchor"><span id="_Toc442215804" class="anchor"><span id="_Toc444258425" class="anchor"><span id="_Toc444611157" class="anchor"><span id="_Toc459584785" class="anchor"></span></span></span></span></span></span></span></span></span></span></span>Actuator

An ACTUATOR[^1] is the entity that puts command and control into motion
or action. The ACTUATOR is the subject of the ACTION which performs the
ACTION on the TARGET. There are varying levels of abstraction and
functionality for an ACTUATOR ranging from a specific sensor to an
entire system or even system of systems.

There will be only one ACTUATOR type per OpenC2 command. OpenC2 will
leverage existing data models to the greatest extent practical (e.g.,
the Secure Automation and Configuration Management (SACM) working group,
ISCM taxonomy). Section 3.5 contains a compiled list of actuators with
definitions used to support OpenC2.

### <span id="_Toc444258426" class="anchor"><span id="_Toc444611158" class="anchor"><span id="_Toc459584786" class="anchor"></span></span></span>Specifiers

“Specifiers” are used to identify specific individual or groups of
targets or actuators. Table 3‑3 illustrates how the commands are
appended with specifiers as context specific details become available.

<span id="_Ref444593865" class="anchor"><span id="_Toc444260050"
class="anchor"><span id="_Toc444611352" class="anchor"><span
id="_Toc459021503" class="anchor"><span id="_Toc459584888"
class="anchor"></span></span></span></span></span>Table ‑. Example Usage
of Specifiers

  Description                                                                       Action       Target                     Actuator                                         Modifier
  --------------------------------------------------------------------------------- ------------ -------------------------- ------------------------------------------------ ----------
                                                                                                 Target-Specifier           Actuator-Specifier                               
  Block malicious URL                                                               DENY         URI/URL                                                                     
                                                                                                 Value Condition = Equals                                                    
  Quarantine Artifact with particular byte string                                   QUARANTINE   Artifact                                                                    
                                                                                                 Condition = Contains                                                        
  Block access to external IP address by null routing at specific network routers   DENY         Network Connection         Network router                                   
                                                                                                 Condition = Contains       Manufacturer, Model, Serial Number Value = 123   

<span id="_Toc442108309" class="anchor"><span id="_Toc442108415"
class="anchor"><span id="_Toc442108518" class="anchor"><span
id="_Toc442171159" class="anchor"><span id="_Toc442171264"
class="anchor"><span id="_Toc442185970" class="anchor"><span
id="_Toc442186151" class="anchor"><span id="_Toc442215807"
class="anchor"><span id="_Ref437348163" class="anchor"><span
id="_Toc444258427"
class="anchor"></span></span></span></span></span></span></span></span></span></span>

### <span id="_Toc444611159" class="anchor"><span id="_Ref444670890" class="anchor"><span id="_Toc459584787" class="anchor"></span></span></span>Modifiers

“Modifiers” provide additional information about the action such as
time, periodicity, duration, and location. Modifiers can denote the
when, where, and how aspects of an action. Modifiers are similar to
specifiers in that they can provide additional context specific details,
and are intended to provide additional details for action/actuator
pairs.

When present, modifiers are always associated with a specific action,
however, some modifiers can generally be applied to more than one action
or to all OpenC2 actions. A modifier is said to be “actuator-specific”,
“action-specific”, or “universal” depending on the applicability of the
modifier within the language.

The modifier can also be used to convey the need for additional status
information about the execution of an action. Modifiers can be used to
indicate whether the actuator should explicitly acknowledge receipt of
the command, respond upon completion of the execution of the command, or
provide some other status information. The requested status/information
will be carried in a RESPONSE. Refer to Section 4.6.

<span id="_Toc444260051" class="anchor"><span id="_Toc444611353"
class="anchor"><span id="_Toc459021504" class="anchor"><span
id="_Toc459584889" class="anchor"></span></span></span></span>Table ‑.
Example Usage of Modifiers

  Description                                                                                                    Action    Target                           Actuator             Modifier
  -------------------------------------------------------------------------------------------------------------- --------- -------------------------------- -------------------- -------------------------------
                                                                                                                           Target-Specifier                 Actuator-Specifier   
  Shutdown a system, immediate                                                                                   STOP      Device                           endpoint             method = immediate
                                                                                                                           Device Object Type               (optional)           
  Start Process with Delay                                                                                       START     Process                          endpoint             Delay = duration
                                                                                                                           Process Object Type              (optional)           
  Quarantine a device                                                                                            CONTAIN   Device                           network              where (network segment, vlan)
                                                                                                                           Device Object Type               (optional)           
  Block access to suspicious external IP address by redirecting external DNS queries to an internal DNS server   DENY      Network Connection               DNS Server           method = sinkhole
                                                                                                                           Network Connection Object Type                        

<span id="_Ref457998374" class="anchor"><span id="_Ref437348731" class="anchor"><span id="_Ref437348740" class="anchor"><span id="_Ref437348789" class="anchor"><span id="_Toc444258428" class="anchor"><span id="_Toc444611160" class="anchor"><span id="_Toc459584788" class="anchor"></span></span></span></span></span></span></span>Actions
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

This section defines the set of OpenC2 actions grouped by their general
activity. The following table summarizes the definition of the OpenC2
actions. Subsequent sections will identify the appropriate targets for
each action and the appropriate actuators for the action target pair.

### <span id="language_action" class="anchor"><span id="_Toc459584789" class="anchor"></span></span>Actions that Gather and Convey Information

These actions are used to gather information needed to further determine
courses of action or assess the effectiveness of courses of action.
These actions can be used to support data enrichment use cases and
maintain situational awareness. These actions typically do not impact
the state of the target and are normally not detectable by external
observers.

### Actions that Control Permissions

These actions are used to control permissions and accesses. The
permissions and accesses can be for person or non-person entities.

### Actions that Control Activities/Devices

These actions are used to control the state or the activity of a system,
a process, a connection, a host, or a device (e.g., endpoint, sensor,
actuator). The actions are used to adjust configurations, set and update
parameters, and modify attributes.

### Sensor-Related Actions

These actions are used to control the activities of a sensor in terms of
how to collect and provide the sensor data.

### Effects-Based Actions

Effects-based actions are at a higher level of abstraction and focus on
the desired impact rather than a command to execute specific task(s)
within an enclave. The benefit of including effects-based actions is
that it permits a higher level or peer enclave to coordinate actions,
while still permitting a local enclave to optimize its workflow for its
specific environment in order to achieve the desired result.

Implementation of an effects-based action requires that the recipient
enclave has a decision making capability because an effects-based action
permits multiple possible responses.

### Response and Alert

RESPONSE is used to provide data requested as a result of an action. The
RESPONSE message will contain the requested data and have a reference to
the action that initiated the response. ALERT is used to signal the
occurrence of an event or error. It is an unsolicited message that does
not reference a previously issued action.

<span id="_Ref459022950" class="anchor"><span id="_Toc459584890"
class="anchor"></span></span>Table ‑. Summary of Action Definitions

  -------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Actions that Gather and Convey Information
  SCAN
  LOCATE
  QUERY
  REPORT
  GET
  NOTIFY
  Actions that Control Permissions
  DENY
  CONTAIN
  ALLOW
  Actions that Control Activities/Devices
  START
  STOP
  RESTART
  PAUSE
  RESUME
  CANCEL
  SET
  UPDATE
  MOVE
  REDIRECT
  DELETE
  SNAPSHOT
  DETONATE
  RESTORE
  SAVE
  MODIFY
  THROTTLE
  DELAY
  SUBSTITUTE
  COPY
  SYNC
  -------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Table 3‑5. Summary of Action Definitions (Cont.)

  ------------------------ -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Sensor-Related Actions
  DISTILL
  AUGMENT
  Effects-Based Actions
  INVESTIGATE
  MITIGATE
  REMEDIATE
  Response and Alert
  RESPONSE
  ALERT
  ------------------------ -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<span id="_Ref457998617" class="anchor"><span id="_Toc459584795" class="anchor"></span></span>Target Vocabulary
---------------------------------------------------------------------------------------------------------------

The TARGET is the object of the ACTION (or alternatively, the ACTION is
performed on the TARGET). OpenC2 will utilize pre-existing data models
to provide the namespace for the TARGETs. This document will reference
the applicable CybOX objects in the OpenC2 TARGET namespace. However,
the OpenC2 syntax supports custom or other data models. Refer to the
following table for a summary o<span id="language_target"
class="anchor"></span>f the OpenC2 TARGET Namespaces.

<span id="_Toc459584891" class="anchor"></span>Table ‑. Target Namespace

  ----------------------------------------------------------------------------------------------------------------------------------------------------
  Type        Description                                                                                                          Options
  ----------- -------------------------------------------------------------------------------------------------------------------- -------------------
  namespace   Used to uniquely identify a set of names so there is no ambiguity; defines the context in which names are defined.   Choice of:
                                                                                                                                   
                                                                                                                                   -   CybOX Version
                                                                                                                                   
                                                                                                                                   -   OpenC2
                                                                                                                                   
                                                                                                                                   -   Custom
                                                                                                                                   
                                                                                                                                   
  ----------------------------------------------------------------------------------------------------------------------------------------------------

Targets include objects such as network connections, URLs, hashes, IP
addresses, files, processes, and domains. Refer to the following table
for a summary of the supported OpenC2 TARGETs in the CybOX 2.1 Namespace
(http://cybox.mitre.org/cybox-2).

<span id="_Ref459023139" class="anchor"><span id="_Toc459584892"
class="anchor"></span></span>Table ‑. Summary of Supported Targets

  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Target Type                 Description                                                                                  Target Specifier
  --------------------------- -------------------------------------------------------------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  cybox:Address               The Address object is intended to specify a cyber address.                                   cybox:AddressObjectType:
                                                                                                                           
                                                                                                                           Address Value, VLAN Name, VLAN Num

  cybox:Device                The Device object is intended to characterize a specific Device.                             cybox:DeviceObjectType:
                                                                                                                           
                                                                                                                           Description, Device Type, Manufacturer, Model, Serial Number, Firmware Version, System Details

  cybox:Disk                  The Disk object is intended to characterize a disk drive.                                    cybox:DiskObjectType:
                                                                                                                           
                                                                                                                           Disk Name, Disk Size, Free Space, Partition List, Type

  cybox:Disk\_Partition       The Disk\_Partition object is intended to characterize a single partition of a disk drive.   cybox:DiskPartitionObjectType:
                                                                                                                           
                                                                                                                           Created, Device Name, Mount Point, Partition ID, Partition Length, Partition Offset, Space Left, Space Used, Total Space, Type

  cybox:Domain\_Name          The Domain\_Name object is intended to characterize network domain names.                    cybox:DomainNameObjectType:
                                                                                                                           
                                                                                                                           Value

  cybox:Email\_Message        The Email\_Message object is intended to characterize an individual email message.           cybox:EmailMessageObjectType:
                                                                                                                           
                                                                                                                           Header, Email Server, Raw Body, Raw Header, Attachments, Links

  cybox:File                  The File object is intended to characterize a generic file.                                  cybox:FileObjectType:
                                                                                                                           
                                                                                                                           File Name, File Path, Device Path, Full Path, File Extension, Size In Bytes, Magic Number, File Format, Hashes, Digital Signatures, Modified Time, Accessed Time, Created Time, File Attributes List, Permissions, User Owner, Packer List, Peak Entropy, Sym Links, Byte Runs, Extracted Features, Encryption Algorithm, Decryption Key, Compression Method, Compression Version, Compression Comment

  cybox:Hostname              The Hostname object is intended to specify a particular network hostname.                    cybox:HostNameObjectType:
                                                                                                                           
                                                                                                                           Hostname Value, Naming System

  cybox:Memory                The Memory\_Region object is intended to characterize generic memory objects.                cybox:MemoryObjectType:
                                                                                                                           
                                                                                                                           Hashes, Name, Memory Source, Region Size, Block Type, Region Start Address, Region End Address, Extracted Features

  cybox:Network\_Connection   The Network\_Connection object is intended to represent a single network connection.         cybox:NetworkConnectionObjectType:
                                                                                                                           
                                                                                                                           Layer3 Protocol, Layer4 Protocol, Source Socket Address (IP Address/Port), Destination Socket Address, (IP Address/Port)
  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Table 3‑7. Summary of Supported Targets (Cont.)

  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Target Type             Description                                                                                                                                                                                                                                                                                                                                                                          Target Specifier
  ----------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  cybox:Network\_Flow     The Network\_Flow\_Object object provides a summary of network traffic, expressed as flows of multiple packets instead of individual packets, without the packet payload data (i.e. the actual data that was uploaded/downloaded to and from the Dest IP to Source IP as included in packet monitoring tools, such as Wireshark).                                                    cybox:NetworkFlowObjectType:
                                                                                                                                                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                                                                                                                               Network Flow Label (Src Socket Address, Dest Socket Address, IP Protocol), Unidirectional Flow Record, Bidrectional Flow Record

  cybox:Network\_Packet   The Network\_Packet object provides the definition of a network packet based on the TCP/IP model/Internet protocol suite. In the TCP/IP stack, “packet” is generally defined as IP header plus payload, but we also include the LinkLayer from the OSI model, which defines the physical network interfaces and routing protocols. The application layer has not yet been defined.   cybox:NetworkPacket:
                                                                                                                                                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                                                                                                                               Link Layer (Physical Interface, Logical Protocols), Internet Layer, Transport Layer

  cybox:Network\_Subnet   The Network\_Subnet object is intended to characterize a generic system network subnet.                                                                                                                                                                                                                                                                                              cybox:NetworkSubnetObjectType:
                                                                                                                                                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                                                                                                                               Name, Description, Number Of IP Addresses, Routes

  cybox:Port              The Port object is intended to characterize networking ports.                                                                                                                                                                                                                                                                                                                        cybox:PortObjectType:
                                                                                                                                                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                                                                                                                               Port Value, Layer4 Protocol

  cybox:Process           The Process object is intended to characterize system processes.                                                                                                                                                                                                                                                                                                                     cybox:ProcessObjectType:
                                                                                                                                                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                                                                                                                               PID, Name, Creation Time, Parent PID, Child PID List, Image Info, Argument List, Environment Variable List, Kernel Time, Post List, Network Connection List, Start Time, Status, Username, User Time, Extracted Features

  cybox:Product           The Product object is intended to characterize software or hardware products.                                                                                                                                                                                                                                                                                                        cybox:ProductObjectType:
                                                                                                                                                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                                                                                                                               Edition, Language, Product, Update, Vendor, Version, Device Details

  cybox:Socket\_Address   The Socket\_Address element is intended to characterize a single network socket address.                                                                                                                                                                                                                                                                                             cybox:SocketAddressObjectType:
                                                                                                                                                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                                                                                                                               IP Address, Hostname, Port

  cybox:System            The System object is intended to characterize computer systems (as a combination of both software and hardware).                                                                                                                                                                                                                                                                     cybox:SystemObjectType:
                                                                                                                                                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                                                                                                                               Available Physical Memory, BIOS Info, Date, Hostname, Local Time, Network Interface List, OS, Processor, Processor Architecture, System Time, Timezone DST, Timezone Standard, Total Physical Memory, Uptime, Username

  cybox:URI               The URI object is intended to characterize Uniform Resource Identifiers (URI’s).                                                                                                                                                                                                                                                                                                     cybox:URIObjectType
                                                                                                                                                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                                                                                                                               Value
  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Table 3‑7. Summary of Supported Targets (Cont.)

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Target Type                    Description                                                                                                                                                                                         Target Specifier
  ------------------------------ --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  cybox:User\_Account            The User\_Account object is intended to characterize generic user accounts.                                                                                                                         cybox:UserAccountObjectType:
                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                     Full Name, Group List, Home Directory, Last Login, Privilege List, Script Path, Username, User Password Age

  cybox:User\_Session            The User\_Session object is intended to characterize user sessions.                                                                                                                                 cybox:UserSessionObjectType:
                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                     Effective Group, Effective Group ID, Effective User, Effective User ID, Login Time, Logout Time

  cybox:Volume                   The Volume object is intended to characterize generic drive volumes.                                                                                                                                cybox:VolumeObjectType:
                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                     Name, Device Path, File System Type, Total Allocation Units, Sectors Per Allocation Unit, Bytes Per Sector, Actual Available Allocation Units, Creation Time, File System Flag List, Serial Number

  cybox:Windows\_Registry\_Key   Windows\_Registry\_Key object characterizes windows registry objects, including Keys and Key/Value pairs. \[Link\](http://msdn.microsoft.com/en-us/library/windows/desktop/ms724871(v=vs.85).asp)   cybox:WindowsRegistryKeyObjectType:
                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                     Key, Hive, Number Values, Values, Modified Time, Creator Username, Handle List, Number Subkeys, Subkeys, Byte Runs

  cybox:Windows\_Service         Windows\_Service object is intended to characterize Windows services. \[Link\](http://msdn.microsoft.com/en-us/library/windows/desktop/ms685141(v=vs.85).aspx)                                      cybox:WindowsServiceObjectType:
                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                     Description List, Display Name, Group Name, Service Name, Service DLL, Service DLL Certificate Issuer, Service DLL Certificate Subject, Service DLL Hashes, Service DLL Signature Description, Startup Command Line, Startup Type, Service Status, Service Type, Started As

  cybox:X509\_Certificate        X509\_Certificate object represents a public key certificate for use in a public key infrastructure.                                                                                                cybox:X509CertificateObjectType:
                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                     Certificate, Raw Certificate, Certificate Signature

  openc2:Command                 The Command object is intended to characterize an OpenC2 command.                                                                                                                                   openc2:CommandObjectType:
                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                     ID

  openc2:Data                    The Data object is intended to characterize the result of information gathering and publishing activities.                                                                                          openc2:DataObjectType:
                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                     Value, Attributes, Search

  openc2:OpenC2                  The OpenC2 object is a subset of the Data object that represents an Actuator’s OpenC2 supported capabilities.                                                                                       openc2:OpenC2ObjectType:
                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                     Value, Attributes, Search
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<span id="_Ref457998644" class="anchor"><span id="_Toc459584796" class="anchor"></span></span>Actuator Vocabulary
-----------------------------------------------------------------------------------------------------------------

An ACTUATOR is the entity that puts command and control into motion or
action. The ACTUATOR executes the ACTION on the TARGET. Implementers of
OpenC2 are encouraged to leverage existing standardized data models for
ACTUATORs (e.g., IETF Security Automation and Continuous Monitoring,
Information Security Continuous Monitoring (ISCM)) however this document
will reference a namespace created by the OpenC2 Forum. Refer to the
following table for a summary of the OpenC2 ACTUATOR Namespaces.<span
id="language_actuator" class="anchor"></span>

<span id="_Toc459584893" class="anchor"></span>Table ‑. Actuator
Namespace

  ------------------------------------------------------------------------------------------------------------------------------------------------------------
  Type        Description                                                                                                          Options
  ----------- -------------------------------------------------------------------------------------------------------------------- ---------------------------
  namespace   Used to uniquely identify a set of names so there is no ambiguity; defines the context in which names are defined.   Choice of:
                                                                                                                                   
                                                                                                                                   -   TBD: e.g., ISCM, SACM
                                                                                                                                   
                                                                                                                                   -   OpenC2
                                                                                                                                   
                                                                                                                                   -   Custom
                                                                                                                                   
                                                                                                                                   
  ------------------------------------------------------------------------------------------------------------------------------------------------------------

ACTUATORs fall into classes (e.g., endpoint device, network,
services/processes, and human). Refer to the following table for a
summary of supported OpenC2 ACTUATORs.

<span id="_Ref459023302" class="anchor"><span id="_Toc459584894"
class="anchor"></span></span>Table ‑. Summary of Supported Actuators

  Actuator Type                        Description              Actuator Specifier
  ------------------------------------ ------------------------ --------------------
  endpoint                             Endpoint Device          
  endpoint.digital-telephone-handset                            
  endpoint.laptop                                               
  endpoint.pos-terminal                                         
  endpoint.printer                                              
  endpoint.sensor                                               
  endpoint.server                                               
  endpoint.smart-meter                                          
  endpoint.smart-phone                                          
  endpoint.tablet                                               
  endpoint.workstation                                          
  network                              Network Platform         
  network.bridge                                                
  network.firewall                                              
  network.gateway                                               
  network.guard                                                 
  network.hips                                                  
  network.hub                                                   
  network.ids                                                   
  network.ips                                                   
  network.modem                                                 
  network.nic                          Network Interface Card   
  network.proxy                                                 
  network.router                                                
  network.security\_manager                                     
  network.sense\_making                                         
  network.sensor                                                
  network.switch                                                

Table 3‑9. Summary of Supported Actuators (Cont.)

  Actuator Type                    Description                  Actuator Specifier
  -------------------------------- ---------------------------- --------------------
  network.vpn                      VPN Concentrator/Appliance   
  network.wap                      Wireless Access Point        
  process                          Services/Processes           
  process.aaa-server                                            
  process.anti-virus-scanner                                    
  process.connection-scanner                                    
  process.directory-service                                     
  process.dns-server                                            
  process.email-service                                         
  process.file-scanner                                          
  process.location-service                                      
  process.network-scanner                                       
  process.remediation-service                                   
  process.reputation-service                                    
  process.sandbox                                               
  process.virtualization-service                                
  process.vulnerability-scanner                                 

Modifier Vocabulary
-------------------

Modifiers provide additional information about the action such as time,
periodicity, duration, and location. Modifiers can denote the when,
where, and how aspects of an action. The modifier can also be used to
convey the need for additional status information about the execution of
an action. Modifiers can be used to indicate whether the actuator should
explicitly acknowledge receipt of the command, respond upon completion
of the execution of the command, or provide some other status
information. The requested status/information will be carried in a
RESPONSE. Refer to Section 4.6.

Modifiers are similar to specifiers in that they can provide additional
context specific details for an action, and are intended to provide
additional details for action/target pairs. Action-specific modifiers
are identified in the sections detailing out each action.

The following table lists the set of universal modifiers that are
applicable to all<span id="language_modifier" class="anchor"></span>
types of actions.

<span id="_Toc459584895" class="anchor"></span>Table ‑. Summary of
Universal Modifiers

  Modifier   Type          Description                                                        Target Applicability
  ---------- ------------- ------------------------------------------------------------------ ----------------------
  delay      duration      Optional. The time to wait before performing the action.           All
  duration   duration      Optional. The period of time that an action is valid.              All
  id         string        The unique identifier for the action.                              All
  response   ack, status   Optional. Indicate the type of response required for the action.   All
  datetime   datetime      Optional. The specific date/time to initiate the action.           All

