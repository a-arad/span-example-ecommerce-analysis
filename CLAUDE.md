# span-example-ecommerce-analysis - Data Analysis Project

## Project Overview
Example e-commerce sales analysis project demonstrating span's data analysis capabilities with CSV data

## Analysis Objectives

### Goal 1: What are the revenue trends by product category over time?
**Hypothesis**: Electronics category generates the highest revenue and shows consistent growth
**Success Criteria**: Clear visualization of revenue trends with statistical significance testing

### Goal 2: Which regions show the strongest performance and growth potential?
**Hypothesis**: North and West regions outperform others due to higher population density
**Success Criteria**: Identify top 2 regions with growth rate analysis

### Goal 3: What is the impact of discounts on revenue and order volume?
**Hypothesis**: Higher discounts correlate with increased order volume but lower profit margins
**Success Criteria**: Quantify the relationship between discount rates and key metrics
### Goal {{@index}}: {{question}}
{{#if hypothesis}}
**Hypothesis**: {{hypothesis}}
{{/if}}
{{#if success_criteria}}
**Success Criteria**: {{success_criteria}}
{{/if}}



## Data Source Configuration

### Connection Details
- **Type**: CSV
- **Connection**: See `.mcp.json` for MCP server configuration
- **Tables/Data**: See data source configuration

### Data Access Guidelines
- All data access is **read-only**
- Use MCP tools for querying data
- Cache expensive queries in `data/cache/`
- Document any data quality issues found

## Key Metrics

### Total Revenue
- **Description**: Sum of all revenue across all transactions
- **Calculation**: `SUM(revenue)`
- **Dimensions**: product_category, region, date

### Average Order Value
- **Description**: Average revenue per transaction
- **Calculation**: `AVG(revenue)`
- **Dimensions**: product_category, region

### Units Sold
- **Description**: Total quantity of items sold
- **Calculation**: `SUM(quantity)`
- **Dimensions**: product_category, region

### Discount Impact
- **Description**: Average discount rate applied
- **Calculation**: `AVG(discount)`
- **Dimensions**: product_category, region

### Customer Count
- **Description**: Number of unique customers
- **Calculation**: `COUNT(DISTINCT customer_id)`
- **Dimensions**: region, date
### {{name}}
- **Description**: Example e-commerce sales analysis project demonstrating span's data analysis capabilities with CSV data
- **Calculation**: `{{calculation}}`
- **Dimensions**: {{dimensions}}


## Analysis Phases

### Phase 1: Data Profiling & Quality Assessment
1. **Validate Data Access**
   - Test MCP connection to data source
   - Verify table/schema access
   - Document connection issues if any

2. **Data Quality Check**
   - Check for missing values
   - Identify outliers and anomalies
   - Validate data types and formats
   - Document data quality issues in `analysis/data_quality_report.md`

3. **Initial Statistics**
   - Row counts and date ranges
   - Basic distributions
   - Correlation analysis
   - Save profiling results to `data/profiling/`

### Phase 2: Exploratory Data Analysis
1. **Metric Calculation**
   - Calculate all defined metrics
   - Generate time series if applicable
   - Create segmentation analysis

2. **Pattern Discovery**
   - Identify trends and seasonality
   - Find interesting correlations
   - Detect anomalies or outliers

3. **Hypothesis Testing**
   - Test each analysis goal hypothesis
   - Document statistical significance
   - Create supporting visualizations

### Phase 3: Visualization & Insights
1. **Create Visualizations**
   - Generate charts for key findings
   - Build interactive dashboards if needed
   - Export static images to `reports/figures/`

2. **Statistical Analysis**
   - Run appropriate statistical tests
   - Build predictive models if required
   - Validate model performance

3. **Document Insights**
   - Summarize key findings
   - Provide actionable recommendations
   - Create executive summary

### Phase 4: Report Generation
1. **Technical Report**
   - Full methodology documentation
   - Detailed statistical results
   - Code and query documentation

2. **Executive Summary**
   - High-level findings
   - Key visualizations
   - Recommendations with impact estimates

3. **Deliverables**
   - markdown format report
   - Reusable analysis code
   - Documentation for future runs

## Output Structure

```
span-example-ecommerce-analysis/
├── analysis/
│   ├── exploration/       # Jupyter notebooks or Python scripts
│   ├── statistical/       # Statistical test results
│   └── visualizations/    # Chart generation code
├── data/
│   ├── cache/            # Cached query results
│   ├── profiling/        # Data profiling outputs
│   └── processed/        # Cleaned/transformed data
├── reports/
│   ├── executive_summary.markdown
│   ├── technical_report.markdown
│   └── figures/          # Exported visualizations
└── src/
    ├── queries/          # SQL queries
    ├── utils/            # Helper functions
    └── pipelines/        # Data processing pipelines
```

## Technical Guidelines

### Query Optimization
- Use appropriate indexes and filters
- Limit data scans with date ranges
- Aggregate at the source when possible
- Cache results for expensive queries

### Code Standards
- Use type hints in Python code
- Document all functions and queries
- Create unit tests for calculations
- Use consistent naming conventions

### Visualization Standards
- Use consistent color schemes
- Include proper labels and legends
- Export high-resolution images
- Create both static and interactive versions

## Success Criteria

1. **Data Quality**: 
   - All data quality issues documented
   - Missing data handled appropriately
   - Outliers identified and addressed

2. **Analysis Completeness**:
   - All defined metrics calculated
   - All analysis goals addressed
   - Statistical validation completed

3. **Deliverables**:
   - Reports in requested format
   - Reproducible analysis code
   - Clear documentation
   - Actionable insights

## Troubleshooting

### Common Issues
1. **MCP Connection Failed**: Check credentials in GitHub Secrets
2. **Query Timeout**: Reduce data range or add filters
3. **Memory Issues**: Use chunked processing for large datasets
4. **Missing Dependencies**: Install required packages with `pip install -r requirements.txt`

## Next Steps

After completing the analysis:
1. Review all findings with stakeholders
2. Plan follow-up analyses based on discoveries
3. Schedule recurring runs if needed
4. Archive successful queries for reuse