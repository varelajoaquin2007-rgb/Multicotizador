# Diseño de datos — Catálogo vehicular, Geografía y Zona de riesgo

## Principio clave
**Geografía ≠ Tarifa.** La API Georef oficial de Argentina proporciona provincia/localidad y sus IDs; no proporciona códigos postales ni la segmentación de riesgo y recargos de cada aseguradora. Los datos geográficos están disponibles bajo licencia CC BY 4.0, según Datos Argentina.

Fuente: https://www.datos.gob.ar/dataset/jgm-servicio-normalizacion-datos-geograficos
API: https://apis.datos.gob.ar/georef/api

El frontend actual obtiene provincias y localidades en vivo; usa carga manual si el proveedor no responde. Código postal independiente introducido por el operador. No se asigna factor de precio por ubicación.

## Esquema para futura base PostgreSQL (aún no desplegada)
```sql
CREATE TABLE vehicle_brands (
  id BIGSERIAL PRIMARY KEY, source TEXT NOT NULL, external_id TEXT,
  name TEXT NOT NULL, UNIQUE(source, external_id)
);
CREATE TABLE vehicle_models (
  id BIGSERIAL PRIMARY KEY, brand_id BIGINT NOT NULL REFERENCES vehicle_brands(id),
  source TEXT NOT NULL, external_id TEXT, name TEXT NOT NULL,
  UNIQUE(source, external_id)
);
CREATE TABLE vehicle_versions (
  id BIGSERIAL PRIMARY KEY, model_id BIGINT NOT NULL REFERENCES vehicle_models(id),
  source TEXT NOT NULL, external_id TEXT, infoauto_code TEXT,
  name TEXT NOT NULL, model_year INTEGER NOT NULL,
  UNIQUE(source, external_id, model_year)
);
CREATE TABLE geo_provinces (
  id TEXT PRIMARY KEY, name TEXT NOT NULL
);
CREATE TABLE geo_localities (
  id TEXT PRIMARY KEY, province_id TEXT NOT NULL REFERENCES geo_provinces(id),
  name TEXT NOT NULL
);
CREATE TABLE insurers (
  id TEXT PRIMARY KEY, name TEXT NOT NULL
);
CREATE TABLE insurer_vehicle_map (
  insurer_id TEXT NOT NULL REFERENCES insurers(id),
  version_id BIGINT NOT NULL REFERENCES vehicle_versions(id),
  insurer_vehicle_code TEXT NOT NULL,
  PRIMARY KEY(insurer_id,version_id)
);
CREATE TABLE insurer_risk_zones (
  id BIGSERIAL PRIMARY KEY, insurer_id TEXT NOT NULL REFERENCES insurers(id),
  zone_code TEXT NOT NULL, label TEXT,
  UNIQUE(insurer_id,zone_code)
);
CREATE TABLE insurer_zone_mappings (
  id BIGSERIAL PRIMARY KEY,
  risk_zone_id BIGINT NOT NULL REFERENCES insurer_risk_zones(id),
  postal_code TEXT,
  geo_locality_id TEXT REFERENCES geo_localities(id),
  effective_from DATE NOT NULL,
  effective_to DATE,
  source TEXT NOT NULL,
  CHECK (postal_code IS NOT NULL OR geo_locality_id IS NOT NULL)
);
CREATE TABLE insurer_discount_rules (
  insurer_id TEXT PRIMARY KEY REFERENCES insurers(id),
  max_percent NUMERIC(5,2) NOT NULL CHECK (max_percent BETWEEN 0 AND 100),
  source TEXT NOT NULL,
  validated_at TIMESTAMPTZ
);
```

Los límites comunicados por Majoseg (no verificados contractualmente): Mercantil Andina 35%, MAPFRE 25%, Victoria 50%. Hasta obtener documentación real, NO agregar factores de zona ni tablas tarifarias calculadas.

**Importante:** los porcentajes de descuento configurados para Majoseg NO implican una ecuación uniforme de premio final. El valor válido se obtiene del tarifador de cada compañía, que además puede considerar uso, edad, seguridad, franquicia, cobertura, pagos, promociones, historial y suma asegurada.

## Catálogo de vehículos
El catálogo actual de la demo está limitado a ejemplos y no es exhaustivo; no atribuirle fidelidad técnica. Para producción:
1. Obtener licencia/canal autorizado de InfoAuto, proveedor de catálogo o API de aseguradora.
2. Importar versiones y años con identificadores del proveedor y mantener versión/fuente/fecha.
3. Mapear cada vehículo a códigos usados por los cotizadores de Mercantil, MAPFRE y Victoria.
4. Validar que modelo, versión y año estén efectivamente disponibles para cotizar.
5. Búsqueda por patente como fuente secundaria, con confirmación del operador.

## Próximo paso con compañías
Solicitar a cada compañía: códigos de vehículos, método de consulta de localidad/CP/zona de riesgo, disponibilidad de coberturas por riesgo y cotizaciones, forma válida de descuentos y vigencias. Si la API devuelve premio real, NO se recalcula la tarifa geográfica en Majoseg.

## Nota de seguridad
Un repositorio público/GitHub Pages no es una base de datos privada. En producción, catálogos y tarifas se sirven desde un backend seguro y los resultados, cotizaciones y datos de clientes se guardan con controles de acceso.
