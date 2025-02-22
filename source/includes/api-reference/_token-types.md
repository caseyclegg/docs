# Token Types

Basis Theory offers several pre-configured token types for various use-cases and compliance requirements.
Token Types define the rules around a data type such as validation requirements, [data classification](#tokens-token-classifications), 
[data impact level](#tokens-token-impact-levels), and [data restriction policy](#tokens-token-restriction-policies).

- [Token](#token-types-token)
- [Card](#token-types-card)
- [Bank](#token-types-bank)
- [Card Number](#token-types-card-number)
- [US Bank Account Number](#token-types-us-bank-account-number)
- [US Bank Routing Number](#token-types-us-bank-routing-number)
- [Social Security Number](#token-types-social-security-number)
- [Employer ID Number](#token-types-employer-id-number)

## Token

The `token` type is used for general data types that don't require input validation or formatting restrictions.

| Token Attribute                    | Value                                   |
|------------------------------------|-----------------------------------------|
| **Type**                           | `token`                                 |
| **Default Classification**         | `general`                               |
| **Default Impact Level**           | `high`                                  |
| **Minimum Impact Level**           | `low`                                   |
| **Default Restriction Policy**     | `redact`                                |
| **Default Container**              | `/general/high/`                        |
| **Input Validation**               | None                                    |
| **Input Length**                   | Any                                     |
| **Default Fingerprint Expression** | <code>{{ data &#124; stringify}}</code> |
| **Default Mask Expression**        | `null`                                  |


## Card

| Token Attribute                    | Value                                                                                                                                                                                                                    |
|------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                           | `card`                                                                                                                                                                                                                   |
| **Default Classification**         | `pci`                                                                                                                                                                                                                    |
| **Default Impact Level**           | `high`                                                                                                                                                                                                                   |
| **Minimum Impact Level**           | `high`                                                                                                                                                                                                                   |
| **Default Restriction Policy**     | `mask`                                                                                                                                                                                                                   |
| **Default Container**              | `/pci/high/`                                                                                                                                                                                                             |
| **Input Validation**               | See [Card Object](#tokens-token-data-validations) for validation requirements                                                                                                                                            |
| **Default Fingerprint Expression** | `{{ data.number }}`                                                                                                                                                                                                      |
| **Default Mask Expression**        | <code>{<br>&nbsp;&nbsp;"number": "{{ data.number &#124; reveal_last: 4 }}",<br>&nbsp;&nbsp;"expiration_month": "{{ data.expiration_month }}",<br>&nbsp;&nbsp;"expiration_year": "{{ data.expiration_year }}"<br>}</code> |


## Bank

| Token Attribute                    | Value                                                                                                                                                                |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                           | `bank`                                                                                                                                                               |
| **Default Classification**         | `bank`                                                                                                                                                               |
| **Default Impact Level**           | `high`                                                                                                                                                               |
| **Minimum Impact Level**           | `high`                                                                                                                                                               |
| **Default Restriction Policy**     | `mask`                                                                                                                                                               |
| **Default Container**              | `/bank/high/`                                                                                                                                                        |
| **Input Validation**               | See [Bank Object](#tokens-token-data-validations) for validation requirements                                                                                        |
| **Default Fingerprint Expression** | <code>{{ data.account_number }}&#124;{{ data.routing_number }}</code>                                                                                                |
| **Default Mask Expression**        | <code>{<br>&nbsp;&nbsp;"routing_number": "{{ data.routing_number }}",<br>&nbsp;&nbsp;"account_number": "{{ data.account_number &#124; reveal_last: 4 }}"<br>}</code> |


## Card Number

| Token Attribute                    | Value                                         |
|------------------------------------|-----------------------------------------------|
| **Type**                           | `card_number`                                 |
| **Default Classification**         | `pci`                                         |
| **Default Impact Level**           | `high`                                        |
| **Minimum Impact Level**           | `high`                                        |
| **Default Restriction Policy**     | `mask`                                        |
| **Default Container**              | `/pci/high/`                                  |
| **Input Validation**               | Luhn-valid, numeric                           |
| **Input Length**                   | 13 - 19                                       |
| **Default Fingerprint Expression** | `{{ data }}`                                  |
| **Default Mask Expression**        | <code>{{ data &#124; reveal_last: 4 }}</code> |

Examples:

| Input Data       | Masked Value     |
|------------------|------------------|
| 4242424242424242 | XXXXXXXXXXXX4242 |
| 36227206271667   | XXXXXXXXXX1667   |


## US Bank Account Number

| Token Attribute                    | Value                                         |
|------------------------------------|-----------------------------------------------|
| **Type**                           | `us_bank_account_number`                      |
| **Default Classification**         | `bank`                                        |
| **Default Impact Level**           | `high`                                        |
| **Minimum Impact Level**           | `low`                                         |
| **Default Restriction Policy**     | `mask`                                        |
| **Default Container**              | `/bank/high/`                                 |
| **Input Validation**               | Numeric                                       |
| **Input Length**                   | 3 - 17                                        |
| **Default Fingerprint Expression** | `{{ data }}`                                  |
| **Default Mask Expression**        | <code>{{ data &#124; reveal_last: 4 }}</code> |

Examples: 

| Input Data          | Masked Value        |
|---------------------|---------------------|
| 1234567890          | XXXXXX7890          |


## US Bank Routing Number

| Token Attribute                    | Value                                         |
|------------------------------------|-----------------------------------------------|
| **Type**                           | `us_bank_routing_number`                      |
| **Default Classification**         | `bank`                                        |
| **Default Impact Level**           | `low`                                         |
| **Minimum Impact Level**           | `low`                                         |
| **Default Restriction Policy**     | `redact`                                      |
| **Default Container**              | `/bank/low/`                                  |
| **Input Validation**               | Numeric, ABA-valid                            |
| **Input Length**                   | 9                                             |
| **Default Fingerprint Expression** | `{{ data }}`                                  |
| **Default Mask Expression**        | <code>{{ data &#124; reveal_last: 4 }}</code> |


## Social Security Number

| Token Attribute                    | Value                                         |
|------------------------------------|-----------------------------------------------|
| **Type**                           | `social_security_number`                      |
| **Default Classification**         | `pii`                                         |
| **Default Impact Level**           | `high`                                        |
| **Minimum Impact Level**           | `low`                                         |
| **Default Restriction Policy**     | `mask`                                        |
| **Default Container**              | `/pii/high/`                                  |
| **Input Validation**               | Numeric with optional delimiter of `"-"`      |
| **Input Length**                   | 9 (not including delimiting characters)       |
| **Default Fingerprint Expression** | <code>{{ data &#124; remove: '-' }}</code>    |
| **Default Mask Expression**        | <code>{{ data &#124; reveal_last: 4 }}</code> |

Examples:

| Input Data  | Masked Value |
|-------------|--------------|
| 123456789   | XXXXX6789    |
| 123-45-6789 | XXX-XX-6789  |


## Employer ID Number

| Token Attribute                    | Value                                         |
|------------------------------------|-----------------------------------------------|
| **Type**                           | `employer_id_number`                          |
| **Default Classification**         | `pii`                                         |
| **Default Impact Level**           | `high`                                        |
| **Minimum Impact Level**           | `low`                                         |
| **Default Restriction Policy**     | `mask`                                        |
| **Default Container**              | `/pii/high/`                                  |
| **Input Validation**               | Numeric with optional delimiter of `"-"`      |
| **Input Length**                   | 9 (not including delimiting characters)       |
| **Default Fingerprint Expression** | <code>{{ data &#124; remove: '-' }}</code>    |
| **Default Mask Expression**        | <code>{{ data &#124; reveal_last: 4 }}</code> |

Examples:

| Input Data | Masked Value |
|------------|--------------|
| 123456789  | XXXXX6789    |
| 12-3456789 | XX-XXX6789   |
