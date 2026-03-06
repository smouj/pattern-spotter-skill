---
name: pattern-spotter
version: 1.2.0
description: Identifies hidden patterns, anomalies, and correlations in complex datasets using statistical and machine learning techniques
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
  - imbalanced-learn>=0.9.0  # for imbalanced pattern detection
requirements_file: requirements-pattern-spotter.txt
categories: [analysis, intelligence]
compatible_runtimes: [python, docker]
---

# Pattern Spotter Skill

## Purpose

Pattern Spotter is a specialized analysis engine for discovering hidden structures, anomalies, trends, and correlations in multi-dimensional datasets. Unlike simple aggregation tools, Pattern Spotter applies advanced statistical tests, unsupervised learning algorithms, and time-series decomposition to reveal insights that manual analysis would miss.

**Real-world use cases:**
- Detect fraud patterns in transaction logs (identifying subtle behavioral anomalies across 50+ features)
- Find seasonality and trend breaks in server metrics (CPU, memory, network traffic patterns across 1M+ data points)
- Identify customer segmentation clusters from behavioral data (uncovering 5-7 distinct personas from 200+ attributes)
- Discover failure precursors in industrial equipment sensor data (detecting 15-minute warning patterns before downtime)
- Uncover hidden correlations in marketing attribution data (attributing conversions to indirect touchpoints)
- Detect data drift in ML model training pipelines (identifying distribution shifts >2σ in feature space)

## Scope

Pattern Spotter operates on structured data (CSV, Parquet, JSON) and provides:

### Core Commands

**`pattern detect`** - Primary pattern discovery engine
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

**`pattern correlations`** - Multi-variate correlation discovery
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

**`pattern anomalies`** - Real-time anomaly detection
```bash
pattern anomalies \
  --stream stdin \
  --window-size 1000 \
  --model isolation-forest \
  --retrain-frequency 10000 \
  --alert-threshold 0.95 \
  --output-format jsonlines
```

**`pattern visualize`** - Generate pattern visualizations
```bash
pattern visualize \
  --input anomalies.csv \
  --type heatmap|cluster|timeseries|scatter-matrix \
  --dimensions 2|3 \
  --output viz/pattern-clusters.html \
  --interactive \
  --color-by cluster_label
```

**`pattern benchmark`** - Compare pattern detection algorithms
```bash
pattern benchmark \
  --dataset labeled-data.csv \
  --label-column anomaly_flag \
  --algorithms [isolation-forest,one-class-svm,autoencoder] \
  --metrics [precision,recall,f1,auc] \
  --cross-validation 5 \
  --output benchmarks/comparison.xlsx
```

**`pattern export`** - Export detected patterns to operational systems
```bash
pattern export \
  --patterns patterns.json \
  --format sql|json|api \
  --destination postgresql://localhost/patterns \
  --table detected_anomalies \
  --mode upsert \
  --batch-size 1000
```

## Detailed Work Process

### Phase 1: Data Preparation & Validation
```bash
# 1. Validate dataset structure and types
pattern validate \
  --input raw_data.parquet \
  --schema schemas/transaction_schema.json \
  --missing-strategy impute|drop \
  --outlier-strategy winsorize \
  --report validation-report.html

# 2. Handle class imbalance (for supervised pattern detection)
pattern balance \
  --input labeled.csv \
  --method smote|adasyn|undersample \
  --ratio 1.0 \
  --output balanced.parquet
```

### Phase 2: Exploratory Pattern Analysis
```bash
# 1. Auto-correlation and partial auto-correlation analysis
pattern acf-pacf \
  --timeseries server_load.csv \
  --column cpu_usage \
  --lags 48 \
  --alpha 0.05 \
  --output acf_plot.png

# 2. Seasonal decomposition
pattern decompose \
  --input metrics.csv \
  --value-column requests_per_sec \
  --period 1440 \
  --model additive|multiplicative \
  --output decomposition.png
```

### Phase 3: Unsupervised Pattern Discovery
```bash
# 1. Dimensionality reduction for pattern visualization
pattern reduce \
  --input features.npy \
  --method pca|tsne|umap \
  --components 2-3 \
  --perplexity 30 \
  --output reduced_features.parquet

# 2. Clustering algorithm selection and tuning
pattern cluster \
  --input reduced.parquet \
  --algorithms kmeans|dbscan|gaussian-mixture \
  --k-range 2-12 \
  --metric silhouette|calinski-harabasz \
  --output cluster-evaluation.json
```

### Phase 4: Anomaly Detection Pipeline
```bash
# 1. Train ensemble of detectors
pattern train-anomalies \
  --train data/train.csv \
  --validation data/val.csv \
  --detectors isolation-forest,local-outlier-factor,one-class-svm \
  --weights auto|manual:0.4,0.3,0.3 \
  --output models/anomaly_ensemble.pkl

# 2. Evaluate on test set with labeled anomalies
pattern test-anomalies \
  --model models/anomaly_ensemble.pkl \
  --test data/test.csv \
  --thresholds 0.90,0.95,0.99 \
  --output results/anomaly_scores.csv \
  --metrics pr-curve,roc-curve

# 3. Deploy ensemble for streaming detection
pattern serve \
  --model models/anomaly_ensemble.pkl \
  --port 8080 \
  --batch-size 100 \
  --format protobuf|json \
  --metrics-port 9090
```

### Phase 5: Pattern Validation & Reporting
```bash
# 1. Statistical significance testing
pattern significance \
  --patterns detected.json \
  --method bootstrap|permutation \
  --iterations 10000 \
  --confidence 0.95 \
  --output significance_report.md

# 2. Generate comprehensive HTML report
pattern report \
  --analysis results/ \
  --template templates/pattern_report.html \
  --title "Q4 2025 Pattern Analysis" \
  --executive-summary \
  --include viz/ \
  --output reports/pattern-analysis-Q4-2025.html
```

## Golden Rules

1. **Always validate before analysis**: Run `pattern validate` on every dataset. Never skip schema checks or missing value analysis.
2. **Never trust raw p-values**: Always apply Bonferroni or Benjamini-Hochberg correction when doing multiple hypothesis testing in correlation analysis.
3. **Isolation first, identification second**: Always use unsupervised detection first (no labels), then validate patterns against business logic.
4. **Document all parameter choices**: Every algorithm parameter (contamination, epsilon, perplexity) must be justified in the analysis log.
5. **Preserve raw data**: Never modify original input files. Always work on copies and track transformations.
6. **Business context required**: All detected patterns must be validated against domain knowledge. Statistical significance ≠ business significance.
7. **Ensemble over single model**: Always use at least 3 different detection algorithms and ensemble their results.
8. **Time-series integrity**: For temporal data, never shuffle before splitting train/test. Use strict chronological splits.
9. **Reproducibility mandatory**: Set random seeds (SEED env var) and log all package versions with `pip freeze > environment.txt`.
10. **Threshold transparency**: Never use default thresholds. Always justify based on business cost of false positives vs false negatives.
11. **Visualize everything**: Every pattern claim must be backed by at least one visualization (scatter plot, cluster map, time series).
12. **Monitor drift**: Patterns degrade over time. Set up weekly `pattern benchmark` against recent data to detect model decay.

## Examples

### Example 1: Detect fraud patterns in transaction data
```bash
# Input: 2M transaction records with features
# - amount, tx_count (last 24h), time_since_last_tx, ip_risk_score, device_fingerprint_hash

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

# Output: reports/fraud_patterns_Q1.json
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

### Example 2: Find correlations in server metrics
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

# Output: Interactive network graph showing:
# - Strong correlation (0.87, p<0.001): cpu_usage → context_switches (lag=2)
# - Moderate correlation (0.63, p<0.001): memory_usage → swap_in (lag=5)
# - Inverse correlation (-0.71, p<0.01): cache_hit_ratio → db_query_latency
```

### Example 3: Real-time streaming anomaly detection
```bash
# Start the streaming anomaly detection service
pattern serve \
  --model models/anomaly_ensemble_20250301.pkl \
  --port 8080 \
  --batch-size 500 \
  --format protobuf \
  --metrics-port 9090 \
  --retrain-schedule "daily" \
  --retrain-threshold 0.15 \
  --alert-webhook https://alerts.rpgclaw.org/webhooks/anomalies

# Pipe metrics to it
cat live_metrics.jsonl | curl -X POST http://localhost:8080/detect \
  -H "Content-Type: application/jsonlines" \
  -d @- \
  --output anomalies_realtime.jsonl

# Anomalies are automatically posted to webhook
```

### Example 4: Benchmark multiple algorithms on labeled dataset
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

# Output: Excel report with sheets:
# - Summary (F1 scores: Autoencoder 0.89, Isolation Forest 0.84, One-Class SVM 0.76)
# - Confusion matrices per algorithm
# - Feature importance (top 10 for autoencoder: vibration_rms, temperature_delta, pressure_fluctuation)
# - Training time and resource usage
```

## Environment Variables

```bash
# Required
PATTERN_SPOTTER_DATA_PATH="/data/pattern-analysis"      # Base path for datasets
PATTERN_SPOTTER_MODEL_REGISTRY="/models/pattern-models" # Where to store trained models
PATTERN_SPOTTER_LOG_LEVEL="INFO"                        # DEBUG, INFO, WARNING, ERROR

# Optional (with defaults)
PATTERN_SPOTTER_N_JOBS=-1                               # Number of parallel jobs (-1 = all cores)
PATTERN_SPOTTER_MEMORY_LIMIT="8GB"                      # Maximum memory for large datasets
PATTERN_SPOTTER_RANDOM_SEED=42                         # For reproducibility
PATTERN_SPOTTER_CACHE_DIR="/tmp/pattern-cache"         # Cache location for repeated computations
PATTERN_SPOTTER_MAX_DATASET_SIZE="10GB"                # Reject datasets larger than this
PATTERN_SPOTTER_DB_CONNECTION_TIMEOUT=30               # Database connection timeout (seconds)
```

## Dependencies & Requirements

**Core libraries** (automatically installed):
```
numpy>=1.21.0
pandas>=1.3.0
scikit-learn>=1.0.0
scipy>=1.7.0
matplotlib>=3.5.0
seaborn>=0.11.0
imbalanced-learn>=0.9.0
plotly>=5.3.0                 # For interactive visualizations
dask[dataframe]>=2022.1.0    # For out-of-core computation on large datasets
sqlalchemy>=1.4.0             # For database connectivity
psycopg2-binary>=2.9.0       # PostgreSQL driver (optional)
protobuf>=3.20.0             # For streaming format
```

**System dependencies**:
```bash
# Ubuntu/Debian
apt-get update && apt-get install -y \
  python3-dev \
  gcc \
  g++ \
  libopenblas-dev \
  liblapack-dev \
  pkg-config \
  libpq-dev  # for PostgreSQL
```

## Verification Steps

### After installation
```bash
# 1. Check all dependencies
pattern --version
pattern doctors

# 2. Run sanity check with sample data
pattern detect \
  --input tests/sample_data.csv \
  --output /tmp/test_output.json \
  --algorithms isolation-forest \
  --quick-validation

# 3. Verify output format
cat /tmp/test_output.json | jq '.algorithm, .detected_anomalies'
```

### After pattern detection run
```bash
# 1. Check exit code
echo $?
# Expected: 0

# 2. Verify output file exists and has content
test -s reports/pattern-analysis.json
jq empty reports/pattern-analysis.json 2>/dev/null

# 3. Validate pattern count
ANOMALIES=$(jq '.detected_anomalies' reports/pattern-analysis.json)
if [ "$ANOMALIES" -lt 100 ] && [ "$ANOMALIES" -gt 0 ]; then
  echo "WARNING: Very few anomalies detected. Check data quality and contamination parameter."
fi

# 4. Check visualization generation (if requested)
test -f viz/pattern-clusters.png || echo "WARNING: Visualization not generated"
```

### Production readiness check
```bash
pattern doctor \
  --input production_data.csv \
  --quick \
  --checks memory,disk,algorithm-suitability

# Expected output: "✓ All checks passed" or detailed warnings
```

## Troubleshooting

### Issue: "MemoryError: Unable to allocate X GB"
**Solution**: Enable Dask out-of-core processing or sample the dataset
```bash
pattern detect \
  --input huge_dataset.csv \
  --use-dask \
  --dask-scheduler localhost:8786 \
  --sample-fraction 0.1 \
  --output report.json
```

### Issue: "ConvergenceWarning: Number of iterations exceeded"
**Solution**: Increase max_iter or use different initialization
```bash
pattern detect \
  --algorithms kmeans \
  --max-iter 500 \
  --init k-means++ \
  --n-init 20
```

### Issue: "All correlations are near zero"
**Solution**: Check for non-linear relationships or apply transformations
```bash
pattern correlations \
  --method spearman \  # Try rank-based instead of linear
  --transform log,sqrt \  # Apply to skewed features
  --output report.png
```

### Issue: "Too many false positives in streaming mode"
**Solution**: Increase threshold or add contextual features
```bash
pattern anomalies \
  --stream stdin \
  --model ensemble.pkl \
  --alert-threshold 0.99 \  # Increase from 0.95
  --context-features hour_of_day,day_of_week \
  --output anomalies.jsonl
```

### Issue: "Patterns not reproducible across runs"
**Solution**: Fix all random seeds and ensure deterministic operations
```bash
export PATTERN_SPOTTER_RANDOM_SEED=42
pattern detect \
  --random-seed 42 \  # CLI flag overrides env var if present
  --deterministic \
  --output report.json
```

### Issue: "Database connection timeout"
**Solution**: Increase timeout or use connection pooling
```bash
export PATTERN_SPOTTER_DB_CONNECTION_TIMEOUT=120
pattern correlations \
  --dataset postgresql://... \
  --pool-size 10 \
  --max-overflow 20
```

### Issue: "Cannot install imbalanced-learn"
**Solution**: Some scikit-learn versions conflict. Pin compatible versions:
```bash
pip install "scikit-learn>=1.0.0,<1.3.0" "imbalanced-learn>=0.9.0"
```

## Rollback Commands

### If analysis produces false positives/incorrect patterns
```bash
# 1. Revert to previous trustworthy model/analysis
pattern rollback \
  --to-timestamp 2025-02-28T14:30:00Z \
  --output-dir reports/rollback_$(date +%Y%m%d_%H%M%S) \
  --preserve-current

# 2. Restore previous database state (if patterns were exported)
psql -d patterns_db -c "
  DELETE FROM detected_anomalies 
  WHERE detected_at > '2025-03-01 10:00:00';
  INSERT INTO detected_anomalies 
  SELECT * FROM detected_anomalies_backup 
  WHERE detected_at <= '2025-03-01 10:00:00';
"

# 3. Revert model deployment
kubectl rollout undo deployment/anomaly-detection \
  --to-revision=5

# 4. Clear cached results (if used)
rm -rf /tmp/pattern-cache/*
PATTERN_SPOTPER_CACHE_DIR="" pattern detect ...  # Force recompute
```

### If analysis corrupts data
```bash
# Restore from backup (assuming daily backups)
pattern restore \
  --backup backups/transactions_20250228.parquet \
  --output data/transactions.parquet \
  --force

# Verify restore integrity
pattern validate \
  --input data/transactions.parquet \
  --quick
```

### Full system rollback (emergency)
```bash
# Stop streaming service
pkill -f "pattern serve"
# or
systemctl stop pattern-spotter.service

# Restore database from last known good backup
pg_restore -d patterns_db --clean --if-exists backups/patterns_20250228.dump

# Redeploy previous Docker image
docker pull rpgclaw/pattern-spotter:v1.1.0
docker-compose up -d pattern-spotter
```

---

**Maintained by**: SMOUJBOT  
**Last updated**: 2025-03-01  
**Support**: `openclaw pattern-spotter --help` or see `/docs/pattern-spotter/`
```