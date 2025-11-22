# Statistics of Medical Diagnostics

## Overview
Medical diagnostic tests are evaluated using statistical measures that help healthcare professionals understand test accuracy, reliability, and clinical utility. These metrics are essential for interpreting test results and making informed clinical decisions.

## Key Statistical Measures

### Sensitivity (True Positive Rate)
- **Definition**: The probability that a test correctly identifies individuals who have the disease
- **Formula**: Sensitivity = TP / (TP + FN)
- **Interpretation**: A highly sensitive test has few false negatives
- **Clinical Use**: Ideal for screening tests where missing a disease could be serious
- **Example**: A sensitivity of 95% means the test correctly identifies 95% of people with the disease

### Specificity (True Negative Rate)
- **Definition**: The probability that a test correctly identifies individuals who do not have the disease
- **Formula**: Specificity = TN / (TN + FP)
- **Interpretation**: A highly specific test has few false positives
- **Clinical Use**: Ideal for confirmatory tests to avoid unnecessary treatment
- **Example**: A specificity of 90% means the test correctly identifies 90% of people without the disease

### Positive Predictive Value (PPV)
- **Definition**: The probability that a person with a positive test result actually has the disease
- **Formula**: PPV = TP / (TP + FP)
- **Interpretation**: Depends on disease prevalence in the population
- **Clinical Significance**: Answers "If my patient tests positive, what's the chance they have the disease?"
- **Note**: PPV increases with higher disease prevalence

### Negative Predictive Value (NPV)
- **Definition**: The probability that a person with a negative test result truly does not have the disease
- **Formula**: NPV = TN / (TN + FN)
- **Interpretation**: Depends on disease prevalence in the population
- **Clinical Significance**: Answers "If my patient tests negative, what's the chance they're disease-free?"
- **Note**: NPV decreases with higher disease prevalence

## The 2×2 Contingency Table

|                    | Disease Present | Disease Absent | Total         |
|--------------------|----------------|----------------|---------------|
| **Test Positive**  | TP             | FP             | TP + FP       |
| **Test Negative**  | FN             | TN             | FN + TN       |
| **Total**          | TP + FN        | FP + TN        | TP+FP+FN+TN   |

**Key:**
- TP = True Positive
- TN = True Negative
- FP = False Positive (Type I Error)
- FN = False Negative (Type II Error)

## Additional Important Measures

### Accuracy
- **Formula**: Accuracy = (TP + TN) / (TP + TN + FP + FN)
- **Definition**: Overall proportion of correct test results
- **Limitation**: Can be misleading with imbalanced disease prevalence

### Likelihood Ratios

#### Positive Likelihood Ratio (LR+)
- **Formula**: LR+ = Sensitivity / (1 - Specificity)
- **Interpretation**: How much a positive test increases the odds of having the disease
- **Clinical Use**:
  - LR+ > 10: Large increase in probability of disease
  - LR+ 5-10: Moderate increase
  - LR+ 2-5: Small increase
  - LR+ < 2: Minimal increase

#### Negative Likelihood Ratio (LR-)
- **Formula**: LR- = (1 - Sensitivity) / Specificity
- **Interpretation**: How much a negative test decreases the odds of having the disease
- **Clinical Use**:
  - LR- < 0.1: Large decrease in probability of disease
  - LR- 0.1-0.2: Moderate decrease
  - LR- 0.2-0.5: Small decrease
  - LR- > 0.5: Minimal decrease

### Prevalence
- **Formula**: Prevalence = (TP + FN) / Total Population
- **Definition**: Proportion of population with the disease
- **Impact**: Affects PPV and NPV significantly

## Clinical Application Example

### Scenario: Mammography Screening
- **Sensitivity**: 85%
- **Specificity**: 95%
- **Prevalence**: 1% (in general population)

**For 1,000 women screened:**
- 10 have breast cancer (1% prevalence)
- 990 do not have breast cancer

**Test Results:**
- True Positives (TP): 10 × 0.85 = 8.5 ≈ 9
- False Negatives (FN): 10 × 0.15 = 1.5 ≈ 1
- True Negatives (TN): 990 × 0.95 = 940.5 ≈ 941
- False Positives (FP): 990 × 0.05 = 49.5 ≈ 49

**Calculated Values:**
- PPV = 9 / (9 + 49) = 15.5%
- NPV = 941 / (941 + 1) = 99.9%

**Clinical Interpretation:**
- Only 15.5% of positive mammograms actually indicate cancer
- 99.9% of negative mammograms correctly rule out cancer
- This demonstrates why many positive screenings require follow-up testing

## Important Considerations for Nursing Practice

### Trade-offs Between Sensitivity and Specificity
- Increasing sensitivity often decreases specificity (and vice versa)
- The "cutoff point" for a test determines this balance
- Choice depends on clinical context and consequences of false results

### Disease Prevalence Impact
- **High Prevalence Settings**: PPV increases, NPV decreases
- **Low Prevalence Settings**: PPV decreases, NPV increases
- **Example**: COVID-19 testing during outbreak vs. routine screening

### Sequential Testing Strategies
1. **Parallel Testing**: Multiple tests performed simultaneously
   - Increases sensitivity
   - Useful when disease must not be missed

2. **Serial Testing**: Second test only if first is positive
   - Increases specificity
   - Reduces false positives
   - Common approach: screening test → confirmatory test

### Clinical Decision Making
- Consider pre-test probability (clinical assessment + prevalence)
- Use likelihood ratios to calculate post-test probability
- No test is perfect—always integrate with clinical findings
- Patient values and preferences matter in interpreting results

## Common Nursing School Questions

### Q: When do we prioritize high sensitivity?
**A:** When the consequence of missing the disease is severe:
- Cancer screening
- Infectious disease surveillance
- Emergency conditions requiring immediate treatment

### Q: When do we prioritize high specificity?
**A:** When false positives have serious consequences:
- Before invasive procedures
- Confirmatory testing for serious diagnoses
- When treatment has significant side effects

### Q: Why does a positive HIV screening test require confirmation?
**A:** Screening tests (ELISA) are highly sensitive but less specific. In low-prevalence populations, many positives are false positives. The Western blot confirmatory test is highly specific, reducing false positives.

## Study Tips

1. **Remember the formulas** using the 2×2 table
2. **Sensitivity = TP rate** (finds the sick)
3. **Specificity = TN rate** (finds the healthy)
4. **PPV and NPV** depend on prevalence—same test, different populations, different values
5. **Practice calculations** with different scenarios
6. **Understand trade-offs** between different measures

## Further Reading Topics
- ROC (Receiver Operating Characteristic) curves
- Bayes' Theorem in diagnostic testing
- Screening vs. diagnostic tests
- Gold standard tests
- Inter-rater reliability (kappa statistic)
- Continuous vs. categorical test results

---

*Last Updated: November 22, 2025*
