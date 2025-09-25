# GPS Accuracy Analysis Pipeline

A comprehensive analysis tool for evaluating GPS accuracy using multi-tag data. This pipeline processes GPS logs from multiple tags kept together and provides detailed accuracy metrics, visualizations, and sales-ready reports.

## Features

- **Time Alignment**: Aligns GPS data from multiple tags using configurable time windows
- **Accuracy Metrics**: Computes pairwise distances and centroid-based error measurements
- **Speed Analysis**: Analyzes GPS performance across different speed bins
- **Motion Segmentation**: Identifies trip vs stationary periods
- **Outlier Detection**: Flags problematic time windows with excessive GPS spread
- **Rich Visualizations**: Generates publication-ready plots and charts
- **Sales Reports**: Creates both Markdown and PowerPoint presentations
- **Data Export**: Exports processed data in multiple formats (Parquet, CSV)

## Installation

1. Clone or download this repository
2. Install required dependencies:

```bash
pip install -r requirements.txt
```

## Quick Start

### Basic Analysis

```bash
python -m src.main analyze data/gps_data.xlsx
```

### Advanced Options

```bash
python -m src.main analyze data/gps_data.xlsx \
    --time-tz "America/New_York" \
    --align-window-seconds 2 \
    --rounding-seconds 1 \
    --outlier-threshold-m 20 \
    --sample-day "2024-07-15" \
    --output-dir "results"
```

### Data Validation

```bash
python -m src.main validate data/gps_data.xlsx
```

## Input Data Format

The Excel file should contain the following columns:

| Column | Description | Example |
|--------|-------------|---------|
| Vehicle Name | Tag identifier | "ALPHA [PB703-45672]" |
| Event Time | Timestamp | "7/3/25 13:44" |
| Label | Event label | "GPS" |
| Event Code | Event code | "001" |
| Valid Position | Boolean | TRUE/FALSE |
| Speed [mph] | Speed in mph | 25.5 |
| Latitude | Latitude | 40.7128 |
| Longitude | Longitude | -74.0060 |
| Altitude [m] | Altitude in meters | 10.5 |
| Map URL | Map link | "https://..." |

## Output Files

The analysis generates the following outputs in the `outputs/` directory:

### Data Files
- `aligned_samples.parquet` - Complete aligned dataset with computed metrics
- `summary_by_tag.csv` - Per-tag accuracy statistics
- `summary_by_speed_bin.csv` - Accuracy by speed category
- `summary_by_segment.csv` - Trip vs stationary performance
- `outlier_windows.csv` - Problematic time windows
- `pairwise_stats_overall.csv` - Overall pairwise distance statistics

### Visualizations (`outputs/plots/`)
- `error_timeseries_*.png` - Error trends over time
- `pairwise_distances_histogram.png` - Distance distribution
- `error_by_speed_bin.png` - Performance by speed
- `trajectory_scatter_*.png` - GPS trajectory plots
- `segment_analysis.png` - Motion segment analysis

### Reports
- `report.md` - Comprehensive Markdown report
- `gps_accuracy_findings.pptx` - Sales-ready PowerPoint presentation
- `RUNLOG.md` - Analysis execution log

## Configuration

Key parameters can be adjusted in `src/config.py`:

- **Speed Bins**: Categories for speed-based analysis
- **Motion Thresholds**: Trip vs stationary detection criteria
- **Error Thresholds**: Outlier detection limits
- **Visualization Settings**: Plot formatting and DPI

## Understanding the Results

### Key Metrics

- **Median GPS Error**: Typical accuracy under normal conditions
- **95th Percentile Error**: Worst-case accuracy for 95% of measurements
- **Inter-Tag Distance**: Consistency between multiple tags
- **Speed Performance**: How accuracy varies with vehicle speed

### Interpretation Guidelines

**Excellent Performance** (Median < 3m):
- Suitable for precise asset tracking
- Good for geofencing applications
- Reliable for fleet management

**Good Performance** (Median 3-5m):
- Adequate for general vehicle tracking
- Suitable for most commercial applications
- Consider environmental factors

**Moderate Performance** (Median 5-10m):
- Acceptable for approximate location tracking
- May need additional filtering or smoothing
- Consider stationary vs mobile use cases

### Use Case Recommendations

**Ideal For:**
- Stationary asset monitoring
- Fleet management
- Geofencing applications
- Low-speed vehicle tracking

**Consider Alternatives For:**
- High-speed precision navigation
- Real-time turn-by-turn directions
- Centimeter-level accuracy requirements
- Time-critical safety applications

## Troubleshooting

### Common Issues

1. **No Aligned Data Found**
   - Check that all 4 tags have data in the same time periods
   - Try increasing `--align-window-seconds`
   - Verify timestamp formats

2. **High Outlier Rates**
   - Check for environmental factors (buildings, tunnels, etc.)
   - Consider data quality issues
   - Review tag placement and mounting

3. **Poor Performance at High Speeds**
   - Normal GPS behavior
   - Consider speed-based filtering
   - Focus on stationary/low-speed applications

### Performance Tips

- For large datasets (>100k rows), consider sampling
- Use `--sample-day` for detailed analysis of specific periods
- Adjust alignment parameters based on your data frequency

## Technical Details

### Time Alignment Algorithm

1. Round timestamps to specified interval (default: 1 second)
2. For each rounded timestamp, find nearest record per tag within window
3. Only keep timestamps where all 4 tags are present
4. Compute average speed across all tags

### Error Metrics

- **Haversine Distance**: Great-circle distance between GPS coordinates
- **Centroid Error**: Distance from each tag to the group centroid
- **Pairwise Distance**: Distance between each pair of tags

### Motion Segmentation

- **Trip**: Speed ≥ 10 mph for ≥ 3 minutes
- **Stationary**: Speed ≤ 1 mph for ≥ 2 minutes
- **Other**: All other periods

## Contributing

This is a specialized analysis tool. For modifications:

1. Follow the existing code structure
2. Add type hints and docstrings
3. Test with sample data
4. Update documentation

## License

This tool is provided as-is for GPS accuracy analysis. Modify as needed for your specific requirements.
# notion-widget
