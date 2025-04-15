# Digital Regulatory Reporting (DRR) System

// ... existing code ...

## Sample Output Breakdown

### Complete Sample Output
```
14:18:32.470 [DEBUG] Setting resolved object [key=bb8afac8, type=cdm.base.staticdata.party.NaturalPerson]
14:18:32.473 [DEBUG] Setting resolved object [key=bbb6a9ab, type=cdm.base.staticdata.party.NaturalPerson]
14:18:32.477 [DEBUG] Setting resolved object [key=a3344cf2, type=cdm.base.staticdata.party.Party]
14:18:34.091 [INFO] QualificationReport SUCCESS [
  qualifiable objects found 3,
  uniquely qualified objects 3,
  results: [
    QualificationResult { SUCCESS on [BusinessEvent:ContractFormation] },
    QualificationResult { SUCCESS on [EconomicTerms:CreditDefaultSwap_SingleName] },
    QualificationResult { SUCCESS on [EconomicTerms:CreditDefaultSwap_SingleName] }
  ]
]
14:18:36.341 [DEBUG] Validation FAILURE on [ReportableEvent.originatingWorkflowStep.businessEvent.instruction(0).primitiveInstruction.contractFormation.legalAgreement(2).legalAgreementIdentification.agreementName] for [DATA_RULE] [AgreementNameCreditSupportAgreement]
```

### Step-by-Step Output Explanation

#### 1. Reference Resolution (Lines 1-3)
```
Setting resolved object [key=bb8afac8, type=cdm.base.staticdata.party.NaturalPerson]
```
- **What it means**: The system is linking people and parties in the trade
- **Example**: Linking a trader (NaturalPerson) to their role in the trade
- **Why it's important**: Ensures all parties are properly identified and connected

#### 2. Qualification Report (Line 4)
```
QualificationReport SUCCESS [
  qualifiable objects found 3,
  uniquely qualified objects 3,
  results: [
    QualificationResult { SUCCESS on [BusinessEvent:ContractFormation] },
    QualificationResult { SUCCESS on [EconomicTerms:CreditDefaultSwap_SingleName] },
    QualificationResult { SUCCESS on [EconomicTerms:CreditDefaultSwap_SingleName] }
  ]
]
```
- **What it means**: The system checked if the trade data is properly structured
- **Breaking it down**:
  - Found 3 objects to check
  - All 3 were successfully qualified
  - The trade is a new contract formation
  - It's a Credit Default Swap (CDS) trade
  - The economic terms are for a single-name CDS
- **Why it's important**: Confirms the trade is properly categorized

#### 3. Validation Results (Line 5)
```
Validation FAILURE on [ReportableEvent.originatingWorkflowStep.businessEvent.instruction(0).primitiveInstruction.contractFormation.legalAgreement(2).legalAgreementIdentification.agreementName] for [DATA_RULE] [AgreementNameCreditSupportAgreement]
```
- **What it means**: The system found some issues with the trade data
- **Breaking it down**:
  - Issue is with the credit support agreement
  - Missing or incorrect agreement type
  - Located in the legal agreement section
- **Why it's important**: Highlights what needs to be fixed

### Common Output Patterns

#### 1. Reference Resolution Messages
```
Setting resolved object [key=XXXX, type=YYYY]
```
- **Format**: `[key=unique identifier, type=entity type]`
- **Example**: `[key=bb8afac8, type=cdm.base.staticdata.party.NaturalPerson]`
- **Meaning**: Successfully linked an entity (person/party) in the trade

#### 2. Qualification Messages
```
QualificationReport SUCCESS [qualifiable objects found X, uniquely qualified objects Y]
```
- **Format**: `[qualifiable objects found X, uniquely qualified objects Y]`
- **Example**: `[qualifiable objects found 3, uniquely qualified objects 3]`
- **Meaning**: All objects were successfully categorized

#### 3. Validation Messages
```
Validation FAILURE on [path.to.field] for [DATA_RULE] [RuleName]
```
- **Format**: `[path to field] for [rule type] [rule name]`
- **Example**: `[ReportableEvent...agreementName] for [DATA_RULE] [AgreementNameCreditSupportAgreement]`
- **Meaning**: Found an issue with specific data

### Understanding the Output Flow

1. **First Step - Reference Resolution**
   - System checks all party references
   - Links people to their roles
   - Verifies all connections are valid

2. **Second Step - Qualification**
   - System categorizes the trade
   - Identifies product type
   - Confirms business event type

3. **Final Step - Validation**
   - System checks all data rules
   - Verifies required fields
   - Ensures compliance with standards

### Common Output Scenarios

#### Successful Trade
```
[DEBUG] Setting resolved object [successful references]
[INFO] QualificationReport SUCCESS [all objects qualified]
[DEBUG] Validation PASS [no failures]
```

#### Trade with Warnings
```
[DEBUG] Setting resolved object [successful references]
[INFO] QualificationReport SUCCESS [all objects qualified]
[DEBUG] Validation WARNING [non-critical issues]
```

#### Failed Trade
```
[DEBUG] Setting resolved object [some references failed]
[INFO] QualificationReport FAILURE [some objects not qualified]
[DEBUG] Validation FAILURE [critical issues]
```

// ... rest of existing code ...