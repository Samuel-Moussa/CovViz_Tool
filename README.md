# CovViz — Coverage Report Analyzer

**Zero-dependency QuestaSim coverage report visualizer. Interactive dashboards, multi-run analysis, CI/CD ready. Single HTML file, offline-capable.**

![Version](https://img.shields.io/badge/version-4.1-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-production--ready-brightgreen)

## 🎯 What is CovViz?

CovViz transforms raw QuestaSim coverage reports into **interactive, intuitive dashboards** that reveal coverage gaps, track metrics across multiple test runs, and enable data-driven verification decisions.

Unlike traditional coverage viewers:
- **No Installation** — Single HTML file, works in any browser
- **No Dependencies** — Zero external requirements
- **No Server** — Completely offline-capable
- **No License** — MIT licensed, use freely
- **No Learning Curve** — Intuitive UI, instant insights

## ✨ Key Features

### 📊 Interactive Dashboards
- **KPI Cards** — Branch, Statement, Toggle, Expression, Functional coverage at a glance
- **Progress Bars** — Visual comparison of coverage metrics
- **Radar Chart** — Multi-dimensional coverage view
- **Donut Chart** — Top bins by hit count
- **Quadrant Plot** — Code vs. Functional coverage with projection
- **Instance Tabs** — Per-DUT breakdown with toggle signal details
- **Bin Details** — Comprehensive bin-level coverage data
- **Assertion Tracking** — Pass/fail counts per assertion

### 🔍 Smart Analysis
- **Automatic Issue Detection** — Flags zero-hit bins and partial toggle misses
- **Parse Log** — Shows exactly what was detected and what was skipped
- **Multi-File Merge** — Aggregate coverage from multiple test runs
- **Regression Trending** — Track coverage improvements over time
- **Export Ready** — CSV and JSON outputs for further analysis

### 🚀 Production Ready
- **CI/CD Integration** — 5 ready-to-use integration patterns
- **Portable** — Email the HTML file, commit to Git, embed in CI artifacts
- **Offline-First** — Works on air-gapped networks
- **No Setup** — Open in browser and start analyzing

## 🚀 Quick Start

### 1. Download
```bash
# Clone this repository or download CovViz-Enhanced-Fixed.html
git clone https://github.com/Samuel-Moussa/CovViz.git
cd CovViz
```

### 2. Open
```bash
# Simply open the HTML file in any modern browser
open CovViz-Enhanced-Fixed.html
# or
firefox CovViz-Enhanced-Fixed.html
```

### 3. Load Report
Choose one of three methods:
- **Drag & Drop** — Drag your `coverage.txt` onto the drop zone
- **File Browser** — Click "Load Report" and select a file
- **Paste Text** — Paste report text directly

### 4. Analyze
- View KPI cards for quick metrics
- Check alerts for issues
- Explore dashboards for detailed analysis
- Export results as CSV or JSON

## 📋 Supported Formats

### QuestaSim Text Report (Primary)
```bash
vcover report -details -cvg -code bst -output coverage.txt
```

Supported sections:
- Branch Coverage (with details)
- Statement Coverage (with details)
- Toggle Coverage (with signal-level breakdown)
- Covergroup Coverage (with coverpoints and bins)
- Assertion Results
- FSM Coverage (basic support)

### JSON Format (Secondary)
```bash
vcover report -format json -output coverage.json
```

## 🔧 CI/CD Integration

### Use Case 1: Post-Simulation Report
```yaml
# .gitlab-ci.yml
simulate:
  script:
    - vsim -c -do "run -all; quit" testbench
    - vcover report -details -cvg -code bst -output coverage.txt
  artifacts:
    paths:
      - coverage.txt
      - CovViz-Enhanced-Fixed.html
```

### Use Case 2: Coverage Gating
```bash
#!/bin/bash
FUNC_COV=$(grep "TOTAL COVERGROUP COVERAGE:" coverage.txt | grep -oP '\d+\.\d+')
if (( $(echo "$FUNC_COV < 85.0" | bc -l) )); then
  echo "ERROR: Coverage $FUNC_COV% below threshold"
  exit 1
fi
```

### Use Case 3: Multi-Run Analysis
```bash
# Run multiple tests and merge reports
for test in test_add test_sub test_and test_or; do
  vsim -c -do "run -all; quit" $test
  vcover report -details -cvg -code bst -output coverage_${test}.txt
done

# Upload all coverage_*.txt files to CovViz for merged analysis
```

See [CI/CD Integration Guide](docs/CI-CD-Integration.md) for 5 complete examples.

## 📚 Documentation

- **[User Guide](docs/User-Guide.md)** — Step-by-step workflow and feature explanations
- **[Technical Architecture](docs/Technical-Architecture.md)** — Parser design, data structures, rendering pipeline
- **[Limitations & Workarounds](docs/Limitations.md)** — Honest assessment with solutions
- **[Troubleshooting](docs/Troubleshooting.md)** — Common issues and fixes
- **[Full Documentation](docs/CovViz-Documentation.md)** — Complete 50+ page reference

## 🛠️ System Requirements

| Component | Requirement |
|-----------|-------------|
| **Browser** | Chrome 90+, Firefox 88+, Safari 14+, Edge 90+ |
| **OS** | Windows, macOS, Linux |
| **RAM** | 100 MB minimum |
| **Disk** | 2 MB (single HTML file) |
| **Network** | None required (offline-capable) |

## 📊 Example Output

### KPI Cards
```
Branch: 100.0%  |  Statement: 100.0%  |  Toggle: 95.0%  |  Functional: 87.5%
```

### Alerts
```
✗ 1 zero-hit bin: ALU_cg.cp_carry.has_carry
⚠ 2 toggle misses: /tb_ALU/DUT.carry_out, /tb_ALU/DUT.rst_n
```

### Parse Log
```
✓ Parsing coverage.txt...
✓ Total Covergroup Coverage: 87.50%
✓ Instance: /tb_ALU/DUT
✓ Coverpoint: cp_op (100.00%)
⚠ Zero bin: ALU_cg.cp_carry.has_carry
✓ Done. Instances:2 Covergroups:1 Assertions:1
```

## 🎓 How It Works

CovViz uses a **three-layer architecture**:

### Layer 1: Parser
- Two-pass algorithm for accuracy
- Context-aware state machine
- Hierarchical data accumulation
- Handles indentation-based structure

### Layer 2: Aggregation
- Multi-file merge for regression analysis
- Metric averaging across runs
- Bin deduplication
- Assertion aggregation

### Layer 3: Visualization
- Chart.js for radar and donut charts
- Canvas 2D for quadrant plot
- HTML/CSS for responsive UI
- Interactive tabs and tables

See [Technical Architecture](docs/Technical-Architecture.md) for detailed explanation.

## ⚠️ Known Limitations

| Limitation | Impact | Workaround |
|-----------|--------|-----------|
| Text-only parsing | Misses cross-run merged data | Use JSON export |
| Single hierarchy level | Deep hierarchies collapse | Flatten or use JSON |
| Bus signal handling | Signals like `[3:0]` may truncate | Use underscores |
| FSM coverage | Limited pattern support | Use JSON export |
| Per-test breakdown | Can't show which test hit bin | Run separate simulations |

See [Limitations & Workarounds](docs/Limitations.md) for complete list and solutions.

## 🚀 Roadmap

### Phase 1: Robustness (Q2 2026)
- [ ] VCS (Xcelium, imc) report support
- [ ] Better bus signal and escaped identifier handling
- [ ] Improved FSM coverage parsing
- [ ] Cross-coverage bin detection

### Phase 2: Backend Integration (Q3 2026)
- [ ] Optional Python Flask server for live refresh
- [ ] File-watch mechanism
- [ ] WebSocket support for real-time dashboards
- [ ] Database backend for regression trending

### Phase 3: EDA Plugin (Q4 2026)
- [ ] QuestaSim Tcl plugin
- [ ] VSCode extension
- [ ] Xcelium integration
- [ ] VCS integration

### Phase 4: Advanced Analytics (Q1 2027)
- [ ] Regression trending with statistics
- [ ] Coverage prediction based on test history
- [ ] Automated test generation recommendations
- [ ] Machine learning-based anomaly detection

## 🐛 Troubleshooting

### "No data found" or blank dashboard
1. Check file encoding: `file coverage.txt` (should be ASCII or UTF-8)
2. Verify report format: Look for "Branch Coverage:", "Covergroup Coverage:"
3. Try JSON export: `vcover report -format json -output coverage.json`

### Parse error in console
1. Check browser console (F12 → Console tab)
2. Reduce report size: Filter to specific instances
3. Try JSON format: More robust than text parsing

### Incorrect bin counts
1. Check bin status format in coverage.txt
2. Rename bins: Avoid special characters, use underscores
3. Export to JSON: Preserves data more accurately

See [Troubleshooting Guide](docs/Troubleshooting.md) for more issues and solutions.

## 💡 Use Cases

✅ **Verification Teams** — Daily coverage analysis  
✅ **CI/CD Pipelines** — Automated gating and trending  
✅ **Management** — High-level metrics and dashboards  
✅ **Customers** — Portable, shareable reports  
✅ **Consultants** — No-install, no-license tool  
✅ **Enterprises** — Air-gapped network support  

## 📦 Project Structure

```
CovViz/
├── CovViz-Enhanced-Fixed.html      # Main tool (single file)
├── README.md                        # This file
├── LICENSE                          # MIT License
├── CHANGELOG.md                     # Version history
├── CONTRIBUTING.md                  # Contribution guidelines
├── docs/
│   ├── CovViz-Documentation.md      # Full 50+ page reference
│   ├── User-Guide.md                # Step-by-step workflow
│   ├── Technical-Architecture.md    # Parser and design
│   ├── Limitations.md               # Known issues
│   ├── Troubleshooting.md           # Common problems
│   └── CI-CD-Integration.md         # 5 integration patterns
├── examples/
│   ├── coverage_simple.txt          # Simple example report
│   ├── coverage_complex.txt         # Complex example report
│   └── coverage.json                # JSON format example
└── .github/
    ├── workflows/
    │   └── tests.yml                # CI/CD workflow
    └── ISSUE_TEMPLATE/
        ├── bug_report.md            # Bug report template
        └── feature_request.md       # Feature request template
```

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for:
- Code style guidelines
- Testing requirements
- Pull request process
- Commit message format

## 📞 Support

- **Documentation:** See [docs/](docs/) folder
- **Issues:** [GitHub Issues](https://github.com/Samuel-Moussa/CovViz/issues)
- **Email:** support@icsam.dev
- **Website:** https://icsam.dev

## 📄 License

This project is licensed under the MIT License — see [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built for QuestaSim verification workflows
- Inspired by the need for portable, zero-dependency coverage analysis
- Thanks to the verification community for feedback and use cases

## 🌟 Show Your Support

If CovViz helps your verification workflow, please:
- ⭐ Star this repository
- 📢 Share with your team
- 💬 Provide feedback and suggestions
- 🐛 Report bugs and issues
- 🤝 Contribute improvements

---

**CovViz v4.1** — Making coverage analysis simple, portable, and powerful.

**Developed by:** Samuel Moussa (IC_SAM)  
**Last Updated:** March 2026  
**Status:** Production Ready
