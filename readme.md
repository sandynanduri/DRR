# Digital Regulatory Reporting (DRR) System

// ... existing code ...

## Validation Output Explanation

### Reference Resolution
The system first performs reference resolution, linking various entities in the trade data:
```
Setting resolved object [key=bb8afac8, type=cdm.base.staticdata.party.NaturalPerson, path=ReportableEvent.reportableInformation.partyInformation(0).relatedPerson(0).personReference]
```
This shows the system mapping party references to their corresponding entities.

### Qualification Report
The qualification report indicates whether the trade data can be properly categorized:
```
QualificationReport SUCCESS [ qualifiable objects found 3, uniquely qualified objects 3, results: [
  QualificationResult { SUCCESS on [BusinessEvent:ContractFormation] },
  QualificationResult { SUCCESS on [EconomicTerms:CreditDefaultSwap_SingleName] },
  QualificationResult { SUCCESS on [EconomicTerms:CreditDefaultSwap_SingleName] }
]]
```
This shows:
- The trade was successfully qualified as a Credit Default Swap
- The business event was properly identified as a Contract Formation
- All required product information was present

### Common Validation Messages

#### 1. Reference Resolution Messages
```
Setting resolved object [key=bb8afac8, type=cdm.base.staticdata.party.NaturalPerson]
```
- Indicates successful linking of party references
- Shows the type of entity being resolved
- Includes the path to the reference in the data structure

#### 2. Qualification Messages
```
QualificationReport SUCCESS [ qualifiable objects found 3, uniquely qualified objects 3 ]
```
- Shows number of objects that were qualified
- Indicates whether the qualification was successful
- Lists the specific types that were qualified

#### 3. Validation Failure Messages
```
Validation FAILURE on [ReportableEvent.originatingWorkflowStep.businessEvent.instruction(0).primitiveInstruction.contractFormation.legalAgreement(2).legalAgreementIdentification.agreementName] for [DATA_RULE] [AgreementNameCreditSupportAgreement]
```
Common validation failures include:
- Missing required fields
- Invalid data formats
- Business rule violations
- Reference integrity issues

### Understanding Validation Results

1. **Reference Resolution**
   - Checks if all party references are valid
   - Verifies entity relationships
   - Ensures data consistency

2. **Qualification**
   - Identifies product type
   - Validates business event type
   - Confirms data completeness

3. **Validation**
   - Checks data format compliance
   - Verifies business rules
   - Ensures regulatory requirements are met

### Common Validation Rules

1. **Agreement Validation**
   ```
   [AgreementName->getCreditSupportAgreementType] does not exist
   ```
   - Ensures proper agreement type specification
   - Validates agreement structure
   - Checks for required agreement fields

2. **Trade Data Validation**
   ```
   [Trade->getTradableProduct->getProduct->getContractualProduct->getEconomicTerms->getTerminationDate->getAdjustableDate->getDateAdjustments] does not exist
   ```
   - Validates trade structure
   - Checks economic terms
   - Ensures date adjustments are properly specified

3. **Settlement Terms Validation**
   ```
   [Trade->getTradableProduct->getProduct->getContractualProduct->getEconomicTerms->getPayout->getCreditDefaultPayout->getSettlementTerms] does not exist
   ```
   - Verifies settlement terms
   - Checks payout specifications
   - Validates credit default swap specifics

### Troubleshooting Validation Issues

1. **Missing Fields**
   - Check if all required fields are present
   - Verify field names and structure
   - Ensure proper nesting of data

2. **Invalid References**
   - Verify party references exist
   - Check reference formats
   - Ensure proper linking between entities

3. **Business Rule Violations**
   - Review specific rule requirements
   - Check data values against allowed ranges
   - Verify compliance with regulatory requirements

// ... existing code ...