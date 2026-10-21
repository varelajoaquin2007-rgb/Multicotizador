# Majoseg — Multicotizador Autos

Prototipo estático para GitHub Pages: https://varelajoaquin2007-rgb.github.io/Multicotizador/

## Versión 0.2

- Ramo **Autos**.
- Aseguradoras seleccionables: **Mercantil Andina (35%)**, **MAPFRE (25%)**, **Victoria (50%)**.
- Descuentos precargados conforme a los valores informados por Majoseg. **No están verificados como límites contractuales**. Los porcentajes editables son solo para probar la interfaz.
- Selector dependiente de **marca → año → modelo → versión**, con catálogo **ilustrativo y limitado** que no representa los padrones oficiales de las compañías.
- Opción de modelo y versión manual.
- Patente y código postal del riesgo.
- Cobertura predeterminada: **Todas las coberturas disponibles**. En la demo se muestran RC, terceros completo y todo riesgo con franquicia; en producción cada compañía debe responder con sus coberturas efectivamente disponibles.
- Tabla de resultados simulados, ordenados por precio ficticio.

## Importante

Esta versión **NO se conecta a ninguna aseguradora ni consulta patentes**. Los precios, bases y cálculos son totalmente ficticios. No usar como presupuesto ni oferta comercial. Los datos del formulario permanecen en el navegador y no se envían a ningún servicio.

## Publicación

En GitHub, abrir **Settings > Pages**. Elegir **Deploy from a branch**, rama **main** y carpeta **/(root)**. El archivo `index.html` es autocontenido y no requiere compilación.

## Camino hacia cotizaciones reales

1. Confirmar los descuentos autorizados y posibles restricciones por compañía, producto y PAS.
2. Solicitar documentación y habilitación de API/Web Service de cada compañía; si no existe, evaluar integrador autorizado.
3. Elegir un proveedor permitido de información de vehículos y validar su cobertura, exactitud, precio y licencia. El catálogo de demostración deberá reemplazarse por un catálogo real y por adaptadores de códigos de modelos de cada compañía.
4. Crear backend privado fuera de GitHub Pages para manejar secretos, autenticación, llamadas a APIs y reglas comerciales.
5. Consultar en paralelo todas las coberturas disponibles cuando el operador no seleccione ninguna; conservar descripción, límites, franquicias, impuestos y fecha de validez de cada oferta.
6. Validar resultados contra cotizaciones manuales reales con los permisos correspondientes.

**Nunca guardar credenciales de PAS, contraseñas, tokens, datos de asegurados ni claves API en GitHub Pages o repositorios públicos.**
