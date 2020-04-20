
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

**Each Patient resource must support:**

1. A patient name
2. A gender
3. Contact detail (e.g. a telephone number or an email address)
4. A birth date
5. An address
6. A communication language
7. An ethnicity

**Profile specific implementation guidance:**

When the Patient profile is being used with an NHS Number in the patient.identifier element, the verification status of the NHS Number MUST also be carried.


### Example ###

- [UK Core AllergyIntolerance Example](UKCore-AllergyIntolerance-Example.html)

