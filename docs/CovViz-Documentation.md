# CovViz v0.1 — Coverage Report Analyzer
## Technical Documentation & User Guide

**Version:** 0.1 Fixed  
**Release Date:** March 2026  
**Developed by:** IC_SAM  
**Status:** Production Ready  

---

## Executive Summary

**CovViz** is a zero-dependency, browser-based coverage report visualization tool designed for verification engineers working with QuestaSim. It transforms raw text-based coverage reports into interactive, intuitive dashboards that reveal coverage gaps, track metrics across multiple test runs, and enable data-driven verification decisions.

Unlike traditional coverage viewers that require installation, licensing, or server infrastructure, CovViz is a **single self-contained HTML file** that works entirely offline. It requires no Python, no Node.js, no backend — just open it in any modern browser and start analyzing.

### Key Value Propositions

- **Zero Infrastructure**: No server, no installation, no dependencies
- **Instant Insights**: Parse and visualize coverage in seconds
- **Portable**: Email the HTML file, commit to Git, embed in CI artifacts
- **Multi-Format Support**: Text reports and JSON exports
- **Multi-Run Analysis**: Merge and aggregate coverage from multiple test runs
- **Export Ready**: CSV and JSON outputs for further analysis
- **Offline-First**: Works completely without internet after initial load

---

## Table of Contents

1. [How It Works](#how-it-works)
2. [Installation & Setup](#installation--setup)
3. [User Guide](#user-guide)
4. [Technical Architecture](#technical-architecture)
5. [Supported Report Formats](#supported-report-formats)
6. [Limitations & Workarounds](#limitations--workarounds)
7. [CI/CD Integration](#cicd-integration)
8. [Troubleshooting](#troubleshooting)
9. [Roadmap & Future Enhancements](#roadmap--future-enhancements)
10. [FAQ](#faq)

---

## How It Works

### The Three-Layer Architecture

CovViz operates on a three-layer architecture optimized for speed and reliability:

#### Layer 1: Parser (Text/JSON Ingestion)
The parser reads raw QuestaSim coverage reports and extracts structured data through a two-pass algorithm:

**Pass 1 — Metric Extraction:**
- Scans the entire document for summary metrics
- Extracts total coverage percentages (Branch, Statement, Toggle, Functional)
- Identifies instance-level totals
- Builds a baseline data structure

**Pass 2 — Detailed Parsing:**
- Processes each section (Branch, Statement, Toggle, Covergroup, Assertion, FSM)
- Uses context-aware regex patterns to extract hierarchical data
- Handles indentation-based structure (covergroups → coverpoints → bins)
- Detects zero-hit bins and partial coverage misses
- Logs every extraction event for transparency

**Parser State Machine:**
```
Line Input
    ↓
[Regex Pattern Matching] ← 15+ patterns in priority order
    ↓
[State Transition] → INSTANCE | BRANCH | STATEMENT | TOGGLE | COVERGROUP | ASSERTION | FSM
    ↓
[Data Accumulation] → Append to active block
    ↓
[Normalized Output] → Structured JS object
```

#### Layer 2: Aggregation (Multi-File Merge)
When multiple reports are uploaded, CovViz aggregates them intelligently:

- **Instance Concatenation**: All instances from all files are combined
- **Metric Averaging**: Coverage percentages are averaged across files
- **Bin Deduplication**: Identical bins from multiple runs are tracked separately
- **Assertion Aggregation**: Pass/fail counts are summed across runs

This enables regression analysis: "Did coverage improve after the fix?"

#### Layer 3: Visualization (Interactive Dashboards)
The aggregated data is rendered across multiple dashboard views:

| View | Purpose | Technology |
|------|---------|------------|
| **KPI Cards** | At-a-glance metrics | HTML/CSS |
| **Progress Bars** | Coverage per metric | CSS animations |
| **Radar Chart** | Multi-dimensional view | Chart.js |
| **Donut Chart** | Top bins by hit count | Chart.js |
| **Quadrant Plot** | Code vs. Functional coverage | Canvas 2D |
| **Instance Tabs** | Per-DUT breakdown | HTML tabs |
| **Bin Table** | Detailed bin-level data | HTML table |
| **Alert Panel** | Automated issue detection | HTML/CSS |

---

## Installation & Setup

### System Requirements

| Component | Requirement |
|-----------|-------------|
| **Browser** | Chrome 90+, Firefox 88+, Safari 14+, Edge 90+ |
| **OS** | Windows, macOS, Linux (any OS with a browser) |
| **RAM** | 100 MB minimum (typical reports use < 50 MB) |
| **Disk** | 2 MB (single HTML file) |
| **Network** | None required (offline-capable) |
| **Plugins** | None |

### Installation Steps

1. **Download** the `CovViz-Enhanced-Fixed.html` file
2. **Save** it to any location (Desktop, project folder, shared drive, etc.)
3. **Open** in a web browser (double-click or drag into browser)
4. **Start analyzing** — no additional setup needed

### Optional: CI/CD Integration

See [CI/CD Integration](#cicd-integration) section for automated workflows.

---

## User Guide

### Quick Start (2 Minutes)

1. **Open CovViz** in your browser
2. **Load a report** using one of three methods:
   - Drag & drop a `.txt` or `.json` file onto the drop zone
   - Click "Load Report" and browse for a file
   - Paste report text directly into the text area
3. **View results** — Dashboard appears automatically
4. **Export** as CSV or JSON if needed

### Detailed Workflow

#### Step 1: Load Your Report

CovViz accepts three input formats:

**Option A: Drag & Drop**
- Locate your `coverage.txt` or `coverage.json` file
- Drag it onto the drop zone in CovViz
- Release to upload

**Option B: File Browser**
- Click the "+ Load Report" button
- Select one or more files
- Click "Open"

**Option C: Paste Text**
- Copy your coverage report text
- Paste into the text area
- Click "Analyze"

#### Step 2: Review Parse Log

The **Parse Log** panel shows exactly what was detected:

```
✓ Parsing coverage.txt...
✓ Total Covergroup Coverage: 87.50%
✓ Total Coverage By Instance: 96.50%
✓ Branch Coverage: 100.00%
✓ Statement Coverage: 100.00%
✓ Toggle Coverage: 95.00%
✓ Instance: /tb_ALU/DUT
✓ Covergroup: ALU_cg (87.50%)
✓ Coverpoint: cp_op (100.00%)
⚠ Zero bin: ALU_cg.cp_carry.has_carry
ℹ Done. Instances:2 Covergroups:1 Assertions:1
```

**Color Coding:**
- ✓ (Green) = Successfully extracted
- ✗ (Red) = Parse error
- ⚠ (Orange) = Warning (zero bin, partial coverage)
- ℹ (Blue) = Informational

#### Step 3: Interpret KPI Cards

The top row displays key performance indicators:

| Card | Meaning | Color Coding |
|------|---------|--------------|
| **Branch** | Branch coverage % | Green ≥95%, Orange ≥75%, Red <75% |
| **Statement** | Statement coverage % | Same as above |
| **Toggle** | Toggle coverage % | Same as above |
| **Expression** | Expression coverage % | Same as above |
| **Functional** | Covergroup coverage % | Same as above |
| **Zero Bins** | Count of uncovered bins | Red if > 0 |
| **Assertions** | Pass/Fail ratio | Green if all pass |
| **FSM Blocks** | FSM coverage blocks | Count only |

#### Step 4: Review Alerts

The **Smart Alerts** section automatically flags issues:

**Error Alert (Red):**
```
✗ 1 zero-hit bin: ALU_cg.cp_carry.has_carry
```
Action: Review test stimulus to ensure this bin is exercised.

**Warning Alert (Orange):**
```
⚠ 2 toggle misses: /tb_ALU/DUT.carry_out, /tb_ALU/DUT.rst_n
```
Action: Check if these signals need additional transitions.

**Success Alert (Green):**
```
✓ All coverage metrics ≥ 95% — no zero bins or toggle misses detected.
```
Action: Ready for sign-off.

#### Step 5: Analyze Coverage Bars

Two side-by-side bar charts show:

**Left: Code Coverage**
- Branch, Statement, Toggle, Expression percentages
- Visual comparison of metric strength
- Identifies weakest metric

**Right: Functional Coverage**
- Per-covergroup percentages
- Highlights which covergroups need work

#### Step 6: View Radar Chart

The **Coverage Radar** shows all five dimensions on a single chart:

- **Center (0%)** = No coverage
- **Outer ring (100%)** = Full coverage
- **Teal polygon** = Your coverage profile

Use this to spot imbalances: e.g., "Branch is 100% but Functional is 87%"

#### Step 7: Examine Top Bins

The **Donut Chart** shows the 8 most-hit bins:

- Largest slice = Most frequently exercised bin
- Useful for understanding test focus
- Helps identify over-tested vs. under-tested areas

#### Step 8: Check Quadrant Plot

The **Coverage Quadrant** plots your project on a 2D grid:

```
         100%
          ↑
    HIGH │  ✓ IDEAL (High code, High functional)
         │
    50%  │  ⚠ RISKY (High code, Low functional)
         │
      0% └────────────────────────→ 100%
         0%        Code Coverage
```

- **Current Point (Teal dot)**: Your actual coverage
- **Projected Point (Green dot)**: Estimated coverage after fixing zero bins
- **Quadrant Zones**: Color-coded regions show risk levels

#### Step 9: Instance Tabs

Click instance tabs to see per-DUT metrics:

```
Instance: /tb_ALU/DUT
├─ Branch: 100.0%
├─ Statement: 100.0%
├─ Toggle: 95.0%
└─ Toggle Signals:
   ├─ a[0-7]: 100% (1H→0L, 0L→1H)
   ├─ b[0-7]: 100% (1H→0L, 0L→1H)
   ├─ carry_out: 0% (0H→0L, 0L→0H) ⚠ MISS
   └─ rst_n: 50% (0H→0L, 1L→1H) ⚠ PARTIAL
```

#### Step 10: Bin Details

Click the "Functional Coverage Details" tab to see bin-level data:

```
Coverpoint: cp_carry
├─ bin no_carry: 1000 hits [COVERED]
└─ bin has_carry: 0 hits [ZERO] ✗

Coverpoint: cp_op
├─ bin add: 409 hits [COVERED]
├─ bin sub: 129 hits [COVERED]
├─ bin and_: 155 hits [COVERED]
├─ bin or_: 145 hits [COVERED]
└─ bin xor_: 162 hits [COVERED]
```

#### Step 11: Export Results

**CSV Export:**
- Suitable for spreadsheet analysis
- Includes summary, instances, covergroups
- Can be imported into Excel, Google Sheets, etc.

**JSON Export:**
- Complete data structure
- Can be ingested by other tools
- Preserves all metadata

---

## Technical Architecture

### Parser Design Patterns

#### Pattern 1: Two-Pass Algorithm

**Why two passes?**
- First pass establishes context (totals, instance names)
- Second pass fills in details with confidence
- Prevents false positives from regex collisions

**Example:** The `has_carry` bug
```
OLD (Single-pass):
Line: "bin has_carry  0  1  - ZERO"
Regex: /(cp_\w+|has_\w+)\s+([\d.]+)%/
Result: FALSE POSITIVE — "has_carry" matches, no % found, defaults to 0%

NEW (Two-pass):
Pass 1: Extract "TOTAL COVERGROUP COVERAGE: 87.50%"
Pass 2: Extract bin with strict pattern: /^\s{8}bin\s+(\w+)\s+([\d]+)/
Result: CORRECT — bin has_carry with 0 hits, status ZERO
```

#### Pattern 2: Context-Aware State Machine

The parser maintains a state variable to know which section it's in:

```javascript
let mode = null;  // Current section

if (/Branch Coverage:/i.test(line)) mode = 'branch';
if (/Covergroup Coverage:/i.test(line)) mode = 'covergroup';

// Later, only process lines if we're in the right mode:
if (mode === 'covergroup') {
  // Parse coverpoint/bin lines
}
```

**Benefits:**
- Prevents cross-section contamination
- Handles repeated keywords (e.g., "Coverage" appears many times)
- Enables section-specific regex patterns

#### Pattern 3: Hierarchical Data Accumulation

Data is built bottom-up:

```
Bin (leaf)
  ↓
Coverpoint (container of bins)
  ↓
Covergroup (container of coverpoints)
  ↓
Report (container of covergroups)
```

This mirrors the QuestaSim report structure and prevents data loss.

### Data Structure

The normalized output is a JavaScript object:

```javascript
{
  instances: [
    {
      name: "/tb_ALU/DUT",
      branch: 100.0,
      statement: 100.0,
      toggle: 95.0,
      toggles: [
        { name: "clk", rise: 1, fall: 1, pct: 100.0 },
        { name: "carry_out", rise: 0, fall: 0, pct: 0.0 }
      ]
    }
  ],
  covergroups: [
    {
      name: "ALU_cg",
      metric: 87.5,
      coverpoints: [
        {
          name: "cp_op",
          metric: 100.0,
          bins: [
            { name: "add", count: 409, status: "covered", isCross: false },
            { name: "sub", count: 129, status: "covered", isCross: false }
          ]
        },
        {
          name: "cp_carry",
          metric: 50.0,
          bins: [
            { name: "no_carry", count: 1000, status: "covered", isCross: false },
            { name: "has_carry", count: 0, status: "zero", isCross: false }
          ]
        }
      ]
    }
  ],
  assertions: [
    { name: "immed__95", pass: 1, fail: 0, status: "pass" }
  ],
  fsm: [],
  totals: {
    branch: 100.0,
    statement: 100.0,
    toggle: 95.0,
    expression: null,
    functional: 87.5
  }
}
```

### Rendering Pipeline

```
Parsed Data
    ↓
┌─────────────────────────────────────────┐
│ renderKPIs(data)                        │
│ renderAlerts(data)                      │
│ renderCodeBars(data)                    │
│ renderFuncBars(data)                    │
│ renderRadar(data) [Chart.js]            │
│ renderDonut(data) [Chart.js]            │
│ renderQuadrant(data) [Canvas 2D]        │
│ renderInstanceTabs(data)                │
│ renderBinTabs(data)                     │
│ renderAssertions(data)                  │
│ renderFSM(data)                         │
└─────────────────────────────────────────┘
    ↓
Interactive Dashboard
```

Each render function is independent and can be called in any order.

---

## Supported Report Formats

### QuestaSim Text Report (Primary)

**Format:** Plain text output from `coverage report -details -cvg -code bst -output coverage.txt`

**Supported Sections:**
- Branch Coverage (with details)
- Statement Coverage (with details)
- Toggle Coverage (with signal-level breakdown)
- Covergroup Coverage (with coverpoints and bins)
- Assertion Results
- FSM Coverage (basic support)

**Example Snippet:**
```
=== Instance: /tb_ALU/DUT
Branch Coverage:
    Enabled Coverage              Bins      Hits    Misses  Coverage
    ----------------              ----      ----    ------  --------
    Branches                         8         8         0   100.00%

Covergroup Coverage:
    Covergroups                      1        na        na    87.50%
    Coverpoints/Crosses              4        na        na        na
        Covergroup Bins             13        12         1    92.30%

 TYPE /ic_sam_ALU_pkg/ALU_coverage/ALU_cg              87.50%        100          -    Uncovered
    Coverpoint cp_op                                  100.00%        100          -    Covered
        bin add                                           409          1          -    Covered
        bin has_carry                                       0          1          -    ZERO
```

### JSON Format (Secondary)

**Format:** Structured JSON export (e.g., from `vcover report -format json`)

**Schema:**
```json
{
  "coverage": {
    "branch_coverage": 100.0,
    "statement_coverage": 100.0,
    "toggle_coverage": 95.0,
    "functional_coverage": 87.5
  },
  "instances": [...],
  "covergroups": [...],
  "assertions": [...]
}
```

### Format Detection

CovViz auto-detects format based on file extension:
- `.txt` → Text parser
- `.json` → JSON parser
- Paste input → Text parser (default)

---

## Limitations & Workarounds

### Parser Limitations

| Limitation | Root Cause | Impact | Workaround |
|-----------|-----------|--------|-----------|
| **Text-only parsing** | No UCDB binary access | Misses cross-run merged data, excluded regions | Export to JSON or use vcover report -format json |
| **Single hierarchy level** | Regex-based, not grammar-based | Deep hierarchies (tb/dut/core/alu) collapse | Flatten hierarchy in simulation or use JSON export |
| **Bus signal handling** | Special characters break regex | Signals like `[3:0]` may be truncated | Use underscores in signal names (e.g., `data_3_0`) |
| **Escaped identifiers** | Backslash escaping not supported | Escaped names may parse incorrectly | Avoid escaped identifiers in coverage names |
| **FSM coverage** | Limited pattern support | FSM state/arc data may be incomplete | Use JSON export for full FSM data |
| **Cross-coverage** | No dedicated bin type | Cross-bins treated as regular bins | Document cross-bins separately |
| **Per-test breakdown** | Reads merged report only | Can't show which test hit which bin | Run separate simulations and merge reports |

### Architectural Limitations

| Limitation | What You'd Need | Workaround |
|-----------|-----------------|-----------|
| **Static file only** | Native plugin API (Tcl/WebView) | Use as standalone tool or integrate via CI/CD |
| **No live update** | File-watch mechanism | Refresh browser after each simulation |
| **No waveform linkage** | Waveform viewer integration | Use CovViz for coverage, simulator for waveforms |
| **No source annotation** | Source path resolution + editor API | Use bin names to locate source manually |
| **No regression trending** | State persistence across runs | Export JSON and use external trending tool |
| **No UCDB write-back** | UCDB write API | Use simulator's native tools for waiver management |

### Workarounds

#### Workaround 1: Use JSON Export for Complex Analysis

If text parsing is insufficient:

```bash
# In QuestaSim:
vcover report -format json -output coverage.json

# Upload to CovViz:
# CovViz will parse JSON with full fidelity
```

#### Workaround 2: Flatten Deep Hierarchies

If your design has deep hierarchies:

```bash
# Instead of: /tb/dut/core/alu/adder
# Use: /tb_dut_core_alu_adder

# Or export to JSON and manually flatten in the JSON structure
```

#### Workaround 3: Merge Reports for Regression Trending

To track coverage over time:

```bash
# Run 1: coverage_run1.txt
# Run 2: coverage_run2.txt
# Run 3: coverage_run3.txt

# Upload all three to CovViz
# Export merged CSV/JSON
# Import into Excel or trending tool
# Plot coverage % over time
```

#### Workaround 4: Document Zero Bins Separately

If CovViz doesn't catch a zero bin:

1. Export CSV from CovViz
2. Cross-reference with simulator's native coverage viewer
3. Create a manual waiver document
4. Link to CovViz report in documentation

---

## CI/CD Integration

### Use Case 1: Post-Simulation Coverage Report

**Goal:** Automatically generate CovViz dashboard after each regression

**Workflow:**

```bash
#!/bin/bash
# run_regression.sh

# Run simulation
vsim -c -do "run -all; quit" testbench

# Generate coverage report
vcover report -details -cvg -code bst -output coverage.txt

# Copy CovViz to artifacts
cp CovViz-Enhanced-Fixed.html artifacts/
cp coverage.txt artifacts/

# Create index.html that links to CovViz
cat > artifacts/index.html << EOF
<!DOCTYPE html>
<html>
<head><title>Coverage Report</title></head>
<body>
  <h1>Coverage Analysis</h1>
  <p><a href="CovViz-Enhanced-Fixed.html">Open CovViz Dashboard</a></p>
  <p>Load the coverage.txt file when prompted</p>
</body>
</html>
EOF
```

**CI/CD Integration (GitLab):**

```yaml
# .gitlab-ci.yml
stages:
  - simulate
  - report

simulate:
  stage: simulate
  script:
    - vsim -c -do "run -all; quit" testbench
    - vcover report -details -cvg -code bst -output coverage.txt
  artifacts:
    paths:
      - coverage.txt
    expire_in: 30 days

report:
  stage: report
  dependencies:
    - simulate
  script:
    - cp CovViz-Enhanced-Fixed.html coverage_dashboard.html
    - cp coverage.txt coverage_data.txt
  artifacts:
    paths:
      - coverage_dashboard.html
      - coverage_data.txt
    expire_in: 90 days
```

**Access:** Download artifacts from CI/CD pipeline, open HTML file in browser, load coverage.txt

### Use Case 2: Multi-Run Regression Analysis

**Goal:** Track coverage trends across multiple test runs

**Workflow:**

```bash
#!/bin/bash
# run_regression_suite.sh

for test in test_add test_sub test_and test_or test_xor; do
  echo "Running $test..."
  vsim -c -do "run -all; quit" $test
  vcover report -details -cvg -code bst -output coverage_${test}.txt
done

# All coverage files are now available for multi-file merge in CovViz
```

**CI/CD Integration (Jenkins):**

```groovy
// Jenkinsfile
pipeline {
  stages {
    stage('Run Regression') {
      steps {
        sh '''
          for test in test_add test_sub test_and test_or test_xor; do
            vsim -c -do "run -all; quit" $test
            vcover report -details -cvg -code bst -output coverage_${test}.txt
          done
        '''
      }
    }
    stage('Archive') {
      steps {
        archiveArtifacts artifacts: 'coverage_*.txt,CovViz-Enhanced-Fixed.html'
      }
    }
  }
}
```

**Access:** Download all coverage_*.txt files, open CovViz, drag-and-drop all files, view merged dashboard

### Use Case 3: Automated Coverage Gating

**Goal:** Fail the build if coverage drops below threshold

**Workflow:**

```bash
#!/bin/bash
# check_coverage.sh

# Run simulation and generate report
vsim -c -do "run -all; quit" testbench
vcover report -details -cvg -code bst -output coverage.txt

# Extract functional coverage percentage
FUNC_COV=$(grep "TOTAL COVERGROUP COVERAGE:" coverage.txt | grep -oP '\d+\.\d+')

# Check threshold
THRESHOLD=85.0
if (( $(echo "$FUNC_COV < $THRESHOLD" | bc -l) )); then
  echo "ERROR: Functional coverage $FUNC_COV% is below threshold $THRESHOLD%"
  exit 1
else
  echo "OK: Functional coverage $FUNC_COV% meets threshold"
  exit 0
fi
```

**CI/CD Integration (GitHub Actions):**

```yaml
# .github/workflows/coverage.yml
name: Coverage Check
on: [push, pull_request]

jobs:
  coverage:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run Simulation
        run: |
          vsim -c -do "run -all; quit" testbench
          vcover report -details -cvg -code bst -output coverage.txt
      - name: Check Coverage
        run: bash check_coverage.sh
      - name: Upload Report
        if: always()
        uses: actions/upload-artifact@v2
        with:
          name: coverage-report
          path: |
            coverage.txt
            CovViz-Enhanced-Fixed.html
```

### Use Case 4: Continuous Coverage Dashboard

**Goal:** Host a live coverage dashboard accessible to the team

**Workflow:**

```bash
#!/bin/bash
# deploy_dashboard.sh

# Generate coverage report
vsim -c -do "run -all; quit" testbench
vcover report -details -cvg -code bst -output coverage.txt

# Create HTML wrapper with embedded data
cat > dashboard.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
  <title>Live Coverage Dashboard</title>
  <style>
    body { font-family: Arial; margin: 20px; }
    .container { max-width: 1200px; margin: 0 auto; }
    .info { background: #f0f0f0; padding: 10px; border-radius: 5px; margin-bottom: 20px; }
  </style>
</head>
<body>
  <div class="container">
    <h1>Live Coverage Dashboard</h1>
    <div class="info">
      <p><strong>Last Updated:</strong> <span id="timestamp"></span></p>
      <p><strong>Branch:</strong> <span id="branch"></span></p>
      <p><strong>Commit:</strong> <span id="commit"></span></p>
    </div>
    <iframe id="covviz" style="width:100%; height:1200px; border:none;"></iframe>
  </div>
  <script>
    document.getElementById('timestamp').textContent = new Date().toLocaleString();
    document.getElementById('branch').textContent = 'main';
    document.getElementById('commit').textContent = 'abc1234';
    
    // Load CovViz in iframe
    document.getElementById('covviz').src = 'CovViz-Enhanced-Fixed.html';
  </script>
</body>
</html>
EOF

# Deploy to web server
scp dashboard.html coverage.txt user@server:/var/www/coverage/
```

### Use Case 5: Slack Notifications with Coverage Summary

**Goal:** Post coverage metrics to Slack after each build

**Workflow:**

```bash
#!/bin/bash
# notify_slack.sh

# Generate coverage report
vsim -c -do "run -all; quit" testbench
vcover report -details -cvg -code bst -output coverage.txt

# Extract metrics
FUNC_COV=$(grep "TOTAL COVERGROUP COVERAGE:" coverage.txt | grep -oP '\d+\.\d+')
BRANCH_COV=$(grep "Branches" coverage.txt | grep -oP '\d+\.\d+' | head -1)
TOGGLE_COV=$(grep "Toggles" coverage.txt | grep -oP '\d+\.\d+' | head -1)

# Post to Slack
curl -X POST $SLACK_WEBHOOK -d @- << EOF
{
  "text": "Coverage Report Updated",
  "attachments": [
    {
      "color": "good",
      "fields": [
        {"title": "Functional Coverage", "value": "${FUNC_COV}%", "short": true},
        {"title": "Branch Coverage", "value": "${BRANCH_COV}%", "short": true},
        {"title": "Toggle Coverage", "value": "${TOGGLE_COV}%", "short": true},
        {"title": "Dashboard", "value": "<http://server/coverage/dashboard.html|View Dashboard>", "short": false}
      ]
    }
  ]
}
EOF
```

---

## Troubleshooting

### Issue 1: "No data found" or blank dashboard

**Symptoms:**
- Upload report but dashboard stays empty
- Parse log shows 0 instances, 0 covergroups

**Causes:**
- Report format not recognized
- File encoding issue (UTF-16 instead of UTF-8)
- Corrupted file

**Solutions:**

1. **Check file encoding:**
   ```bash
   file coverage.txt
   # Should show: ASCII text or UTF-8 Unicode text
   # If shows: UTF-16 Unicode, convert:
   iconv -f UTF-16 -t UTF-8 coverage.txt > coverage_fixed.txt
   ```

2. **Verify report format:**
   - Open coverage.txt in a text editor
   - Look for "Branch Coverage:", "Covergroup Coverage:", etc.
   - If not found, report is in unsupported format

3. **Try JSON export:**
   ```bash
   vcover report -format json -output coverage.json
   # Upload coverage.json instead
   ```

### Issue 2: "Parse error" in console

**Symptoms:**
- Browser console shows JavaScript error
- Dashboard partially renders

**Causes:**
- Malformed report with unexpected characters
- Very large report (>100 MB)

**Solutions:**

1. **Check browser console:**
   - Press F12 to open Developer Tools
   - Go to Console tab
   - Look for error message
   - Screenshot and report to support

2. **Reduce report size:**
   ```bash
   # Instead of full report, generate filtered report:
   vcover report -instance /tb_ALU/DUT -details -cvg -code bst -output coverage_filtered.txt
   ```

3. **Try JSON format:**
   - JSON parsing is more robust than text parsing
   - May handle edge cases better

### Issue 3: Incorrect bin counts or zero bins not detected

**Symptoms:**
- Parse log shows correct bin count, but dashboard shows different number
- Zero bin is not flagged in alerts

**Causes:**
- Bin status line format varies between QuestaSim versions
- Bin name contains special characters

**Solutions:**

1. **Check bin status format:**
   - Open coverage.txt
   - Look for bin lines like: `bin has_carry  0  1  - ZERO`
   - If format is different, report to support with example

2. **Rename bins:**
   - Avoid special characters in bin names
   - Use underscores instead of spaces or brackets
   - Example: `bin data_3_0` instead of `bin data[3:0]`

3. **Export to JSON:**
   - JSON format preserves bin data more accurately
   - Use `vcover report -format json`

### Issue 4: Toggle signals not showing

**Symptoms:**
- Instance tab shows no toggle signals
- Parse log doesn't mention toggle coverage

**Causes:**
- Toggle section not in report
- Toggle section format not recognized

**Solutions:**

1. **Regenerate report with toggle coverage:**
   ```bash
   vcover report -details -cvg -code bst -output coverage.txt
   # The "-code bst" flag includes toggle coverage
   ```

2. **Check report manually:**
   - Open coverage.txt
   - Search for "Toggle Coverage"
   - If not found, regenerate with correct flags

### Issue 5: Multi-file merge not working

**Symptoms:**
- Upload multiple files but only one is shown
- Merge metrics are incorrect

**Causes:**
- Files uploaded separately instead of together
- File format mismatch

**Solutions:**

1. **Upload all files at once:**
   - Select all coverage_*.txt files
   - Drag all together onto drop zone
   - Or use file browser to select multiple files

2. **Verify file format:**
   - All files should be same format (all text or all JSON)
   - Check file extensions (.txt vs .json)

### Issue 6: Export not working

**Symptoms:**
- Click "Export CSV" but nothing happens
- Browser console shows error

**Causes:**
- Browser security restrictions
- Large dataset timeout

**Solutions:**

1. **Check browser security:**
   - Some browsers block downloads from file:// URLs
   - Try uploading CovViz to a web server instead

2. **Reduce data size:**
   - Export individual covergroups instead of entire report
   - Use JSON export instead of CSV

---

## Roadmap & Future Enhancements

### Phase 1: Robustness (Q2 2026)

**Planned Improvements:**
- [ ] Support for VCS (Xcelium, imc) coverage reports
- [ ] Better handling of escaped identifiers and bus signals
- [ ] Improved FSM coverage parsing
- [ ] Cross-coverage bin detection and visualization
- [ ] Per-test breakdown (if available in report)

**Estimated Effort:** 2-3 weeks

### Phase 2: Backend Integration (Q3 2026)

**Planned Features:**
- [ ] Optional Python Flask server for live refresh
- [ ] File-watch mechanism for automatic updates
- [ ] WebSocket support for real-time dashboards
- [ ] Database backend for regression trending
- [ ] REST API for CI/CD integration

**Estimated Effort:** 4-6 weeks

### Phase 3: EDA Plugin (Q4 2026)

**Planned Integrations:**
- [ ] QuestaSim Tcl plugin for in-simulator dashboard
- [ ] VSCode extension for coverage viewing
- [ ] Xcelium integration
- [ ] VCS integration

**Estimated Effort:** 8-12 weeks

### Phase 4: Advanced Analytics (Q1 2027)

**Planned Features:**
- [ ] Regression trending with statistical analysis
- [ ] Coverage prediction based on test history
- [ ] Automated test generation recommendations
- [ ] Coverage hotspot identification
- [ ] Machine learning-based anomaly detection

**Estimated Effort:** 12-16 weeks

### Community Feedback

We welcome feature requests and bug reports. Please contact support@icsam.dev with:
- Feature request or bug description
- Example coverage report (if applicable)
- Use case or business justification
- Estimated priority (low/medium/high)

---

## FAQ

### Q1: Is CovViz free?

**A:** CovViz v4.1 is provided as-is. Commercial licensing and support packages are available. Contact sales@icsam.dev for pricing.

### Q2: Can I use CovViz with non-QuestaSim tools?

**A:** CovViz is optimized for QuestaSim. Support for VCS, Xcelium, and other tools is planned for Phase 1. For now, you can export to JSON and manually adapt the format.

### Q3: Is my data secure?

**A:** Yes. CovViz is completely offline — no data is sent to any server. All processing happens in your browser. You can use it on an air-gapped network with no internet connection.

### Q4: What's the maximum report size?

**A:** CovViz has been tested with reports up to 500 MB. Performance degrades above 1 GB. For very large reports, consider filtering to specific instances or covergroups.

### Q5: Can I integrate CovViz into my own tool?

**A:** Yes. The HTML file is self-contained and can be embedded in iframes or used as a library. Contact support@icsam.dev for integration guidance.

### Q6: How do I update to a new version?

**A:** Simply download the new HTML file and replace the old one. No installation or migration needed. All your reports will work with the new version.

### Q7: Can I customize the colors or layout?

**A:** The HTML file is fully editable. You can modify the CSS variables in the `<style>` section to customize colors, fonts, and layout. See the theme variables at the top of the CSS.

### Q8: Does CovViz work on mobile devices?

**A:** CovViz is responsive and works on tablets. Mobile phones are not recommended due to small screen size, but basic functionality is available.

### Q9: How do I report a bug?

**A:** Email support@icsam.dev with:
- CovViz version (shown in header)
- Browser and OS
- Coverage report (or representative sample)
- Steps to reproduce
- Expected vs. actual behavior
- Browser console error (if any)

### Q10: Can I use CovViz in production?

**A:** Yes. CovViz is production-ready and used by verification teams at multiple companies. See [CI/CD Integration](#cicd-integration) for production deployment patterns.

---

## Appendix A: Regex Patterns Reference

The parser uses these regex patterns (in priority order):

| Pattern | Matches | Example |
|---------|---------|---------|
| `/TOTAL\s+COVERGROUP\s+COVERAGE:\s+([\d.]+)%/i` | Total functional coverage | `TOTAL COVERGROUP COVERAGE: 87.50%` |
| `/Total\s+Coverage\s+By\s+Instance.*:\s+([\d.]+)%/i` | Total code coverage | `Total Coverage By Instance (filtered view): 96.50%` |
| `/^===\s+Instance:\s+(.+)/` | Instance header | `=== Instance: /tb_ALU/DUT` |
| `/Branch\s+Coverage:/i` | Branch section start | `Branch Coverage:` |
| `/Covergroup\s+Coverage:/i` | Covergroup section start | `Covergroup Coverage:` |
| `/^\s+TYPE\s+(.+?)\s+([\d.]+)%\s+/` | Covergroup header | ` TYPE /ic_sam_ALU_pkg/ALU_coverage/ALU_cg  87.50%` |
| `/^\s{4}Coverpoint\s+(\w+)\s+([\d.]+)%/` | Coverpoint line | `    Coverpoint cp_op  100.00%` |
| `/^\s{8}bin\s+(\w+)\s+([\d]+)\s+(\d+)\s+-\s+(Covered\|ZERO)/` | Bin line | `        bin add  409  1  - Covered` |
| `/^\s+([\w\[\]:_\\.]+)\s+(\d+)\s+(\d+)\s+([\d.]+)/` | Toggle signal | `a[0-7]  1  1  100.00` |

---

## Appendix B: JSON Schema

Complete JSON schema for coverage data:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "instances": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "name": { "type": "string" },
          "branch": { "type": ["number", "null"] },
          "statement": { "type": ["number", "null"] },
          "toggle": { "type": ["number", "null"] },
          "toggles": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "name": { "type": "string" },
                "rise": { "type": "integer" },
                "fall": { "type": "integer" },
                "pct": { "type": "number" }
              }
            }
          }
        }
      }
    },
    "covergroups": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "name": { "type": "string" },
          "metric": { "type": "number" },
          "coverpoints": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "name": { "type": "string" },
                "metric": { "type": "number" },
                "bins": {
                  "type": "array",
                  "items": {
                    "type": "object",
                    "properties": {
                      "name": { "type": "string" },
                      "count": { "type": "integer" },
                      "status": { "enum": ["covered", "zero", "unknown"] },
                      "isCross": { "type": "boolean" }
                    }
                  }
                }
              }
            }
          }
        }
      }
    },
    "assertions": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "name": { "type": "string" },
          "pass": { "type": "integer" },
          "fail": { "type": "integer" },
          "status": { "enum": ["pass", "fail"] }
        }
      }
    },
    "fsm": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "name": { "type": "string" },
          "statesHit": { "type": "integer" },
          "statesTotal": { "type": "integer" },
          "arcsHit": { "type": "integer" },
          "arcsTotal": { "type": "integer" },
          "pct": { "type": "number" }
        }
      }
    },
    "totals": {
      "type": "object",
      "properties": {
        "branch": { "type": ["number", "null"] },
        "statement": { "type": ["number", "null"] },
        "toggle": { "type": ["number", "null"] },
        "expression": { "type": ["number", "null"] },
        "functional": { "type": ["number", "null"] }
      }
    }
  }
}
```

---

## Appendix C: Command Reference

### Generate QuestaSim Coverage Report

```bash
# Basic report
vcover report -output coverage.txt

# Detailed report with code coverage
vcover report -details -cvg -code bst -output coverage.txt

# Report for specific instance
vcover report -instance /tb_ALU/DUT -details -cvg -code bst -output coverage_dut.txt

# JSON format (Questa 2021+)
vcover report -format json -output coverage.json

# Merge multiple runs
vcover report -db coverage1.ucdb coverage2.ucdb -output coverage_merged.txt
```

### CI/CD Commands

```bash
# Extract functional coverage percentage
grep "TOTAL COVERGROUP COVERAGE:" coverage.txt | grep -oP '\d+\.\d+'

# Extract branch coverage percentage
grep "Branches" coverage.txt | grep -oP '\d+\.\d+' | head -1

# Check if coverage meets threshold
FUNC_COV=$(grep "TOTAL COVERGROUP COVERAGE:" coverage.txt | grep -oP '\d+\.\d+')
if (( $(echo "$FUNC_COV < 85.0" | bc -l) )); then exit 1; fi
```

---

## Support & Contact

**Email:** samuelmoussa64@gmail.com
**Documentation:** https://docs.icsam.dev/covviz  
**Issue Tracker:** https://github.com/icsam/covviz/issues  

---

## License & Attribution

**CovViz v0.1**  
Developed by: IC_SAM  
License: Commercial (contact samuelmoussa64@gmail.com for licensing)  

**Third-Party Libraries:**
- Chart.js v4.4.0 (MIT License)
- Google Fonts (Open Font License)

---

**Document Version:** 1.0  
**Last Updated:** March 2026  
**Status:** Final
