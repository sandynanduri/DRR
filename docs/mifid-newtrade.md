# MiFID Interest Rate Swap Examples

## New Trade Example

### Sample Input (IRS Trade)
```json
{
  "originatingWorkflowStep": {
    "businessEvent": {
      "intent": "ContractFormation",
      "eventType": "NEWT",
      "eventDate": "2024-03-15",
      "effectiveDate": "2024-03-17",
      "instruction": [{
        "primitiveInstruction": {
          "contractFormation": {
            "legalAgreement": [{
              "agreementDate": "2024-01-01",
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
                  "value": "IRS202403150001"
                }
              }],
              "tradeDate": "2024-03-15",
              "tradableProduct": {
                "product": {
                  "contractualProduct": {
                    "productTaxonomy": [{
                      "primaryAssetClass": {
                        "value": "InterestRate"
                      }
                    }, {
                      "source": "ISDA",
                      "productQualifier": "InterestRateSwap"
                    }],
                    "economicTerms": {
                      "effectiveDate": "2024-03-17",
                      "terminationDate": "2029-03-17",
                      "payout": {
                        "interestRatePayout": {
                          "payerReceiver": {
                            "payer": "Party1",
                            "receiver": "Party2"
                          },
                          "calculationPeriod": {
                            "periodMultiplier": 3,
                            "period": "Month"
                          },
                          "fixedRate": {
                            "rate": 0.035
                          },
                          "floatingRate": {
                            "floatingRateIndex": "EURIBOR",
                            "indexTenor": {
                              "periodMultiplier": 3,
                              "period": "Month"
                            }
                          },
                          "notional": {
                            "amount": 10000000,
                            "currency": "EUR"
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

### Sample Output
```
[DEBUG] Setting resolved object [key=irs_party1, type=cdm.base.staticdata.party.Party]
[DEBUG] Setting resolved object [key=irs_party2, type=cdm.base.staticdata.party.Party]
[INFO] Event Type: NEWT (New Trade)
[INFO] QualificationReport SUCCESS [
  qualifiable objects found 2,
  uniquely qualified objects 2,
  results: [
    QualificationResult { SUCCESS on [BusinessEvent:ContractFormation] },
    QualificationResult { SUCCESS on [EconomicTerms:InterestRateSwap] }
  ]
]
[DEBUG] Validation PASS on [ReportableEvent...agreementName] for [DATA_RULE] [AgreementNameMasterAgreement]
```

## Understanding the Output

### 1. Reference Resolution
```
Setting resolved object [key=irs_party1, type=cdm.base.staticdata.party.Party]
```
- **What it means**: System is linking the parties in the IRS trade
- **Example**: Linking Party1 and Party2 to their roles (payer/receiver)
- **Why it matters**: Ensures both parties are properly identified

### 2. Event Type and Qualification
```
[INFO] Event Type: NEWT (New Trade)
QualificationReport SUCCESS [...]
```
- **What it means**: System identified this as a new trade event
- **Breaking it down**:
  - Event type is "NEWT" (New Trade)
  - Found 2 objects to check (parties)
  - All were successfully qualified
  - Trade is a new contract formation
  - It's an Interest Rate Swap (IRS) trade
- **Why it matters**: Confirms this is a new trade and properly categorized as an IRS

### 3. Validation Results
```
Validation PASS on [ReportableEvent...agreementName] for [DATA_RULE] [AgreementNameMasterAgreement]
```
- **What it means**: System verified all required fields
- **Breaking it down**:
  - Master Agreement is properly specified
  - All required fields are present
- **Why it matters**: Shows trade meets regulatory requirements

## Key IRS Components Explained

### 1. Trade Details
- **Event Type**: NEWT (New Trade)
- **Trade ID**: Unique identifier for the trade
- **Trade Date**: When the trade was executed
- **Effective Date**: When the IRS starts
- **Termination Date**: When the IRS ends

### 2. Product Details
- **Product Type**: Interest Rate Swap
- **Asset Class**: Interest Rate
- **Master Agreement**: ISDA Master Agreement

### 3. Economic Terms
- **Notional Amount**: 10,000,000 EUR
- **Fixed Rate**: 3.5%
- **Floating Rate**: 3M EURIBOR
- **Payment Frequency**: Quarterly (3 months)
- **Payer/Receiver**: Party1 pays fixed, Party2 receives fixed

## Common IRS Validation Rules

### 1. Required Fields
- Event type (must be NEWT for new trades)
- Trade identifier
- Trade date
- Effective date
- Termination date
- Notional amount
- Fixed/floating rates
- Payment frequency

### 2. Business Rules
- Event type must match the business event
- Effective date must be after trade date
- Termination date must be after effective date
- Notional amount must be positive
- Rates must be within reasonable ranges

### 3. Party Requirements
- Both parties must be identified
- Payer/receiver roles must be specified
- Party references must be valid

## Troubleshooting IRS Issues

### Common Problems
1. **Event Type Issues**
   - Verify event type is NEWT for new trades
   - Check event type matches business event

2. **Missing Rate Information**
   - Check both fixed and floating rate specifications
   - Verify rate formats and values

3. **Date Issues**
   - Ensure dates follow correct sequence
   - Check date formats (YYYY-MM-DD)

4. **Party Reference Issues**
   - Verify party identifiers
   - Check payer/receiver assignments

## Best Practices for IRS Trades

1. **Data Entry**
   - Always specify correct event type (NEWT for new trades)
   - Use consistent date formats
   - Include all required fields
   - Verify rate specifications

2. **Validation**
   - Check event type first
   - Check qualification report
   - Review all validation messages
   - Verify economic terms

3. **Testing**
   - Test with sample data
   - Verify rate calculations
   - Check date logic 

example to run 
mvn exec:java -Dexec.mainClass="org.rosetta.examples.regulatory.ValidateAndQualifySample" -Dexec.args="examples/src/main/resources/regulatory-reporting/input/events/MiFID-IRS-New-Trade.json"