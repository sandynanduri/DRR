# Digital Regulatory Reporting (DRR) System

## Overview
A system for validating and processing regulatory trade reports, supporting various financial products like Credit Default Swaps (CDS), Interest Rate Swaps, and more.

## Quick Start
```bash
# Build the project
mvn clean install -DskipTests

# Run the example
mvn exec:java -Dexec.mainClass="org.rosetta.examples.regulatory.ValidateAndQualifySample"
```

## Understanding Trade Reports

### 1. Sample Input (CDS Trade)
```json
{
  "originatingWorkflowStep": {
    "businessEvent": {
      "intent": "ContractFormation",
      "eventDate": "2011-02-04",
      "effectiveDate": "2011-02-09",
      "instruction": [{
        "primitiveInstruction": {
          "contractFormation": {
            "legalAgreement": [{
              "agreementDate": "2002-01-05",
              "legalAgreementIdentification": {
                "agreementName": {
                  "agreementType": "MasterAgreement",
                  "masterAgreementType": {
                    "value": "ISDAMaster"
                  }
                }
              }
            }]
          }
        },
        "before": {
          "value": {
            "trade": {
              "tradeIdentifier": [{
                "identifier": {
                  "value": "NEWTRADEXMLXXXXXXX02"
                }
              }],
              "tradeDate": "2011-02-12",
              "tradableProduct": {
                "product": {
                  "contractualProduct": {
                    "productTaxonomy": [{
                      "primaryAssetClass": {
                        "value": "Credit"
                      }
                    }, {
                      "source": "ISDA",
                      "productQualifier": "CreditDefaultSwap_SingleName"
                    }],
                    "economicTerms": {
                      "effectiveDate": "2009-03-26",
                      "terminationDate": "2014-06-20",
                      "payout": {
                        "creditDefaultPayout": {
                          "payerReceiver": {
                            "payer": "Party1",
                            "receiver": "Party2"
                          },
                          "transactedPrice": {
                            "marketFixedRate": 0.02
                          }
                        }
                      }
                    }
                  }
                }
              }
            }
          }
        }
      }]
    }
  }
}
```

### 2. Sample Output
```
[DEBUG] Setting resolved object [key=bb8afac8, type=cdm.base.staticdata.party.NaturalPerson]
[INFO] QualificationReport SUCCESS [
  qualifiable objects found 3,
  uniquely qualified objects 3,
  results: [
    QualificationResult { SUCCESS on [BusinessEvent:ContractFormation] },
    QualificationResult { SUCCESS on [EconomicTerms:CreditDefaultSwap_SingleName] }
  ]
]
[DEBUG] Validation FAILURE on [ReportableEvent...agreementName] for [DATA_RULE] [AgreementNameCreditSupportAgreement]
```

## Understanding the Output

### 1. Reference Resolution
```
Setting resolved object [key=bb8afac8, type=cdm.base.staticdata.party.NaturalPerson]
```
- **What it means**: System is linking people/parties in the trade
- **Example**: Linking a trader to their role
- **Why it matters**: Ensures all parties are properly identified

### 2. Qualification Report
```
QualificationReport SUCCESS [
  qualifiable objects found 3,
  uniquely qualified objects 3,
  results: [
    QualificationResult { SUCCESS on [BusinessEvent:ContractFormation] },
    QualificationResult { SUCCESS on [EconomicTerms:CreditDefaultSwap_SingleName] }
  ]
]
```
- **What it means**: System checked if trade data is properly structured
- **Breaking it down**:
  - Found 3 objects to check
  - All were successfully qualified
  - Trade is a new contract formation
  - It's a Credit Default Swap (CDS) trade
- **Why it matters**: Confirms trade is properly categorized

### 3. Validation Results
```
Validation FAILURE on [ReportableEvent...agreementName] for [DATA_RULE] [AgreementNameCreditSupportAgreement]
```
- **What it means**: System found issues with the trade data
- **Breaking it down**:
  - Issue is with credit support agreement
  - Missing or incorrect agreement type
- **Why it matters**: Shows what needs to be fixed

## Common Scenarios

### Successful Trade
```
[DEBUG] Setting resolved object [successful references]
[INFO] QualificationReport SUCCESS [all objects qualified]
[DEBUG] Validation PASS [no failures]
```

### Trade with Issues
```
[DEBUG] Setting resolved object [successful references]
[INFO] QualificationReport SUCCESS [all objects qualified]
[DEBUG] Validation FAILURE [specific issues found]
```

## Troubleshooting

### Common Issues
1. **Missing Fields**
   - Check if all required fields are present
   - Verify field names and structure

2. **Invalid References**
   - Verify party references exist
   - Check reference formats

3. **Validation Failures**
   - Review specific field requirements
   - Check data formats

## Support
For issues and questions:
1. Check validation logs for specific error messages
2. Review example files in the `examples` directory
3. Consult product documentation for requirements



