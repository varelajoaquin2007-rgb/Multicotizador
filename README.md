# Majoseg · Multicotizador Automotor

Prototipo inicial del multicotizador de Majoseg, preparado para GitHub Pages.

## Funcionalidades de la versión 0.1

- Carga manual de patente, año, marca, modelo y código postal.
- Selección de uso y cobertura.
- Activación/desactivación de aseguradoras de demostración.
- Descuentos individuales sujetos a **topes ficticios**.
- Comparador de resultados con **precios ficticios**.
- Interfaz adaptable a computadoras y celulares.

**No hay integración real con aseguradoras, consulta de patente, login ni datos guardados.** No utilizar resultados simulados como presupuestos para clientes.

## Publicación en GitHub Pages

1. Abrir **Settings → Pages** en este repositorio.
2. En *Build and deployment*, elegir **Deploy from a branch**.
3. Seleccionar rama **main**, carpeta **/(root)** y guardar.
4. Abrir [Multicotizador](https://varelajoaquin2007-rgb.github.io/Multicotizador/) una vez que GitHub publique la página.

El archivo `index.html` es autocontenido, sin herramientas de compilación ni dependencias externas.

## Arquitectura proyectada

- **Frontend**: sitio público de presentación y entorno de prueba; más adelante interfaz autenticada.
- **Backend (separado de GitHub Pages)**: API segura para proveedores de datos vehiculares y conectores a compañías aseguradoras.
- **Reglas comerciales**: por compañía, cobertura, producto, cuenta de productor, límites y vigencias, verificadas por el backend.
- **Respuestas**: comparar premios reales, impuestos, franquicias, vigencia y errores de cada cotizador.
- **Seguridad**: control de acceso, registros de auditoría, protección de datos y secretos solo en backend.

## Seguridad

GitHub Pages publica recursos accesibles a cualquiera. **Nunca subir claves API, contraseñas de compañías, cookies de sesión, tokens, datos personales de asegurados ni credenciales de PAS al repositorio**.

## Próximo paso

Definir compañías prioritarias, comprobar mecanismos de integración autorizados y validar reglas comerciales reales. Empezar por una única aseguradora, con su ambiente de pruebas, antes de automatizar cotizaciones reales.
