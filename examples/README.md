# CovViz Examples

This folder contains sample coverage reports for testing and demonstration purposes.

## Files

### `coverage_simple.txt`
A simple QuestaSim coverage report from an ALU design with:
- **Branch Coverage:** 100% (8/8 branches)
- **Statement Coverage:** 100% (8/8 statements)
- **Toggle Coverage:** 95% (57/60 toggles)
- **Functional Coverage:** 87.5% (12/13 bins)
- **Assertions:** 1 (passing)

**Features Demonstrated:**
- Branch details with IF and CASE statements
- Statement coverage with source code
- Toggle signal breakdown (rise/fall transitions)
- Covergroup with 4 coverpoints and 13 bins
- One zero-hit bin (`has_carry`) for testing alert detection
- Partial toggle miss (`rst_n` at 50%) for testing warnings

**How to Use:**
1. Open CovViz in your browser
2. Drag & drop `coverage_simple.txt` onto the drop zone
3. View the dashboard and verify all sections render correctly
4. Check that alerts flag the zero bin and toggle miss

## Testing Checklist

When testing CovViz with these examples, verify:

- [ ] Dashboard renders without errors
- [ ] KPI cards show correct values
- [ ] Parse log shows successful extraction
- [ ] Alerts flag zero-hit bins
- [ ] Alerts flag partial toggle misses
- [ ] Instance tabs show correct data
- [ ] Covergroup tabs show all bins
- [ ] Assertion results display correctly
- [ ] Charts render properly (radar, donut, quadrant)
- [ ] Export to CSV works
- [ ] Export to JSON works

## Creating Your Own Examples

To add more example reports:

1. Generate a coverage report from your simulation:
   ```bash
   vcover report -details -cvg -code bst -output coverage_mydesign.txt
   ```

2. Copy to this folder:
   ```bash
   cp coverage_mydesign.txt examples/
   ```

3. Update this README with a description

4. Commit and push:
   ```bash
   git add examples/
   git commit -m "Add example: mydesign coverage report"
   git push
   ```

## Report Format Requirements

For CovViz to parse reports correctly, ensure your QuestaSim report includes:

```bash
vcover report -details -cvg -code bst -output coverage.txt
```

**Required Sections:**
- Branch Coverage (with details)
- Statement Coverage (with details)
- Toggle Coverage (with details)
- Covergroup Coverage (with coverpoints and bins)

**Optional Sections:**
- Assertion Results
- FSM Coverage

## Troubleshooting

If CovViz doesn't parse your example report:

1. Check file encoding: `file coverage_*.txt` (should be ASCII or UTF-8)
2. Verify report format: Look for "Branch Coverage:", "Covergroup Coverage:"
3. Check for special characters in signal/bin names
4. Try exporting to JSON format instead

For more help, see [Troubleshooting Guide](../docs/Troubleshooting.md).
