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