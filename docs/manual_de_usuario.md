# Manual de Usuario
## FMA Adoption Intelligence — Tablero Analitico

**Version:** 1.0  
**Fecha:** Mayo 2026  
**Repositorio:** https://github.com/fma-adoption-intelligence  
**Usuarios destinatarios:** Product Managers del equipo FMA, ServiceTitan

---

## 1. Introduccion

Se desarrolló el tablero FMA Adoption Intelligence ya que el equipo de Product de ServiceTitan requería una herramienta centralizada para monitorear, predecir y priorizar la migración de técnicos desde la plataforma TMA hacia FMA. El sistema consolida tres niveles de análisis en un solo entorno ya que la gestión de aproximadamente 15.700 técnicos activos exige tanto una visión descriptiva del comportamiento de adopción como una capacidad predictiva que permita actuar con anticipación.

El tablero se diseñó para usuarios no técnicos ya que los principales destinatarios son Product Managers que toman decisiones de priorización sin necesidad de interpretar código ni modelos estadísticos. Toda la información se presenta en lenguaje de negocio ya que se busca reducir la fricción entre el análisis de datos y la acción operativa.

### 1.1 Alcance del sistema

Se cubre el ciclo completo de adopción de FMA desde la primera sesión del técnico hasta los 60 días posteriores ya que ese es el período crítico de consolidación del hábito. Se analizan las primeras dos semanas de actividad ya que los patrones de uso en ese período son los predictores más fuertes de retención sostenida.

### 1.2 Estructura del documento

Se describe en este manual la instalación del sistema, el acceso al tablero y el uso de cada una de las tres vistas disponibles. Se incluyen también las preguntas frecuentes y un glosario de términos ya que se anticipa que algunos conceptos técnicos pueden requerir clarificación para usuarios no familiarizados con modelos predictivos.

---

## 2. Acceso al sistema

### 2.1 Acceso local (primera vez)

Se requiere Python 3.10 o superior ya que las librerías utilizadas no son compatibles con versiones anteriores. Se recomienda seguir los cuatro pasos en el orden indicado ya que cada paso depende del anterior.

**Paso 1. Crear el entorno virtual**

Se crea un entorno virtual aislado ya que esto evita conflictos con otras instalaciones de Python en el equipo.

```
python3 -m venv .venv
```

**Paso 2. Activar el entorno e instalar dependencias**

Se activan las dependencias del proyecto ya que sin este paso el tablero no puede ejecutarse.

```
# macOS / Linux
source .venv/bin/activate

# Windows (PowerShell)
.venv\Scripts\Activate.ps1

pip install -r requirements.txt
```

**Paso 3. Generar los datos analiticos**

Se ejecuta el pipeline de datos ya que este paso transforma los datos fuente en los archivos que el tablero necesita para funcionar. La ejecución tarda aproximadamente 21 segundos con los datos de muestra incluidos (n=140 tecnicos, 700K+ eventos de sesion). El tiempo en produccion dependera del hardware y del volumen de datos.

```
python pipeline.py
```

**Paso 4. Iniciar el tablero**

Se lanza el tablero con el siguiente comando ya que Streamlit inicia un servidor local accesible desde el navegador.

```
streamlit run app.py
```

Se abre automáticamente en `http://localhost:8501`. Se puede compartir el tablero en la red local usando la URL `Network URL` que imprime Streamlit en la terminal.

---

### 2.2 Acceso en la nube

El tablero está publicado en Streamlit Community Cloud y puede accederse directamente desde cualquier navegador sin instalación local:

**https://fma-adoption-intelligence-4czccfxlxzkfdm6prmcns8.streamlit.app**

No se requieren credenciales ni configuración adicional ya que el acceso es público y el tablero carga automáticamente al abrir la URL.

---

## 3. Estructura general del tablero

Se organiza el tablero en tres vistas accesibles mediante pestañas en la parte superior ya que cada vista responde a un tipo de pregunta diferente.

| Pestana | Tipo de analisis | Pregunta que responde |
|---------|-----------------|----------------------|
| Factores de Adopcion | Descriptivo | Que comportamientos distinguen al tecnico que adopta FMA de forma sostenida? |
| Puntuacion de Riesgo | Predictivo | Que clientes tienen mayor riesgo de migracion fallida? |
| Guia de Migracion | Prescriptivo | Que acciones concretas se deben tomar por segmento de cliente? |

Se muestra en la barra superior la fecha de última actualización de los datos ya que permite al usuario verificar si el análisis refleja el estado actual de la migración.

---

## 4. Vista 1 — Factores de Adopcion

Se utiliza esta vista para comprender qué comportamientos en las primeras dos semanas de uso distinguen a los técnicos que adoptan FMA de manera sostenida de los que abandonan ya que esta información guía las estrategias de onboarding y activación temprana.

### 4.1 Filtros disponibles

Se pueden aplicar tres filtros ya que el análisis de factores varía por tipo de cliente y dispositivo.

| Filtro | Efecto |
|--------|--------|
| Industria | Se restringe el análisis a clientes de una industria específica ya que los patrones de adopción difieren entre sectores como HVAC, Plomería o Electricidad |
| Tamaño del tenant | Se filtra por tamaño del cliente (Small, Medium, Large, Enterprise) |
| Plataforma del dispositivo | Se filtra por sistema operativo del dispositivo del técnico ya que el comportamiento en iOS y Android puede presentar diferencias |

Se actualizan automáticamente todos los indicadores y gráficos al cambiar cualquier filtro ya que el tablero recalcula los resultados en tiempo real.

### 4.2 Indicadores hero

Se presentan cuatro métricas globales en la parte superior ya que ofrecen una lectura rápida del estado de adopción en la muestra filtrada.

- **Adoptantes sostenidos:** técnicos que completaron 15 o más días activos en los 60 días posteriores al período de observación ya que ese umbral define retención sostenida según la calibración del modelo.
- **Abandonaron FMA:** técnicos que no alcanzaron ese umbral.
- **Tasa de retención:** proporción de adoptantes sostenidos sobre el total ya que refleja la efectividad global de la migración.
- **Técnicos en muestra:** total de técnicos con datos suficientes para el análisis.

### 4.3 Recuadro de hallazgos clave

Se muestra un recuadro resumen con las diferencias más relevantes entre los dos grupos ya que sintetiza la información de la tabla comparativa en lenguaje de negocio. Los valores se actualizan automáticamente según los filtros aplicados.

### 4.4 Tabla comparativa — Perfil de las primeras 2 semanas en FMA

Se construye esta tabla para identificar qué variables diferencian estadísticamente a los técnicos que adoptan de los que abandonan ya que el análisis estadístico permite determinar cuáles factores son relevantes y cuáles son ruido.

Se muestran los promedios de cada grupo ya que la comparación directa de medias permite identificar la magnitud de la diferencia. Se incluye el p-valor de cada variable ya que indica si la diferencia observada es estadísticamente significativa o producto del azar.

- **p-valor menor a 0.05:** diferencia estadísticamente significativa.
- Las variables numéricas se analizan con prueba t de Welch ya que este test es robusto ante varianzas desiguales entre grupos.
- Las variables categóricas se analizan con chi cuadrado ya que permite comparar distribuciones de frecuencia entre grupos.
- Se indica el conteo de factores significativos ya que el requerimiento mínimo del sistema es 5 factores con p menor a 0.05.

### 4.5 Grafico de uso de funcionalidades

Se muestra el porcentaje de técnicos que utilizó cada funcionalidad en la semana 1 ya que el uso temprano de ciertas funcionalidades es un predictor fuerte de retención.

Se lee el gráfico comparando las barras de cada funcionalidad ya que las diferencias más grandes entre el grupo adoptante (azul) y el grupo abandono (gris) indican las funcionalidades críticas que se deben priorizar en el onboarding.

### 4.6 Top-10 variables — Importancia SHAP global

Se presenta el ranking de las 10 variables que más influyen en las predicciones del modelo ya que esta información orienta qué comportamientos deben activarse en los técnicos nuevos.

Se interpreta la longitud de cada barra como la importancia relativa de esa variable ya que el valor SHAP representa el impacto promedio de esa variable en la predicción del modelo. Las barras más largas indican los factores que más determinan si un técnico retiene o abandona FMA.

**Nota sobre causalidad:** los valores SHAP miden la asociación estadística entre cada variable y el riesgo de abandono ya que el modelo identifica patrones en los datos históricos. Una variable con alta importancia SHAP indica correlación con el resultado ya que no implica necesariamente una relación causal directa.

---

## 5. Vista 2 — Puntuacion de Riesgo

Se utiliza esta vista para identificar qué clientes requieren atención prioritaria ya que el modelo predice la probabilidad de retención de cada técnico y la agrega a nivel de cliente (tenant).

### 5.1 Filtros disponibles

| Filtro | Efecto |
|--------|--------|
| Nivel de riesgo | Se muestran solo los clientes del nivel seleccionado (Critico, Alto, Medio, Bajo) ya que permite concentrar la vista en los casos más urgentes |
| Estado de migracion | Se filtra por el porcentaje de técnicos ya migrados (Partial, Minimal, Majority, Complete) |
| Industria | Se filtra por sector del cliente |

### 5.2 Indicadores hero

Se presentan cinco métricas ya que reflejan el estado de riesgo general de la cartera de clientes en la muestra filtrada.

- **Clientes riesgo crítico:** clientes con score agregado de retención inferior al 30% ya que indican que la mayoría de sus técnicos tienen baja probabilidad de retener FMA.
- **Clientes riesgo alto:** clientes con score entre 30% y 50%.
- **Total clientes en muestra:** total de clientes con al menos un técnico nuevo en FMA.
- **Estancados en riesgo alto/crítico:** porcentaje de clientes con retención real inferior al 50% que el modelo clasifica en riesgo alto o crítico ya que este indicador valida la capacidad del modelo para identificar a los clientes realmente en dificultades (R1: umbral minimo = 70%; valor observado en la muestra: 86%).
- **AUC-ROC del modelo:** capacidad discriminativa del modelo (0.829) ya que supera el umbral mínimo requerido de 0.70. Se muestra un aviso de advertencia si este valor cae por debajo del umbral.

### 5.3 Tabla de clientes ordenados por riesgo

Se ordenan los clientes de menor a mayor score de retención ya que los que aparecen primero son los que requieren intervención más urgente.

| Columna | Descripcion |
|---------|-------------|
| Cliente | Identificador interno del cliente (ID) ya que no se muestran nombres por política de privacidad |
| Riesgo | Clasificación del nivel de riesgo del cliente en cuatro categorías |
| Score retención | Probabilidad media de retención de los técnicos del cliente ya que se representa visualmente con una barra de color: rojo indica mayor riesgo y verde indica menor riesgo |
| % en FMA | Porcentaje de técnicos del cliente que ya utilizan FMA |
| Tecnicos | Número total de técnicos del cliente en la muestra |
| En riesgo crítico | Número de técnicos individuales del cliente con score inferior al 30% |
| Accion sugerida | Recomendación de acción según el nivel de riesgo del cliente |

**Escala de niveles de riesgo:**

| Score | Nivel | Accion sugerida |
|-------|-------|-----------------|
| Menor al 30% | Critico | Contactar cliente inmediatamente |
| 30 a 50% | Alto | Atención prioritaria en próximos 14 días |
| 50 a 70% | Medio | Monitorear en ciclo bisemanal |
| Mayor al 70% | Bajo | En buen camino: seguimiento estándar |

### 5.4 Detalle de tecnicos por cliente

Se selecciona un cliente del menú desplegable ya que la tabla inferior muestra todos los técnicos de ese cliente con su score individual y sus factores SHAP determinantes.

Se utiliza la columna **Factores SHAP (top-3)** ya que indica las tres variables que más influyen en el score de ese técnico específico. Esta información permite articular intervenciones concretas con el equipo de Customer Success ya que cada técnico tiene un perfil de riesgo diferente.

---

## 6. Vista 3 — Guia de Migracion

Se utiliza esta vista para definir qué acciones tomar con cada cliente según su situación actual de migración ya que la priorización basada en datos permite enfocar los recursos del equipo donde el impacto es mayor.

### 6.1 Banner de estado del modelo

Se muestra un banner verde al inicio de la vista cuando el modelo supera el umbral de calidad (AUC mayor o igual a 0.70) ya que esto confirma que las recomendaciones están respaldadas por el modelo predictivo y no son meramente orientativas.

### 6.2 Indicadores de cartera

Se presentan dos métricas ya que resumen el estado general de la cartera de migración.

- **Clientes en estado intermedio:** total de clientes con al menos un técnico nuevo en FMA ya que estos son los clientes activos en el proceso de migración.
- **Umbral de inflexión social (40%):** porcentaje de técnicos en FMA a partir del cual la adopción tiende a auto-sostenerse ya que el efecto de red entre colegas impulsa la incorporación de nuevos usuarios sin intervención activa.

### 6.3 Segmentos de accion

Se clasifica cada cliente en uno de cuatro segmentos según el porcentaje de sus técnicos que ya utilizan FMA ya que la acción recomendada difiere significativamente según el nivel de penetración de la plataforma.

| Segmento | Criterio | Prioridad |
|----------|----------|-----------|
| Migrar con prioridad | Menos del 20% de técnicos en FMA | Maxima: intervención inmediata |
| Activar funcionalidades clave | 20 a 39% en FMA | Alta: acelerar hasta superar el 40% |
| Monitorear adopcion | 40 a 69% en FMA | Media: mantener ritmo y recuperar rezagados |
| En buen camino | 70% o mas en FMA | Estandar: seguimiento rutinario |

Se interpreta el número dentro de cada tarjeta como la cantidad de clientes en ese segmento ya que permite dimensionar el esfuerzo requerido para cada categoría.

### 6.4 Tabla de priorizacion por cliente

Se ordenan los clientes por score de riesgo dentro de cada segmento ya que los clientes con mayor riesgo dentro del mismo nivel de adopción deben atenderse primero.

Se utiliza la columna **Retención real** para validar si el modelo está prediciendo correctamente ya que permite comparar el score del modelo con el resultado observado en los datos históricos.

### 6.5 Tarjetas de recomendacion por segmento

Se presentan tres tarjetas ya que cada una detalla las acciones específicas recomendadas para los segmentos de mayor riesgo junto con el respaldo evidencial del modelo.

**Segmento 1. Migrar con prioridad (menos del 20% en FMA)**

Se priorizan las funcionalidades de mayor retención ya que Job Close Out y Uso Time son los factores SHAP dominantes en este segmento. Se coordina con Customer Success ya que los clientes en este segmento requieren acompañamiento activo durante los primeros 30 días. Se evalúa la viabilidad de la migración ya que clientes con menos de 5 técnicos activos en FMA pueden necesitar incentivos explícitos para continuar.

**Segmento 2. Activar funcionalidades clave (20 a 39% en FMA)**

Se activan las funcionalidades según el factor SHAP dominante de cada técnico ya que la Vista 2 permite identificar el factor principal de cada uno. Se busca superar el umbral del 40% en las próximas 4 semanas ya que ese es el punto de inflexión a partir del cual la adopción tiende a auto-sostenerse. Se realiza seguimiento bisemanal del ratio de días activos ya que es el indicador adelantado más predictivo de retención.

**Segmento 3. Monitorear y consolidar (40% o mas en FMA)**

Se mantiene el ritmo de adopción sin interrupciones ya que los cambios de versión en los primeros 60 días del técnico nuevo están asociados a mayor riesgo de abandono. Se recuperan los técnicos en zona de riesgo ya que incluso en clientes con alta penetración pueden existir técnicos individuales con score entre 30 y 50%. Se documentan estos clientes como casos de éxito ya que sirven de referencia para los segmentos 1 y 2.

---

## 7. Actualizacion de datos

Se actualiza el análisis ejecutando el pipeline cada 14 días (bisemanal) ya que este es el ciclo de actualización definido para mantener las predicciones relevantes.

**Para actualizar los datos:**

1. Se reemplazan los archivos CSV en la carpeta `data/raw/` ya que estos contienen los datos fuente exportados de Snowflake.
2. Se ejecuta `python pipeline.py` ya que este paso regenera todos los archivos analíticos.
3. Se recarga el tablero ya que los nuevos datos se cargan automáticamente.

Se verifica la fecha de última actualización en la barra superior del tablero ya que confirma que el análisis refleja el período correcto.

---

## 8. Preguntas frecuentes

**Los datos se actualizan automaticamente?**  
No. Se actualizan ejecutando `python pipeline.py` cada 14 días o bajo demanda ya que el proceso requiere los datos más recientes exportados desde Snowflake. La fecha de última actualización aparece en el encabezado del tablero.

**Por que un tecnico tiene score bajo si esta usando FMA?**  
Se calcula el score con base en el comportamiento de las primeras 2 semanas ya que ese período define los patrones de uso. Un técnico que inicia sesión pero usa pocas funcionalidades o con baja frecuencia puede tener score bajo ya que el modelo detecta esos patrones como señal de riesgo.

**Los nombres de los clientes son visibles?**  
No. Se utilizan únicamente identificadores internos (IDs) ya que la política de privacidad impide mostrar nombres de clientes o técnicos.

**Que significa AUC-ROC?**  
Se utiliza el AUC-ROC como medida de la capacidad del modelo para distinguir entre técnicos que retienen y técnicos que abandonan ya que valores cercanos a 1 indican mayor precisión. El modelo actual tiene AUC = 0.829 ya que supera el umbral mínimo de calidad de 0.70.

**Que pasa si el modelo tiene AUC menor a 0.70?**  
Se muestra una advertencia en la Vista 3 ya que en ese caso las recomendaciones prescriptivas deben tomarse como orientativas hasta que el modelo se re-entrene con más datos.

**Como se interpreta el factor SHAP de un tecnico?**  
Se interpreta el factor SHAP como la variable que más influyó en el score de ese técnico específico ya que el análisis SHAP descompone la predicción individual en contribuciones de cada variable. Por ejemplo, si el factor principal es "Ratio días activos", indica que la frecuencia de uso en la primera semana fue el elemento más determinante en el score de ese técnico.

---

## 9. Glosario de terminos

| Termino | Definicion |
|---------|------------|
| **Adoptante sostenido** | Técnico que registró 15 o más días activos en los 60 días posteriores al período de observación |
| **Abandono** | Técnico que no alcanzó el umbral de 15 días activos en 60 días |
| **Score de retencion** | Probabilidad de que un técnico retenga FMA según el modelo ya que se expresa como un porcentaje entre 0% y 100% |
| **SHAP** | Método de interpretabilidad que cuantifica la contribución de cada variable al score del modelo ya que permite identificar los factores determinantes por técnico |
| **AUC-ROC** | Metrica de calidad del modelo ya que mide su capacidad para distinguir correctamente entre técnicos que retienen y los que abandonan |
| **Tenant** | Cliente de ServiceTitan (empresa contratista) |
| **Tecnico** | Empleado del cliente que usa FMA en el campo |
| **TMA** | Technician Mobile Application: aplicación anterior a FMA |
| **FMA** | Field Mobile App: plataforma de migración objetivo |
| **Pipeline** | Proceso automatizado que transforma los datos fuente en los indicadores del tablero |
| **Umbral de inflexion social** | Punto a partir del 40% de técnicos en FMA en el que la adopción tiende a auto-sostenerse por efecto de red |
| **Periodo de observacion** | Las primeras 14 días de actividad de un técnico en FMA ya que es la ventana usada por el modelo para generar el score |

---

*FMA Adoption Intelligence — ServiceTitan, Version 1.0, Mayo 2026*
