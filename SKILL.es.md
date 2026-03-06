---
name: pattern-spotter
version: 1.2.0
description: Identifica patrones ocultos, anomalías y correlaciones en conjuntos de datos complejos utilizando técnicas estadísticas y de aprendizaje automático
maintainer: SMOUJBOT <smouj@rpgclaw.org>
tags: [pattern, analysis, data, detection, ml, anomaly, correlation]
dependencies:
  - python>=3.9
  - numpy>=1.21.0
  - pandas>=1.3.0
  - scikit-learn>=1.0.0
  - scipy>=1.7.0
  - matplotlib>=3.5.0
  - seaborn>=0.11.0
  - imbalanced-learn>=0.9.0  # para detección de patrones desbalanceados
requirements_file: requirements-pattern-spotter.txt
categories: [analysis, intelligence]
compatible_runtimes: [python, docker]
---

# Patrón Spotter Skill

## Propósito

Pattern Spotter es un motor de análisis especializado para descubrir estructuras, anomalías, tendencias y correlaciones ocultas en conjuntos de datos multidimensionales. A diferencia de las herramientas de agregación simples, Pattern Spotter aplica pruebas estadísticas avanzadas, algoritmos de aprendizaje no supervisado y descomposición de series temporales para revelar información que el análisis manual pasaría por alto.

**Casos de uso del mundo real:**
- Detectar patrones de fraude en registros de transacciones (identificando anomalías de comportamiento sutiles en más de 50 características)
- Encontrar estacionalidad y rupturas de tendencia en métricas de servidor (patrones de CPU, memoria, tráfico de red en más de 1M de puntos de datos)
- Identificar clusters de segmentación de clientes a partir de datos de comportamiento (descubriendo 5-7 personalidades distintas de más de 200 atributos)
- Descubrir precursores de fallos en datos de sensores de equipos industriales (detectando patrones de advertencia de 15 minutos antes del tiempo de inactividad)
- Poner al descubierto correlaciones ocultas en datos de atribución de marketing (atribuyendo conversiones a puntos de contacto indirectos)
- Detectar deriva de datos en pipelines de entrenamiento de modelos de ML (identificando cambios de distribución >2σ en el espacio de características)

## Alcance

Pattern Spotter opera sobre datos estructurados (CSV, Parquet, JSON) y proporciona:

### Comandos Principales

**`pattern detect`** - Motor de descubrimiento de patrones principal
```bash
pattern detect \
  --input data/transactions.csv \
  --output reports/pattern-analysis.json \
  --algorithms [isolation-forest,local-outlier-factor,dbscan] \
  --features amount,tx_count,time_since_last,ip_risk_score \
  --contamination 0.05 \
  --time-column timestamp \
  --seasonality daily \
  --threshold 0.85
```

**`pattern correlations`** - Descubrimiento de correlaciones multivariantes
```bash
pattern correlations \
  --dataset metrics.db \
  --table server_metrics \
  --method pearson|spearman|kendall \
  --min-correlation 0.3 \
  --max-lag 24 \
  --output correlations-network.png \
  --p-value 0.01
```

**`pattern anomalies`** - Detección de anomalías en tiempo real
```bash
pattern anomalies \
  --stream stdin \
  --window-size 1000 \
  --model isolation-forest \
  --retrain-frequency 10000 \
  --alert-threshold 0.95 \
  --output-format jsonlines
```

**`pattern visualize`** - Generar visualizaciones de patrones
```bash
pattern visualize \
  --input anomalies.csv \
  --type heatmap|cluster|timeseries|scatter-matrix \
  --dimensions 2|3 \
  --output viz/pattern-clusters.html \
  --interactive \
  --color-by cluster_label
```

**`pattern benchmark`** - Comparar algoritmos de detección de patrones
```bash
pattern benchmark \
  --dataset labeled-data.csv \
  --label-column anomaly_flag \
  --algorithms [isolation-forest,one-class-svm,autoencoder] \
  --metrics [precision,recall,f1,auc] \
  --cross-validation 5 \
  --output benchmarks/comparison.xlsx
```

**`pattern export`** - Exportar patrones detectados a sistemas operativos
```bash
pattern export \
  --patterns patterns.json \
  --format sql|json|api \
  --destination postgresql://localhost/patterns \
  --table detected_anomalies \
  --mode upsert \
  --batch-size 1000
```

## Proceso de Trabajo Detallado

### Fase 1: Preparación y Validación de Datos
```bash
# 1. Validar estructura y tipos de conjunto de datos
pattern validate \
  --input raw_data.parquet \
  --schema schemas/transaction_schema.json \
  --missing-strategy impute|drop \
  --outlier-strategy winsorize \
  --report validation-report.html

# 2. Manejar desbalance de clases (para detección de patrones supervisada)
pattern balance \
  --input labeled.csv \
  --method smote|adasyn|undersample \
  --ratio 1.0 \
  --output balanced.parquet
```

### Fase 2: Análisis Exploratorio de Patrones
```bash
# 1. Análisis de auto-correlación y auto-correlación parcial
pattern acf-pacf \
  --timeseries server_load.csv \
  --column cpu_usage \
  --lags 48 \
  --alpha 0.05 \
  --output acf_plot.png

# 2. Descomposición estacional
pattern decompose \
  --input metrics.csv \
  --value-column requests_per_sec \
  --period 1440 \
  --model additive|multiplicative \
  --output decomposition.png
```

### Fase 3: Descubrimiento de Patrones No Supervisado
```bash
# 1. Reducción de dimensionalidad para visualización de patrones
pattern reduce \
  --input features.npy \
  --method pca|tsne|umap \
  --components 2-3 \
  --perplexity 30 \
  --output reduced_features.parquet

# 2. Selección y ajuste de algoritmo de clustering
pattern cluster \
  --input reduced.parquet \
  --algorithms kmeans|dbscan|gaussian-mixture \
  --k-range 2-12 \
  --metric silhouette|calinski-harabasz \
  --output cluster-evaluation.json
```

### Fase 4: Pipeline de Detección de Anomalías
```bash
# 1. Entrenar ensamble de detectores
pattern train-anomalies \
  --train data/train.csv \
  --validation data/val.csv \
  --detectors isolation-forest,local-outlier-factor,one-class-svm \
  --weights auto|manual:0.4,0.3,0.3 \
  --output models/anomaly_ensemble.pkl

# 2. Evaluar en conjunto de prueba con anomalías etiquetadas
pattern test-anomalies \
  --model models/anomaly_ensemble.pkl \
  --test data/test.csv \
  --thresholds 0.90,0.95,0.99 \
  --output results/anomaly_scores.csv \
  --metrics pr-curve,roc-curve

# 3. Desplegar ensamble para detección en streaming
pattern serve \
  --model models/anomaly_ensemble.pkl \
  --port 8080 \
  --batch-size 100 \
  --format protobuf|json \
  --metrics-port 9090
```

### Fase 5: Validación y Reporte de Patrones
```bash
# 1. Pruebas de significancia estadística
pattern significance \
  --patterns detected.json \
  --method bootstrap|permutation \
  --iterations 10000 \
  --confidence 0.95 \
  --output significance_report.md

# 2. Generar reporte HTML completo
pattern report \
  --analysis results/ \
  --template templates/pattern_report.html \
  --title "Q4 2025 Pattern Analysis" \
  --executive-summary \
  --include viz/ \
  --output reports/pattern-analysis-Q4-2025.html
```

## Reglas de Oro

1. **Validar siempre antes del análisis**: Ejecutar `pattern validate` en cada conjunto de datos. Nunca omitir verificaciones de esquema o análisis de valores faltantes.
2. **Nunca confiar en los p-valores crudos**: Aplicar siempre corrección Bonferroni o Benjamini-Hochberg cuando se realicen pruebas de hipótesis múltiples en análisis de correlación.
3. **Aislamiento primero, identificación después**: Usar siempre detección no supervisada primero (sin etiquetas), luego validar patrones contra lógica de negocio.
4. **Documentar todas las elecciones de parámetros**: Cada parámetro de algoritmo (contamination, epsilon, perplexity) debe justificarse en el registro de análisis.
5. **Preservar datos originales**: Nunca modificar archivos de entrada originales. trabajar siempre con copias y rastrear transformaciones.
6. **Contexto de negocio requerido**: Todos los patrones detectados deben validarse contra conocimiento del dominio. Significancia estadística ≠ significancia de negocio.
7. **Ensamble sobre modelo único**: Usar siempre al menos 3 algoritmos de detección diferentes y combinar sus resultados.
8. **Integridad de series temporales**: Para datos temporales, nunca mezclar antes de dividir train/test. Usar divisiones cronológicas estrictas.
9. **Reproducibilidad obligatoria**: Establecer semillas aleatorias (variable de entorno SEED) y registrar todas las versiones de paquetes con `pip freeze > environment.txt`.
10. **Transparencia de umbrales**: Nunca usar umbrales por defecto. Justificar siempre basado en costo de negocio de falsos positivos vs falsos negativos.
11. **Visualizar todo**: Cada afirmación de patrón debe estar respaldada por al menos una visualización (gráfico de dispersión, mapa de clusters, serie temporal).
12. **Monitorear deriva**: Los patrones se degradan con el tiempo. Configurar `pattern benchmark` semanal contra datos recientes para detectar decaimiento del modelo.

## Ejemplos

### Ejemplo 1: Detectar patrones de fraude en datos de transacciones
```bash
# Entrada: 2M registros de transacciones con características
# - amount, tx_count (últimas 24h), time_since_last_tx, ip_risk_score, device_fingerprint_hash

pattern detect \
  --input data/transactions_2025Q1.csv \
  --output reports/fraud_patterns_Q1.json \
  --algorithms isolation-forest,local-outlier-factor \
  --features amount:minmax,tx_count:log,time_since_last:zscore,ip_risk_score:raw \
  --contamination 0.001 \
  --time-column transaction_ts \
  --seasonality hourly \
  --threshold 0.87 \
  --sample-fraction 0.3 \
  --n-jobs 8 \
  --random-seed 42

# Salida: reports/fraud_patterns_Q1.json
# {
#   "algorithm": "ensemble_isolation_forest_lof",
#   "detected_anomalies": 2457,
#   "false_positive_rate_estimate": 0.023,
#   "top_patterns": [
#     {
#       "feature_combination": "amount>10000 AND tx_count<3 AND time_since_last<60",
#       "confidence": 0.94,
#       "business_impact": "high"
#     },
#     {
#       "feature_combination": "ip_risk_score>0.8 AND device_fingerprint_hash_changed=1",
#       "confidence": 0.91,
#       "business_impact": "critical"
#     }
#   ],
#   "visualization": "reports/viz/fraud_clusters_2025Q1.png"
# }
```

### Ejemplo 2: Encontrar correlaciones en métricas de servidor
```bash
pattern correlations \
  --dataset postgresql://metrics-db \
  --table prod_server_metrics_30d \
  --method spearman \
  --min-correlation 0.4 \
  --max-lag 12 \
  --p-value 0.001 \
  --exclude-columns server_id,timestamp \
  --output reports/metrics_correlations_network.png \
  --filter "pod_name='api-server' AND metric_type='cpu'"

# Salida: Grafico de red interactivo mostrando:
# - Fuerte correlación (0.87, p<0.001): cpu_usage → context_switches (lag=2)
# - Correlación moderada (0.63, p<0.001): memory_usage → swap_in (lag=5)
# - Correlación inversa (-0.71, p<0.01): cache_hit_ratio → db_query_latency
```

### Ejemplo 3: Detección de anomalías en streaming en tiempo real
```bash
# Iniciar el servicio de detección de anomalías en streaming
pattern serve \
  --model models/anomaly_ensemble_20250301.pkl \
  --port 8080 \
  --batch-size 500 \
  --format protobuf \
  --metrics-port 9090 \
  --retrain-schedule "daily" \
  --retrain-threshold 0.15 \
  --alert-webhook https://alerts.rpgclaw.org/webhooks/anomalies

# Enviar métricas a través de pipe
cat live_metrics.jsonl | curl -X POST http://localhost:8080/detect \
  -H "Content-Type: application/jsonlines" \
  -d @- \
  --output anomalies_realtime.jsonl

# Las anomalías se publican automáticamente en el webhook
```

### Ejemplo 4: Benchmark de múltiples algoritmos en dataset etiquetado
```bash
pattern benchmark \
  --dataset labeled/industrial_sensors_2024.csv \
  --label-column failure_within_24h \
  --algorithms isolation-forest,one-class-svm,autoencoder \
  --autoencoder-layers 128,64,32,16,8 \
  --autoencoder-epochs 100 \
  --metrics precision,recall,f1,auc,matthews \
  --cross-validation 5 \
  --train-size 0.7 \
  --output reports/algorithm_benchmark_20250301.xlsx \
  --save-models models/benchmark_ \
  --feature-importance

# Salida: Reporte Excel con hojas:
# - Resumen (F1 scores: Autoencoder 0.89, Isolation Forest 0.84, One-Class SVM 0.76)
# - Matrices de confusión por algoritmo
# - Importancia de características (top 10 para autoencoder: vibration_rms, temperature_delta, pressure_fluctuation)
# - Tiempo de entrenamiento y uso de recursos
```

## Variables de Entorno

```bash
# Requeridas
PATTERN_SPOTTER_DATA_PATH="/data/pattern-analysis"      # Ruta base para datasets
PATTERN_SPOTTER_MODEL_REGISTRY="/models/pattern-models" # Donde almacenar modelos entrenados
PATTERN_SPOTTER_LOG_LEVEL="INFO"                        # DEBUG, INFO, WARNING, ERROR

# Opcionales (con valores por defecto)
PATTERN_SPOTTER_N_JOBS=-1                               # Número de trabajos paralelos (-1 = todos los cores)
PATTERN_SPOTTER_MEMORY_LIMIT="8GB"                      # Memoria máxima para datasets grandes
PATTERN_SPOTTER_RANDOM_SEED=42                         # Para reproducibilidad
PATTERN_SPOTTER_CACHE_DIR="/tmp/pattern-cache"          # Ubicación de caché para computaciones repetidas
PATTERN_SPOTPER_MAX_DATASET_SIZE="10GB"                # Rechazar datasets más grandes que esto
PATTERN_SPOTTER_DB_CONNECTION_TIMEOUT=30              # Timeout de conexión a base de datos (segundos)
```

## Dependencias y Requisitos

**Bibliotecas principales** (instaladas automáticamente):
```
numpy>=1.21.0
pandas>=1.3.0
scikit-learn>=1.0.0
scipy>=1.7.0
matplotlib>=3.5.0
seaborn>=0.11.0
imbalanced-learn>=0.9.0
plotly>=5.3.0                 # Para visualizaciones interactivas
dask[dataframe]>=2022.1.0    # Para computación fuera de memoria en datasets grandes
sqlalchemy>=1.4.0             # Para conectividad de bases de datos
psycopg2-binary>=2.9.0       # Driver PostgreSQL (opcional)
protobuf>=3.20.0             # Para formato de streaming
```

**Dependencias del sistema**:
```bash
# Ubuntu/Debian
apt-get update && apt-get install -y \
  python3-dev \
  gcc \
  g++ \
  libopenblas-dev \
  liblapack-dev \
  pkg-config \
  libpq-dev  # para PostgreSQL
```

## Pasos de Verificación

### Después de la instalación
```bash
# 1. Verificar todas las dependencias
pattern --version
pattern doctors

# 2. Ejecutar verificación de cordura con datos de muestra
pattern detect \
  --input tests/sample_data.csv \
  --output /tmp/test_output.json \
  --algorithms isolation-forest \
  --quick-validation

# 3. Verificar formato de salida
cat /tmp/test_output.json | jq '.algorithm, .detected_anomalies'
```

### Después de la ejecución de detección de patrones
```bash
# 1. Verificar código de salida
echo $?
# Esperado: 0

# 2. Verificar que el archivo de salida existe y tiene contenido
test -s reports/pattern-analysis.json
jq empty reports/pattern-analysis.json 2>/dev/null

# 3. Validar recuento de patrones
ANOMALIES=$(jq '.detected_anomalies' reports/pattern-analysis.json)
if [ "$ANOMALIES" -lt 100 ] && [ "$ANOMALIES" -gt 0 ]; then
  echo "ADVERTENCIA: Muy pocas anomalías detectadas. Verificar calidad de datos y parámetro de contaminación."
fi

# 4. Verificar generación de visualización (si se solicitó)
test -f viz/pattern-clusters.png || echo "ADVERTENCIA: Visualización no generada"
```

### Verificación de preparación para producción
```bash
pattern doctor \
  --input production_data.csv \
  --quick \
  --checks memory,disk,algorithm-suitability

# Salida esperada: "✓ Todas las verificaciones pasaron" o advertencias detalladas
```

## Solución de Problemas

### Problema: "MemoryError: Unable to allocate X GB"
**Solución**: Habilitar procesamiento Dask fuera de memoria o muestrear el conjunto de datos
```bash
pattern detect \
  --input huge_dataset.csv \
  --use-dask \
  --dask-scheduler localhost:8786 \
  --sample-fraction 0.1 \
  --output report.json
```

### Problema: "ConvergenceWarning: Number of iterations exceeded"
**Solución**: Aumentar max_iter o usar inicialización diferente
```bash
pattern detect \
  --algorithms kmeans \
  --max-iter 500 \
  --init k-means++ \
  --n-init 20
```

### Problema: "All correlations are near zero"
**Solución**: Verificar relaciones no lineales o aplicar transformaciones
```bash
pattern correlations \
  --method spearman \  # Intentar basado en rangos en lugar de lineal
  --transform log,sqrt \  # Aplicar a características sesgadas
  --output report.png
```

### Problema: "Too many false positives in streaming mode"
**Solución**: Aumentar umbral o agregar características contextuales
```bash
pattern anomalies \
  --stream stdin \
  --model ensemble.pkl \
  --alert-threshold 0.99 \  # Aumentar desde 0.95
  --context-features hour_of_day,day_of_week \
  --output anomalies.jsonl
```

### Problema: "Patterns not reproducible across runs"
**Solución**: Fijar todas las semillas aleatorias y asegurar operaciones determinísticas
```bash
export PATTERN_SPOTTER_RANDOM_SEED=42
pattern detect \
  --random-seed 42 \  # La bandera CLI sobreescribe la variable env si está presente
  --deterministic \
  --output report.json
```

### Problema: "Database connection timeout"
**Solución**: Aumentar timeout o usar pooling de conexiones
```bash
export PATTERN_SPOTTER_DB_CONNECTION_TIMEOUT=120
pattern correlations \
  --dataset postgresql://... \
  --pool-size 10 \
  --max-overflow 20
```

### Problema: "Cannot install imbalanced-learn"
**Solución**: Algunas versiones de scikit-learn entran en conflicto. Fijar versiones compatibles:
```bash
pip install "scikit-learn>=1.0.0,<1.3.0" "imbalanced-learn>=0.9.0"
```

## Comandos de Rollback

### Si el análisis produce falsos positivos/patrones incorrectos
```bash
# 1. Revertir a modelo/anterior confiable
pattern rollback \
  --to-timestamp 2025-02-28T14:30:00Z \
  --output-dir reports/rollback_$(date +%Y%m%d_%H%M%S) \
  --preserve-current

# 2. Restaurar estado anterior de base de datos (si se exportaron patrones)
psql -d patterns_db -c "
  DELETE FROM detected_anomalies 
  WHERE detected_at > '2025-03-01 10:00:00';
  INSERT INTO detected_anomalies 
  SELECT * FROM detected_anomalies_backup 
  WHERE detected_at <= '2025-03-01 10:00:00';
"

# 3. Revertir despliegue de modelo
kubectl rollout undo deployment/anomaly-detection \
  --to-revision=5

# 4. Limpiar resultados cacheados (si se usaron)
rm -rf /tmp/pattern-cache/*
PATTERN_SPOTTER_CACHE_DIR="" pattern detect ...  # Forzar recomputación
```

### Si el análisis corrompe datos
```bash
# Restaurar desde backup (asumiendo backups diarios)
pattern restore \
  --backup backups/transactions_20250228.parquet \
  --output data/transactions.parquet \
  --force

# Verificar integridad de restauración
pattern validate \
  --input data/transactions.parquet \
  --quick
```

### Rollback completo de sistema (emergencia)
```bash
# Detener servicio de streaming
pkill -f "pattern serve"
# o
systemctl stop pattern-spotter.service

# Restaurar base de datos desde último backup bueno conocido
pg_restore -d patterns_db --clean --if-exists backups/patterns_20250228.dump

# Redesplegar imagen Docker anterior
docker pull rpgclaw/pattern-spotter:v1.1.0
docker-compose up -d pattern-spotter
```

---

**Mantenido por**: SMOUJBOT  
**Última actualización**: 2025-03-01  
**Soporte**: `openclaw pattern-spotter --help` o ver `/docs/pattern-spotter/`
```