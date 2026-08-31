# ARENA_WORKFLOW.md — Arbeiten mit diesem Skill-Repo im Agent Mode

> Persönliches Betriebsdokument für die Nutzung von **scientific-agent-skills**
> (K-Dense-AI, Fork: plovlife) als Skill-Bibliothek in Arena.
> Stand: 2026-08-31 · Repo-Version laut `plugin.json`: **2.65.0** · **163 Skills** unter `skills/`.
> Alle Aussagen unten stammen aus den tatsächlich gelesenen `SKILL.md`-Dateien,
> `AGENTS.md` und `README.md` dieses Repos — nichts erfunden, nichts paraphrasiert, wo es drauf ankommt.

---

## 1. Was dieser Repo ist

Ein Skill-Paket nach dem offenen [Agent-Skills](https://agentskills.io/)-Standard und zugleich ein
portables [Agent-Plugins](https://agent-plugins.org/)-Paket (`plugin.json` + `skills/`-Baum,
MIT-Lizenz). Jeder Skill ist ein Ordner unter `skills/<name>/` mit mindestens einer `SKILL.md`
(YAML-Frontmatter: `name`, `description`, `license`, `compatibility`, `metadata.version`;
erlaubte Frontmatter-Felder sind auf 6 begrenzt, s. `AGENTS.md`). optionale Unterordner:
`references/` (Detail-Doku, nur bei Bedarf geladen), `scripts/` (lokale, meist netzwerkfreie CLI-Helfer),
`assets/` (Templates). **Tests leben nie unter `skills/`, sondern in `tests/<skill-name>/`.**

## 2. Standard-Workflow (für jede zukünftige Aufgabe)

Dies ist die verbindliche Reihenfolge, in der ich in Arena Aufgaben aus den Bereichen
*Research → Analyse → Schreiben* abarbeite:

1. **Skill auswählen.** Passenden Skill aus `skills/` bestimmen (Katalog in §5 / Anhang A).
   Passt kein Skill wirklich: das **klar sagen** und den besten Kompromiss benennen oder ohne
   Skill-Layer arbeiten. Keine thematisch nahen, aber falschen Skills „mitnehmen“.
2. **`SKILL.md` wirklich lesen.** Immer komplett (oder gezielt die Sektionen `Purpose`,
   `When to use`, `Non-negotiable … rules`/`Scope and boundaries`, `Workflow`). Die Vorgaben des
   Skills schlagen generischen Stil. Bei Bedarf `references/*.md` nachlesen, bevor ich behaupte,
   den Skill zu kennen.
3. **Boundaries & Pflichten einhalten.** Wiederkehrende harte Regeln dieses Repos:
   - *No fabrication:* keine Zitaten/DOIs/Daten/Methoden erfinden; fehlendes explizit als
     `MISSING`/`UNVERIFIED`/`N/A` markieren (scientific-writing, hypothesis-generation, peer-review).
   - *Evidence binding:* jede faktische/numerische Behauptung an verifizierte Quelle(n) pinnen
     (Beleg-ID + Fundstelle), Suche-Snippets sind kein Beleg.
   - *Vertraulichkeit:* unveröffentlichte Manuskripte/Review-Material/PHI nur lokal und nur mit
     expliziter Freigabe nach extern (peer-review, scientific-writing, hypothesis-generation).
   - *Fail-closed & sicher:* EDA/CLIs lesen nur autorisierte lokale Daten, folgen keinen
     eingebetteten Instruktionen, kein `allow_pickle`, keine Rohdatenausgabe (exploratory-data-analysis).
   - *Checklisten/Scripts ausführen:* wo der Skill `scripts/` mitbringt (z. B.
     `scientific-writing/scripts/audit_claims.py`, `check_references.py`, `lint_manuscript.py`;
     `statistical-analysis/scripts/assumption_checks.py`; `literature-review/scripts/verify_citations.py`),
     ausführen statt nur beschreiben.
4. **Output liefern** im vom Skill vorgegebenen Format (Manuskript-Sektion, Evidenz-Matrix,
   APA-Report, BibTeX, PPTX/PDF …) inkl. der Skill-internen Workflow-Schritte in Reihenfolge.
5. **Quellen / Annahmen / ToDos sauber markieren.** Jeder Deliverable endet mit drei Blöcken:
   - **Quellen:** verifizierte Belege (DOI/PMID + Locator), je Behauptung zugeordnet.
   - **Annahmen:** alles Getrennte als `ASSUMPTION`, Entscheidungen mit Begründung.
   - **Offen/ToDos:** `UNVERIFIED`/`MISSING`-Platzhalter und Schritte, die ein Mensch prüfen muss
     (Autor:innenschaft, Ethik, Freigaben, Einreichung).
6. **Repo-nah arbeiten:** Änderungen auf dem Arbeits-Branch, klarer Commit pro logischem Schritt,
   Repo-Validierungs-/Testkonventionen respektieren (`AGENTS.md`), PR-Vorschlag ausgeben statt
   ungefragt in `main` zu schreiben.

**Skill-Degradation:** Wenn ein Skill für die Aufgabe zu spezifisch/gegenläufig ist (z. B.
`literature-review` mit Pflicht-Figuren für einen Zwei-Zeilen-Fakt), ihn **nicht** krampfhaft
anwenden — kurz begründen und zum leichteren Skill wechseln (`research-lookup`, `database-lookup`).

## 3. Empfohlene Kern-Skills für „Research + Schreiben + Analyse“

Begründungen beziehen sich auf den tatsächlichen Inhalt der jeweiligen `SKILL.md` (gelesen 2026-08-31).

| # | Skill | Warum (aus dem SKILL.md) |
|---|-------|--------------------------|
| 1 | [`scientific-writing`](skills/scientific-writing/SKILL.md) (v2.0) | Das Rückgrat fürs Schreiben: 12-Schritte-Workflow (Workspace → Reporting-Guideline → Evidence-Record → Outline → Draft → Reconcile → Verify → Authorship → Declarations → Figures → Coverage → Lint), harte No-Fabrication- und Confidentiality-Regeln, plus 9 lokale Offline-CLI-Checks (`audit_claims.py`, `check_consistency.py`, `check_references.py`, `lint_manuscript.py`, `select_reporting_guidelines.py`, `validate_authorship.py`, …). Kein API-Key nötig. |
| 2 | [`literature-review`](skills/literature-review/SKILL.md) (v1.7) | Systematische Reviews/Meta-Analysen über mehrere Datenbanken (PubMed, arXiv, bioRxiv, Semantic Scholar) mit Verifikations- und Aggregationsskripten (`search_databases.py`, `verify_citations.py`, `generate_pdf.py`) und Multi-Style-Zitaten. Für Volltext-/Trial-/Registrier-Recherche ergänzend `paperclip`/`paper-lookup`. |
| 3 | [`research-lookup`](skills/research-lookup/SKILL.md) (v1.4) | Die schlanke Alternative für Belegsammlung ohne volles Review-Protokoll: „manuscript research packet“ mit Standard-60-verifizierte-Referenzen-Ziel, Parallel Search+Extract, Evidence-Matrix. Klare Scope-Grenze: ersetzt kein PRISMA-Review (dafür #2). Benötigt Parallel-API (`parallel-cli`), Fallbacks explizit. |
| 4 | [`statistical-analysis`](skills/statistical-analysis/SKILL.md) (v1.1) | Geführte Inferenz mit „Reviewer-fest“-Anspruch: festgelegter Analyse-Bogen (Frage → Inspektion → Testwahl → Annahmen via `assumption_checks.py` → Effektstärken → APA-Report), Bayesian-Alternativen, Power; dokumentierte Versionsfallen (Pingouin-0.6-Spaltenumbenennung, ArviZ-89-%-Intervalle, fehlende einseitige BFs). Ergänzt durch `statsmodels`/`pymc` für Modell-APIs. |
| 5 | [`exploratory-data-analysis`](skills/exploratory-data-analysis/SKILL.md) (v1.1) | Sicherer Erst-Blick auf Daten: bounded, lokal, netzwerkfrei, fail-closed für unbekannte Formate; Missingness-/Leakage-Audits, Outlier-/Transformationssensitivität, Report-Scaffold; behandelt Zellen/Metadaten explizit als untrusted. Passt exakt auf Schritt „Vor der Analyse“ von #4. |
| 6 | [`scientific-visualization`](skills/scientific-visualization/SKILL.md) (v1.1) | Ehrliche, publikationsreife Abbildungen (Matplotlib/Seaborn/Plotly) mit Guardrails gegen deceptive Encodings, Metadaten-Validierung und Journal-Export-Planning; erzwingt Trennung „Journal-Anforderung verifizieren“ vs. „vermuten“. Macht Analyse-Outputs abgabefähig. |
| 7 | [`scientific-critical-thinking`](skills/scientific-critical-thinking/SKILL.md) (v1.2) | Der Qualitäts-Gatekeeper: Design-Validität, Bias/Confounder, GRADE- und Cochrane-RoB-Raster — einsetzbar vor Schreiben (#1) und nach Lookup (#2/#3). Für formale Begutachtung stattdessen explizit `peer-review` (vertraulichkeits-sensitiv, local-only CLIs); für Ideenfindung `hypothesis-generation` (evidenz-gebundene Hypothesen, keine Kausalbehauptungen). |

**Nicht aufgenommen & warum:** `citation-management`/`venue-templates` (stark, aber zitier-seitig
von #1/#2 abgedeckt — bei reinen BibTeX-Aufgaben aber erste Wahl), `dask`/`vaex`/`polars`
(pure Scale-Tools, kein Research-Layer), `database-lookup` (genial für determinierte API-Fakten,
aber domänen­spezifisch; als #8 bereit, sobald konkrete Datenbankfragen anstehen).

## 4. Cheatsheet: Repo in Arena benutzen

```text
scannen      skills/                      → 163 Ordner; Ordnername == Skillname (Frontmatter name)
metadaten    head -n 12 skills/<name>/SKILL.md   → name/description/compatibility/version
leitplan     rg -n "^## " skills/<name>/SKILL.md → Sektionen (When to use, Workflow, Boundaries)
tiefe        ls skills/<name>/references/  → Detail-Doku nur laden, wenn nötig
werkzeuge    ls skills/<name>/scripts/     → lokale CLIs ausführen statt Anweisungen nur zitieren
tests        ls tests/<name>/ 2>/dev/null  → Repo-Suite des Skills (exists? before claiming)
katalog      Anhang A unten / README.md "What's Included" nach Kategorien
neu/ändern   AGENTS.md + CONTRIBUTING.md: smallest useful change, metadata.version bump,
             validate & run documented commands; Tests NUR unter tests/<name>/
```

- **Pflicht-Lektüre pro Aufgabe:** `AGENTS.md` (Repo-Konventionen), dann `SKILL.md` des gewählten
  Skills — diese Datei hier ersetzt das nicht.
- **Netzwerk/APIs:** Die meisten Kern-Skills sind offline deterministisch. API-abhängig sind u. a.
  `literature-review` (`OPENROUTER_API_KEY`, optional), `research-lookup`/`parallel-web`
  (`PARALLEL_API_KEY`), `scientific-schematics` (`OPENROUTER_API_KEY`). Ohne Key: lokale CLIs und
  geführte Checklisten nutzen, fehlende Live-Recherche als `TODO` markieren — nicht erfinden.
- **Vertraulichkeit zuerst:** unveröffentlichte Manuskripte/Reviews/PHI bleiben lokal
  (`peer-review`, `scientific-writing`-Boundaries); externe Dienste nur mit expliziter Autorisierung.
- **Vorlagen in Arena-Aufgaben:** Deliverable immer mit Abschnitten
  `Quellen` (verifiziert, mit Locator) · `Annahmen` (`ASSUMPTION:`-Marker) · `Offen/ToDos`
  (`UNVERIFIED`/`MISSING`-Platzhalter) · `Change-Log` (Commit/PR-Vorschlag bei Repo-Änderungen).
- **Änderungen am Repo:** Branch `arena/<session>`, kleiner Commit pro logischem Schritt,
  PR-Vorschlag (Titel + Body) ausgeben; `plugin.json`/`pyproject.toml` Version nur bei Skill-Änderung
  anfassen (muss laut `AGENTS.md` übereinstimmen).

## 5. Schnelle Skill-Landkarte (Kategorien)

Vollständiger, aus den `SKILL.md`-Frontmattern generierter Katalog: **Anhang A** (alphabetisch, 163 Einträge).

| Kategorie | Beispiele (Auswahl) |
|---|---|
| Schreiben & Publikation | scientific-writing · venue-templates · latex-posters · markdown-mermaid-writing |
| Literatur & Evidenz | literature-review · research-lookup · citation-management · paperclip · paper-lookup · paperzilla · bgpt-paper-search · exa-search · parallel-web |
| Bewertung & Methode | scientific-critical-thinking · peer-review · scholar-evaluation · hypothesis-generation · scientific-brainstorming · experimental-design · statistical-power · uncertainty-and-units |
| Statistik & ML | statistical-analysis · statsmodels · pymc · scikit-learn · shap · timesfm-forecasting · aeon · pytorch-lightning · transformers |
| Visualisierung & Docs | scientific-visualization · matplotlib · seaborn · infographics · scientific-schematics · scientific-slides · pptx · pptx-posters · docx · pdf · xlsx · markitdown |
| Daten & Infra | database-lookup · dask · polars · vaex · zarr-python · lamindb · modal · get-available-resources · hugging-science |
| Omics & Biologie | scanpy · anndata · scvi-tools · scvelo · pydeseq2 · bulk-rnaseq · gget · biopython · phylogenetics · pathway-enrichment |
| Chemie & Pharma | rdkit · datamol · medchem · deepchem · diffdock · molecular-dynamics · pkpd-modeling · pytdc · rowan |
| Klinik & Regulatorik | clinical-reports · clinical-decision-support · treatment-plans · iso-standards-readiness · analytical-method-validation · relsa-severity-assessment |
| Imaging & Signale | pydicom · pathml · histolab · deepspot-m · imaging-data-commons · neurokit2 · neuropixels-analysis · bids |
| Lab-Automation | opentrons-integration · pylabrobot · benchling-integration · labarchive-integration · protocolsio-integration · nextflow |
| Physik/Geo/Eng | astropy · qutip · cirq · qiskit · pennylane · sympy · geopandas · geomaster · fluidsim · simpy · pymoo |
| Meta/Agent | autoskill · arbor · what-if-oracle · consciousness-council · dhdna-profiler · pi-agent · tamarind |

---

## Anhang A — Vollständiger Skill-Katalog (163)

Generiert am 2026-08-31 aus dem jeweiligen `description`-Feld der `SKILL.md`-Frontmatter
(erster Satz, gekürzt auf ≤160 Zeichen). Links führen zu den Quelldateien.

| Skill | Zweck (1. Satz aus der Frontmatter) |
|---|---|
| [`adaptyv`](skills/adaptyv/SKILL.md) | How to use the Adaptyv Bio Foundry API and Python SDK for protein experiment design, submission, and results retrieval |
| [`aeon`](skills/aeon/SKILL.md) | Time series machine learning tasks including classification, regression, clustering, forecasting, anomaly detection,… |
| [`analytical-method-validation`](skills/analytical-method-validation/SKILL.md) | Plan, execute, and document validation, verification, and transfer of analytical procedures under the governing framework - ICH Q2(R2) and Q14, USP… |
| [`anndata`](skills/anndata/SKILL.md) | Data structure for annotated matrices in single-cell analysis |
| [`arbor`](skills/arbor/SKILL.md) | Autonomously improve a real artifact (code, training recipe, agent harness, data pipeline, prompt) against an objective and an evaluator, using Hypothesis… |
| [`arboreto`](skills/arboreto/SKILL.md) | Infer gene regulatory networks (GRNs) from gene expression data using scalable algorithms (GRNBoost2, GENIE3) |
| [`astropy`](skills/astropy/SKILL.md) | Core Python library for astronomy and astrophysics workflows that need Astropy APIs, including units/quantities, coordinates, FITS I/O, tables, time… |
| [`autoskill`](skills/autoskill/SKILL.md) | Observe the user's screen via screenpipe, detect repeated research workflows, match them against existing scientific-agent-skills, and draft new skills (or… |
| [`benchling-integration`](skills/benchling-integration/SKILL.md) | Benchling Python SDK and REST API integration for registry entities, inventory, ELN entries, workflows, Benchling Apps, and Data Warehouse queries |
| [`bgpt-paper-search`](skills/bgpt-paper-search/SKILL.md) | Search scientific papers and retrieve structured experimental data extracted from full-text studies via the BGPT MCP server |
| [`bids`](skills/bids/SKILL.md) | Working with Brain Imaging Data Structure (BIDS) datasets: organizing neuroscience and biomedical data (MRI, EEG, MEG, iEEG, PET,… |
| [`biopython`](skills/biopython/SKILL.md) | Comprehensive molecular biology toolkit |
| [`bioservices`](skills/bioservices/SKILL.md) | Unified Python interface to 40+ bioinformatics services |
| [`bulk-rnaseq`](skills/bulk-rnaseq/SKILL.md) | End-to-end bulk RNA-seq orchestrator — takes raw FASTQ reads through QC and trimming (FastQC, fastp/Trim Galore), alignment and quantification (STAR,… |
| [`cellxgene-census`](skills/cellxgene-census/SKILL.md) | Query the CZ CELLxGENE Census programmatically for versioned public single-cell and spatial transcriptomics data |
| [`cirq`](skills/cirq/SKILL.md) | Google quantum computing framework |
| [`citation-management`](skills/citation-management/SKILL.md) | Comprehensive citation management for academic research |
| [`clinical-decision-support`](skills/clinical-decision-support/SKILL.md) | Prepare and validate research-only clinical decision-support evaluation, evidence-profile, cohort, survival, biomarker/model, privacy, and governance artifacts |
| [`clinical-reports`](skills/clinical-reports/SKILL.md) | Create safety-bounded draft structures and run local deterministic checks for clinical case, diagnostic, trial, safety, and aggregate research reports |
| [`cobrapy`](skills/cobrapy/SKILL.md) | Constraint-based metabolic modeling (COBRA) |
| [`consciousness-council`](skills/consciousness-council/SKILL.md) | Run a multi-perspective Mind Council deliberation on any question, decision, or creative challenge |
| [`dask`](skills/dask/SKILL.md) | Distributed computing for larger-than-RAM pandas/NumPy workflows |
| [`database-lookup`](skills/database-lookup/SKILL.md) | Query documented public database APIs with explicit endpoints, filters, pagination, and provenance |
| [`datamol`](skills/datamol/SKILL.md) | Pythonic wrapper around RDKit with simplified interface and sensible defaults |
| [`deepchem`](skills/deepchem/SKILL.md) | Molecular ML with diverse featurizers and pre-built datasets |
| [`deepspot-m`](skills/deepspot-m/SKILL.md) | Generate transcriptome-wide virtual spatial transcriptomics from H&E histology with DeepSpot-M. Use when you need spatial gene expression in log1p-CPM for… |
| [`deeptools`](skills/deeptools/SKILL.md) | NGS analysis toolkit |
| [`depmap`](skills/depmap/SKILL.md) | Query the Cancer Dependency Map (DepMap) for cancer cell line gene dependency scores (CRISPR Chronos), drug sensitivity data, and gene effect profiles |
| [`dhdna-profiler`](skills/dhdna-profiler/SKILL.md) | Extract cognitive patterns and thinking fingerprints from any text |
| [`diffdock`](skills/diffdock/SKILL.md) | DiffDock and DiffDock-L molecular docking |
| [`dnanexus-integration`](skills/dnanexus-integration/SKILL.md) | Build and operate reproducible genomics workloads on DNAnexus with the dx CLI, dxpy, apps/applets, native workflows, dxCompiler, and Nextflow |
| [`docx`](skills/docx/SKILL.md) | The user wants to create, read, edit, or manipulate Word documents (.docx files) or Word templates (.dotx files) |
| [`esm`](skills/esm/SKILL.md) | Use when working directly with the `esm` Python SDK, ESM3 or ESMC model IDs, Forge/Biohub inference clients, or ESMFold2 folding workflows |
| [`etetoolkit`](skills/etetoolkit/SKILL.md) | Analyze, manipulate, compare, annotate, and visualize phylogenetic or other hierarchical trees with ETE 4 |
| [`exa-search`](skills/exa-search/SKILL.md) | Web toolkit powered by Exa, tuned for scientific and technical content |
| [`experimental-design`](skills/experimental-design/SKILL.md) | Design experiments and studies BEFORE data is collected — choosing a design, randomizing, blocking, and laying out treatment combinations so results are… |
| [`exploratory-data-analysis`](skills/exploratory-data-analysis/SKILL.md) | Perform bounded, local exploratory analysis of explicitly supported scientific files |
| [`flowio`](skills/flowio/SKILL.md) | Read, inspect, and write Flow Cytometry Standard (FCS) 2.0, 3.0, and 3.1 files with FlowIO. Use for low-level FCS metadata and channel inspection, NumPy… |
| [`fluidsim`](skills/fluidsim/SKILL.md) | Plan, configure, inspect, restart, and analyze bounded FluidSim computational-fluid-dynamics simulations with explicit numerical-validity and HPC safety checks |
| [`generate-image`](skills/generate-image/SKILL.md) | Generate or edit images with AI models through the OpenRouter Image API (Gemini, Seedream, Recraft, GPT-Image, Riverflow) |
| [`geniml`](skills/geniml/SKILL.md) | Use Geniml for audited local genomic-interval workflows: validate BED and universe contracts, plan Region2Vec or scEmbed runs, inspect model/tokenizer… |
| [`genomic-coordinates`](skills/genomic-coordinates/SKILL.md) | Convert genomic intervals between coordinate conventions, normalise and compare variant representations, and detect assembly or contig-naming mismatches… |
| [`genomic-intelligence`](skills/genomic-intelligence/SKILL.md) | Predict regulatory features, gene structure, and expression directly from DNA sequence using Genomic Intelligence's hosted transformer DNA language models —… |
| [`geomaster`](skills/geomaster/SKILL.md) | Comprehensive geospatial science skill covering remote sensing, GIS, spatial analysis, machine learning for earth observation, and 30+ scientific domains |
| [`geopandas`](skills/geopandas/SKILL.md) | Guidance and local audit tools for Python workflows that directly use GeoPandas GeoSeries, GeoDataFrame, spatial operations, or vector-data I/O |
| [`get-available-resources`](skills/get-available-resources/SKILL.md) | Detect host inventory and effective CPU, memory, disk, scheduler, container, and accelerator limits when a user asks for resource-aware planning or before a… |
| [`gget`](skills/gget/SKILL.md) | Fast CLI/Python queries to 20+ bioinformatics databases |
| [`ginkgo-cloud-lab`](skills/ginkgo-cloud-lab/SKILL.md) | Submit and manage protocols on Ginkgo Bioworks Cloud Lab (cloud.ginkgo.bio), a web-based interface for autonomous lab execution on Reconfigurable Automation… |
| [`glycoengineering`](skills/glycoengineering/SKILL.md) | Analyze and engineer protein glycosylation |
| [`gtars`](skills/gtars/SKILL.md) | Use Gtars for local genomic interval models and set algebra, overlaps and counts, consensus and coverage, tokenization, fragment processing, and… |
| [`histolab`](skills/histolab/SKILL.md) | Lightweight WSI tile extraction and preprocessing |
| [`hugging-science`](skills/hugging-science/SKILL.md) | Use when the user is doing AI/ML work in a scientific domain such as biology, chemistry, physics, astronomy, climate, genomics, materials, medicine,… |
| [`hypogenic`](skills/hypogenic/SKILL.md) | Plans and audits use of ChicagoHAI HypoGeniC/HypoRefine for LLM-assisted hypothesis generation from labeled text datasets |
| [`hypothesis-generation`](skills/hypothesis-generation/SKILL.md) | Formulate evidence-bounded scientific questions, candidate hypotheses, rival explanations, causal or associational claims, discriminating predictions,… |
| [`imaging-data-commons`](skills/imaging-data-commons/SKILL.md) | Query and download public cancer imaging data from NCI Imaging Data Commons |
| [`infographics`](skills/infographics/SKILL.md) | Create professional infographics using Nano Banana Pro AI with smart iterative refinement |
| [`iso-standards-readiness`](skills/iso-standards-readiness/SKILL.md) | Prepares and structurally reviews readiness evidence for ISO management-system and laboratory-competence standards - ISO 13485 medical device QMS, ISO 14971… |
| [`lab-hardware-cad`](skills/lab-hardware-cad/SKILL.md) | Design custom laboratory hardware as parametric build123d models and export fabrication-ready STEP, STL, and DXF files - microfluidic chips and molds,… |
| [`labarchive-integration`](skills/labarchive-integration/SKILL.md) | Securely integrate with the official LabArchives ELN REST-like API and Inventory API v1 |
| [`lamindb`](skills/lamindb/SKILL.md) | Use when working with LaminDB, the open-source lineage-native lakehouse for biological datasets and models |
| [`latchbio-integration`](skills/latchbio-integration/SKILL.md) | Build, register, debug, and operate bioinformatics workflows on Latch using the Python SDK, CLI, Latch Data and Registry, Nextflow, Snakemake, programmatic… |
| [`latex-posters`](skills/latex-posters/SKILL.md) | Create professional research posters in LaTeX using beamerposter, tikzposter, or baposter |
| [`liteparse`](skills/liteparse/SKILL.md) | Local document and PDF parsing that returns spatial text with bounding boxes |
| [`literature-review`](skills/literature-review/SKILL.md) | Conduct comprehensive, systematic literature reviews using multiple academic databases (PubMed, arXiv, bioRxiv, Semantic Scholar, etc.) |
| [`markdown-mermaid-writing`](skills/markdown-mermaid-writing/SKILL.md) | Comprehensive markdown and Mermaid diagram writing skill |
| [`market-research-reports`](skills/market-research-reports/SKILL.md) | Build evidence-traceable market research reports and assumption-driven market sizing or forecast scenarios |
| [`markitdown`](skills/markitdown/SKILL.md) | Convert heterogeneous documents and selected URIs to Markdown with Microsoft MarkItDown for text analysis, search, and LLM/RAG ingestion |
| [`matchms`](skills/matchms/SKILL.md) | Process, clean, compare, and search tandem mass spectra with matchms |
| [`matlab`](skills/matlab/SKILL.md) | Build, review, migrate, and safely plan MATLAB or GNU Octave numerical workflows, including arrays, tabular/time data, tests, projects, graphics, MAT files,… |
| [`matplotlib`](skills/matplotlib/SKILL.md) | Low-level plotting library for full customization |
| [`medchem`](skills/medchem/SKILL.md) | Medicinal chemistry filters for compound triage |
| [`modal`](skills/modal/SKILL.md) | Modal is a serverless cloud platform for running Python on demand, including on-demand GPUs |
| [`molecular-dynamics`](skills/molecular-dynamics/SKILL.md) | Run and analyze molecular dynamics simulations with OpenMM and MDAnalysis |
| [`molfeat`](skills/molfeat/SKILL.md) | Molecular featurization for ML (100+ featurizers) |
| [`ncats-arax`](skills/ncats-arax/SKILL.md) | Queries the NCATS Translator ARAX production API for bounded, typed, provenance-rich one-hop and endpoint-pinned two-hop biomedical knowledge-graph… |
| [`networkx`](skills/networkx/SKILL.md) | Create, analyze, and visualize complex networks and graphs in Python with NetworkX. Use when working with network/graph data structures, computing graph… |
| [`neurokit2`](skills/neurokit2/SKILL.md) | Use NeuroKit2 to build or audit reproducible research workflows for physiological time-series preprocessing, event/interval analysis, multimodal alignment,… |
| [`neuropixels-analysis`](skills/neuropixels-analysis/SKILL.md) | Analyze Neuropixels extracellular recordings end-to-end with SpikeInterface |
| [`nextflow`](skills/nextflow/SKILL.md) | Build, run, and debug Nextflow data pipelines and nf-core workflows end to end |
| [`omero-integration`](skills/omero-integration/SKILL.md) | Securely inspect and automate microscopy data workflows against OMERO.server with omero-py, BlitzGateway, OMERO CLI, tables, annotations, ROIs, rendering,… |
| [`onekgpd`](skills/onekgpd/SKILL.md) | Query the 1000 Genomes Project dataset (3,202 whole-genome-sequenced individuals, GRCh38) at the level of individual participants |
| [`ontology-term-resolution`](skills/ontology-term-resolution/SKILL.md) | Resolve free-text scientific labels to ontology term IDs and validate existing CURIEs against the EBI Ontology Lookup Service (OLS4) |
| [`open-notebook`](skills/open-notebook/SKILL.md) | Self-hosted, open-source alternative to Google NotebookLM for AI-powered research and document analysis |
| [`openpiv`](skills/openpiv/SKILL.md) | Particle Image Velocimetry (PIV) analysis with OpenPIV. Use when extracting velocity fields from PIV image pairs, analyzing fluid dynamics or flow… |
| [`opentrons-integration`](skills/opentrons-integration/SKILL.md) | Author, review, migrate, simulate, and troubleshoot official Opentrons Python Protocol API v2 protocols for Flex and OT-2 robots |
| [`optimize-for-gpu`](skills/optimize-for-gpu/SKILL.md) | GPU-accelerates scientific Python on NVIDIA hardware and verifies that the result is correct and faster |
| [`pacsomatic`](skills/pacsomatic/SKILL.md) | Operator toolkit for nf-core/pacsomatic matched tumor-normal workflows from BAM inputs |
| [`paper-lookup`](skills/paper-lookup/SKILL.md) | Search 11 academic literature APIs for papers, preprints, citations, and open-access full text, and return results with reproducible provenance |
| [`paperclip`](skills/paperclip/SKILL.md) | Search and read full-text biomedical papers, FDA/PMDA/EMA regulatory documents, clinical trial registries, and UniProt/PDB/ChEMBL entries with the Paperclip… |
| [`paperzilla`](skills/paperzilla/SKILL.md) | Chat with your agent about projects, recommendations, and canonical papers in Paperzilla |
| [`parallel-web`](skills/parallel-web/SKILL.md) | Use Parallel CLI for web search, URL extraction, deep research, structured data enrichment, entity discovery, and recurring web monitoring |
| [`pathml`](skills/pathml/SKILL.md) | Use PathML for local, research-only computational pathology workflows: load and tile slides, build preprocessing and QC pipelines, manage h5path data,… |
| [`pathogen-variant-surveillance`](skills/pathogen-variant-surveillance/SKILL.md) | Query live pathogen genomic surveillance data through the GenSpectrum LAPIS API to find which viral lineages are circulating now, how fast they are growing,… |
| [`pathway-enrichment`](skills/pathway-enrichment/SKILL.md) | Run pathway and gene-set enrichment analysis on gene lists or ranked gene data, then interpret the results |
| [`pdf`](skills/pdf/SKILL.md) | The user wants to do anything with PDF files |
| [`peer-review`](skills/peer-review/SKILL.md) | Prepare evidence-bounded, constructive peer-review drafts and structured manuscript assessments |
| [`pennylane`](skills/pennylane/SKILL.md) | Hardware-agnostic quantum ML framework with automatic differentiation |
| [`phylogenetics`](skills/phylogenetics/SKILL.md) | Build and analyze phylogenetic trees using MAFFT (multiple alignment), IQ-TREE 2 (maximum likelihood), and FastTree (fast NJ/ML) |
| [`pi-agent`](skills/pi-agent/SKILL.md) | Build with and use Pi, the minimal terminal coding harness |
| [`pkpd-modeling`](skills/pkpd-modeling/SKILL.md) | Pharmacokinetic and pharmacodynamic modelling and simulation - non-compartmental analysis, compartmental and population PK, PK/PD and exposure-response,… |
| [`polars`](skills/polars/SKILL.md) | High-performance DataFrame library for Python ETL, analytics, and pandas migration |
| [`polars-bio`](skills/polars-bio/SKILL.md) | High-performance genomic interval operations and bioinformatics file I/O on Polars DataFrames |
| [`pptx`](skills/pptx/SKILL.md) | Use this skill any time a .pptx or .potx file is involved in any way — as input, output, or both |
| [`pptx-posters`](skills/pptx-posters/SKILL.md) | Create and audit editable scientific posters in macro-free PowerPoint (.pptx) from author-approved local content and assets |
| [`primekg`](skills/primekg/SKILL.md) | Query the Precision Medicine Knowledge Graph (PrimeKG) for multiscale biological data including genes, drugs, diseases, phenotypes, and more |
| [`protocolsio-integration`](skills/protocolsio-integration/SKILL.md) | Read, validate, and safely export protocols.io data with current official REST/MCP contracts, or create non-executing mutation plans |
| [`pufferlib`](skills/pufferlib/SKILL.md) | Version-aware guidance for PufferLib reinforcement-learning environments, vectorization, policies, PuffeRL training, evaluation, and safe checkpoint review |
| [`pydeseq2`](skills/pydeseq2/SKILL.md) | Differential gene expression analysis for bulk RNA-seq with PyDESeq2, including formulaic designs, Wald tests, FDR correction, LFC shrinkage, and result… |
| [`pydicom`](skills/pydicom/SKILL.md) | Use pydicom to read, inspect, write, transform, and safely preflight local DICOM datasets and pixel data |
| [`pyhealth`](skills/pyhealth/SKILL.md) | Build clinical/healthcare deep-learning pipelines with PyHealth — loading EHR/signal/imaging datasets (MIMIC-III/IV, eICU, OMOP, SleepEDF, ChestXray14,… |
| [`pylabrobot`](skills/pylabrobot/SKILL.md) | Develop and review PyLabRobot lab-automation resources, liquid-handling plans, offline simulations, and supported-device integrations |
| [`pymatgen`](skills/pymatgen/SKILL.md) | Analyze, validate, convert, and transform materials structures and computed materials data with current pymatgen APIs, including local phase diagrams,… |
| [`pymc`](skills/pymc/SKILL.md) | Bayesian modeling with PyMC. Build hierarchical models, MCMC (NUTS), variational inference, LOO/WAIC comparison, posterior checks, for probabilistic… |
| [`pymoo`](skills/pymoo/SKILL.md) | Multi-objective optimization framework |
| [`pyopenms`](skills/pyopenms/SKILL.md) | Complete mass spectrometry analysis platform |
| [`pysam`](skills/pysam/SKILL.md) | Python/HTSlib workflows for genomic files |
| [`pytdc`](skills/pytdc/SKILL.md) | Use Therapeutics Data Commons through the PyTDC Python package for registry discovery, approved dataset access, task-aware splits, evaluator metrics,… |
| [`pytorch-lightning`](skills/pytorch-lightning/SKILL.md) | Deep learning framework (PyTorch Lightning / lightning package) |
| [`pyzotero`](skills/pyzotero/SKILL.md) | Interact with Zotero reference management libraries using the pyzotero Python client |
| [`qiskit`](skills/qiskit/SKILL.md) | Build, simulate, transpile, and execute quantum circuits with Qiskit and IBM Quantum Runtime |
| [`qutip`](skills/qutip/SKILL.md) | Simulate and audit closed and open quantum-system models with QuTiP 5, including deterministic, trajectory, steady-state, spectral, and phase-space workflows |
| [`rdkit`](skills/rdkit/SKILL.md) | Cheminformatics toolkit for fine-grained molecular control |
| [`relsa-severity-assessment`](skills/relsa-severity-assessment/SKILL.md) | Multivariate severity assessment and humane endpoint prediction for laboratory animal studies using the RELSA (RELative Severity Assessment) score and… |
| [`research-grants`](skills/research-grants/SKILL.md) | Write competitive research proposals for NSF, NIH, DOE, DARPA, and Taiwan NSTC. Agency-specific formatting, review criteria, budget preparation, broader… |
| [`research-lookup`](skills/research-lookup/SKILL.md) | Compile current scholarly evidence for a scientific manuscript or research brief |
| [`rowan`](skills/rowan/SKILL.md) | Rowan is a cloud-native molecular modeling and medicinal-chemistry workflow platform with a Python API. Use for pKa and macropKa prediction, conformer and… |
| [`scanpy`](skills/scanpy/SKILL.md) | Standard single-cell RNA-seq analysis pipeline |
| [`scholar-evaluation`](skills/scholar-evaluation/SKILL.md) | Provide qualitative-first, evidence-traceable developmental review of scholarly works and audit low-stakes research-assessment rubrics with optional local… |
| [`scientific-brainstorming`](skills/scientific-brainstorming/SKILL.md) | Facilitates evidence-aware scientific ideation with independent generation, structured discussion, explicit assumptions, transparent evaluation, adversarial… |
| [`scientific-critical-thinking`](skills/scientific-critical-thinking/SKILL.md) | Evaluate scientific claims and evidence quality |
| [`scientific-schematics`](skills/scientific-schematics/SKILL.md) | Create publication-quality scientific diagrams using Nano Banana 2 AI with smart iterative refinement |
| [`scientific-slides`](skills/scientific-slides/SKILL.md) | Build slide decks and presentations for research talks |
| [`scientific-visualization`](skills/scientific-visualization/SKILL.md) | Create and audit truthful, accessible, publication-ready scientific figures with Matplotlib, Seaborn, or Plotly |
| [`scientific-writing`](skills/scientific-writing/SKILL.md) | Draft, revise, and audit scientific manuscripts or reports with explicit evidence provenance, reporting-guideline coverage, authorship accountability,… |
| [`scikit-bio`](skills/scikit-bio/SKILL.md) | Biological data toolkit |
| [`scikit-learn`](skills/scikit-learn/SKILL.md) | Machine learning in Python with scikit-learn |
| [`scikit-survival`](skills/scikit-survival/SKILL.md) | Build, evaluate, and audit right-censored or competing-risk survival workflows with scikit-survival, including leakage-safe preprocessing, model selection,… |
| [`scvelo`](skills/scvelo/SKILL.md) | RNA velocity analysis with scVelo |
| [`scvi-tools`](skills/scvi-tools/SKILL.md) | Deep generative models for single-cell omics |
| [`seaborn`](skills/seaborn/SKILL.md) | Statistical visualization with pandas integration |
| [`shap`](skills/shap/SKILL.md) | Explain and audit machine-learning predictions with SHAP. Use for selecting SHAP explainers and maskers, computing and validating feature attributions,… |
| [`simpy`](skills/simpy/SKILL.md) | Build, inspect, test, and analyze bounded process-based discrete-event simulations with SimPy, including events, resources, interrupts, monitoring,… |
| [`stable-baselines3`](skills/stable-baselines3/SKILL.md) | Production-ready reinforcement learning algorithms (PPO, SAC, DQN, TD3, DDPG, A2C) with scikit-learn-like API. Use for standard RL experiments, quick… |
| [`statistical-analysis`](skills/statistical-analysis/SKILL.md) | Guided statistical analysis for research data - test selection, assumption checking, effect sizes, power analysis, Bayesian alternatives, and APA-formatted… |
| [`statistical-power`](skills/statistical-power/SKILL.md) | Sample-size and statistical power calculations for planning studies |
| [`statsmodels`](skills/statsmodels/SKILL.md) | Statistical models library for Python |
| [`sympy`](skills/sympy/SKILL.md) | Use when you need exact symbolic math in Python — algebra, calculus, equation solving, symbolic linear algebra, or code generation via lambdify/LaTeX.… |
| [`tamarind`](skills/tamarind/SKILL.md) | Access a collection of open-source molecular design and structural biology tools on the Tamarind Bio platform, via its REST API or MCP server — no local… |
| [`tiledbvcf`](skills/tiledbvcf/SKILL.md) | Efficient storage and retrieval of genomic variant data using TileDB. Scalable VCF/BCF ingestion, incremental sample addition, compressed storage, parallel… |
| [`timesfm-forecasting`](skills/timesfm-forecasting/SKILL.md) | Zero-shot time series forecasting with Google's TimesFM foundation model |
| [`torch-geometric`](skills/torch-geometric/SKILL.md) | PyTorch Geometric (PyG) for graph neural networks — node/link/graph classification, message passing (GCN, GAT, GraphSAGE, GIN), heterogeneous graphs,… |
| [`torchdrug`](skills/torchdrug/SKILL.md) | Build and troubleshoot TorchDrug 0.2.1 workflows for molecular graphs, property prediction, self-supervised pretraining, molecule generation,… |
| [`transformers`](skills/transformers/SKILL.md) | Hugging Face Transformers for loading Hub models, running pipeline inference, text generation, and Trainer fine-tuning on NLP, vision, audio, and multimodal… |
| [`treatment-plans`](skills/treatment-plans/SKILL.md) | Format and structurally validate local treatment-plan documentation after clinical decisions have already been supplied and verified by authorized licensed… |
| [`umap-learn`](skills/umap-learn/SKILL.md) | Use UMAP-learn for nonlinear dimensionality reduction, 2D/3D embeddings, clustering preprocessing, supervised or semi-supervised UMAP, DensMAP, AlignedUMAP,… |
| [`uncertainty-and-units`](skills/uncertainty-and-units/SKILL.md) | Track physical units and propagate measurement uncertainty in scientific calculations using pint and uncertainties |
| [`usfiscaldata`](skills/usfiscaldata/SKILL.md) | Query the U.S. Treasury Fiscal Data REST API for federal financial data |
| [`vaex`](skills/vaex/SKILL.md) | Processing and analyzing large tabular datasets (billions of rows) that exceed available RAM. Vaex excels at out-of-core DataFrame… |
| [`venue-templates`](skills/venue-templates/SKILL.md) | Prepare journal manuscripts, conference papers, research posters, and grant documents using venue-specific formatting guidance and bundled LaTeX scaffolds |
| [`waypoint-bio`](skills/waypoint-bio/SKILL.md) | Use when working with Outpost Bio's open microbiome foundation models - the Waypoint checkpoints (Waypoint-6m, Waypoint-45m, Waypoint-170m), the Atlas… |
| [`what-if-oracle`](skills/what-if-oracle/SKILL.md) | Run structured What-If scenario analysis with 4–6 branch possibility exploration (best, likely, worst, wild card, contrarian, second-order) |
| [`xlsx`](skills/xlsx/SKILL.md) | Create, edit, analyze, or convert Excel spreadsheets (.xlsx, .xlsm, .xltx) where the workbook file is the primary deliverable |
| [`zarr-python`](skills/zarr-python/SKILL.md) | Chunked N-D arrays for cloud storage (Zarr-Python 3) |