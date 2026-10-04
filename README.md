# carbon-trace-sc

Product-level carbon footprint calculator across supply chain stages, from raw materials and manufacturing to transport and packaging. Built on open emission-factor datasets with transparent, auditable methodology.

> **Status:** early development. The calculation engine, emission-factor library and reports are being built in the open. See the [Roadmap](#roadmap).

---

## Why this exists

Companies, students and small businesses increasingly need to answer a simple question: *How much carbon does one unit of this product cause, from start to finish?* Answering it is hard:

- A product's footprint comes from **many stages**: raw materials, manufacturing, packaging, transport, storage and sometimes use and disposal.
- **Emission factors** (the carbon cost per kg, per kWh or per tonne-km) are scattered across many sources, in different units and formats.
- Most commercial tools are **closed,** so users cannot see how a number was produced or check it.
- A single figure with no explanation is hard to trust and hard to improve.

`carbon-trace-sc` aims to make this open and auditable:

- **Stage-by-stage calculation** so you can see where the emissions come from.
- **Open emission-factor library** with source, year and unit stored for every factor.
- **Transparent method:** every number in a result links back to its input and its factor.
- **Uncertainty ranges** instead of false precision.
- **Reproducible results** from a plain input file.

---

## What it calculates

| Stage | Example inputs |
|---|---|
| **Raw materials** | Kilograms of each material (cotton, steel, plastic, wheat) |
| **Manufacturing** | Electricity (kWh), fuel, process heat, factory location |
| **Packaging** | Kilograms of cardboard, plastic, glass |
| **Transport** | Distance, weight and transport mode (truck, rail, ship, air) |
| **Storage / retail** | Cold storage days, warehouse energy |
| **Use / end of life** (optional) | Energy during use, disposal route |

Output is the footprint in **kg CO2e** (carbon dioxide equivalent) per product unit, with a breakdown by stage and a ranked list of the biggest contributors ("hotspots").

---

## Data

The project is designed around open and freely available emission-factor sources, such as:

- US EPA emission factors for greenhouse gas inventories
- UK government GHG conversion factors (DEFRA / DESNZ)
- Our World in Data and published studies for food and agriculture footprints
- Open LCA datasets available for free use
- National grid emission factors, including India's grid factor published by the Central Electricity Authority

Some well-known life-cycle databases are paid and are **not** used. Factor tables are **not** redistributed unless the license allows it. Each factor stored by the tool carries its source, year, region and license.

---

## Planned approach

### Method

- Follow the ideas of the **GHG Protocol** (product standard and Scope 3 guidance) and **ISO 14067** for product carbon footprints
- Calculate each stage as *activity amount x emission factor*
- Sum stages into a total, and keep the breakdown
- Choose regional factors where available (for example, the electricity factor of the country where a factory is located)

### Transparency and uncertainty

- Every result lists inputs, factors used, and their sources
- Each factor can carry an uncertainty range, giving a result range instead of one number
- Data-quality flags show which inputs are estimates and which are measured

### Metrics (for checking the tool itself)

- **Reproduction accuracy:** results compared with published product footprint studies that give full inputs
- **Unit-handling tests:** automatic tests that catch unit and conversion mistakes
- **Sensitivity analysis:** which input changes the result the most

---

## How it works

```
product description (YAML / CSV)
 materials, energy, packaging, transport legs
                    |
                    v
   match each input to an emission factor (with source + year)
                    |
                    v
      stage-by-stage CO2e calculation + uncertainty
                    |
                    v
   total footprint + hotspot ranking + audit trail
                    |
                    v
        report (table, chart, JSON)
```

---

## Planned repository structure

```
carbon-trace-sc/
├── data/
│   ├── factors/          # emission-factor tables with source and license
│   └── examples/         # sample product files
├── carbon_trace/
│   ├── factors.py        # factor loading and matching
│   ├── units.py          # unit conversion
│   ├── stages/           # materials, manufacturing, packaging, transport
│   ├── uncertainty.py    # ranges and sensitivity
│   ├── report.py         # tables, charts, audit trail
│   └── evaluate.py       # reproduction checks
├── benchmarks/           # comparison with published studies
├── notebooks/            # examples and plots
├── tests/
├── docs/
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

---

## Quickstart (planned)

```bash
git clone https://github.com/amoltathesoftwareengineer/carbon-trace-sc.git
cd carbon-trace-sc
pip install -r requirements.txt

# calculate a footprint from a product file
python -m carbon_trace.run --input data/examples/tshirt.yaml

# see the hotspots and the audit trail
python -m carbon_trace.report --input data/examples/tshirt.yaml --hotspots --audit
```

An input file will look like this (illustrative):

```yaml
product: cotton t-shirt
unit: 1 piece
materials:
  - name: cotton
    amount_kg: 0.25
manufacturing:
  electricity_kwh: 1.2
  region: IN
transport:
  - mode: ship
    distance_km: 9000
    weight_kg: 0.3
```

---

## Results

Results will be added once the first benchmark against published studies is complete.

| Check | Case | Result |
|---|---|---|
| Reproduction of a published product footprint | _coming soon_ | - |
| Unit conversion tests | _coming soon_ | - |
| Sensitivity analysis | _coming soon_ | - |

---

## Roadmap

- [ ] Build the emission-factor library with source, year, region and license fields
- [ ] Add unit handling and conversion tests
- [ ] Implement the stage calculators (materials, manufacturing, packaging, transport)
- [ ] Add audit trail and hotspot ranking
- [ ] Add uncertainty ranges and sensitivity analysis
- [ ] Reproduce published product footprint studies as benchmarks
- [ ] Publish the first reproducible examples
- [ ] Add a simple web interface for small businesses
- [ ] Add India-specific factors and supply chains

---

## Limitations and responsible use

- Results are **estimates.** Average emission factors can differ a lot from a specific supplier's real figures.
- Results are only as good as the inputs. Missing stages (such as use phase or disposal) will understate the footprint.
- Different methods and system boundaries give different totals. Compare results only when the boundaries match.
- This project is for research and education. It is **not** a certified carbon accounting or verification tool, and should not be used on its own for regulatory reports or marketing claims about a product's footprint.

---

## Contributing

Contributions are welcome, especially:

- New open emission-factor sources (with license information)
- Regional factors, including for India
- Example product files and published studies to reproduce
- Bug reports and unit-conversion fixes

Please open an issue to discuss before sending a large pull request.

---

## License

MIT License. See [LICENSE](LICENSE).

---

## Author

**Amol Tathe**
[LinkedIn](https://linkedin.com/in/amoltathesoftwareengineer) · [GitHub](https://github.com/amoltathesoftwareengineer)
