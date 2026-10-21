# Investigación técnica — AbsaNet, InfoAuto y MAPFRE Next
Fecha: 2026-10-21

## Confirmaciones

- Majoseg actualmente cotiza ingresando por separado a los portales de las compañías; **MAPFRE Autos se cotiza desde MAPFRE Next**, según información operativa provista por Majoseg. MAPFREnet y MAPFRE Next no deben considerarse equivalentes.
- Mercantil Andina: descuento máximo comunicado 35 %.
- MAPFRE: descuento máximo comunicado 25 %.
- Victoria: descuento máximo comunicado 50 %.
- Los descuentos deberán validarse contra las reglas efectivas de cada producto/código de productor.

## AbsaNet
Fuentes:
- https://absanetservices.net/
- https://absanetservices.net/cotizaciones/swagger/index.html
- https://absanetservices.net/utilidades/swagger/index.html
- https://absanetservices.net/emisiones/swagger/index.html

El sitio principal declara cotización y emisión de Autos para MAPFRE, Mercantil Andina y otras. **No se confirmó Victoria** como compañía compatible.

El Swagger de cotizaciones no permitió recuperar en esta investigación el esquema OpenAPI completo, así que **no están verificados** sus endpoints específicos, payloads de cotización, mecanismo de autenticación, descuentos ni operación de «todas las coberturas». No inventar contratos.

El Swagger de utilidades sí publica:
- GET /infoauto/marcas
- GET /infoauto/marcas/{idMarca}/modelos
- GET /infoauto/marcas/{idMarca}/modelos/{idModelo}/vehiculos
- GET /infoauto/vehiculos/{infoAuto}/valores
- GET /infoauto/vehiculos/{anio}/{descripcion}
- GET /infoauto/vehiculos

Se verificó además API de emisión POST /auto. **Emitir no es lo mismo que cotizar**; no usarla sin proceso de autorización y confirmación explícita.

## Identificación vehicular

InfoAuto ofrece codificación de vehículos y referencias de valor; aplicaciones de aseguradoras utilizan códigos InfoAuto. Ejemplo independiente de Experta: https://autos-developers.apiexperta.com.ar/docs/APIS-DOCS/Autos-API/cotizacion-AUTOS/index.html que requiere `codInfoAuto`.

ACARA publica valuaciones por marca/modelo/año pero prohíbe reproducción total o parcial de su guía; no extraer/cargar automáticamente su contenido sin licencia: https://www.acara.org.ar/guia-oficial-de-precios

La patente requiere otra fuente registrada/autorizada. No inferir versión exacta del dominio, ni asumir que la marca/versión devueltas por terceros coinciden con código de tarifación; permitir confirmación y corrección.

## Arquitectura objetivo
1. Frontend GitHub Pages: UI demostrativa **sin secretos**.
2. Backend autenticado alojado fuera de Pages: `GET /catalog/brands`, `GET /catalog/models?brandId=&year=`, `GET /catalog/vehicles?modelId=&year=`, `POST /vehicle/lookup`, `POST /quotes` son **rutas propuestas de Majoseg, NO endpoints AbsaNet**.
3. Adaptador de catálogo InfoAuto por AbsaNet (si hay habilitación): mantener identificador InfoAuto, año de modelo, descripción y equivalencias específicas de aseguradora.
4. Adaptadores cotizadores independientes: AbsaNet para MAPFRE y Mercantil si la relación comercial funciona; integrar Victoria directamente o por tercero autorizado.
5. Si la cobertura es `all`, consultar todas las habilitadas según compañía y riesgo; guardar nombres originales y franquicias, no inventar coberturas.
6. Descuentos con máximos en UI **y backend**: 35 / 25 / 50 respectivamente; enviar porcentaje permitido al tarifador si admite parámetros, nunca restar automáticamente al premio final de un cálculo real.
7. Registrar estados por aseguradora, precio/premio, impuestos, sumas aseguradas, franquicias, vigencia y errores separados.

## Consultas pendientes a AbsaNet
- ¿Pueden habilitar cuenta API o QA para Majoseg con su código PAS/organizador y cotizar MAPFRE/Mercantil?
- ¿Precio del servicio, costos por solicitud, límites, ambiente sandbox, forma de autenticación y licencia de InfoAuto?
- ¿Cómo se parametriza cada código de productor y descuento comercial?
- ¿El endpoint de cotización devuelve **todas** las ofertas/coberturas de cada compañía?
- ¿Los catálogos de InfoAuto admiten filtrado por año de fabricación/modelo y qué validez tienen?
- ¿Tienen servicio de patente argentino o integración con proveedor?
- ¿Conectan Victoria, aunque no figure entre las compañías publicadas?
- ¿MAPFRE Next requiere credenciales separadas, autorización especial o conexión por AbsaNet?

## No verificado
- Acceso real de Majoseg a AbsaNet, InfoAuto o APIs de compañías.
- Equivalencia exacta entre versión InfoAuto y catálogo de cada aseguradora.
- Consulta por patente disponible.
- Tarifas reales o disponibilidad de coberturas.

No almacenar credenciales, tokens o datos personales en GitHub Pages ni en este repositorio público.
