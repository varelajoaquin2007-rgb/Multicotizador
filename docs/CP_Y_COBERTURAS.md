# Autocompletado de CP y agrupación de coberturas — versión 0.3

## Cambios
- Búsqueda por **CP de 4 dígitos** usando `https://api.zippopotam.us/AR/{cp}`; sugiere provincia y localidad cuando encuentra coincidencia.
- Selección de **provincia/localidad** mediante Georef y búsqueda inversa de CP mediante endpoint Zippopotam.us de ciudad. Solo autocompleta CP cuando devuelve exactamente uno.
- Para zonas ambiguas o que no aparecen en el proveedor, el operador debe ingresar/confirmar los campos. Un mismo CP no necesariamente identifica un solo riesgo postal, y una localidad puede tener varios CP.
- **Todos los resultados ahora se agrupan por compañía**. Las coberturas de una misma aseguradora se muestran juntas, cada una con una tarjeta dentro de una grilla adaptable a pantalla.

## Limitaciones
Zippopotam.us es gratuito, con CORS, y utiliza información de GeoNames con cobertura/calidad variables; no es el servicio oficial de Correo Argentino ni un padrón asegurador. Solo soporta CP numéricos de 4 dígitos en Argentina. Para riesgo real, confirmar CP y el criterio de compañía; el CPA completo (letra + números + letras) no se resuelve todavía.

Referencias:
- https://api.zippopotam.us/
- https://docs.zippopotam.us/docs/getting-started/
- https://www.argentina.gob.ar/georef/referencia-completa-de-la-api

El prototipo conserva **cotizaciones ficticias**. Los descuentos y las bases tarifarias reales necesitan validación y acceso a las aseguradoras.
