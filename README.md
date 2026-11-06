# DevOps · K3s en Azure

Repositorio base de la Práctica 5. Reutiliza la API Express y PostgreSQL del ejemplo local y agrega publicación de imágenes con GitHub Actions, aprovisionamiento mediante Terraform e instalación de K3s en una VM Linux de Azure.

La explicación conceptual, el procedimiento de clase y las comprobaciones se encontrarán en el **Markdown del curso**. Este README identifica el material y las intervenciones previstas; no sustituye la guía.

## Archivos

- `app/`: API Express, Dockerfile y pruebas automatizadas.
- `.github/workflows/publish-image.yml`: pruebas y construcción de la imagen; publicación en GHCR inicialmente desactivada.
- `azure/terraform/`: recursos Azure y configuración `cloud-init` para instalar K3s.
- `k8s/`: Namespace, ConfigMap, PostgreSQL, PVC, Services, Deployment de Express e Ingress.

## Trabajo que se completa en la práctica

1. Cambiar `push: false` a `push: true` en el workflow y ejecutarlo desde GitHub Actions.
2. Agregar el output `resource_group_name` en `azure/terraform/outputs.tf`.
3. Sustituir la referencia de imagen de ejemplo en `k8s/06-api-deployment.yaml` por la imagen publicada en GHCR, con su etiqueta asociada al commit.
4. Desplegar la aplicación y comprobar la persistencia de las notas después del reemplazo del Pod de PostgreSQL.

El paquete publicado en GHCR debe poder descargarse desde K3s. La visibilidad del repositorio GitHub y la del paquete GHCR son independientes; la guía explicará cómo habilitar la descarga pública del paquete o una alternativa de autenticación.

## Requisitos

- Git, Azure CLI, Terraform y OpenSSH, desde Windows PowerShell o una terminal Linux.
- Suscripción Azure con permisos y cuota para crear una VM.
- Clave pública SSH y dirección IPv4 pública propia.
- Acceso a GitHub Actions y GitHub Container Registry.

K3s se instala **en la VM Azure**, no en el equipo local. Los estudiantes no deberán ejecutar scripts Bash complejos desde Windows.

## Seguridad y costos

- No versionar `terraform.tfvars`, contraseñas, archivos de estado Terraform ni kubeconfigs.
- El NSG restringe SSH a la dirección `/32` que se configure. HTTP se expone temporalmente en el puerto 80 para la demostración.
- El servidor de un solo nodo y el volumen local `local-path` no proporcionan alta disponibilidad.
- El laboratorio no utiliza TLS y no debe manejar datos reales ni exponerse de forma permanente.
- Al finalizar, ejecutar `terraform destroy` y **comprobar la eliminación de los recursos**. El resultado depende de la suscripción y de las cuotas disponibles.

## Documentación de referencia

- [K3s](https://docs.k3s.io/)
- [Terraform AzureRM](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs)
- [Azure cloud-init](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/using-cloud-init)
- [GitHub Container Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)

**Estado:** estructura inicial publicada. Aún falta validar una ejecución completa en una suscripción de Azure antes de utilizar este material en clase.
