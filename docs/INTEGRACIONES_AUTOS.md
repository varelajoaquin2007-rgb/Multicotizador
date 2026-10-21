# Plan de integraciones — Autos (investigación 2026-10-21)

## Objetivo
Cotizar en Mercantil Andina, MAPFRE y Victoria desde Majoseg, respetando **topes de descuento**: Mercantil 35%, MAPFRE 25%, Victoria 50%. Los topes son los informados por Majoseg, sujetos a validación comercial según el producto y código de productor.

## Estado por aseguradora

| Compañía | Hallazgo público | Próxima acción | Estado |
| --- | --- | --- | --- |
| Mercantil Andina | Portal oficial de APIs de automotores (consulta de vehículo, cotización, emisión). Ofrece entorno QA. https://apiportal.mercantilandina.com.ar/ y https://apiportal.mercantilandina.com.ar/faqs | Solicitar habilitación y documentación técnica para la cuenta Majoseg | Integración no habilitada |
| MAPFRE Argentina | Productores tienen acceso a MAPFREnet, pero no se confirmó API pública argentina de Autos. https://www.mapfre.com.ar/productores/ | Pedir integración API/Web Service o tercero autorizado | Sin API confirmada |
| Victoria Seguros | Web/app para productores y cotización; no se confirmó API pública. https://www.victoria.com.ar/tecnologia/ | Pedir integración API/Web Service o alternativa autorizada | Sin API confirmada |

### Alternativa de integración
AbsaNet API declara servicios de cotización automotor con MAPFRE y Mercantil Andina, entre otras: https://absanetservices.net/ . Confirmar acceso, tarifas, compatibilidad con cuentas de Majoseg y descuentos. **No se confirmó Victoria** en ese servicio.

## Arquitectura recomendada

GitHub Pages solo aloja el frontend público de demostración. Para producción:
- Backend autenticado y separado (por ejemplo FastAPI en un servidor HTTPS).
- Secretos y credenciales almacenados en gestor de secretos, nunca en HTML/JS, repositorio público o localStorage.
- Interfaz estable de cotización: `quote({ vehicle, riskPostalCode, coverageSelection, discounts, producerAccount })`.
- Adaptadores independientes `MercantilAdapter`, `MapfreAdapter`, `VictoriaAdapter`; pueden devolver múltiples ofertas con identificador de cobertura, precio, impuestos, franquicia, vigencia, y estado de error.
- Catálogo vehicular desde fuente licenciada o catálogo autorizado de aseguradora. El mapeo marca/modelo/versión del frontend a códigos propios de las compañías debe ser explícito; validar coincidencias y permitir confirmación manual.
- Consulta por patente exclusivamente a través de un proveedor licenciado o habilitado; requerir consentimiento/base legal aplicable.
- Si el operador selecciona "todas", solicitar todas las coberturas autorizadas para el riesgo, sin asumir que siempre existen RC, terceros o TR en cada compañía.
- Validación estricta de topes en backend, además del selector visual.
- Tarifas: no aplicar porcentajes matemáticamente al importe final si la API maneja descuentos sobre prima u otros conceptos. Conservar respuesta autorizada de la aseguradora.
- Autorización expresa para cualquier automatización de portales con usuarios.

## Preguntas para pedir al ejecutivo comercial/técnico
1. ¿Majoseg tiene autorización de consumo de API para cotizaciones automotor con su/s código/s de productor?
2. ¿Disponen de documentación Swagger/OpenAPI o WSDL y de ambiente QA con credenciales de pruebas?
3. ¿Qué parámetro representa descuentos comerciales, qué topes y combinaciones están autorizados, y en qué instancia se calculan?
4. ¿Cómo se consulta catálogo de marcas, modelos, versiones y códigos internos?
5. ¿Cómo se solicitan **todas las coberturas disponibles**, sus franquicias, vigencia y precio final?
6. ¿Hay restricciones de uso, costos, límites de solicitudes o acuerdos de seguridad?
7. ¿El sistema puede trabajar en nombre de un broker con varios códigos de productor?

## Estado actual del código
`index.html` es una demo estática: ni las compañías ni los proveedores vehiculares están conectados; los precios son **ficticios** y nunca deben presentarse a clientes como ofertas reales.

## Secuencia de implementación
1. Conseguir acceso QA de Mercantil y ejemplos de cotización de vehículo/coberturas.
2. Backend privado y adaptador Mercantil con pruebas comparadas contra portal oficial.
3. Servicio de catálogo vehicular autorizado, luego consulta por patente.
4. Habilitación de MAPFRE y Victoria, directa o mediante tercero con permiso.
5. Comparación de coberturas, franquicias y precios reales y registro de auditoría.
