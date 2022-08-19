# Infura Tutorial

1. register an [infura account](https://infura.io/register)

2. Login to [Infura](https://infura.io/login)

3. Click ETHEREUM in the left side bar
   ![](../image/ethereum.png)

4. Click CREATE NEW PROJECT
   ![](../image/create_new_project.png)

5. Enter project name，and click CREATE
   ![](https://i.imgur.com/26dhmMa.png)

6. Choose Rinkeby in the ENDPOINTS MENU，and copy the URL and paste to the `nodeURL` columns which is in the configuration file. The url consists of "https:// + enviornment name in lower case + .infura.io/v3/ + infuraProjectId. [config.h](../spo-verification-program/src/main/java/com/itrustmachines/verification/config/InfuraNodeUrlConfig.java)
   ![](../image/copy_url.png)
