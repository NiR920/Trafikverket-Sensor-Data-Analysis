# Results

This directory is reserved for selected, reproducible outputs supporting the thesis.

```text
results/
├── figures/   # publication-quality analytical figures
└── tables/    # selected analytical tables
```

## Figure policy

Figures should be:

- generated from documented analysis code where possible;
- labeled with meaningful, stable filenames;
- accompanied by enough context to identify the underlying measure and scope;
- consistent with the definitions in `docs/analysis-methods.md`;
- free from accidental debug output or screenshots unless the purpose is to document an interface.

The submitted thesis PDF remains the authoritative record of the figures that were actually used in the final academic document. The PDF contains the reported analytical plots as well as MCP/Claude Desktop interface screenshots. The latter should remain clearly distinguished from analytical evidence when figures are migrated into this directory.

## Tables

Tables should follow the same principle: preserve the reported values and definitions from the thesis, and include source/provenance notes when a table is regenerated from the database.

Large raw exports and uncontrolled intermediate files should not be stored here.
