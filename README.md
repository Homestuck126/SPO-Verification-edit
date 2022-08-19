# SPO Verification Program User Guide English Version

- [SPO Verification Prgram User Guide Traditional Chinese Version](./README_zh.md)

## Outline

- [SPO Verification Program User Guide English Version](#spo-verification-program-user-guide-english-version)
  - [Outline](#outline)
  - [Requirement](#requirement)
  - [Prepare Verification Data](#prepare-verification-data)
    - [SPO Server](#spo-server)
    - [Dashboard](#dashboard)
  - [Verify Procedure](#verify-procedure)
    - [Verify by program](#verify-by-program)
      - [Establish Verifcation Service](#establish-verifcation-service)
      - [Verify](#verify)
        - [Method 1 : Use json file to verify](#method-1--use-json-file-to-verify)
        - [Method 2 : Use json string to verify](#method-2--use-json-string-to-verify)
      - [Verify Result](#verify-result)
    - [Verify by Command Line Interface](#verify-by-command-line-interface)

## Requirements

- Java 8 or higher
- Maven
- Private Key
  - You can use [VANITY-ETH](https://vanity-eth.tk/) to generate the private key
- Infure Node URL
  - Please refer to [Infura Tutorial](./doc/infura_en.md)

## Configure the settings for the Sample Code
This configuration file is very important. The Main program uses this configuration file to import ITM packages.

Create settings.xml in your .m2 folder and copy the following settings into the file
```
<settings>
  <servers>
    <server>
      <id>kuro-nexus-releases</id>
      <username>guest</username>
      <password>guest</password>
    </server>
  </servers>
</settings>
```

## Prepare Verification Data

### SPO Server

Use `verification-proof-api` to obtain verification with json string, then store in the file with json format. Please refer to [SPO Server API](https://azure-prod-rinkeby.itm.monster:4430/swagger-ui/) for more information.

1. Obtain the ITM proof token by using multiple ClearanceOrder and IndexValue

   - API Path：`/ledger/verify/verificationProof`
   - Request Body：

        ```json
        {
            "clearanceOrderAndIndexValuePairs": [
            {
                "clearanceOrder": 0,
                "indexValue": "string"
            }
            ]
        }
        ```

   - Example :

        ``` json
        {
            "clearanceOrderAndIndexValuePairs": [
            {
                "clearanceOrder": 1,
                "indexValue": "Example_R0"
            },
            {
                "clearanceOrder": 1,
                "indexValue": "Example_R1"
            }
            ]
        }
        ```

2. Obtain ITM proof token by ClearanceOrder interval

   - API Path：`/ledger/verify/verificationProofClearanceOrder`

   - Request Body：

        ```json
        {
            "fromClearanceOrder": 0,
            "indexValueKey": "string",
            "toClearanceOrder": 0
        }
        ```

   - Example：

        ```json
        {
            "fromClearanceOrder": 1,
            "indexValueKey": "Example",
            "toClearanceOrder": 2
        }
        ```

3. Obtain ITM proof token by time interval

    - API Path：`/ledger/verify/verificationProofQuery`

    - Request Body：

        ```json
        {
            "fromTimestamp": 0,
            "indexValueKey": "string",
            "toTimestamp": 0
        }
        ```

    - Example：

        ```json
        {
            "fromTimestamp": 1606780800000,
            "indexValueKey": "Example",
            "toTimestamp": 1606867200000
        }
        ```

### Dashboard

1. Enter the Dashboard to choose the data you want to verify.
2. Download the Off-Chain Proof data, namely, ITM proof token.

## Verify Procedure

### Verify by program

  For the following code, pleaser refer to [VerificationApi.java](./src/main/java/com/itrustmachines/verification/VerificationApi.java).

#### Establish Verifcation Service

  ```java
  final VerificationApi verificationApi = VerificationApi.getInstance();
  ```

#### Verify

If the ITM proof token uses the private chain, you can assign `null` to infuraProjectId.

##### Method 1 : Use the json file to verify

- `filePath` : Path of verification file
- `infuraProjectId` : Enter the infuraProjectId

```java
final VerifyVerificationProofResult result = verificationApi.verify(filePath, infuraProjectId);
```

##### Method 2 : Use json string to verify

- `jsonString` : String wtih json data type
- `infuraProjectId` : Enter the infuraProjectId

```java
final VerifyVerificationProofResult result = verificationApi.verifyJsonString(jsonString, infuraProjectId);
```

#### Verify Result

If verified successfully, the `result.isPass()` will return `true`, otherwise it return `false`.
If `result.isPass()` returns `false`, you can check which data is the error by calling `result.getVerifyReceiptResults()`
### Verify by Command Line Interface

Enter the following command in CLI with your information:

```shell
java -jar spo-verification-program-{VERSION}.jar --proof sample/queryByCO.json --result result.json --infuraProjectId {INFURA_PROJECT_ID}
```

- help

    ```shell
    usage: verification-api
        --infuraProjectId <infuraProjectId>   required if env is MAINNET, KOVAN, GOERLI, RINKEBY, ROPSTEN
        --proof <filePath>                    input verification proof file path (sample/queryByCO.json)
        --result <filePath>                   output verify result file path (result.json)
    ```
