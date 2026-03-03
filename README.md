# OSCAL Catalogs

Machine-readable [OSCAL](https://pages.nist.gov/OSCAL/) (Open Security Controls Assessment Language) representations of security and compliance frameworks.

## Catalogs

### UK Cyber Essentials

The [Cyber Essentials](https://www.ncsc.gov.uk/cyberessentials/overview) scheme is the UK government's minimum standard for cyber security, managed by [IASME](https://iasme.co.uk/cyber-essentials/) on behalf of the [NCSC](https://www.ncsc.gov.uk/). It covers five technical controls:

1. **Firewalls** (A4)
2. **Secure Configuration** (A5)
3. **Security Update Management** (A6)
4. **User Access Control** (A7)
5. **Malware Protection** (A8)

Plus organisational sections for Organisation (A1), Scope (A2), and Insurance (A3).

| Version | Codename | Effective | OSCAL Catalog | Source |
|---------|----------|-----------|---------------|--------|
| 16 | Danzell | April 2026 | [`catalogs/cyber-essentials/danzell-v16/catalog.json`](catalogs/cyber-essentials/danzell-v16/catalog.json) | [Danzell-Willow Comparison](catalogs/cyber-essentials/danzell-v16/source.xlsx) |

#### Danzell v16 Highlights

- **106 controls** with **58 CE requirements** extracted
- **4 auto-fail controls** flagged (A6.4, A6.5 for patching; A7.16, A7.17 for MFA)
- Aligned with [Requirements for IT Infrastructure v3.3](https://www.ncsc.gov.uk/files/cyber-essentials-requirements-for-it-infrastructure-v3-3.pdf)
- Validated via [oscal-pydantic](https://github.com/RS-Credentive/oscal-pydantic/tree/oscal-pydantic-v2) round-trip

## OSCAL Structure

Each catalog follows the [OSCAL 1.1.2 Catalog Model](https://pages.nist.gov/OSCAL/reference/1.1.2/catalog/json-outline/):

```
catalog
├── metadata          # Title, version, parties (IASME, NCSC), roles
├── groups[]          # Sections (A1-A8)
│   ├── parts[]       # Section overview prose
│   ├── controls[]    # Individual questions/requirements
│   │   ├── props[]   # label, sort-id, response-type, auto-fail
│   │   └── parts[]   # statement, guidance (with nested CE requirements)
│   └── groups[]      # Sub-sections (e.g. Admin Accounts, Password Auth)
└── back-matter       # References to NCSC/IASME source documents
```

### Custom Namespace

CE-specific properties use the namespace `https://iasme.co.uk/ns/cyber-essentials`:

| Property | Description |
|----------|-------------|
| `response-type` | Expected answer format (`Yes/No`, `Notes`, `Multiple choice`, etc.) |
| `auto-fail` | `true` if a non-compliant answer results in automatic assessment failure |

### Control Classes

| Class | Sections | Description |
|-------|----------|-------------|
| `organisational` | A1, A2, A3 | Organisation details, scope, and insurance |
| `technical-control` | A4, A5, A6, A7, A8 | The five Cyber Essentials technical controls |

## Regenerating

The catalog is generated from the source spreadsheet using the `tools/generate_oscal.py` script:

```bash
uv run --with 'oscal-pydantic-v2,openpyxl' python3 tools/generate_oscal.py
```

## References

- [NCSC Cyber Essentials Overview](https://www.ncsc.gov.uk/cyberessentials/overview)
- [NCSC Requirements for IT Infrastructure v3.3](https://www.ncsc.gov.uk/files/cyber-essentials-requirements-for-it-infrastructure-v3-3.pdf)
- [IASME Cyber Essentials](https://iasme.co.uk/cyber-essentials/)
- [IASME Question Set Preview](https://iasme.co.uk/cyber-essentials/preview-the-self-assessment-questions-for-cyber-essentials/)
- [NIST OSCAL](https://pages.nist.gov/OSCAL/)
- [oscal-pydantic](https://github.com/RS-Credentive/oscal-pydantic/tree/oscal-pydantic-v2)
- [OSCAL.io](https://oscal.io/)

## License

Apache 2.0 — see [LICENSE](LICENSE).
