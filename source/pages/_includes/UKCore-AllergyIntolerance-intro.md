
{% assign id = {{page.id}} %}

{{site.data.structuredefinitions.[id].description}}

<!-- end TOC -->
### Profile Purpose ###

This profile allows a record of a clinical assessment of an allergy or intolerance; a propensity, or a potential risk to an individual, to have an adverse reaction on future exposure to the specified substance, or class of substance.

Where a propensity is identified, to record information or evidence about a reaction event that is characterised by any harmful or undesirable physiological response that is specific to the individual and triggered by exposure of an individual to the identified substance or class of substance.

Substances include but are not limited to a therapeutic substance administered correctly at an appropriate dosage for the individual; food; material derived from plants or animals; or venom from insect stings.


### Example Usage Scenarios ###

The following are example usage scenarios for the UK Core AllergyIntolerance profile:

- Query for Patient allergy information 
- Exchange Patient allergy information within a FHIR document or message.

### Profile Minimum Viable Content (Mandatory and Must Support Data Elements) ###

The following data-elements are mandatory (i.e data MUST be present) or must be supported if the data is present in the sending system (Must Support definition). They are presented below in a simple human-readable explanation. Profile specific guidance and examples are provided as well. The formal Profile Definition below provides the formal summary, definitions, and terminology requirements.

**Each AllergyIntolerance resource must have:**

1. A status of the allergy*
2. A code which tells you what the patient is allergic to
3. A patient

*The status element has the following constraints: SHALL be present if verification status is not entered-in-error and SHALL NOT be present if verification Status is entered-in-error.

**Each AllergyIntolerance resource must support:**

1. The lastOccurrence
2. The reaction
3. The reaction.substance
4. The reaction.manifestation
5. The reaction.onset


**Profile specific implementation guidance:**

**Allergic Response or Adverse Reaction Event and Propensity**
1. Recording an Allergic Response or Adverse Reaction to an item of medication or a substance
2. Recording a clinician’s opinion about future risk of (or propensity to) an Allergy or other Adverse Reaction if the patient is exposed to a substance.

**Resources Used for Profile Design**
Allergies should be constructed as a list of allergies using the List resource which acts as a container for the allergies. The following is an example of the main elements expected to be used:

**List resource**
identifier - uniquely identifies this list of allergies. Normally using a UUID.
code - the type of list (for example SNOMED CT concept for "ended allergy")
status - should be "current"
mode - should be "snapshot"
subject - a reference to the patient whose allergy list this is
Encounter - a reference to the context in which the list was created (the inpatient stay for example)
date - when the list was prepared
source - who or what defined the list
entry - a reference to the allergyIntolerance Resource entry

**AllergyIntolerance resource**
This Resource details the actual allergy or adverse reaction. The following is an example of the main elements expected to be used:

identifier - uniquely identifies this allergy or adverse reaction. Normallynmusing a (UUID)
clinicalStatus - should always be active
category - whether the allergy or adverse reaction is to food, medication etc
criticality - low, high, unable-to-assess etc.
code - identifies the allergy expected to be SNOMED CT.
patient - a reference to the patient
recordedDate - date first version of the resource instance was recorded
asserter - the source of the information about the allergy (a refernce to patient, related person, practitioner practitionerRole)
lastOccurrence - when it last occurred if known
reaction - details of the reaction
substance - Specific substance or pharmaceutical product considered to be responsible for event

**Guidance of the use of SNOMED CT for causative agents is as follows**

Everything from the Product (373873005\|Pharmaceutical/biologic product(product)\|) hierarchy and everything from the substance (105590001\|substance\|) hierarchy.

For pre-coordinated allergy terms use a degrade code - see below:

Degrade codes (196461000000101\|transfer-degraded drug allergy(record artifact)\|&196471000000108\|transfer-degraded non-drug allergy(record artifact)\|) can be used if only a text representation of the allergy is known & pre coordinated allergy codes (for example 213020009\|egg protein allergy\|).

As a SNOMED CT expression

(<<105590001 \|Substance\|
(OR <<373873005 \|Pharmaceutical / biologic product\|
(OR <<716186003 \|No known allergy\|
(OR 196461000000101 \|Transfer-degraded drug allergy\|
(OR 196471000000108 \|Transfer-degraded non-drug allergy\|)
                                                                     
(^999000801000001108 \|Allergy Archetypes Drug Groups simple reference set\|
OR ^999000631000001100\|National Health Service dictionary of medicines and devices trade family simple reference set\|
OR ^999000641000001107\|National Health Service dictionary of medicines and devices trade family group simple reference set\|
OR ^999000771000001105\|National Health Service dictionary of medicines and devices combination drug virtual therapeutic moiety simple reference set\|
OR ^999000561000001109\|National Health Service dictionary of medicines and devices virtual medicinal product simple reference set\|
OR ^999000541000001108\|National Health Service dictionary of medicines and devices actual medicinal product simple reference set\|
OR ^999000791000001109\|NHS dm+d (dictionary of medicines and devices) ingredient simple reference set\|
OR <<716186003 \|No known allergy\|
OR 196461000000101 \|Transfer-degraded drug allergy\|
OR 196471000000108 \|Transfer-degraded non-drug allergy\|)

**Severity**
valueSet applicable for severity is as folllows:

Mild [The reaction was mild.][SNOMED-CT::255604002] (Mild (qualifiervalue))
Moderate [The reaction was moderate.][SNOMED-CT::6736007] (Moderate (severity modifier) (qualifier value))
Severe [The reaction was severe.][SNOMED-CT::24484000] (Severe (severity modifier) (qualifier value))
Life threatening [The reaction was life-threatening.][SNOMED-CT::442452003] (Life threatening severity (qualifier value))
Fatal [The reaction was fatal.][SNOMED-CT::399166001] (Fatal (qualifier value))
  
Important note: reaction.severity is a required terminology binding in FHIR with values (mild \| moderate \| severe)
"Life threatening" and "Fatal" cannot currently be mapped.

As SNOMED CT Expression but see note above on not using 'life threatening' or 'Fatal':

(255604002 \|Mild\|
OR 6736007 \|Moderate\|
OR 24484000 \|Severe|
OR 399166001 |Fatal|
442452003 |Life threatening severity|)
Certainty
PRSB valueSet applicable for certainty is as follows:

Unlikely - [The reaction is thought unlikely to have been caused by the agent.][SNOMED-CT::1491118016]
Likely - [The reaction is thought likely to have been caused by the agent.][SNOMED-CT::5961011]
Certain - [The agent is thought to be certain to have caused the reaction but this has not been confirmed by challenge testing.][SNOMED-CT::255545003] (Definite(qualifier value))
Confirmed by challenge testing - [The reaction to the agent has been confirmed by challenge testing or other concrete evidence.][SNOMED-CT::410605003] Confirmed present (qualifier value))
The FHIR element AllergyIntolerance.verificationStatus is mandatory and the ValueSet verificationStatus has a required terminology binding and uses values (unconfirmed | confirmed | refuted | entered-in-error)
the values ( refuted | entered-in-error) MUST NOT be used for Transfer of Care Documents.
The values Certain and Confirmed by Challenge = FHIR value "confirmed". The values Likely and Unlikely = FHIR value "unconfirmed". If AllergyIntolerance.verificationStatus is not known, then set to FHIR value "unconfirmed". If extra information about certainty is known, this should reported as a note. The ValueSet guidance for implementers is to default to FHIR value of "unconfirmed" for all Transfer of Care Document types.

As SNOMED Expressions (Note that 1491118016 |unlikely| and 5961011 |likely|  above are description identifiers for synonyms of concepts below)
(385434005 |Improbable diagnosis|
OR 2931005 |Probable diagnosis|
OR 255545003 |Definite|
OR 410605003 |Confirmed present|)
Reaction Details
AllergyIntolerance.reaction.manifestation is a sub-element of AllergyIntolerance.reaction, which is optional (0..*) - so if there is no manifestation known, then don't send a AllergyIntolerance.reaction FHIR element.

Anything from the clinical finding hierarchy ( 404684003 | clinical finding (finding) | ). Plus the HL7 nullFlavors documented here.
The AllergyIntolerance.reaction.manifestation CodeableConcept ValueSet is Extensible. If you have a code, then goes in Manifestation CodeableConcept. Where no code is known (but a manifestation needs to be recorded) then populate the AllergyIntolerance.reaction.manifestation CodeableConcept with the value from the HL7 FHIR NullFlavor ValueSet of "UNC" - "un-encoded" and populate text of manifestation in AllergyIntolerance.reaction.description. When patient is asked about reaction, but doesn't know the reaction then populate the AllergyIntolerance.reaction.manifestation CodeableConcept with the value from the HL7 FHIR NullFlavor ValueSet of "ASKU" - "asked but unknown". When the reaction details cannot be determined/verified, then then populate the AllergyIntolerance.reaction.manifestation CodeableConcept with the value from the HL7 FHIR NullFlavor ValueSet of "NI" - "No Information".

Type of Reaction
For the AllergyIntolerance.type FHIR has a required terminology binding using the values of (allergy | intolerance) - as this is a required ValueSet these values must be used.
The values "Adverse Reaction" and "Not Known" are currently not supported However adverse reaction is closer to intolerance by the FHIR definition. An absence of AllergyIntollerance.type implies Not Known. Advice from FHIR patient care WGM is that adverse reaction should recorded under AllergyIntolerance.reaction.description.

Date first experienced
This is mapped to the "higher level" of the FHIR element AllergyIntolerance.onset[x] estimated or actual date, date-time, or age when allergy or intolerance was identified. The definition of AllergyIntolerance.onset[x] is: Record of the date and/or time of the onset of the Reaction. The reason for mapping to higher onset is that this is the date when the reaction was experienced by the patient for the first time, the onset under reaction could be multiple.

Clinicalstatus
The FHIR element AllergyIntolerance.clinicalStatus for Transfer of Care Documents should if present be set to "active".

AllergyIntolerance.category
The category of the identified substance, this may be of use to receivers and can be populated with a value from the FHIR required ValueSet AllergyIntoleranceCategory.

AllergyIntolerance.criticality
This FHIR element may be used to express life threatening (high) in conjunction with AllergyIntolerance.severity FHIR element.

AllergyIntolerance.lastOccurrence
This FHIR element may be used to represents the date and/or time of the last known occurrence of a reaction event. May be populated if known.

AllergyIntolerance.reaction.substance
This FHIR element SHOULD NOT populated for Transfer of Care Documents.

AllergyIntolerance.reaction.onset
This FHIR element may be populated if known.

AllergyIntolerance.exposureroute
This FHIR element may be populated with a value from dm+d routes refset. As a SNOMED Expression

^999000051000001100 |ePrescribing route of administration simple reference set|
AllergyIntolerance.reaction.note
This FHIR element MUST NOT be used and the information must where available, always be carried in the Composition.section.text FHIR element.

AllergyInterolerance.ReasonEnded
This FHIR element must be supported and is to allow for clinical content for "RESOLVED" status allergies. This is where AllergyIntolerance.clinicalStatus FHIR element contains the value of "resolved" - FHIR definition is "A reaction to the identified substance has been clinically reassessed by testing or re-exposure and considered to be resolved."

Allergy Snapshot
The allergies list is a “Snapshot” of the known allergies at a point in time (for example on discharge from hospital). It is not a master list of the patient’s allergies. Other lists of allergies for the patient may exist on other systems.

How the Allergy List is Constructed
The allergy record is constructed as a single list for Transfer of Care Documents. The diagram below shows the Resources used and relationships between the Resources.



Handling Negated Codes e.g. “No Known Drug allergy”
When there are negated codes which are explicitly recorded by the clinician then:

FHIR element Text.Narrative MUST match text of code e.g. “No known drug allergies”, PRSB guidance to be revised.
List is NOT empty therefore the FHIR element List.EmptyReason not needed.
Place negated code in the FHIR element AllergyIntolerance.code.
The negated codes to use are:

716186003 - No known allergy
409137002 - No known drug allergy
429625007 - No known food allergy
Handling an EMPTY Allergy list (no allergies recorded in EOR)
Option 1 : Fhir element Text.Narrative = "Information not available" this is the PRSB preferred option.

The FHIR element List.EmptyReason a code from the ValueSet Care Connect List Empty Reason Code which is the code "no-content-recorded".

Option 2 : FHIR element Text.Narrative = displayName of the code in AllergyIntolerance.code and include List Resource and AllergyIntolerance Resource. The FHIR element AllergyIntolerance.code contains a SNOMED CT concept for example "1631000175102:Patient not asked" . The List Resource is not empty and FHIR element List.EmptyReason MUST NOT be populated.




### Example ###

- [UK Core AllergyIntolerance Example](UKCore-AllergyIntolerance-Example.html)
- [UK Core Allergy List Example](UKCore-Allergy-List-Example.html)

