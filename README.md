# npm-guardian 🛡️

Multi-layer security scanner for npm packages to detect supply chain attacks.

## Features

- **Typosquatting Detection**: Identifies packages with names similar to popular packages
- **Metadata Analysis**: Checks package age, popularity, and maintainer info
- **Code Analysis**: Scans for malicious patterns like eval(), obfuscation, suspicious API usage
- **Install Script Detection**: Flags dangerous install/postinstall scripts
- **Dependency Mapping**: Analyzes full dependency tree

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/npm-guardian.git
cd npm-guardian

# Install dependencies
npm install

# Make it executable globally (optional)
npm link
```

## Usage

### Basic Scan

```bash
# Scan current directory
npm-guardian

# Scan specific project
npm-guardian /path/to/project
```

### Options

```bash
# Quiet mode (minimal output)
npm-guardian --quiet

# Verbose mode (detailed logging)
npm-guardian --verbose

# Skip metadata checks (faster scan)
npm-guardian --skip-metadata

# Skip code analysis
npm-guardian --skip-code

# JSON output
npm-guardian --json

# Save results to file
npm-guardian --output results.json
```

### Examples

```bash
# Quick scan
npm-guardian --skip-metadata --quiet

# Thorough scan with detailed output
npm-guardian --verbose

# Generate JSON report
npm-guardian --json --output security-report.json

# CI/CD integration
npm-guardian --quiet && echo "All clear!" || echo "Threats detected!"
```

## How It Works

### Layer 1: Pre-Install Scanning
- Compares package names against 50+ popular packages
- Uses Levenshtein distance algorithm
- Checks package age and download statistics

### Layer 2: Code Analysis
- Parses JavaScript using Abstract Syntax Tree (AST)
- Detects suspicious patterns:
  - `eval()` usage
  - `child_process` execution
  - Obfuscated code
  - File system access
  - Network requests

### Layer 3: Behavioral Monitoring
- Analyzes install scripts (preinstall, postinstall)
- Flags scripts that:
  - Download files
  - Execute commands
  - Modify system files

### Layer 4: Dependency Graph
- Maps complete dependency tree
- Identifies deep dependencies
- Detects dependency confusion risks

## Exit Codes

- `0`: No threats detected
- `1`: Medium severity threats found
- `2`: High severity threats found
- `3`: Scan failed (error occurred)

## Example Output

```
🛡️  npm-guardian - Package Security Scanner
══════════════════════════════════════════════════

📁 Scanning: /home/user/my-project

🔍 Building dependency tree...
📦 Found 42 direct dependencies
🔎 Analyzing packages...

Progress: 42/42 packages

⚠️  Found 2 potentially risky packages:

📦 crossenv@1.0.0
┌─────────────────────┬──────────┬─────────────────────────────────┐
│ Type                │ Severity │ Details                         │
├─────────────────────┼──────────┼─────────────────────────────────┤
│ TYPOSQUATTING       │ HIGH     │ Similar to 'cross-env' (dist: 1)│
│ NEW_PACKAGE         │ MEDIUM   │ Created only 5 days ago         │
│ LOW_POPULARITY      │ LOW      │ Only 23 downloads last week     │
└─────────────────────┴──────────┴─────────────────────────────────┘

📦 suspicious-lib@2.1.0
┌─────────────────────┬──────────┬─────────────────────────────────┐
│ Type                │ Severity │ Details                         │
├─────────────────────┼──────────┼─────────────────────────────────┤
│ INSTALL_SCRIPT      │ HIGH     │ Has postinstall script          │
│ EVAL_USAGE          │ HIGH     │ Uses eval() 3 time(s)           │
│ NETWORK_ACCESS      │ MEDIUM   │ Makes network requests          │
└─────────────────────┴──────────┴─────────────────────────────────┘

📊 Summary:
──────────────────────────────────────────────────
   Total packages flagged: 2
   HIGH severity risks: 4
   MEDIUM severity risks: 2
   LOW severity risks: 1

💡 Recommendations:
   1. Review flagged packages manually
   2. Check package source code on GitHub/npm
   3. Consider alternative packages for HIGH risks
   4. Run npm audit for known vulnerabilities
   5. Use package lock files to prevent updates

⚠️  WARNING: High severity risks found!
   Immediate action recommended.
```

## Project Structure

```
npm-guardian/
├── package.json       # Project dependencies and metadata
├── index.js          # CLI interface
├── scanner.js        # Core scanning logic
├── detector.js       # Detection modules
├── utils.js          # Helper functions
└── README.md         # Documentation
```

## Technical Details

### Dependencies

- **@npmcli/arborist**: Dependency tree analysis
- **acorn**: JavaScript AST parser
- **axios**: HTTP client for npm registry API
- **chalk**: Terminal styling
- **cli-table3**: Formatted table output
- **commander**: CLI argument parsing

### Detection Techniques

#### Typosquatting Detection
Uses Levenshtein distance algorithm to compare package names against a curated list of 50+ popular packages. Flags packages with edit distance of 1-2.

#### Code Pattern Analysis
- Regular expression matching for suspicious patterns
- AST analysis to detect dangerous API usage
- Obfuscation detection using multiple heuristics

#### Metadata Analysis
- Package age verification
- Download statistics checking
- Maintainer validation

#### Install Script Analysis
- Detects lifecycle scripts (preinstall, postinstall, etc.)
- Analyzes script content for dangerous commands
- Severity assessment based on command type

## Limitations

- Only scans direct dependencies (not transitive dependencies)
- Requires network access for metadata checks
- May produce false positives for legitimate packages
- Cannot detect all obfuscation techniques
- Performance depends on network speed and package count

## Future Enhancements

- [ ] Transitive dependency scanning
- [ ] Machine learning-based detection
- [ ] Integration with vulnerability databases
- [ ] Real-time monitoring mode
- [ ] GitHub repository analysis
- [ ] License compliance checking
- [ ] Custom detection rules
- [ ] CI/CD plugin support

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT License - see LICENSE file for details

## Author

Nitish Sahni (0263900)

## Acknowledgments

- Inspired by npm audit and Socket.dev
- Built for cybersecurity research and education
- Uses open-source tools and public APIs

## Disclaimer

This tool is for educational and research purposes. Always verify findings manually before taking action. The tool may produce false positives and should not be the sole basis for security decisions.

## Support

For issues, questions, or contributions, please visit the GitHub repository or contact the author.

---

**Stay safe and secure! 🛡️**
# npm-gaurdian
