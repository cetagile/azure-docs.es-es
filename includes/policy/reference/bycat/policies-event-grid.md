---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/29/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: f98ff82ce09e7401c66cec6283544002a2b8bff0
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2021
ms.locfileid: "99220533"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Los dominios de Azure Event Grid deben usar Private Link](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9830b652-8523-49cc-b1b3-e17dce1127ca) |Azure Private Link permite conectar la red virtual a los servicios de Azure sin una dirección IP pública en el origen o el destino. La plataforma de Private Link controla la conectividad entre el consumidor y los servicios en la red troncal de Azure. Al asignar puntos de conexión privados a los dominios de Event Grid en lugar de a todo el servicio, además se protege frente a riesgos de pérdida de datos. Más información en: [https://aka.ms/privateendpoints](https://aka.ms/privateendpoints). |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Event%20Grid/EventGridDomains_EnablePrivateEndpoint_Audit.json) |
|[Los temas de Azure Event Grid deben usar Private Link](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4b90e17e-8448-49db-875e-bd83fb6f804f) |Azure Private Link permite conectar la red virtual a los servicios de Azure sin una dirección IP pública en el origen o el destino. La plataforma de Private Link administra la conectividad entre el consumidor y los servicios en la red troncal de Azure. Mediante la asignación de puntos de conexión privados a los temas en lugar de a todo el servicio, además se protege frente a riesgos de pérdida de datos. Más información en: [https://aka.ms/privateendpoints](https://aka.ms/privateendpoints). |Audit, Disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Event%20Grid/EventGridTopics_EnablePrivateEndpoint_Audit.json) |
