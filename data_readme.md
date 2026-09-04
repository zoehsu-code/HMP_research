# PATH Data README

## 1. Data Structure

```text
PATH Study - ICPSR 36498
|
+-- Wave 1
|   |
|   +-- DS1001
|   |   +-- Adult questionnaire data + weights
|   |
|   +-- DS1002
|       +-- Youth/Parent questionnaire data + weights
|
+-- Wave 2
|   |
|   +-- DS2001
|   |   +-- Adult questionnaire data + weights
|   |
|   +-- DS2002
|       +-- Youth/Parent questionnaire data + weights
|
+-- Wave 3
|   |
|   +-- Main questionnaire data
|   |   +-- DS3001 -> Adult
|   |   +-- DS3002 -> Youth/Parent
|   |
|   +-- All-Waves longitudinal weights
|   |   +-- DS3101 -> Adult
|   |   +-- DS3201 -> Youth/Parent
|   |
|   +-- Ever/Never Reference Data
|       +-- DS3503 -> All participants
|
+-- Wave 4
|   |
|   +-- Main questionnaire data
|   |   +-- DS4001 -> Adult
|   |   +-- DS4002 -> Youth/Parent
|   |
|   +-- Wave 1 Cohort weights
|   |   +-- DS4111 -> Adult, All-Waves longitudinal weights
|   |   +-- DS4112 -> corresponding Youth weights
|   |
|   +-- Other longitudinal weight files
|   |   +-- DS4211
|   |   +-- DS4212
|   |
|   +-- Wave 4 Cohort cross-sectional weights
|   |   +-- DS4321 -> Adult
|   |   +-- DS4322 -> corresponding Youth weights
|   |
|   +-- Ever/Never Reference Data
|       +-- DS4503
|
+-- Wave 5
|   +-- DS5001 -> Adult main data
|   +-- DS5002 -> Youth/Parent main data
|   +-- DS51xx -> weight files
|   +-- DS52xx -> weight files
|   +-- ...
|
+-- Later Waves
    +-- Follow the same general organization, with additional
        weight files depending on cohort and analysis design.
```

TODO: Verify each DS description against the corresponding codebook/User Guide once those files are added to this repository. The current repository contains no dataset folders, codebooks, User Guide, or other PATH documentation beyond this README.

## 2. Longitudinal Structure

| Concept | Practical meaning |
| --- | --- |
| Wave | When measurement occurred. |
| Cohort | Recruitment/sample cohort. |
| `PERSONID` | Participant identifier used to link the same person across waves. |

- Wave and cohort are different concepts.
- Youth participants may transition into Adult data as they age.

```text
PERSONID = 000123

Wave 1 Youth  ----->  Wave 2 Youth  ----->  Wave 3 Adult
    |                    |                      |
    +-- same PERSONID ---+-- same PERSONID -----+
```

## 3. How to Find and Understand Variables

Use this workflow when preparing longitudinal features:

```text
Crosswalk
  -> identify corresponding variables across waves
  -> Codebook
  -> check variable definition and coding
  -> Questionnaire, when necessary
  -> check exact wording and skip logic
```

Variable naming conventions to verify in repository documentation:

| Pattern | Meaning |
| --- | --- |
| `R01` | TODO: Verify from PATH documentation. Expected to indicate Wave 1. |
| `R02` | TODO: Verify from PATH documentation. Expected to indicate Wave 2. |
| `R01R` | TODO: Verify from PATH documentation. Expected to indicate a Wave 1 recoded/derived variable. |
| `A` | TODO: Verify from PATH documentation. Expected to indicate Adult. |
| `Y` | TODO: Verify from PATH documentation. Expected to indicate Youth. |
| `P` | TODO: Verify from PATH documentation. Expected to indicate Parent. |

Do not infer a variable's meaning from its name alone. Confirm the definition, coding, universe, and skip pattern in the codebook/questionnaire.

## 4. Important Data Handling Notes

### Missing values

PATH uses special missing-value codes for cases such as inapplicable, refused, don't know, and not ascertained. These codes must not automatically be treated as ordinary numeric values in machine-learning models.

### Survey weights

- Weights are not predictor features.
- Cross-sectional weights are used for single-wave population inference.
- Longitudinal/all-waves weights are used for longitudinal analyses.
- Replicate weights are mainly used for variance or standard-error estimation.

### Ever/Never Reference variables

Ever/Never Reference variables may summarize tobacco-use information across multiple waves. Check their derivation before using them as predictors, because they can introduce temporal data leakage.

## 5. Notes for Longitudinal Prediction

Organize prediction datasets so predictor information comes from earlier wave(s), and the target outcome comes from a later wave.

```text
Earlier Wave(s)                         Later Wave
------------------------------------------------->
demographics --------\
tobacco use ----------\
health ----------------> nicotine/tobacco outcome
psychosocial ---------/
environment ----------/

    X                         Y
```

Rules:

1. Link the same participant across waves using `PERSONID`.
2. Predictor information must temporally precede the target outcome.
3. Check derived/reference variables for future-information leakage.
