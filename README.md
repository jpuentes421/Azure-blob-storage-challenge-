# Azure-blob-storage-challenge-


Author: Jessica Puentes


Published: January 22, 2026

## Objective

 The Azure blob storage challenge is provided the the SANS Holiday Hack Challenge 2025 which is a free story driven onlin cybersecurity CTF. In this challenge, the neighborhood HOA uses Azure storage accounts for various IT operations and we have been asked to audit their storage security configuration to ensure no sensitive data is publicly available. 
 

### Tools Used

- Azure CLI provided by the challenge


## Steps

1. To start this challenge off, we launch the terminal. The challenge wants us to become familiar with the Azure CLI, to do this we will first type:
   <kbd>AZ help | less </kbd>
<img width="1556" height="776" alt="image" src="https://github.com/user-attachments/assets/9b562743-d3ed-40b9-9033-f8e664c0b9c6" />

 This will display the details of the Azure subscription that the Neighborhood HOA is currently using. We also have been informed that we have been configured with credentials. 

 2. We are instructed to type in the command: <kbd>Az account show | less </kbd>
    <img width="1347" height="723" alt="image" src="https://github.com/user-attachments/assets/d1f3cfd1-e1b8-49fa-8d15-8848b09dbf18" />
 
    This is the Azure subscription account details.


3. Now we will view storage account names: <kbd>Az storage account list | less</kbd>
<img width="1565" height="780" alt="image" src="https://github.com/user-attachments/assets/0913d109-ae3d-446e-a1ac-beb1246d63f3" />

This shows a list of storage accounts including:
-Neighborhood1
-Neighborhood2
-Neighborhood3
-Neighborhood4
-Neighborhood5
-Neighborhood6 

The prompt hints that there may be a misconfiguration within one of the storage accounts, and to try to locate the account that has the common misconfiguration. In this instance, it looks like Neighborhood2 has a misconfiguration.

4. We will input the command: <kbd>az storage account show --name neighborhood2 | less</kbd>

<img width="1233" height="621" alt="image" src="https://github.com/user-attachments/assets/2f67141d-feb7-44f8-98dc-086a6f94ebe2" />

In the Neighborhood2 account details, we can clearly see the misconfiguration here, "allowblobpublicaccess" is true.

This means the misconfiguration is that blob public access ie enabled on neighborhood2. This implies that containers or blobs in that account can be made publicly readable if a user creates/sets them that way, which is not secure especially for sensitive data.

5. Now we will list all the containers in the Neighborhood2 storage account in a scrollable view:
<kbd>az storage container list \ --account-name neighborhood2 \ --resource-group theneighbhorhood-rg1 \ --output table | less</kbd>
<img width="932" height="645" alt="image" src="https://github.com/user-attachments/assets/08de466d-69f0-4096-b6e2-c33195de079d" />

6. Now we will look at the blob list in the public container for neighborhood2 using this command: <kbd>az storage blob list \ --account-name neighborhood2 \ --container-name public \ --auth-mode login \ --output table | less</kbd>
<img width="1171" height="1006" alt="image" src="https://github.com/user-attachments/assets/e57156a5-948e-4cb5-b738-0b154d3720a7" />

7. The admin credentials.txt looks interesting something more we should dig into. To download and view the blob file from the public container, we will use this command:
   <kbd>az storage blob download \ --account-name neighbhorhood2 \ --container-name public \ --name admin_credentials.txt \ --file /dev/stdout \ --auth-mode login | less</kbd>

<img width="863" height="887" alt="image" src="https://github.com/user-attachments/assets/4b5505ae-1450-46ac-8caf-2fbc9584cfcb" />

The challenge is completed, the misconfiguration was found allowing public access to sensitive information, in this case credentials for the Azure portal, Windows server, SQL server, Active directory domain admin, Exchange admin, VMware vSphere, Network Switch, and Firewall admin. 

Always disable public access to blob containers especially if sensitive data like admin credentials are stored in these containers. 


