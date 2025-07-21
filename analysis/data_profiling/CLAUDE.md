# Data Profiling Guidelines

## Objective
Thoroughly understand the data structure, quality, and characteristics before beginning analysis.

## Profiling Tasks

### 1. Schema Discovery
```sql
-- Example for PostgreSQL
SELECT 
    table_name,
    column_name,
    data_type,
    is_nullable,
    column_default
FROM information_schema.columns
WHERE table_schema = '{{schema}}'
ORDER BY table_name, ordinal_position;
```

### 2. Row Counts & Date Ranges
```sql
-- Get row counts for each table
{{#each tables}}
SELECT '{{this}}' as table_name, COUNT(*) as row_count
FROM {{this}}
UNION ALL
{{/each}}
```

### 3. Missing Data Analysis
For each important column:
- Count NULL values
- Count empty strings
- Identify patterns in missingness
- Document impact on analysis

### 4. Data Distribution Analysis
- Numeric columns: min, max, mean, median, std dev, percentiles
- Categorical columns: unique values, frequency distribution
- Date columns: range, gaps, timezone considerations
- Text columns: length distribution, common patterns

### 5. Outlier Detection
- Use IQR method for numeric columns
- Check for impossible values (negative ages, future dates)
- Identify data entry errors
- Document handling strategy

### 6. Relationship Validation
- Verify foreign key relationships
- Check referential integrity
- Identify orphan records
- Document cardinality

## Output Format

Create `data/profiling/data_profile_report.md` with:

```markdown
# Data Profiling Report

## Executive Summary
- Total records: X
- Date range: YYYY-MM-DD to YYYY-MM-DD
- Data quality score: X/10
- Critical issues: [list]

## Table Profiles

### Table: {{table_name}}
- Row count: X
- Columns: Y
- Primary key: {{key}}
- Update frequency: {{frequency}}

#### Column Statistics
| Column | Type | Nulls | Unique | Min | Max | Mean | Notes |
|--------|------|-------|--------|-----|-----|------|-------|
| ...    | ...  | ...   | ...    | ... | ... | ...  | ...   |

## Data Quality Issues

### Critical Issues
1. Issue description
   - Impact: High/Medium/Low
   - Affected records: X
   - Recommended action: ...

### Warnings
1. Warning description
   - Affected columns: ...
   - Potential impact: ...

## Recommendations
1. Data cleaning steps needed
2. Filters to apply for analysis
3. Columns to exclude
```

## Quality Metrics

Calculate and report:
1. **Completeness**: % of non-null values per column
2. **Uniqueness**: % of unique values where expected
3. **Validity**: % of values meeting business rules
4. **Consistency**: % of records with consistent formats
5. **Timeliness**: Data freshness and update patterns

## Profiling Code Template

```python
import pandas as pd
import numpy as np
from datetime import datetime

class DataProfiler:
    def __init__(self, table_name: str):
        self.table_name = table_name
        self.profile = {}
    
    def profile_numeric_column(self, data: pd.Series) -> dict:
        return {
            'count': data.count(),
            'null_count': data.isna().sum(),
            'null_pct': data.isna().mean() * 100,
            'unique': data.nunique(),
            'min': data.min(),
            'max': data.max(),
            'mean': data.mean(),
            'median': data.median(),
            'std': data.std(),
            'q25': data.quantile(0.25),
            'q75': data.quantile(0.75),
            'outliers': self.detect_outliers(data)
        }
    
    def profile_categorical_column(self, data: pd.Series) -> dict:
        value_counts = data.value_counts()
        return {
            'count': data.count(),
            'null_count': data.isna().sum(),
            'null_pct': data.isna().mean() * 100,
            'unique': data.nunique(),
            'top_values': value_counts.head(10).to_dict(),
            'rare_values': (value_counts == 1).sum()
        }
    
    def detect_outliers(self, data: pd.Series) -> dict:
        q1 = data.quantile(0.25)
        q3 = data.quantile(0.75)
        iqr = q3 - q1
        lower_bound = q1 - 1.5 * iqr
        upper_bound = q3 + 1.5 * iqr
        
        outliers = data[(data < lower_bound) | (data > upper_bound)]
        return {
            'count': len(outliers),
            'percentage': len(outliers) / len(data) * 100,
            'bounds': {'lower': lower_bound, 'upper': upper_bound}
        }
```

## Remember
- Profile data before any analysis
- Document all assumptions
- Save profiling results for reference
- Re-profile if data is updated