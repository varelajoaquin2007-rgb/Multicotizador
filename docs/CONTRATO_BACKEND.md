# Contrato de integración previsto para el backend de Majoseg

Este documento define **rutas propias a desarrollar**. No representa la API real de AbsaNet ni de las aseguradoras.

## Flujo
1. `GET /api/vehicles/brands` -> [{id, name}].
2. `GET /api/vehicles/models?brandId=...&year=...` -> [{id, name}].
3. `GET /api/vehicles/versions?modelId=...&year=...` -> [{id, name, infoautoCode, year}].
4. `POST /api/vehicles/by-plate` con {plate} -> {found, vehicle?, confidence?, source?}; requerirá servicio licenciado y autorización.
5. `POST /api/quotes` con vehículo confirmado, zona, uso, descuentos por compañía y cobertura solicitada. Responde por compañía con ofertas, importes reales, vigencia, deducibles, errores y estado del conector.

## Descuentos
Aplicar límite inclusivo en el **servidor**:
- mercantil: 0–35%
- mapfre: 0–25%
- victoria: 0–50%

No asumir que es válido elegir cualquier porcentaje intermedio; el adaptador deberá validar la lista o política real de cada compañía. Si un descuento no está autorizado, responder error, nunca aproximar el cálculo.

## Coberturas
Un `coverageSelection: "all"` significa obtener las coberturas reales habilitadas para ese riesgo en cada compañía. Una compañía puede no ofrecer todo riesgo por antigüedad, uso o zona. No sintetizar productos.

## Seguridad
Endpoints autenticados, HTTPS, secretos en servidor, rate limiting, auditoría, datos mínimos, tratamiento de datos personales según la legislación aplicable. GitHub Pages solo para interfaz; **no colocar claves ni contraseñas allí**.
