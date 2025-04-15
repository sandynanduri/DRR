# Digital Regulatory Reporting (DRR) System

## Overview
The Digital Regulatory Reporting (DRR) system is a framework for validating and processing regulatory trade reports. It supports various financial products including Interest Rate Swaps, Credit Default Swaps, Foreign Exchange trades, and more.

## Getting Started

### Prerequisites
- Java 11 or higher
- Maven 3.6 or higher

### Building the Project
```bash
mvn clean install -DskipTests
```

### Running the Example
```bash
mvn exec:java -Dexec.mainClass="org.rosetta.examples.regulatory.ValidateAndQualifySample"
```

## Sample Input/Output Analysis

### Sample Input (Credit Default Swap Trade)
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
                },
                "vintage": 1992
              },
              "contractualParty": [{
                "globalReference": "5da152cd",
                "externalReference": "party2"
              }, {
                "globalReference": "59b3a237",
                "externalReference": "party1"
              }]
            }]
          }
        },
        "before": {
          "value": {
            "trade": {
              "tradeIdentifier": [{
                "issuer": {
                  "value": "549300IB5Q45JGNPND58"
                },
                "assignedIdentifier": [{
                  "identifier": {
                    "value": "NEWTRADEXMLXXXXXXX02"
                  }
                }],
                "identifierType": "UniqueTransactionIdentifier"
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
                          "priceQuantity": {
                            "quantitySchedule": {
                              "address": {
                                "scope": "DOCUMENT",
                                "value": "quantity-1"
                              }
                            }
                          },
                          "generalTerms": {
                            "referenceInformation": {
                              "referenceEntity": {
                                "entityId": ["8G836J"],
                                "name": "TENET HEALTHCARE CORPORATION"
                              },
                              "referenceObligation": [{
                                "security": {
                                  "productIdentifier": [{
                                    "identifier": {
                                      "value": "8G836JAF2"
                                    }
                                  }],
                                  "securityType": "Debt"
                                }
                              }]
                            }
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

### Sample Output Breakdown

#### 1. Reference Resolution
```
Setting resolved object [key=bb8afac8, type=cdm.base.staticdata.party.NaturalPerson]
```
This indicates successful linking of party references in the trade data.

#### 2. Qualification Report
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
This shows successful qualification of:
- Business Event as Contract Formation
- Product as Credit Default Swap
- Economic Terms as Single Name CDS

#### 3. Validation Results
```
Validation FAILURE on [ReportableEvent.originatingWorkflowStep.businessEvent.instruction(0).primitiveInstruction.contractFormation.legalAgreement(2).legalAgreementIdentification.agreementName] for [DATA_RULE] [AgreementNameCreditSupportAgreement]
```
Indicates missing or invalid fields in the trade data.

## Key Components Explained

### 1. Trade Structure
- **Originating Workflow Step**: Contains the business event context
- **Business Event**: Defines the type and timing of the event
- **Instruction**: Contains the actual trade details
- **Legal Agreement**: Specifies the governing agreements

### 2. Product Details
- **Product Taxonomy**: Identifies the product type (Credit Default Swap)
- **Economic Terms**: Contains pricing and settlement details
- **Reference Information**: Specifies the underlying reference entity
- **Payout Structure**: Defines payment flows and amounts

### 3. Party Information
- **Contractual Parties**: The main parties to the trade
- **Party References**: Unique identifiers for each party
- **Party Roles**: Specifies the role of each party

## Common Validation Messages

### Success Indicators
- "Qualification successful"
- "Reference resolution complete"
- "All required fields present"

### Warning Messages
- "Optional field missing"
- "Value outside typical range"
- "Reference not found"

### Error Messages
- "Required field missing"
- "Invalid format"
- "Business rule violation"
- "Reference integrity failure"

## Troubleshooting Guide

### 1. Reference Resolution Issues
- Check party reference formats
- Verify all referenced entities exist
- Ensure proper linking between parties

### 2. Qualification Failures
- Verify product type specification
- Check business event type
- Ensure all required fields are present

### 3. Validation Errors
- Review specific field requirements
- Check data formats
- Verify business rule compliance

## Best Practices

1. **Data Preparation**
   - Use correct message types
   - Include all required fields
   - Follow specified data formats

2. **Validation**
   - Check qualification report first
   - Review all validation messages
   - Address critical errors before warnings

3. **Testing**
   - Test with sample data first
   - Verify all product types
   - Check edge cases

## Support

For issues and questions:
1. Check the validation logs for specific error messages
2. Review the example files in the `examples` directory
3. Consult the product documentation for specific requirements