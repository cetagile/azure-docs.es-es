---
title: 'Generación y transferencia de claves protegidas con HSM: Azure Key Vault'
description: Aprenda a planear, generar y, después, a transferir sus propias claves protegidas con HSM para utilizar con Azure Key Vault. También se denomina BYOK o "traiga su propia clave".
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: tutorial
ms.date: 02/01/2021
ms.author: ambapat
ms.openlocfilehash: 17ef0cb7efcd2a76b46a6bb0dbc82268895dae00
ms.sourcegitcommit: 983eb1131d59664c594dcb2829eb6d49c4af1560
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99222207"
---
# <a name="import-hsm-protected-keys-to-key-vault"></a>Importación de claves protegidas con HSM en Key Vault

Para obtener una mayor seguridad, cuando utilice Azure Key Vault, puede importar o generar claves en módulos de seguridad de hardware (HSM) que no se salen nunca del límite de los HSM. Con frecuencia este escenario se conoce también como *Aportar tu propia clave*, o BYOK. En Azure Key Vault se usa la familia nCipher nShield de HSM (validado con FIPS 140-2 de nivel 2) para proteger las claves.

Esta funcionalidad no está disponible para Azure China 21Vianet.

> [!NOTE]
> Para obtener más información sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](../general/overview.md)  
> Para ver un tutorial introductorio que incluya la creación de un almacén de claves para claves protegidas con HSM, consulte [¿Qué es Azure Key Vault?](../general/overview.md).

## <a name="supported-hsms"></a>HSM compatibles

La transferencia de claves protegidas con HSM a Key Vault se admite a través de dos métodos diferentes en función de los HSM que se usen. Use la tabla siguiente para determinar qué método debe usar para generar los HSM y transferir sus propias claves protegidas con HSM para usarlas con Azure Key Vault. 

|Nombre del proveedor|Tipo de proveedor|Modelos de HSM compatibles|Método de transferencia de clave HSM compatible|
|---|---|---|---|
|[nCipher](https://www.ncipher.com/products/key-management/cloud-microsoft-azure)|Fabricante,<br/>HSM como servicio|<ul><li>Familia nShield de HSM</li><li>nShield como servicio</ul>|**Método 1:** [nCipher BYOK](hsm-protected-keys-ncipher.md) (con atestación segura para la importación de claves y la validación de HSM)<br/>**Método 2:** [Usar el nuevo método BYOK](hsm-protected-keys-byok.md) |
|Thales|Fabricante|<ul><li>Familia Luna HSM 7 con la versión del firmware 7.3 o posterior</li></ul>| [Usar el nuevo método BYOK](hsm-protected-keys-byok.md)|
|Fortanix|Fabricante,<br/>HSM como servicio|<ul><li>Self-Defending Key Management Service (SDKMS)</li><li>Equinix SmartKey</li></ul>|[Usar el nuevo método BYOK](hsm-protected-keys-byok.md)|
|Marvell|Fabricante|Todos los HSM de LiquidSecurity con<ul><li>versión de firmware 2.0.4 o posterior</li><li>versión de firmware 3.2 o posterior</li></ul>|[Usar el nuevo método BYOK](hsm-protected-keys-byok.md)|
|Cryptomathic|ISV (sistema de administración de claves empresariales)|Varias marcas y modelos de HSM, como<ul><li>nCipher</li><li>Thales</li><li>Utimaco</li></ul>Consulte el [sitio de Cryptomathic para obtener más información](https://www.cryptomathic.com/azurebyok).|[Usar el nuevo método BYOK](hsm-protected-keys-byok.md)|
|Securosys SA|Fabricante, HSM como servicio|Familia Primus HSM, Securosys Clouds HSM|[Usar el nuevo método BYOK](hsm-protected-keys-byok.md)|
|StorMagic|ISV (sistema de administración de claves empresariales)|Varias marcas y modelos de HSM, como<ul><li>Utimaco</li><li>Thales</li><li>nCipher</li></ul>Consulte el [sitio web de StorMagic para más información](https://stormagic.com/doc/svkms/Content/Integrations/Azure_KeyVault_BYOK.htm).|[Usar el nuevo método BYOK](hsm-protected-keys-byok.md)|
|IBM|Fabricante|IBM 476x, CryptoExpress|Usar el nuevo método BYOK](hsm-protected-keys-byok.md)|
|||||

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Introducción a la seguridad de Azure Key Vault](../general/security-overview.md) para garantizar la seguridad, la durabilidad y la supervisión de las claves.
* Consulte la [especificación de BYOK](./byok-specification.md) para obtener una descripción completa del nuevo método BYOK.