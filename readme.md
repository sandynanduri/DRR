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

## Sample Input/Output

### Input Example (New Trade)
```json
{
  "originatingWorkflowStep": {
    "businessEvent": {
      "intent": "ContractFormation",
      "eventDate": "2024-04-15",
      "effectiveDate": "2024-04-15",
      "instruction": [{
        "primitiveInstruction": {
          "contractFormation": {
            "legalAgreement": [{
              "agreementDate": "2024-04-15",
              "legalAgreementIdentification": {
                "agreementName": {
                  "agreementType": "MasterAgreement",
                  "masterAgreementType": {
                    "value": "ISDAMaster"
                  }
                },
                "vintage": 2002
              },
              "contractualParty": [{
                "globalReference": "LEI12345678901234567890",
                "externalReference": "party1"
              }, {
                "globalReference": "LEI09876543210987654321",
                "externalReference": "party2"
              }]
            }]
          }
        }
      }],
      "messageInformation": {
        "messageType": "TRAD",
        "messageTimestamp": "2024-04-15T10:00:00Z"
      },
      "reportableInformation": {
        "trade": {
          "tradeId": "TRADE-001",
          "tradeDate": "2024-04-15",
          "product": {
            "productType": "INTEREST_RATE_SWAP",
            "notionalAmount": 1000000.00,
            "notionalCurrency": "USD",
            "fixedRate": 0.05,
            "floatingRateIndex": "LIBOR",
            "floatingRateSpread": 0.0025,
            "dayCountConvention": "ACT/360",
            "paymentFrequency": "SEMI_ANNUAL",
            "maturityDate": "2029-04-15"
          },
          "counterparty1": {
            "entityId": "LEI12345678901234567890",
            "entityName": "Counterparty 1"
          },
          "counterparty2": {
            "entityId": "LEI09876543210987654321",
            "entityName": "Counterparty 2"
          }
        }
      }
    }
  }
}
```

### Output Breakdown

The system produces two main types of output:

1. **Qualification Report**
   - Product type identification
   - Mapping to regulatory taxonomy
   - Validation of product-specific rules

2. **Validation Report**
   - Data completeness checks
   - Format validation
   - Business rule compliance
   - Reference integrity verification

### Sample Output Structure
```json
{
  "qualificationReport": {
    "success": true,
    "qualifiedName": "InterestRate_IRSwap_FixedFloat",
    "qualifiedObjectClass": "cdm.product.template.EconomicTerms"
  },
  "validationReport": {
    "validationFailures": [],
    "warnings": [],
    "processedData": {
      // Processed and validated trade data
    }
  }
}
```

## Key Components

### 1. Trade Data Structure
- **Originating Workflow Step**: Contains the business event context
- **Business Event**: Defines the type and timing of the event
- **Message Information**: Contains message type and timestamp
- **Reportable Information**: Contains the actual trade details

### 2. Product Types Supported
- Interest Rate Swaps (IRS)
- Credit Default Swaps (CDS)
- Foreign Exchange (FX) trades
- Commodity trades
- Equity derivatives
- Custom scenarios

### 3. Validation Rules
- **Data Completeness**: All required fields must be present
- **Format Validation**: Values must match expected formats
- **Business Rules**: Values must be within acceptable ranges
- **Reference Integrity**: All referenced entities must exist

## Common Validation Messages

### Success Indicators
- "Qualification successful"
- "All required fields present"
- "Business rules satisfied"

### Warning Messages
- "Optional field missing"
- "Value outside typical range"
- "Reference not found"

### Error Messages
- "Required field missing"
- "Invalid format"
- "Business rule violation"
- "Reference integrity failure"

## Troubleshooting

### Common Issues
1. **Unknown Lifecycle Error**
   - Check if the event type is correctly specified
   - Verify the message structure follows the expected format

2. **Validation Failures**
   - Review the validation report for specific field errors
   - Check data formats and ranges
   - Verify all required fields are present

3. **Qualification Failures**
   - Ensure product details are complete
   - Verify product type matches the data provided
   - Check for missing mandatory fields

## Best Practices

1. **Data Preparation**
   - Use the correct message type for the event
   - Include all required fields
   - Follow the specified data formats

2. **Validation**
   - Always check the qualification report first
   - Review all validation messages
   - Address critical errors before warnings

3. **Testing**
   - Test with sample data first
   - Verify all product types
   - Check edge cases and error conditions

## Support

For issues and questions:
1. Check the validation logs for specific error messages
2. Review the example files in the `examples` directory
3. Consult the product documentation for specific requirements