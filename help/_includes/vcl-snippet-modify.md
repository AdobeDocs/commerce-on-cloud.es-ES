---
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---
# Incluir archivo para modificar VCL personalizado para Fastly

## Modificación del fragmento de VCL personalizado

1. [Inicie sesión](/help/get-started/onboarding.md#access-your-admin-panel) en el administrador.

1. Haga clic en **Tiendas** > **Configuración** > **Configuración** > **Avanzado** > **Sistema**.

1. Expandir **Caché De Página Completa** > **Configuración Rápida** > **Fragmentos De VCL Personalizados**.

   ![Administrar fragmentos personalizados de VCL](/help/assets/cdn/fastly-manage-snippets.png)

1. En la columna _Acción_, haga clic en el icono de configuración situado junto al fragmento que desea editar.

1. Después de que la página se vuelva a cargar, haz clic en **Cargar VCL a Fastly** en la sección _Configuración de Fastly_.

1. Una vez finalizada la carga, actualice la caché según la notificación que aparece en la parte superior de la página.

>[!WARNING]
>
>La opción de interfaz de usuario _Fragmentos personalizados de VCL_ solo muestra los fragmentos agregados mediante el administrador de Adobe Commerce. Si agrega fragmentos mediante la API de Fastly, use la API para [administrarlos](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
