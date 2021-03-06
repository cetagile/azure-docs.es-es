---
title: 'Tutorial: Configuración de DocuSign para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y DocuSign.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/21/2020
ms.author: jeedes
ms.openlocfilehash: dc3f307a21b746981a84b1c0747c4b22c448541f
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96349912"
---
# <a name="tutorial-configure-docusign-for-automatic-user-provisioning"></a>Tutorial: Configuración de DocuSign para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es explicar los pasos que hay que realizar en DocuSign y Azure AD para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Azure AD en DocuSign.

## <a name="prerequisites"></a>Requisitos previos

En la situación descrita en este tutorial se supone que ya cuenta con los elementos siguientes:

*   Un inquilino de Azure Active Directory.
*   Una suscripción habilitada para el inicio de sesión único en DocuSign.
*   Una cuenta de usuario de DocuSign con permisos de administrador de equipo.

## <a name="assigning-users-to-docusign"></a>Asignación de usuarios a DocuSign

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de cuentas de usuario, solo se sincronizarán los usuarios y grupos que se han "asignado" a una aplicación en Azure AD.

Antes de configurar y habilitar el servicio de aprovisionamiento, debe decidir qué usuarios o grupos de Azure AD representan a los usuarios que necesitan acceso a la aplicación DocuSign. Una vez decidido, puede asignar estos usuarios a la aplicación de DocuSign siguiendo estas instrucciones:

[Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-docusign"></a>Sugerencias importantes para asignar usuarios a DocuSign

*   Se recomienda asignar un único usuario de Azure AD a DocuSign para probar la configuración de aprovisionamiento. Más tarde, se pueden asignar otros usuarios.

*   Al asignar a un usuario a DocuSign, debe seleccionar un rol de usuario válido. El rol "Acceso predeterminado" no funciona para realizar el aprovisionamiento.

> [!NOTE]
> Azure AD no admite el aprovisionamiento de grupos con la aplicación de Docusign, solo se pueden aprovisionar usuarios.

## <a name="enable-user-provisioning"></a>Habilitación del aprovisionamiento de usuarios

Esta sección lo guía a través de los pasos necesarios para conectar la API de aprovisionamiento de cuentas de usuario de DocuSign, así como para configurar el servicio de aprovisionamiento con el fin de crear, actualizar y deshabilitar cuentas de usuario asignadas en DocuSign en función de la asignación de grupos y usuarios de Azure AD.

> [!Tip]
> También puede habilitar el inicio de sesión único basado en SAML para DocuSign siguiendo las instrucciones de [Azure Portal](https://portal.azure.com). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático, aunque estas dos características se complementan entre sí.

### <a name="to-configure-user-account-provisioning"></a>Para configurar el aprovisionamiento automático de cuentas de usuario:

El objetivo de esta sección es describir cómo habilitar el aprovisionamiento de usuarios de Active Directory para DocuSign.

1. En [Azure Portal](https://portal.azure.com), vaya a la sección **Azure Active Directory > Aplicaciones empresariales > Todas las aplicaciones**.

1. Si ya ha configurado DocuSign para el inicio de sesión único, busque la instancia de DocuSign mediante el campo de búsqueda. En caso contrario, seleccione **Agregar** y busque **DocuSign** en la galería de aplicaciones. Seleccione DocuSign en los resultados de búsqueda y agréguelo a la lista de aplicaciones.

1. Seleccione la instancia de DocuSign y luego, la pestaña **Aprovisionamiento**.

1. Establezca el **modo de aprovisionamiento** en **Automático**. 

    ![Captura de pantalla de la pestaña Aprovisionamiento de DocuSign en Azure Portal. El modo de aprovisionamiento se establece en Automático y Nombre de usuario de administrador, Contraseña y Probar conexión se resaltan.](./media/docusign-provisioning-tutorial/provisioning.png)

1. En la sección **Credenciales de administrador**, proporcione los siguientes valores de configuración:
   
    a. En el cuadro de texto **Nombre de usuario administrador**, escriba un nombre de cuenta de DocuSign que tenga asignado el perfil **Administrador del sistema** en DocuSign.com.
   
    b. En el cuadro de texto **Contraseña de administrador**, escriba la contraseña de esta cuenta.

> [!NOTE]
> Si se ha configurado el inicio de sesión único y el aprovisionamiento de usuarios, las credenciales de autorización usadas para el aprovisionamiento deben estar configuradas para funcionar tanto con el inicio de sesión único como con el nombre de usuario y contraseña.

1. En Azure Portal, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a la aplicación de DocuSign.

1. Escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error aprovisionamiento en el campo **Correo electrónico de notificación** y active la casilla.

1. Haga clic en **Guardar**.

1. En la sección Asignaciones, seleccione **Synchronize Azure Active Directory Users to DocuSign** (Sincronizar usuarios de Azure Active Directory con DocuSign).

1. En la sección **Attribute Mappings** (Asignaciones de atributos), revise los atributos de usuario que se sincronizan entre Azure AD y DocuSign. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de DocuSign con el objetivo de realizar operaciones de actualización. Seleccione el botón Guardar para confirmar los cambios.

1. Para habilitar el servicio de aprovisionamiento de Azure AD para DocuSign, cambie el **Estado de aprovisionamiento** a **Activado** en la sección Configuración.

1. Haga clic en **Guardar**.

Esta acción inicia la sincronización inicial de todos los usuarios asignados a DocuSign en la sección Usuarios y grupos. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si se está ejecutando el servicio. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y hacer clic en los vínculos a los registros de actividad de aprovisionamiento, que describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento en la aplicación de DocuSign.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="troubleshooting-tips"></a>Sugerencias para la solución de problemas
* El aprovisionamiento de un rol o perfil de permiso para un usuario de Docusign se puede realizar mediante una expresión en las asignaciones de atributos con las funciones [switch](../app-provisioning/functions-for-customizing-application-data.md#switch) y [singleAppRoleAssignment](../app-provisioning/functions-for-customizing-application-data.md#singleapproleassignment). Por ejemplo, la expresión siguiente aprovisionará el identificador "8032066" cuando un usuario tenga el rol "Administrador de DS" asignado en Azure AD. No se aprovisionará ningún perfil de permiso si el usuario no tiene asignado un rol en Azure AD. El identificador se puede recuperar en el portal de [DocuSign](https://support.docusign.com/articles/Default-settings-for-out-of-the-box-DocuSign-Permission-Profiles).

Switch(SingleAppRoleAssignment([appRoleAssignments])," ", "8032066", "DS Admin")


## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configuración del inicio de sesión único](docusign-tutorial.md)