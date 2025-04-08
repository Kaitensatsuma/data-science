# Researcher Data Browser Cohort Definition

Description: The cohort consists of all real-world individuals who are
participating in the Research Registry (excluding reporters), ALLFTD study,
Contact Registry, and/or Insights survey. The cohort includes exactly one record
for each real-world individual.

## Unit Of Analysis

The unit of analysis for the researcher data browser is a single, unique,
real-world individual. Individuals must be included in the data browser dataset
exactly once if they meet ANY of these criteria, no matter how many of the
criteria they meet:

- The individual is represented by a diagnosed person profile in the Research
  Registry, and the profile is not a migrated profile, and the profile has
  consented to participate in research, as indicated by any of these status
  codes:

  - `enrollment-consented`
  - `enrollment-reporter-invited`
  - `enrollment-enrolled`
  - `enrolled`
  - `withdrawn`
  - `deceased`

- The individual is represented by a diagnosed person profile in the research
  Registry, and the profile is a migrated profile, regardless of consent status

- The individual is represented by a LAR profile in the Research Registry, and
  the profile is not a migrated profile, and the profile has consented to
  participate in research, as indicated by any of these status codes:

  - `enrollment-consented`
  - `enrollment-reporter-invited` !! TODO verifying
  - `enrollment-enrolled` !! TODO verifying
  - `enrolled`
  - `withdrawn`
  - `deceased`

- The individual is represented by a LAR profile in the Research Registry, and
  the profile is a migrated profile, regardless of consent status

- The individual is represented by at least one biological family member and/or
  caregiver profile in the Research Registry, and the profile has consented to
  participate in research, as indicated by any of these status codes:

  - `enrollment-consented`
  - `enrollment-reporter-invited` !! TODO verifying
  - `enrollment-enrolled` !! TODO verifying
  - `enrolled`
  - `withdrawn`
  - `deceased`

- The individual is represented by a Contact Registry profile, and the account
  holder has verified their eligibility to participate in the Contact Registry,
  as indicated by any of these status codes:

  - `enrollment-eligible`
  - `enrollment-interest-1`
  - `enrollment-interest-2`
  - `enrollment-interest-3`
  - `enrollment-interest-4`
  - `about-registry`
  - `enrollment-confirmed`
  - `enrollment-accepted` !! TODO verifying
  - `enrollment-role-not-eligible`
  - `enrollment-role-eligible`
  - `enrolled`
  - `enrolled-inactive`
  - `withdrawn`
  - `deceased`

- The individual is represented by an ALLFTD profile, with any enrollment
  status, and with any verification status for the account holder's email

- The individual is found in the Insights survey.

Each row in the dataset represents a composite of all data stored in the
Registry across all profiles that represent the individual.

The dataset must apply the following filters on participants when creating
individuals from the study model:

- At least one profile representing the individual and meeting the criteria
  above must NOT be flagged as suspicious.

Additional notes:

- If the individual is only represented by migrated profiles and/or a single
  ALLFTD profile, then the account holder's email verification status should be
  ignored. However, if the individual is represented by at least one research
  Registry and/or contact profile that was not migrated, then the account
  holder's email must be verified.

### Definitions

**Not-null**: The term "not-null" used through this document should be
interpreted to exclude any missing, empty, blank, or NULL value, as well as any
empty strings. Empty strings are defined as strings of length 0, after the value
has been trimmed of any leading and/or trailing spaces, regardless of whether
the string is single, double, or triple quoted.

**TRUE**: The term "TRUE" used in this document means any of the following
values: `True`, `TRUE`, `true`, and `1`. It does not mean any other values, even
if they would evaluate to true in a given framework.

### Legally Authorized Representatives (LARs)

The person who is the LAR for someone else is only counted as an additional
individual if they are also represented by a caregiver, biological family
member, and/or ALLFTD profile. Otherwise, only the diagnosed person the LAR
represents is counted.

For example, an account holder with only a contact profile who then enrolls
another person as that person's LAR should only be counted once, unless the
account holder also creates another enrollment for him/herself as a caregiver or
biological family member.

## Estimation of Cohort Size

The total size of the cohort should equal the result of this formula:

1.  Start by counting every Research Registry participant represented by a LAR
    (i.e., each subject-LAR pair), where the participant is NOT flagged as
    suspicious

2.  Add the number of account holders meeting all these criteria:

    - The account has at least 1 diagnosed individual and/or caregiver and/or
      biological family member profiles, where the profile is NOT flagged as
      suspicious, and

    - The account has no LAR profiles.

3.  Add the number of account holders with only a contact profile, where the
    contact profile is NOT flagged as suspicious

4.  Add the number of account holders with only an ALLFTD profile, where the
    ALLFTD profile is NOT flagged as suspicious

5.  Add the number of Insights survey records

## Data Summaries

Like all data elements, the goal is for the Data Summaries to reflect an accurate count of the eligible records in the above described cohort. Unlike the data elements the summaries by definition will be one dimensional in some cases and necessarily exclude certain participant types who may not be asked to provide a response to a certain question or that question not having been available in migrated, legacy or insights data.

### Participant Demographics: Disorder Type

  Description: For each option in the participant profile conditions
  multiselect list, indicates whether the individual self-reported carrying the specific FTD diagnosis.


  Variable Type: Enumeration

  Source: Participant

  Default Value: No

  Eligibility:<br> Any account with either a Research and/or ALLFTD profile that has indicated a condition or has an associated diagnosis.<br>
  Contact only accounts are not included<br>
  Account inclusion is **not** exclusive to one condition, an account can be represented in more than one of the bars.<br>
  For a Research or Contact account to be included it has to have at **least** one profile that has completed enrollment `enrolled, enrolled-consented, withdrawn or deceased` <br>
  For an ALLFTD Only account they must be either `enrolled-site` or `allftd-enrolled` and their associated contact account can be in `any status`

  Total #: Disabled

  Numerator and Denominator on Hover: Enabled

  **There is a probability that the condition bars will add up to more than 100% and this is expected**

  Alternative Values:

  1.  Yes

      Condition: Any of the following criteria are met:

      - For each option, there exists at least one profile representing the individual where the condition is selected, or
      - For each option, there exists an Insights survey record representing the individual where the condition is selected in the `diagnosis` variable and the variable is harmonized.


  2.  Censored

      Condition: The individual is only represented by an ALLFTD profile.

  Data Harmonization:

  - `diagnosis` should be mapped to values from the participant profile using
    this map (from --> to):

    - `bvFTD — behavioral variant FTD, frontotemporal dementia, or FTD dementia`
      --> `Behavioral variant (bvFTD)`
    - `PSP — progressive supranuclear palsy or Richardson's syndrome` -->
      `Progressive supranuclear palsy (PSP)`
    - `FTD with ALS (amyotrophic lateral sclerosis) or FTD with MND (motor neuron disease)`
      --> `FTD with motor neuron disease (also called FTD-ALS)`
    - `PPA — primary progressive aphasia (no subtype given)` -->
      `Aphasia / Primary Progressive Aphasia (PPA)`
    - `svPPA — semantic variant aphasia or semantic dementia` -->
      `Semantic variant (svPPA)`
    - `lvPPA — logopenic aphasia or logopenia` --> `Logopenic variant (lvPPA)`
    - `nfvPPA - nonfluent agrammatic variant aphasia or progressive nonfluent aphasia (PNFA)`
      --> `Non-fluent / Agrammatic variant (nfvPPA, AKA PPA-G)`
    - `CBD or CBS — corticobasal degeneration or corticobasal syndrome` -->
      `Corticobasal degeneration (CBD) or corticobasal syndrome (CBS)`
    - `Pick’s disease` --> `Pick's disease`
    - `I’m not sure` --> `Unsure of FTD type`

Disorder Types:

  - Semantic variant (svPPA)

    Any of the following criteria are met:
    - Existence of at least one profile that is non-ALLFTD which indicates PSP as a condition
    - `diagnosis` indicates `svPPA — semantic variant aphasia or semantic dementia`


  - Richardson syndrome

    Any of the following criteria are met:
    - Existence of at least one profile that is non-ALLFTD which indicates PSP as a condition


  - Progressive supranuclear palsy (PSP)

    Any of the following criteria are met:
      - Existence of at least one profile that is non-ALLFTD which indicates PSP as a condition
      - `diagnosis` indicates `PSP — progressive supranuclear palsy or Richardson's syndrome`


  - Pick's disease

    Any of the following criteria are met:
    - Existence of at least one profile that is non-ALLFTD which indicates `Pick's disease` as a condition
    - `diagnosis` indicates `Pick’s disease`


  - Nonfluent/Agrammatic variant (nfvPPA, also known as PPA-G)

    Any of the following criteria are met:
    - Existence of at least one profile that is non-ALLFTD which indicates `Nonfluent/Agrammatic variant (nfvPPA, also known as PPA-G)` as a condition
    - `diagnosis` indicates `nfvPPA - nonfluent agrammatic variant aphasia or progressive nonfluent aphasia (PNFA)`


  - Logopenic variant (lvPPA)

    Any of the following criteria are met:
    - Existence of at least one profile that is non-ALLFTD which indicates `Logopenic variant (lvPPA)` as a condition
    - `diagnosis` indicates `lvPPA — logopenic aphasia or logopenia`


  - FTD with motor neuron disease (also called FTD-ALS)

    Any of the following criteria are met:
    - Existence of at least one profile that is non-ALLFTD which indicates `FTD with motor neuron disease (also called FTD-ALS)` as a condition
    - `diagnosis` indicates `FTD with ALS (amyotrophic lateral sclerosis) or FTD with MND (motor neuron disease)`


  - FTD suspected but no formal diagnosis

    Any of the following criteria are met:
    - Existence of at least one profile that is non-ALLFTD which indicates `FTD suspected but no formal diagnosis` as a condition


  - Frontotemporal Dementia (FTD)

    Any of the following criteria are met:
    - Existence of at least one profile that is non-ALLFTD which indicates `Frontotemporal Dementia (FTD)` as a condition


  - Corticobasal degeneration (CBD) or corticobasal syndrome (CBS)

    Any of the following criteria are met:
    - Existence of at least one profile that is non-ALLFTD which indicates `Corticobasal degeneration (CBD) or corticobasal syndrome (CBS)` as a condition
    - `diagnosis` indicates `CBD or CBS — corticobasal degeneration or corticobasal syndrome`


  - Behavioral variant (bvFTD)

    Any of the following criteria are met:
    - Existence of at least one profile that is non-ALLFTD which indicates `Behavioral variant (bvFTD)` as a condition
    - `diagnosis` indicates `bvFTD — behavioral variant FTD, frontotemporal dementia, or FTD dementia`


  - Aphasia/Primary Progressive Aphasia (PPA)

    Any of the following criteria are met:
    - Existence of at least one profile that is non-ALLFTD which indicates `Aphasia/Primary Progressive Aphasia (PPA)` as a condition
    - `diagnosis` indicates `PPA — primary progressive aphasia (no subtype given)`


  - Unsure of FTD Type

    Any of the following criteria are met:
    - Existence of at least one profile that is non-ALLFTD which indicates `Unsure of FTD Type` as a condition
    - `diagnosis` indicates `I’m not sure`


  - Censored

    The following criteria is met:
    - The individual is only represented by an ALLFTD Profile that does not have any Research or contact profiles.


  4. Additional Notes:

  - This element will be broken out into multiple yes/no data elements in the researcher data browser, one for each condition in the multiselect list.
  - For the denominator this should include all participants who have provided disorder information including "Nos" and "Censored" values.

### Participant Demographics: Role within the Research Study

  Description: Indicates what Role within the Research Study a participant represents and their relationship regarding the disorder and participant

  Variable Type: Enumeration

  Source: Participant

  Default Value: No

  Alternative Values:

  1.  Person Diagnosed

      Condition: Any of the following criteria are met:

      - There exists a diagnosed person profile representing this individual, or
      - The individual is represented by an Insights survey record, and the value of the `i_am_diagnosed` variable is TRUE.


  2.  Legally Authorized Representative

      Condition: Any of the following criteria are met:

      - There exists a Legally Authorized Representative profile representing this individual.
        - Can be counted more than once for each account in this visualization, but will need to take into account elsewhere when we are concerned about "How many points of contact are they" at the end of the day.


  3.  Current Caregiver or Carepartner

      Condition: Any of the following criteria are met:

      - There exists a Caregiver profile representing this individual where the person being cared for is not marked deceased or
      - The individual is represented by an Insights survey record, and the following values are All true:
        - The variable `i_am_caregiver` = '1' And
        - The `caregiverstatus` variable is **either**
          - "I am currently a primary caregiver for someone with FTD" **or**
          - "I am currently a secondary caregiver for someone with FTD (involved in care but not the primary caregiver)""


  4.  Former Caregiver or Carepartner

      Condition: Any of the following criteria are met:

      - There exists a Caregiver profile representing this individual where the person being cared for is marked deceased or
      - The individual is represented by an Insights survey record and the following statements are true:
        - The variable `i_am_caregiver` = '1' And
        - The `caregiverstatus` variable is "I was a primary or secondary caregiver for someone with FTD who is now deceased"


  5.  Family Member or Biological Relative

      Condition: Any of the following criteria are met:

      - There exists a Biological Family Member profile representing this individual or
      - The individual is represented by an Insights survey record, and the value of the `i_am_relative` variable is '1'.

  6.  Reporter

      Condition: All of the following criteria are met:

      - There exists a Reporter profile associated with this individual
        - Has the reporter responded to their invitation (created an log-in account check with Shreyas about status of when a Reporter can get to their dashboard)
        - Can be counted more than once but take into account elsewhere when we are concerned about "How many points of contact are they" at the end of the day.

  7.  Censored

      Condition: The individual is only represented by an ALLFTD profile
      -   For an ALLFTD Only account they must be either `enrolled-site` or `allftd-enrolled` and their associated contact account can be in `any status`

### Participant Demographics: Participant breakdown by protocol (%)

  Description: The participant representation in the FTDDR amongst the
  different protocols they are part of.

  Variable Type: Enumeration

  Source: Account

  Default Value: No

  1. Contact Only

    Condition: One of the following criteria is met:
      - If the account is not migrated and the **only**
        profile on the account is the Contact profile
        - `enrollment-eligible`
        - `enrollment-interest-1`
        - `enrollment-interest-2`
        - `enrollment-interest-3`
        - `enrollment-interest-4`
        - `about-registry`
        - `enrollment-confirmed`
        - `enrolled`
        - `withdrawn`
        - `deceased`
        - but **not** `enrolled-inactive`
      - If the account is migrated and the **only** profile
        on the account, regardless of whether it is in Pending status
        or not, is the Contact profile

  2. Research Only

      Condition: One of the following criteria is met:
        - If the account is not migrated and the **only**
        profiles on the account are in the Research Study Arm
          - `enrolled`, `enrolled-consented`, `withdrawn` or `deceased` profiles are okay as
          long as **one** of the Research Protocol profiles can be considered consented
        - If the account is migrated and there are **any** Research
        Study Arm profiles including Reporter - that has accepted their invite into the application


  3.  ALLFTD Only

      Condition: One of the following criteria is met:
      - If the account is not migrated and there exists an **active**
      ALLFTD Study Arm profile **AND** no consented Research Study Arm profiles
        - If there is an ALLFTD profile then it doesn't matter if a contact profile exists
      - If the account is migrated and there exists an ALLFTD Study Arm profile **AND** no associated Research Study Arm profiles
        - If there is a Contact profile then it doesn't matter if a contact profile exists


  4. Research + ALLFTD
    Condition: The following criteria are met:
    - If the account is not migrated and there exists **both** an active
    ALLFTD Study Arm profile **AND** an active Research Study Arm profile
    - If the account is migrated and there exists **both** an ALLFTD
    Study Arm profile **AND** a Research Study Arm profile
      - Report Accounts are included as long as they have activated in the system
      - LAR profiles are included in Research as well


  5. Additional Notes and Questions:
     - Previously this visualization was profile based but contained a combined Research + ALLFTD protocol overlap slice which did not seem to accurately represent the actual data available across protocol types.
     After the update to the Data Elements that might be the better place for Researchers to look for that overlap
     - How should Insights data be handled for this visualization, or should
     it be left out entirely? I think it's entirely research - do we want to include it in Research
      - **Not really "Participants" in the normal sense, should not be included in this count**
     - Should reporters be included in Research? They are not used in the Data Browser for calculations currently.
      - Yes but only once we have confirmed they have completed enrollment and not in a pending-invite status.
     - When you withdraw a Research account but **do not** activate a Contact account should you be counted as Research (this is our expectation based on data browser)

### % of tasks completed since May 2024
#### Medical History
Description: Indicates if the individual has provided survey data on
comorbidities in the **completed** Medical History section of Survey 1

Variable Type: Enumeration

Source: Survey 1 task assignments

Denominator: All Assigned Survey 1 survey assignments complete and incomplete

Numerator:

Default Value: No

Alternative Values:

1.  Yes

    Condition: The survey task included in the above denominator is in a **completed** state amd has at least one **non-null* response to the following questions:


    - Survey 1 (Diagnosed Person):

      - `mh_1`
      - `mh_3`

    - Survey 1 (LAR):

      - `mh_1`
      - `mh_3`

    - Survey 1 (Biological Family Member):

      - `mh_1`
      - `mh_3_biological`

    - Survey 1 (Current or Former Caregiver)

      - `mh_2`
      - `me_4`

    - Survey 1 (Reporter)

      - `mh_2`
      - `mh_4`


#### Diagnostic Journey
Description: Indicates if the individual has provided survey data on the
patient diagnostic journey.

Variable Type: Enumeration

Source: Survey 4 task assignments

Denominator: All Assigned Survey 4 survey assignments complete and incomplete

Numerator:

Default Value: No

Alternative Values:

1.  Yes

    Condition: The survey task included in the above denominator is in a **completed** state amd has at least one **non-null* response to the following questions:


    - Survey 4 (Diagnosed Person):

      - `dj_1`
      - `dj-1a`
      - `dj_2`
      - `dj_2_specialty`
      - `dj_2_speciality_other`
      - `dj_3`
      - `dj_4`
      - `dj_5`
      - `dj_6`:

        - 1 or TRUE for any attribute in contained JSON object

      - `dj_6_sd_or_other`
      - `dj_7`:

        - 1 or TRUE for any attribute in contained JSON object, **except**
          `dj_7_delusionsOrHallucinations`
        - Any not-null for `dj_7_delusionsOrHallucinations`

      - `dj_7_other`
      - `dj_8`
      - `dj_9`
      - `dj_10`
      - `dj_11`
      - `dj_12`
      - `dj_13`
      - `dj_14`
      - `dj_15`
      - `dj_15a`:

        - 1 or TRUE for any attribute in contained JSON object

      - `dj_15b`
      - `dj_17`:

        - 1 or TRUE for any attribute in contained JSON object

      - `dj_17_sd_or_other`
      - `dj_19`:

        - 1 or TRUE for any attribute in contained JSON object, except
          `languageSpeakingFindingWordsUnderstandingKnowingTheMeaningOfObjectsOMemoryRememberingRecentEventsLearningNewInformation`
        - Any not-null for
          `languageSpeakingFindingWordsUnderstandingKnowingTheMeaningOfObjectsOMemoryRememberingRecentEventsLearningNewInformation`

      - `dj_19_sd_or_other`
      - `dj_21`:

        - 1 or TRUE for any attribute in contained JSON object

      - `dj_21_other`


    - Survey 4 (LAR):

      - `dj_1`
      - `dj-1a`
      - `dj_2`
      - `dj_2_specialty`
      - `dj_2_speciality_other`
      - `dj_3`
      - `dj_4`
      - `dj_5`
      - `dj_6`:

        - 1 or TRUE for any attribute in contained JSON object

      - `dj_6_sd_or_other`
      - `dj_7`:

        - 1 or TRUE for any attribute in contained JSON object, except
          `dj_7_delusionsOrHalucinations`
        - Any not-null for `dj_7_delusionsOrHallucinations`

      - `dj_7_other`
      - `dj_8`
      - `dj_9`
      - `dj_10`
      - `dj_11`
      - `dj_12`
      - `dj_13`
      - `dj_14`
      - `dj_15`
      - `dj_15a`:

        - 1 or TRUE for any attribute in contained JSON object

      - `dj_15b`
      - `dj_17`:

        - 1 or TRUE for any attribute in contained JSON object

      - `dj_17_sd_or_other`
      - `dj_19`:

        - 1 or TRUE for any attribute in contained JSON object, except
          `languageSpeakingFindingWordsUnderstandingKnowingTheMeaningOfObjectsOMemoryRememberingRecentEventsLearningNewInformation`
        - Any not-null for
          `languageSpeakingFindingWordsUnderstandingKnowingTheMeaningOfObjectsOMemoryRememberingRecentEventsLearningNewInformation`

      - `dj_19_sd_or_other`
      - `dj_21`:

        - 1 or TRUE for any attribute in contained JSON object

      - `dj_21_other`

    - Survey 4 (Biological Family Member or Caregiver):

      - `dj_1`
      - `dj-1a`
      - `dj_2`
      - `dj_2_specialty`
      - `dj_2_speciality_other`
      - `dj_3`
      - `dj_4`
      - `dj_5`
      - `dj_6`:

        - 1 or TRUE for any attribute in contained JSON object

      - `dj_6_sd_or_other`
      - `dj_7`:

        - 1 or TRUE for any attribute in contained JSON object, except
          `dj_7_delusionsOrHalucinations`
        - Any not-null for `dj_7_delusionsOrHallucinations`

      - `dj_7_other`
      - `dj_8`
      - `dj_9`
      - `dj_10`
      - `dj_11`
      - `dj_12`
      - `dj_13`
      - `dj_14`
      - `dj_15`
      - `dj_15a`:

        - 1 or TRUE for any attribute in contained JSON object

      - `dj_15b`
      - `dj_16`
      - `dj_17`:

        - 1 or TRUE for any attribute in contained JSON object

      - `dj_17_sd_or_other`
      - `dj_18`:

        - 1 or TRUE for any attribute in contained JSON object

      - `dj_18_sd_or_other`
      - `dj_19`:

        - 1 or TRUE for any attribute in contained JSON object, except
          `languageSpeakingFindingWordsUnderstandingKnowingTheMeaningOfObjectsOMemoryRememberingRecentEventsLearningNewInformation`
        - Any not-null for
          `languageSpeakingFindingWordsUnderstandingKnowingTheMeaningOfObjectsOMemoryRememberingRecentEventsLearningNewInformation`

      - `dj_19_sd_or_other`
      - `dj_20`:

        - 1 or TRUE for any attribute in contained JSON object

      - `dj_20_other`


#### Family History
Description: Indicates if the individual has provided survey data on family history of FTD disorders.

Variable Type: Enumeration

Source: Survey 5 task assignments

Denominator: All Assigned Survey 5 survey assignments complete and incomplete

Numerator:

Default Value: No

Alternative Values:

1.  Yes

    Condition: The survey task included in the above denominator is in a **completed** state amd has at least one **non-null* response to the following questions:


    - Survey 5 (Diagnosed Person):

      - `fh_1`

    - Survey 5 (LAR):

      - `fh_1`

    - Survey 5 (Biological Family Member):

      - `fh_2`

    - Survey 5 (Current Caregivers, Former Caregivers, Reporters):

      - `fh_2`



#### Environmental Exposure
Description: Indicates if the individual has provided survey data on
environmental exposures.

Variable Type: Enumeration

Source: Survey 5 task assignments

Denominator: All Assigned Survey 5 survey assignments complete and incomplete

Numerator:

Default Value: No

Alternative Values:

1.  Yes

    Condition: The survey task included in the above denominator is in a **completed** state amd has at least one **non-null* response to the following questions:


    - Survey 5 (Diagnosed Person):

      - `e_1`
      - `e_1a`
      - `e_3`
      - `e_5`
      - `e_7`
      - `e_10`

    - Survey 5 (LAR):

      - `e_1`
      - `e_1a`
      - `e_3`
      - `e_5`
      - `e_7`
      - `e_10`

    - Survey 5 (Biological Family Member):

      - `e_1`
      - `e_1a`
      - `e_3`
      - `e_5`
      - `e_8`
      - `e_10`

    - Survey 5 (Current Caregivers, Former Caregivers, Reporters):

      - `e_2`
      - `e_1a1`
      - `e_4`
      - `e_6`
      - `e_9`
      - `e_11`


#### Treatment Insights
Description: Indicates if the individual has provided survey data on
treatment history.

Variable Type: Enumeration

Source: Survey 6 task assignments

Denominator: All Assigned Survey 6 survey assignments complete and incomplete

Numerator:

Default Value: No

Alternative Values:

1.  Yes

    Condition: The survey task included in the above denominator is in a **completed** state amd has at least one **non-null* response to the following questions:


    - Survey 6 (Diagnosed Person):

      - `t_1`
      - `t_2`
      - `t_3`:

        - not-null for any attribute in contained JSON object

      - `t_3_other`
      - `t_4`

    - Survey 6 (LAR):

      - `t_1`
      - `t_2`
      - `t_3`:

        - not-null for any attribute in contained JSON object

      - `t_3_other`
      - `t_4`

    - Survey 6 (Biological Family Member):

      - `t_3`:

        - not-null for any attribute in contained JSON object

      - `t_3_other`
      - `t_4`

    - Survey 6 (Caregiver - Current):

      - `t_1`
      - `t_2`
      - `t_3`:

        - not-null for any attribute in contained JSON object

      - `t_3_other`
      - `t_4`

    - Survey 6 (Caregiver - Former):

      - `t_1`
      - `t_2`
      - `t_3`:

        - not-null for any attribute in contained JSON object

      - `t_3_other`
      - `t_4`

#### QOL/Functional Tasks
Description: Indicates if the individual has provided survey data on quality of life and/or functional tasks.

Variable Type: Enumeration

Source: Survey 3 task assignments

Denominator: All Assigned Survey 3 survey assignments complete and incomplete

Numerator:

Default Value: No

Alternative Values:

1.  Yes

    Condition: The survey task included in the above denominator is in a **completed** state

    - This survey cannot be saved part way, resulting in the simplified condition for "Yes"


#### Coded Genetic Testing Results
Description: Indicates if the individual has a confirmed and reviewed coded genetic test report data available within the Registry. The results of the genetic report may be positive or negative.

Variable Type: Enumeration

Source: Genetic Testing Results Survey and eCRF Form task assignments

Denominator: All Genetic Testing Results Surveys completed with an associated file uploaded for them

Numerator:

Default Value: No

Alternative Values:

1.  Yes

    Condition: A `completed` eCRF form with a file uploaded to it and a non-null response in any of the following:

    - `dateOfSampleCollection`
    - `sampleType`
    - `dateOfResults`
    - `testPurpose`


#### EMR Providers Linked

Description: Indicates if the Registry has received electronic medical
record (EMR) data from at least one health care provider for the individual.

Variable Type: Enumeration

Source: Share Medical Record task assignments

Denominator: All Share Medical Record assignments associated to a Participant Profile where the Participant is a unique self regardless of completion status.

Numerator:

Default Value: No

Alternative Values:

1.  Yes

    Condition: There exists at least one profile representing the individual containing at least one provider linked, regardless of completion status


### Participant Demographics: Sex (%)

  Description: The sex at birth of the individual.

  Variable Type: Enumeration

  Source: Participant

  Default Value: Unknown

  Alternative Values:

  - All values available in the `sex at birth` field taken from the participant profile tab in the DTP application

    Selection 1: If the individual is represented by any profile in the Registry, select the sex at birth value from the most recently updated profile.

    Selection 2: If the individual is represented by an Insights survey record, select the value of the `my_sex` variable.

  2.  Censored

      Condition: The individual is only represented by an ALLFTD profile.

### Participant Availability for Outreach

  Description: Indicates whether an individual is available for recontact and/or recruitment based on their consent, availability, and preferences. As of March 2025, this number may be an overestimate while the Registry integrates additional communication platforms.

  Variable Type: Enumeration

  Source: Account holder

  Default Value: No

  Alternative Values:

  1.  Yes

      Condition: All of the following criteria are met:

      - At least one profile, of any profile type/role, exists representing the individual where the enrollment status is not withdrawn or deceased, and
        - How does this handle with an account that their contact account still marked as inactive **until** they formally re-activate their contact profile?
      - There does not exist any profile representing the individual that contains an autopsy survey, and
      - There does not exists any note in the contact history for any profile representing the individual, where the status of the contact note is "Waiting for Follow-Up".

  Additional Notes:

  - The diagnosed person deceased flag for caregiver profiles is not included in the criteria above and shall be ignored.
