
# Automatización de Despliegues con GitHub Actions y Docker 🚀🐳

## ¿Por qué es importante DevOps para la integración y despliegue continuo de contenedores?

La integración y despliegue continuo (CI/CD) son fundamentales en el mundo de DevOps. Cuando gestionas múltiples contenedores en producción, realizar tareas manuales como construcción de imágenes o despliegues se vuelve insostenible.

➡️ Aquí es donde entra la automatización: permite que cada cambio en el código se despliegue automáticamente al hacer un **commit**. Esto garantiza:

- Flujo de trabajo más eficiente
- Menos errores humanos
- Despliegue más rápido y seguro

---

## ¿Cómo se configura un flujo de trabajo de integración continua?

1. **Organiza tus archivos**  
   Crea una carpeta en tu repo llamada `CI/` que contenga tu Dockerfile:

```Dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
```

2. **Sube los cambios al repositorio de GitHub:**

```bash
git status
git add .
git commit -m "carpeta CI agregada"
git push
```

---

## ¿Cómo integrar GitHub Actions con Docker?

Crea un archivo `.yml` dentro de `.github/workflows/`:

```yaml
name: DockerCI

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Login to DockerHub
        run: echo "${{ secrets.DOCKERHUB_TOKEN }}" | docker login -u "${{ secrets.DOCKERHUB_USER }}" --password-stdin

      - name: Build Docker image
        run: docker build --platform linux/amd64 -t ${{ secrets.DOCKERHUB_USER }}/tu-imagen:latest .

      - name: Push Docker image
        run: docker push ${{ secrets.DOCKERHUB_USER }}/tu-imagen:latest
```

🔐 **Credenciales necesarias** (en GitHub > Settings > Secrets > Actions):
- `DOCKERHUB_USER` → tu usuario de DockerHub
- `DOCKERHUB_TOKEN` → token generado en https://hub.docker.com/settings/security

---

## ¿Qué ventajas ofrece automatizar el despliegue de contenedores?

✅ **Ahorro de tiempo:** despliegues más rápidos  
✅ **Menos errores:** evita pasos manuales propensos a fallos  
✅ **Consistencia:** mismo proceso en cada commit  
✅ **Escalabilidad:** ideal para microservicios

---

## 🌟 Consejos adicionales

- Usa tags semánticos (`v1.0.0`, `v2.1.5`) para versiones estables
- Integra pruebas automatizadas antes del build
- Usa `--platform linux/amd64` si trabajas desde macOS con M1/M2
- Puedes combinar con Deploy to Render, Railway, ECS, etc.

---

¡Ahora estás listo para automatizar tus builds y despliegues como un pro DevOps! 🔧🤖
