# CMMC SSP Parser Setup Guide

## Step-by-Step Setup Instructions

### Step 1: Environment Setup

#### Option A: Using Virtual Environment (Recommended)
```bash
# Create a new virtual environment
python3 -m venv cmmc_env

# Activate the virtual environment
# On Linux/Mac:
source cmmc_env/bin/activate
# On Windows:
cmmc_env\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

#### Option B: Direct Installation
```bash
# Install dependencies directly
pip install pandas python-docx beautifulsoup4 openpyxl lxml
```

### Step 2: Directory Structure Setup

Create the following directory structure:
```
cmmc_ssp_parser/
│
├── cmmc_parser.py          # Main parser script
├── config.json             # Configuration file
├── requirements.txt        # Python dependencies
├── ssp_prime_to_csv.csv    # Your input CSV file
│
├── output/                 # Output directory (auto-created)
│   ├── AC_controls.html
│   ├── AC_controls.docx
│   ├── MA_controls.html
│   ├── MA_controls.docx
│   └── validation_report.txt
│
└── logs/                   # Log files (auto-created)
```

### Step 3: Prepare Your CSV File

Ensure your CSV file (`ssp_prime_to_csv.csv`) has the following columns:
- CMMC_ID
- Control
- Score (values: 1, 3, or 5)
- AR_CAP_POAM (values: 0, POA&M, Audit Ready)
- Policy_Statement
- Azure
- Azure_Evidence
- Azure_O365
- Azure_O365_Evidence
- AVD_Laptop
- AVD_Laptop_Evidence

**Data Entry Tips:**
- Use semicolons (;) to separate multiple items within a cell
- For Policy_Statement, use format: `header;bullet_1;bullet_2`
- Evidence paths should be complete: `/CMMC_Evidence/Policies/policy.pdf`

### Step 4: Configure the Parser

Edit `config.json` to customize settings:
```json
{
    "input_csv": "ssp_prime_to_csv.csv",  // Your CSV filename
    "output_dir": "./output",              // Where to save outputs
    "generate_html": true,                 // Generate HTML files
    "generate_docx": true,                 // Generate DOCX files
    "validate_poam_rules": true,           // Enable validation
    "evidence_base_path": "/CMMC_Evidence/"
}
```

### Step 5: Run the Parser

#### Basic Usage:
```bash
python cmmc_parser.py
```

#### Advanced Usage:
```bash
# Use custom config file
python cmmc_parser.py -c my_config.json

# Override input/output paths
python cmmc_parser.py -i my_data.csv -o ./my_output

# Generate only HTML
python cmmc_parser.py --html-only

# Generate only DOCX
python cmmc_parser.py --docx-only

# Skip validation (not recommended)
python cmmc_parser.py --skip-validation
```

### Step 6: Review Outputs

After running, check the following:

1. **Validation Report** (`output/validation_report.txt`)
   - Review any critical errors (Score 3/5 marked as POA&M)
   - Check warnings for missing evidence
   - Verify control counts

2. **HTML Files** (`output/*_controls.html`)
   - Open in browser to review formatting
   - Check all sections are populated correctly

3. **DOCX Files** (`output/*_controls.docx`)
   - Open in Word to review formatting
   - These are ready to merge into your SSP template

4. **Parser Log** (`output/parser_log_*.log`)
   - Check for any processing errors
   - Review which files were generated

## Validation Rules

The parser enforces these critical rules:
- **Score = 1**: CAN be marked as POA&M
- **Score = 3**: CANNOT be marked as POA&M (must be implemented)
- **Score = 5**: CANNOT be marked as POA&M (critical control)

## Troubleshooting

### Common Issues:

1. **"Module not found" error**
   - Solution: Install missing dependencies with `pip install -r requirements.txt`

2. **"File not found" error**
   - Solution: Ensure CSV file is in the correct location
   - Check filename in config.json matches actual file

3. **Validation errors for Score/POA&M**
   - Solution: Review controls with Score=3 or 5 that are marked as POA&M
   - Either change Score to 1 or remove POA&M designation

4. **Empty output files**
   - Solution: Check CSV has data in required columns
   - Review parser log for specific errors

5. **Encoding errors**
   - Solution: Save CSV as UTF-8 encoding
   - In Excel: Save As > CSV UTF-8

### Testing Your Setup

Run this test to verify everything works:
```bash
# Create a test CSV with one control
echo "CMMC_ID,Control,Score,AR_CAP_POAM,Policy_Statement,Azure,Azure_Evidence,Azure_O365,Azure_O365_Evidence,AVD_Laptop,AVD_Laptop_Evidence" > test.csv
echo "3.1.1,Test control description,5,0,header;bullet1;bullet2,Azure implementation,/evidence/path1,O365 implementation,/evidence/path2,AVD implementation,/evidence/path3" >> test.csv

# Run parser on test file
python cmmc_parser.py -i test.csv -o ./test_output

# Check if files were created
ls test_output/
```

## Next Steps

After successful parsing:

1. **Review validation report** for any issues
2. **Open DOCX files** in Word to verify formatting
3. **Merge content** into your SSP template
4. **Update evidence paths** if needed
5. **Run again** after making CSV updates

## Getting Help

If you encounter issues:
1. Check the parser log file for detailed error messages
2. Verify CSV format matches requirements
3. Ensure all Python dependencies are installed
4. Review validation report for data issues

## Quick Reference

### Command Line Options:
- `-c, --config`: Specify config file
- `-i, --input`: Override input CSV
- `-o, --output`: Override output directory
- `--html-only`: Generate only HTML
- `--docx-only`: Generate only DOCX
- `--skip-validation`: Skip POA&M validation
- `--controls`: Process specific controls
- `--families`: Process specific control families
- `--range`: Process a range of controls

### Filtering Options (NEW):

#### Process Specific Controls
```bash
# Process only specific controls by their ID
python cmmc_parser.py --controls 3.1.1 3.1.2 3.7.3

# Example: Just test one control
python cmmc_parser.py --controls 3.1.1
```

#### Process Specific Control Families
```bash
# Process entire control families
python cmmc_parser.py --families AC MA SC

# Family codes:
# AC = Access Control
# AT = Awareness and Training
# AU = Audit and Accountability
# CM = Configuration Management
# IA = Identification and Authentication
# IR = Incident Response
# MA = Maintenance
# MP = Media Protection
# PS = Personnel Security
# PE = Physical Protection
# RA = Risk Assessment
# SA = Security Assessment
# SC = System and Communications Protection
# SI = System and Information Integrity

# Example: Just process Maintenance controls
python cmmc_parser.py --families MA
```

#### Process Control Ranges
```bash
# Process a range of controls (within same family)
python cmmc_parser.py --range 3.1.1-3.1.10

# Example: Process first 5 Access Control controls
python cmmc_parser.py --range 3.1.1-3.1.5
```

#### Combine Filtering with Other Options
```bash
# Generate only DOCX for specific families
python cmmc_parser.py --families AC MA --docx-only

# Generate only HTML for specific controls
python cmmc_parser.py --controls 3.7.3 3.8.1 --html-only

# Process Maintenance family without validation
python cmmc_parser.py --families MA --skip-validation

# Custom output directory for specific controls
python cmmc_parser.py --controls 3.1.1 -o ./test_output
```

### Use Cases for Filtering:

**1. Testing Changes**
```bash
# Test formatting on one control before processing all
python cmmc_parser.py --controls 3.1.1
```

**2. Incremental Processing**
```bash
# Process one family at a time
python cmmc_parser.py --families AC
python cmmc_parser.py --families AU
python cmmc_parser.py --families CM
```

**3. Update Specific Sections**
```bash
# Regenerate just the sections you updated in CSV
python cmmc_parser.py --families MA MP
```

**4. Priority Controls**
```bash
# Process high-priority controls first
python cmmc_parser.py --controls 3.1.1 3.5.3 3.6.1 3.13.1
```

**5. Quick Validation Check**
```bash
# Check POA&M validation for specific controls
python cmmc_parser.py --controls 3.1.1 3.1.2 3.1.3
# Then review validation_report.txt for just those controls
```

### CSV Format:
- Delimiter: Comma (,)
- Multi-value separator: Semicolon (;)
- Encoding: UTF-8
- Required columns: CMMC_ID, Control, Score, AR_CAP_POAM

### Output Files:
- `[FAMILY]_controls.html`: HTML version by family
- `[FAMILY]_controls.docx`: Word document by family
- `validation_report.txt`: Data validation results
- `parser_log_*.log`: Processing log
