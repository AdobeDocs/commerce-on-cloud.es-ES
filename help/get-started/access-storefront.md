---
title: Acceso a su panel de administración de Commerce
description: Obtenga información sobre cómo acceder a su Panel de administración de Commerce.
recommendations: noDisplay, catalog
exl-id: 827417b0-9048-44d8-8c82-07befba476c7
TQID: https://experienceleague.adobe.com/V3BXuCc9aqT5YuyIS8WAZgUdPAYNhQunAgg2i2FCaOs
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 361
ht-degree: 0%

---

# Acceso a su panel de administración de Commerce

Los usuarios que tienen acceso administrativo al Panel de administración de Commerce pueden agregar usuarios, configurar servicios de tienda, completar el trabajo de configuración y personalización de la tienda, etc.

En un proyecto nuevo, el primer paso después de recibir el correo electrónico de bienvenida es proteger el acceso de los administradores al proyecto cambiando la contraseña en la cuenta del propietario de la licencia. El nombre de usuario predeterminado para esta cuenta es la dirección de correo electrónico del propietario de la licencia.

Puede enviar una solicitud de cambio de contraseña utilizando cualquiera de los siguientes métodos:

- Busque el correo electrónico de bienvenida enviado a la dirección de correo electrónico del propietario de la licencia, siga el vínculo y cambie la contraseña.

- Copie la dirección URL del almacén de [[!DNL Cloud Console]](../cloud-guide/project/overview.md) en un explorador. A continuación, anexe `/admin` al final de la dirección URL para abrir la página de inicio de sesión. Haga clic en **Olvidó la contraseña?** para enviar una solicitud de cambio de contraseña a la dirección de correo electrónico del propietario de la licencia.

Después de enviar la solicitud de cambio de contraseña, compruebe en su correo electrónico la notificación de restablecimiento de contraseña. Si no recibe el correo electrónico, compruebe su carpeta de correo no deseado.

>[!TIP]
>
>Si el restablecimiento de la contraseña falla o no puede iniciar sesión en el Panel de administración, un usuario con acceso de administrador puede conectarse al proyecto mediante SSH y agregar un usuario administrador mediante el comando CLI `admin:user:create`. Consulte [Crear, editar o desbloquear una cuenta de administrador](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html?lang=es) en la _Guía de instalación_.

## Monitorización del estado del sitio

[Site-Wide Analysis Tool](https://experienceleague.adobe.com/es/docs/commerce-operations/tools/site-wide-analysis-tool/intro) es una herramienta proactiva de autoservicio y un repositorio central que incluye información detallada del sistema y recomendaciones para garantizar la seguridad y la operabilidad de la instalación de Adobe Commerce. Proporciona monitorización del rendimiento, informes y consejos en tiempo real las 24 horas del día, los 7 días de la semana para identificar posibles problemas y una mejor visibilidad de las configuraciones de salud, seguridad y aplicaciones del sitio. Ayuda a reducir el tiempo de resolución y a mejorar la estabilidad y el rendimiento del sitio. Puede acceder a la herramienta de análisis de todo el sitio directamente desde el [panel de administración](https://experienceleague.adobe.com/es/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel).
