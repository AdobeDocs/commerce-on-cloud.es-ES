---
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---
# Incluir archivo para eliminar VCL personalizado para Fastly

## Eliminar el fragmento de VCL personalizado

1. [Inicie sesión](/help/get-started/onboarding.md#access-your-admin-panel) en el administrador.

1. Haga clic en **Tiendas** > **Configuración** > **Configuración** > **Avanzado** > **Sistema**.

1. Expandir **Caché De Página Completa** > **Configuración Rápida** > **Fragmentos De VCL Personalizados**.

   ![Administrar fragmentos personalizados de VCL](/help/assets/cdn/fastly-manage-snippets.png)

1. En la columna _Acción_, haga clic en el icono de papelera situado junto al fragmento que desea eliminar.

1. En la siguiente ventana modal, haz clic en **DELETE** y activa una nueva versión.

>[!WARNING]
>
>La opción de interfaz de usuario _Fragmentos personalizados de VCL_ solo muestra los fragmentos agregados mediante el administrador de Adobe Commerce. Si agrega fragmentos mediante la API de Fastly, use la API para [administrarlos](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-vcl-using-the-api).
