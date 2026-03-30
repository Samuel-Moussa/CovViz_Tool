# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1] - 2026-03-30

### Fixed
- **Critical Parser Bug**: Fixed regex false-positive that incorrectly parsed bin names containing coverage keywords (e.g., `has_carry` was misidentified as coverpoint metric)
- **Bin Status Detection**: Improved parsing of bin status (Covered/ZERO) with stricter regex patterns
- **Coverpoint Extraction**: Fixed indentation-based coverpoint and bin extraction for proper hierarchy parsing
- **Toggle Signal Parsing**: Enhanced signal name handling for bus signals and special characters
- **Assertion Results**: Improved multi-line assertion entry parsing

### Added
- **Two-Pass Parser**: Implemented two-pass parsing algorithm for improved accuracy and reduced false positives
- **Parse Log Panel**: Added detailed parse log showing exactly what was detected with color-coded status (✓ ok, ✗ error, ⚠ warning, ℹ info)
- **Multi-File Merge**: Support for uploading and merging multiple coverage reports for regression analysis
- **CSV Export**: Export coverage data to CSV format for spreadsheet analysis
- **JSON Export**: Export complete data structure as JSON for external tool integration
- **Enhanced Quadrant Plot**: Added projected coverage point calculation for zero-bin scenarios
- **IC_SAM Branding**: Integrated user logo in header with glowing cyan border
- **Smart Alerts**: Automated detection of zero-hit bins, partial toggle misses, and FSM coverage issues
- **FSM Coverage Support**: Basic FSM state and arc coverage parsing

### Improved
- **UI/UX**: Better visual hierarchy, responsive design, intuitive navigation
- **Documentation**: Comprehensive 50+ page technical documentation with CI/CD integration examples
- **Error Handling**: More informative error messages and troubleshooting guidance
- **Performance**: Optimized rendering pipeline for faster dashboard generation

### Changed
- **Version Badge**: Updated to v4.1 Fixed
- **Parser Strategy**: Changed from single-pass to two-pass algorithm for accuracy

## [4.0] - 2026-03-15

### Added
- **Interactive Dashboards**: KPI cards, progress bars, radar chart, donut chart, quadrant plot
- **Instance Tabs**: Per-DUT coverage breakdown with toggle signal details
- **Bin Details**: Comprehensive bin-level coverage data with status badges
- **Assertion Tracking**: Pass/fail counts per assertion
- **Smart Alerts**: Automatic detection of coverage issues
- **Chart.js Integration**: Professional data visualization
- **Responsive Design**: Works on desktop, tablet, and mobile browsers
- **Offline-Capable**: Complete offline functionality, no internet required
- **Zero Dependencies**: Single HTML file, no external dependencies

### Features
- Drag & drop file upload
- Paste text directly into analyzer
- Real-time dashboard rendering
- Export to CSV and JSON
- Color-coded metrics (Green ≥95%, Orange ≥75%, Red <75%)
- Parse log with detailed extraction information

## [3.0] - 2026-02-28

### Added
- **Basic Parser**: Initial QuestaSim text report parser
- **Simple Dashboard**: Basic coverage metrics display
- **Instance Support**: Per-instance coverage tracking
- **Covergroup Support**: Functional coverage visualization

### Known Issues
- Regex false-positives with bin names containing coverage keywords
- Limited error handling and reporting
- No multi-file merge support
- No export functionality

## [2.0] - 2026-02-15

### Added
- **Initial Release**: Prototype version with basic parsing

## [1.0] - 2026-02-01

### Added
- **Project Inception**: Initial concept and architecture design

---

## Upgrade Guide

### From v3.x to v4.1
1. Download new `CovViz-Enhanced-Fixed.html`
2. Replace old HTML file
3. No migration needed — all reports work with new version
4. Enjoy improved parser accuracy and new features

### Breaking Changes
- None — v4.1 is fully backward compatible

---

## Known Issues

### Current Version (v4.1)
- Bus signals like `[3:0]` may be truncated in display
- Deep hierarchies (>2 levels) collapse to single level
- FSM coverage parsing is basic (full support in Phase 1)
- Cross-coverage bins treated as regular bins

### Workarounds
See [docs/Limitations.md](docs/Limitations.md) for detailed workarounds.

---

## Planned Releases

### Phase 1: Robustness (Q2 2026)
- VCS (Xcelium, imc) report support
- Better bus signal handling
- Improved FSM coverage parsing
- Cross-coverage bin detection

### Phase 2: Backend Integration (Q3 2026)
- Optional Python Flask server
- File-watch mechanism
- WebSocket support
- Database backend for trending

### Phase 3: EDA Plugin (Q4 2026)
- QuestaSim Tcl plugin
- VSCode extension
- Xcelium integration
- VCS integration

### Phase 4: Advanced Analytics (Q1 2027)
- Regression trending with statistics
- Coverage prediction
- Automated test generation recommendations
- Machine learning-based anomaly detection

---

## Support

For issues, feature requests, or questions:
- Create an issue on [GitHub Issues](https://github.com/Samuel-Moussa/CovViz/issues)
- Email: support@icsam.dev
- Website: https://icsam.dev

---

**CovViz** — Making coverage analysis simple, portable, and powerful.
