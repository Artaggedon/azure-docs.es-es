---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: 6195b949cc71043dfa7a12bdece7a311dbde5e21
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "95560736"
---
## <a name="next-steps"></a>Pasos siguientes

Después de habilitar la integración de Azure Key Vault, puede habilitar el cifrado de SQL Server en la máquina virtual de SQL. En primer lugar, tendrá que crear una clave asimétrica en el almacén de claves y una clave simétrica dentro de SQL Server en la máquina virtual. A continuación, podrá ejecutar las instrucciones de T-SQL para habilitar el cifrado de las bases de datos y las copias de seguridad.

Hay varias formas de cifrado que puede aprovechar:

* [Cifrado de datos transparente (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption)
* [Copias de seguridad cifradas](/sql/relational-databases/backup-restore/backup-encryption)
* [Cifrado de nivel de columna (CLE)](/sql/t-sql/functions/cryptographic-functions-transact-sql)

Los siguientes scripts de Transact-SQL proporcionan ejemplos para cada una de estas áreas.

### <a name="prerequisites-for-examples"></a>Requisitos previos para los ejemplos

Todos los ejemplos se basa en los dos requisitos previos: una clave asimétrica del almacén de claves denominada **CONTOSO_KEY** y una credencial creada por la característica de integración de AKV denominada **Azure_EKM_cred**. Los siguientes comandos Transact-SQL configuran estos requisitos previos para ejecutar los ejemplos.

``` sql
USE master;
GO

--create credential
--The <<SECRET>> here requires the <Application ID> (without hyphens) and <Secret> to be passed together without a space between them.
CREATE CREDENTIAL Azure_EKM_cred
    WITH IDENTITY = 'keytestvault', --keyvault
    SECRET = '<<SECRET>>'
FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov;


--Map the credential to a SQL login that has sysadmin permissions. This allows the SQL login to access the key vault when creating the asymmetric key in the next step.
ALTER LOGIN [SQL_Login]
ADD CREDENTIAL Azure_EKM_cred;


CREATE ASYMMETRIC KEY CONTOSO_KEY
FROM PROVIDER [AzureKeyVault_EKM_Prov]
WITH PROVIDER_KEY_NAME = 'KeyName_in_KeyVault',  --The key name here requires the key we created in the key vault
CREATION_DISPOSITION = OPEN_EXISTING;
```

### <a name="transparent-data-encryption-tde"></a>Cifrado de datos transparente (TDE)

1. Cree un inicio de sesión de SQL Server que el motor de base de datos usará para TDE, después agréguele la credencial.

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it loads a database
   -- encrypted by TDE.
   CREATE LOGIN EKM_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the TDE Login to add the credential for use by the
   -- Database Engine to access the key vault
   ALTER LOGIN EKM_Login
   ADD CREDENTIAL Azure_EKM_cred;
   GO
   ```

1. Cree la clave de cifrado de base de datos que se usará para TDE.

   ``` sql
   USE ContosoDatabase;
   GO

   CREATE DATABASE ENCRYPTION KEY 
   WITH ALGORITHM = AES_128 
   ENCRYPTION BY SERVER ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the database to enable transparent data encryption.
   ALTER DATABASE ContosoDatabase
   SET ENCRYPTION ON;
   GO
   ```

### <a name="encrypted-backups"></a>Copias de seguridad cifradas

1. Cree un inicio de sesión de SQL Server que el motor de base de datos usará para cifrar las copias de seguridad, después agréguele la credencial.

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it is encrypting the backup.
   CREATE LOGIN EKM_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the Encrypted Backup Login to add the credential for use by
   -- the Database Engine to access the key vault
   ALTER LOGIN EKM_Login
   ADD CREDENTIAL Azure_EKM_cred ;
   GO
   ```

1. Realice una copia de seguridad de la base de datos especificando el cifrado con la clave asimétrica guardada en el almacén de claves.

   ``` sql
   USE master;
   BACKUP DATABASE [DATABASE_TO_BACKUP]
   TO DISK = N'[PATH TO BACKUP FILE]'
   WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD,
   ENCRYPTION(ALGORITHM = AES_256, SERVER ASYMMETRIC KEY = [CONTOSO_KEY]);
   GO
   ```

### <a name="column-level-encryption-cle"></a>Cifrado de nivel de columna (CLE)

Este script crea una clave simétrica protegida por la clave asimétrica en el almacén de claves y, a continuación, usa la clave simétrica para cifrar los datos de la base de datos.

``` sql
CREATE SYMMETRIC KEY DATA_ENCRYPTION_KEY
WITH ALGORITHM=AES_256
ENCRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

DECLARE @DATA VARBINARY(MAX);

--Open the symmetric key for use in this session
OPEN SYMMETRIC KEY DATA_ENCRYPTION_KEY
DECRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

--Encrypt syntax
SELECT @DATA = ENCRYPTBYKEY(KEY_GUID('DATA_ENCRYPTION_KEY'), CONVERT(VARBINARY,'Plain text data to encrypt'));

-- Decrypt syntax
SELECT CONVERT(VARCHAR, DECRYPTBYKEY(@DATA));

--Close the symmetric key
CLOSE SYMMETRIC KEY DATA_ENCRYPTION_KEY;
```

## <a name="additional-resources"></a>Recursos adicionales

Para más información sobre cómo usar estas características de cifrado, vea [Uso de la administración extensible de claves con las características de cifrado de SQL Server](/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server#UsesOfEKM).

Tenga en cuenta que en los pasos descritos en este artículo se supone que ya tiene SQL Server en ejecución en una máquina virtual de Azure. De lo contrario, consulte [Aprovisionamiento de una máquina virtual de SQL Server en Azure Portal](../articles/azure-sql/virtual-machines/windows/create-sql-vm-portal.md). Para otros temas sobre la ejecución de SQL Server en Azure Virtual Machines, consulte [Información general de SQL Server en Azure Virtual Machines](../articles/azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md).