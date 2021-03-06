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

OpenC2 is defined at a level of abstraction such that an inter-domain tasking or coordination effort can be described without requiring in depth knowledge of the recipient network’s components, but through the use of specifiers and modifiers, enough detail can be appended to carry out specific tasks on particular devices to support intra-domain command and control.

This level of abstraction permits end to end applicability of OpenC2. As depicted in Figure 2-1, an OpenC2 command is sent to enable coordination or send a high level tasking from the peer or upper tier enclave. An OpenC2 command received by an enclave will trigger events within the enclave to annotate the command with context specific information so that specific devices within the enclave can respond appropriately. This allows the enclave to take advantage of this context-specific knowledge to interpret OpenC2 commands (e.g., inventory of actuators controlled by the enclave, the local security policy, the communication paths and protocols available, and the command structure of the enclave).

Each domain or enclave contextualizes an OpenC2 action for the specific sensors and actuators within its environment so it can further specify the command to reflect the implementations of which it is capable. Context-specific modifiers provide an ability to further specify the action while enabling the set of actions to remain tightly constrained. This minimizes the overhead, permits further contextualization of the OpenC2 commands for specific environments, and thereby enables flexibility and extensibility. 

[](figure_2-1.emf){width="6.5in" height="3.6417410323709536in"}

Figure 2-1. OpenC2 Deployment Environments

For example, an organization may have executed a series of actions to protect against a particular attack that was signaled by an external indicator (such as a STIX message). In order to elicit a consistent response across an organization (whether hierarchical or peer to peer), a complex course of action can be constructed and shared. The use of standardized and unambiguous OpenC2 commands to communicate a responsive action between enterprises/enclaves will be more precise and more quickly actionable than a set of recommended steps within a text document (e.g., flash), which must be parsed, analyzed, interpreted, and executed. Standardizing OpenC2 commands helps to ensure a more uniform response at enterprises/enclaves that reflects enterprise-wide level decisions.

# OpenC2 Language

## Overview

The OpenC2 language is designed at a level of abstraction high enough such that it enables persistence as technologies advance and is implementation agnostic, but detailed enough so that the need for specifiers and modifiers is limited.

## Abstract Syntax

Conceptually, an OpenC2 command has the following form:

```
(
	ACTION (
		type = <ACTION_TYPE>
	),
	TARGET (
		type = <TARGET_TYPE>,
		<target-specifier>
	),
	ACTUATOR (
		type = <ACTUATOR_TYPE>,
		<actuator-specifier>
	),
	MODIFIERS (
		<list-of-modifiers>
	)
)
```

Fields denoted with angle brackets ("&lt;&gt;") are replaced with the appropriate details. Some of the fields are considered optional. The table below describes these fields and whether they are required, optional or ignored in certain situations. Actual implementation approaches will leverage pre-existing conventions and notations such as XML, JSON, or Type-Length-Value delimitation.

The following table contains the description of the fields that can be contained in an OpenC2 command.

Table 3-1. OpenC2 Command Field Descriptions

Field | Description
----- | -----------
ACTION | Required. The task or activity to be performed (i.e., the ‘verb’).
type | Required. The ACTION type is the name of the action.
TARGET | Required for actions, not applicable for responses. The object of the action. The ACTION is performed on the TARGET.
type | Required. The TARGET type will be defined within the context of a namespace.
target-specifier | Optional. The specifier further describes a specific target, a list of targets, or a class of targets.
ACTUATOR | Optional. The subject of the action. The ACTUATOR executes the ACTION on the TARGET.
type | Required. The ACTUATOR type will be defined within the context of a namespace.
actuator-specifier | Optional. The specifier further describes a specific actuator, a list of actuators, or a class of actuators.
MODIFIERS (&lt;list-of-modifiers&gt;) | Optional. Provide additional information about the action such as date/time, periodicity, duration, and location.

There are cases where an ACTION and TARGET are sufficient to complete the command, especially in the case of inter-domain commands where the method or approach to complete or execute the action can be determined within the receiving domain/enclave.

The majority of commands within an enclave will have an ACTION, TARGET and ACTUATOR. Inclusion of the ACTUATOR provides additional context for the command as a whole and enables efficiency.

Specifiers for TARGETs and ACTUATORs are optional and can be used to provide context specific information that could be used to reflect the local environment, policies, and operational conditions within an enterprise/enclave. Specifiers can call out a specific target/actuator, a list of targets/actuators, or a class of targets/actuators.

Modifiers to the ACTION are optional and are used to provide effect based context to the ACTION. Modifiers are further discussed in Section 3.2.5.

Table 3-2 illustrates the use of specifiers and modifiers to extend the range of OpenC2 commands to cover the higher level ‘strategic’ commands to the unambiguous enclave-specific use case. This provides greater flexibility to the language and allows the OpenC2 actions to be further contextualized for the mission environment. The table below provides some examples of the different levels of specificity achievable in an OpenC2 command.

Table 3-2. OpenC2 Syntax Flexibility Examples

Description | Action | Target<hr>Target-Specifier | Actuator<hr>Actuator-Specifier | Modifier
----------- | ------ | -------------------------- | ------------------------------ | --------
Block traffic to/from specific IP address \[effects-based, no actuator specified\]; suitable for inter-domain coordination | DENY | Network Connection<hr>Source and/or Destination IP Address | |
Block traffic at all network devices \[specify actuator class\]; suitable for inter-domain coordination or as a command to an orchestration engine which further contextualizes to the enclave’s environment | DENY | Network Connection<hr>Source and/or Destination IP Address | Network (any devices)   
Block traffic at network routers \[specify type of network device actuator\]; suitable within an enclave | DENY | Network Connection<hr>Source and/or Destination IP Address | Network.router<hr>(optional) |
Block traffic at specific network router; \[specify identity of network router\]; suitable within an enclave | DENY | Network Connection<hr>Source and/or Destination IP Address | Network.router<hr>Router identity | 
Block access to bad external IP by null routing; \[specify method of performing action\]; suitable within an enclave | DENY | Network Connection<hr>Source and/or Destination IP Address | Network.router<hr>(optional) | Method= blackhole


### Action

All OpenC2 commands start with an ACTION which indicates the type of command to perform such as gather and convey information, control activities and devices, and control permissions and access. The range of options and potential impact on the information system associated with a particular ACTION is a function of the ACTUATOR. For cases that involve multiple options for an ACTION, modifiers are used.

Refer to Section 3.3 for the list of ACTIONs and their definitions and usage.

### Target

The TARGET is the object of the ACTION (or alternatively, the ACTION is performed on the TARGET). Targets include objects such as network connections, URLs, hashes, IP addresses, files, processes, and domains.

There will be only one TARGET type per OpenC2 command. By design, OpenC2 will support any data model, but for illustrative purposes OpenC2 TARGETs will reference CybOX objects to the greatest extent practical in this document. Data models will be identified by a namespace before the TARGET type name. Section 3.4 contains a compiled list and definitions of the TARGETs that support the OpenC2 language.

### Actuator

An ACTUATOR[^1] is the entity that puts command and control into motion or action. The ACTUATOR is the subject of the ACTION which performs the ACTION on the TARGET. There are varying levels of abstraction and functionality for an ACTUATOR ranging from a specific sensor to an entire system or even system of systems.

There will be only one ACTUATOR type per OpenC2 command. OpenC2 will leverage existing data models to the greatest extent practical (e.g., the Secure Automation and Configuration Management (SACM) working group, ISCM taxonomy). Section 3.5 contains a compiled list of actuators with definitions used to support OpenC2.

### Specifiers

“Specifiers” are used to identify specific individual or groups of targets or actuators. Table 3-3 illustrates how the commands are appended with specifiers as context specific details become available.

Table 3-3. Example Usage of Specifiers

Description | Action | Target<hr>Target-Specifier | Actuator<hr>Actuator-Specifier | Modifier
----------- | ------ | -------------------------- | ------------------------------ | --------
Block malicious URL | DENY | URI/URL<hr>Value Condition = Equals | | 
Quarantine Artifact with particular byte string | QUARANTINE | Artifact<hr>Condition = Contains | | 
Block access to external IP address by null routing at specific network routers | DENY | Network Connection<hr>Condition = Contains | Network router<hr>Manufacturer, Model, Serial Number Value = 123

### Modifiers

“Modifiers” provide additional information about the action such as time, periodicity, duration, and location. Modifiers can denote the when, where, and how aspects of an action. Modifiers are similar to specifiers in that they can provide additional context specific details, and are intended to provide additional details for action/actuator pairs.

When present, modifiers are always associated with a specific action, however, some modifiers can generally be applied to more than one action or to all OpenC2 actions. A modifier is said to be “actuator-specific”, “action-specific”, or “universal” depending on the applicability of the modifier within the language.

The modifier can also be used to convey the need for additional status information about the execution of an action. Modifiers can be used to indicate whether the actuator should explicitly acknowledge receipt of the command, respond upon completion of the execution of the command, or provide some other status information. The requested status/information will be carried in a RESPONSE. Refer to Section 4.6.

Table 3-4. Example Usage of Modifiers

Description | Action | Target<hr>Target-Specifier | Actuator<hr>Actuator-Specifier | Modifier
----------- | ------ | -------------------------- | ------------------------------ | --------
Shutdown a system, immediate | STOP | Device<hr>Device Object Type | endpoint<hr>(optional) | method = immediate
Start Process with Delay | START | Process<hr>Process Object Type | endpoint<hr>(optional) | Delay = duration
Quarantine a device | CONTAIN | Device<hr>Device Object Type | network<hr>(optional) | where (network segment, vlan)
Block access to suspicious external IP address by redirecting external DNS queries to an internal DNS server | DENY | Network Connection<hr>Network Connection Object Type | DNS Server | method = sinkhole

## Actions

This section defines the set of OpenC2 actions grouped by their general activity. The following table summarizes the definition of the OpenC2 actions. Subsequent sections will identify the appropriate targets for each action and the appropriate actuators for the action target pair.

### Actions that Gather and Convey Information

These actions are used to gather information needed to further determine courses of action or assess the effectiveness of courses of action. These actions can be used to support data enrichment use cases and maintain situational awareness. These actions typically do not impact the state of the target and are normally not detectable by external observers.

### Actions that Control Permissions

These actions are used to control permissions and accesses. The permissions and accesses can be for person or non-person entities.

### Actions that Control Activities/Devices

These actions are used to control the state or the activity of a system, a process, a connection, a host, or a device (e.g., endpoint, sensor, actuator). The actions are used to adjust configurations, set and update parameters, and modify attributes.

### Sensor-Related Actions

These actions are used to control the activities of a sensor in terms of how to collect and provide the sensor data.

### Effects-Based Actions

Effects-based actions are at a higher level of abstraction and focus on the desired impact rather than a command to execute specific task(s) within an enclave. The benefit of including effects-based actions is that it permits a higher level or peer enclave to coordinate actions, while still permitting a local enclave to optimize its workflow for its specific environment in order to achieve the desired result.

Implementation of an effects-based action requires that the recipient enclave has a decision making capability because an effects-based action permits multiple possible responses.

### Response and Alert

RESPONSE is used to provide data requested as a result of an action. The RESPONSE message will contain the requested data and have a reference to the action that initiated the response. ALERT is used to signal the occurrence of an event or error. It is an unsolicited message that does not reference a previously issued action.