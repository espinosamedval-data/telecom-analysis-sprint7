# Análisis de Segmentación de Clientes — ConnectaTel (Telecom-analysis-sprint7)

Análisis exploratorio de datos de uso (llamadas, mensajes y minutos) para segmentar la base de usuarios de ConnectaTel según edad y nivel de uso, con el fin de identificar oportunidades comerciales y mejoras en la oferta de planes.

## 📋 1. Calidad inicial de los datos

El dataset presentaba varios problemas que requirieron limpieza antes de un análisis confiable:

- **Valores sentinel en `age`**: la columna contenía el valor `-999` como marcador de dato faltante, afectando **47 registros**.
- **Ciudades inválidas**: la columna `city` tenía el valor `"?"` en **96 registros** (≈2.4% del total de usuarios), representando datos faltantes disfrazados de categoría válida.
- **Fechas incorrectas o futuras**: al convertir `reg_date` y `date` a formato de fecha, se detectaron ~**50 registros** (~0.1%) que no pudieron convertirse correctamente o correspondían a fechas fuera del rango esperado (posteriores a 2024), marcados como `NaT`.

Estos problemas afectaban una porción pequeña pero significativa del dataset, reforzando la necesidad de un pipeline de limpieza sistemático antes de segmentar usuarios.

## 👥 2. Segmentos identificados

Se crearon dos ejes de segmentación:

- **Por edad (`grupo_edad`)**: los usuarios se distribuyen de forma uniforme entre los 18 y 80 años, sin un grupo etario dominante.
- **Por nivel de uso (`grupo_uso`)**:
  | Grupo | % de usuarios |
  |---|---|
  | Bajo uso | 19.4% |
  | Uso medio | 73.5% |
  | Alto uso | 6.9% |

Al cruzar ambos ejes, el plan **Premium** mantiene una proporción relativa más alta en usuarios de mediana edad (35-50 años) y adultos mayores (75+), mientras que el plan **Básico** domina en el resto de los rangos etarios, sin diferencias dramáticas en el patrón general de uso entre planes.

## 💎 3. Segmentos más valiosos

Los usuarios de **Alto uso**, especialmente en el rango 35-50 años con plan Premium, representan el segmento más valioso: combinan alta frecuencia de interacción con mayor disposición a pagar por un plan premium, sugiriendo mayor lealtad y potencial de ingreso recurrente.

## 📈 4. Patrones de uso extremo (outliers)

- **`cant_mensajes`** y **`cant_llamadas`**: outliers moderados (12-17 mensajes, 11-15 llamadas), consistentes con usuarios de alto potencial real, no errores de datos.
- **`cant_minutos_llamada`**: cola de outliers mucho más extensa, con valores de hasta **155 minutos** frente a un límite superior esperado de ~62 minutos. Sugiere un subgrupo de sesiones inusualmente largas (posible uso profesional/soporte, o casos que requieren revisión).

**Implicación de negocio**: este segmento de alto consumo en minutos podría beneficiarse de tarifas diferenciadas por volumen, en vez de la tarifa estándar actual.

## 💡 5. Recomendaciones

1. **Plan intermedio "Plus"**: oferta entre Básico y Premium dirigida a usuarios de Uso medio, hoy posiblemente sub-atendidos.
2. **Plan de minutos ilimitados o por volumen**: dirigido al segmento de alto consumo detectado en `cant_minutos_llamada`.
3. **Estrategia de retención 35-50 años**: reforzar beneficios Premium hacia este grupo, el de mayor valor identificado.
4. **Prevención en la captura de datos**: implementar validaciones en el formulario de registro (ciudad, edad, fecha) para **evitar de raíz** que valores como `-999` o `"?"` puedan ingresar al sistema, en lugar de depender de limpieza posterior a la carga de datos.
