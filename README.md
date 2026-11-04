# Gateway Bench

**Gateway® cloning in one offline HTML file: BP and LR reactions on real plasmid sequences, protocol-based attB primer design, reading-frame checks and GenBank export.**

[![License: PolyForm Noncommercial 1.0.0](https://img.shields.io/badge/license-PolyForm%20Noncommercial%201.0.0-orange.svg)](LICENSE)
[![Free for academic use](https://img.shields.io/badge/academic%20use-free-brightgreen.svg)](#license)
[![No install](https://img.shields.io/badge/install-none-brightgreen.svg)](#getting-started)
[![Data stays local](https://img.shields.io/badge/data-never%20uploaded-brightgreen.svg)](#privacy)

Gateway Bench simulates BP and LR recombination on the actual sequences of your vectors and returns the complete recombined plasmid, not a size estimate. It designs attB primers from a published protocol, checks fusion reading frames, and exports GenBank files you can open in SnapGene or Benchling. Nothing is installed and nothing leaves your computer.

**[→ Open the live app](https://lao-labtools.org/gateway-bench/)**  ·  **[Download the file](index.html)**  ·  **[Report a bug](https://github.com/laolabtools/gateway-cloning/issues)**  ·  **[More Lao LabTools](https://lao-labtools.org)**

![Gateway Bench, BP reaction tab](docs/screenshot.png)

## Why this exists

Gateway cloning is simple in principle and easy to get subtly wrong in practice. The common mistakes are the wrong attB primer form for the expression host, a fusion that drops out of frame at attB2, or a donor and destination pair that do not match. Commercial sequence editors simulate the reactions but cost money. Free tools usually stop at primer sequences. Gateway Bench puts the whole workflow in one place: design the primers from the protocol, run BP and LR on real sequences, check the junctions and frame, and export an annotated GenBank file of the product.

## Privacy

Your files are read by JavaScript in your own browser. Nothing is uploaded, there is no server, no account and no tracking, and once the page has loaded it keeps working with the network disconnected. That makes it safe for unpublished data and for data you are not allowed to put in a cloud service. When you are online the page also fetches its typefaces from Google Fonts; no data goes with that request, and offline it falls back to your system fonts.

The hosted copy on [lao-labtools.org](https://lao-labtools.org) is covered by the site's [privacy policy](https://lao-labtools.org/privacy.html). A downloaded copy opened from your own disk makes no connection to the site at all.

## Getting started

There is nothing to install.

- **Online:** open **[lao-labtools.org/gateway-bench](https://lao-labtools.org/gateway-bench/)**.
- **Offline:** download [`index.html`](index.html) (open it on GitHub and press the download button) and double-click it. It opens in Chrome, Edge, Safari or Firefox and runs from your disk.

The **BP reaction** tab opens with an EGFP example. Press **Load EGFP sample** to restore it at any time.

## How to use

The five tabs follow the cloning workflow.

### BP reaction

1. **Insert.** Paste the ORF, or upload FASTA or GenBank. Choose **Design attB primers** or **Paste attB product** if you already have the PCR product.
2. **Expression plan.**
   - Choose the protein form: native, N-terminal fusion, C-terminal fusion, or native + N-terminal.
   - Choose the expression host, and the PCR strategy (**non-stop** or **adapter**). A TEV site is optional.
   - The tool applies the protocol's selection table to pick the attB1 form (**A**, **B** or **C**) and the attB2 form (**K** or **L**). It then builds each primer with the right Shine–Dalgarno sequence, Kozak sequence, start codon, stop codon or frame-completing base, and gives the universal adapter primers for the second round.
3. **Donor.** Pick a pDONR from the library or load your own. The reaction returns the entry clone (attL1–insert–attL2) and the by-product.

### LR reaction

Choose an entry clone and a destination vector. The product is the full expression clone, with backbone features carried across at shifted coordinates, the ccdB cassette removed, and the new attB sites and insert annotated.

### Vector library

54 donor, entry and destination vectors:

- pDONR and pENTR series;
- the pDEST *E. coli* set;
- pcDNA, pEF and pT-REx mammalian vectors;
- the lentiviral panel (pLX304, pLX301, pLX307, pLenti CMV/PGK Puro DEST, pInducer20/21, pLIX_403);
- retroviral, adenoviral and baculoviral vectors;
- Y2H and Advanced Gateway yeast vectors;
- plant binary vectors.

To add your own plasmid:

1. On Addgene, open the plasmid → **Sequences** → **Download full sequence (GenBank)**.
2. In the app, go to **Vector library → + Add vector** and drop the file in.

The app finds both att cores and classifies each site as attB, attL, attR or attP from the five bases flanking the core. From that pair it infers whether the plasmid is a donor, entry or destination vector. It also detects the ccdB cassette and reads the resistance marker and promoter from the feature labels. SnapGene `.dna` files are binary, so export them as GenBank first.

### History and att sites

- **History:** your recent reactions and products.
- **att sites:** the reference attB/attP/attL/attR sequences with their cores.

### Reading-frame checking

The frame check understands the protocol's geometry. For a C-terminal fusion, form L deliberately leaves attB2 offset by one base, so that the added base completes a CAC codon running into the tag. The tool verifies that offset rather than flagging it as an error. When the frame really is wrong, it tells you to rebuild with form L.

### Import and export

- In: FASTA, GenBank and plain sequence.
- Out: GenBank, FASTA and a primer CSV.
- Every product can be viewed with position numbering, colour-coded att sites, ribosome-recognition sequences and insert, and as a linear or circular map.

## How the recombination works

Gateway specificity lives in a 15-bp core with a 7-bp strand-exchange overlap at its centre. Each att site is that core plus flanking sequence, and which site you have is decided by what sits on either side of the core:

| left of core | right of core | site |
|---|---|---|
| `ACAAG` | `AGGCT` | attB1 |
| λ arm | `AGGCT` | attL1 |
| `ACAAG` | λ arm | attR1 |
| λ arm | λ arm | attP1 |

The att2 sites are the mirror image. Recombination is then one operation for both BP and LR: keep everything outside the two cores, and splice the new insert between them.

Whether the product carries attL or attB sites falls out of what the input vector contributed. A donor's λ arms give attL; a destination's `ACAAG`/`GTGGT` give attB. The same function therefore handles both reactions correctly. Circular plasmids whose cassette wraps the origin are rotated first.

## Accuracy and limitations

- **Junction sequences, primers and frame logic are exact.** The attB, attL and attR sequences are the published ones, and recombination preserves the core base for base.
- **Plasmid sizes without a loaded sequence are calculated**, as the published vector length minus its att–ccdB cassette plus the new cassette. Load the GenBank file for an exact product.
- **attP1/attP2 are not reproduced in full**, because they carry about 200 bp of λ arms on both sides. Upload your donor's GenBank file and the app reads them directly.
- **Backbone features on calculated products are schematic.** Promoter, marker and origin positions on the circular map come from the vector class, not from a sequenced map.
- Always sequence across both junctions before you use a clone.

## Building from source

The app is written as modules and assembled into the single file:

```bash
git clone https://github.com/laolabtools/gateway-cloning.git
cd gateway-cloning
python3 build.py          # writes index.html and dist/artifact.html
```

Python 3 is used only to concatenate the files; there are no dependencies.

| file | contents |
|---|---|
| `src/main-head.html` | markup, design tokens and stylesheet |
| `src/core.js` | att sequences, sequence utilities, map drawing |
| `src/vectors.js` | the vector catalogue |
| `src/lib.js` | FASTA and GenBank parsers/writers, ZIP writer |
| `src/lib2.js` | att-site detection, recombination engine, primer design |
| `src/app.js` | state, views and event wiring |
| `index.html` | the built, standalone app |
| `dist/artifact.html` | a body-only build for embedding in a host page |

## Reporting bugs and requesting features

Please use **[GitHub Issues](https://github.com/laolabtools/gateway-cloning/issues)** for bugs, wrong results and feature requests. A useful report includes:

- what you did, what you expected and what happened instead;
- your browser and operating system;
- a screenshot, and if you can, the insert or GenBank file so the problem can be reproduced (remove anything confidential first).

If you would rather not post publicly, email [contact@lao-labtools.org](mailto:contact@lao-labtools.org).

## Citing

If this tool contributed to published work, please cite it as:

> Lao L. *Gateway Bench: simulation of Gateway BP/LR cloning and attB primer design in the browser*. 2026. https://github.com/laolabtools/gateway-cloning

Please also cite the methods and resources it is built on (listed below), in particular the Gateway technology paper and, for primer design, Won Do Heo's protocol.

## Acknowledgements and references

**Primer design protocol.** The attB primer design follows **"Primer Design for the Gateway attB primers", modified by Won Do Heo**. The following all come from that protocol:

- the A/B/C and K/L primer forms;
- the host × protein-form selection table;
- the non-stop and adapter PCR strategies;
- the TEV fusion primer;
- the SeqL-A / SeqL-B sequencing primers.

The protocol document itself is not redistributed here.

**Gateway technology and λ site-specific recombination.**
- Hartley JL, Temple GF, Brasch MA. DNA cloning using in vitro site-specific recombination. *Genome Res* 10, 1788–1795 (2000). [doi:10.1101/gr.143000](https://doi.org/10.1101/gr.143000)
- Landy A. Dynamic, structural, and regulatory aspects of λ site-specific recombination. *Annu Rev Biochem* 58, 913–949 (1989). [doi:10.1146/annurev.bi.58.070189.004405](https://doi.org/10.1146/annurev.bi.58.070189.004405)
- Thermo Fisher Scientific (Invitrogen) *Gateway Technology* user manuals. The att site sequences and the specifications of the Invitrogen pDONR, pENTR and pDEST vectors (catalogue numbers are listed in the app).

**Vector catalogue.** The library stores vector names, sizes and features (not sequences), taken from the original publications and from [Addgene](https://www.addgene.org/):

- Yang X *et al.* *Nat Methods* 8, 659–661 (2011). [doi:10.1038/nmeth.1638](https://doi.org/10.1038/nmeth.1638). pLX304; the Broad Institute GPP lentiviral vectors pLX301, pLX307 and pLIX_403 are distributed through Addgene (#25895, #41392, #41395).
- Campeau E *et al.* A versatile viral system for expression and depletion of proteins in mammalian cells. *PLoS One* 4, e6529 (2009). [doi:10.1371/journal.pone.0006529](https://doi.org/10.1371/journal.pone.0006529). pLenti CMV/PGK Puro DEST.
- Meerbrey KL *et al.* The pINDUCER lentiviral toolkit for inducible RNA interference in vitro and in vivo. *Proc Natl Acad Sci USA* 108, 3665–3670 (2011). [doi:10.1073/pnas.1019736108](https://doi.org/10.1073/pnas.1019736108). pInducer20/21.
- Alberti S, Gitler AD, Lindquist S. A suite of Gateway cloning vectors for high-throughput genetic analysis in *Saccharomyces cerevisiae*. *Yeast* 24, 913–919 (2007). [doi:10.1002/yea.1502](https://doi.org/10.1002/yea.1502). Advanced Gateway yeast vectors.
- Gloeckner CJ, Boldt K, Schumacher A, Roepman R, Ueffing M. A novel tandem affinity purification strategy for the efficient isolation and characterisation of native protein complexes. *Proteomics* 7, 4228–4234 (2007). SF-TAP.
- Nallamsetty S, Austin BP, Penrose KJ, Waugh DS. Gateway vectors for the production of combinatorially-tagged His6-MBP fusion proteins in the cytoplasm and periplasm of *Escherichia coli*. *Protein Sci* 14, 2964–2971 (2005). The Waugh lab His6-MBP vectors.
- Earley KW *et al.* Gateway-compatible vectors for plant functional genomics and proteomics. *Plant J* 45, 616–629 (2006). [doi:10.1111/j.1365-313X.2005.02617.x](https://doi.org/10.1111/j.1365-313X.2005.02617.x). pEarleyGate.
- Nakagawa T *et al.* Development of series of gateway binary vectors, pGWBs, for realizing efficient construction of fusion genes for plant transformation. *J Biosci Bioeng* 104, 34–41 (2007). [doi:10.1263/jbb.104.34](https://doi.org/10.1263/jbb.104.34). pGWB.
- Curtis MD, Grossniklaus U. A Gateway cloning vector set for high-throughput functional analysis of genes in planta. *Plant Physiol* 133, 462–469 (2003). [doi:10.1104/pp.103.027979](https://doi.org/10.1104/pp.103.027979). pMDC.
- Karimi M, Inzé D, Depicker A. GATEWAY vectors for *Agrobacterium*-mediated plant transformation. *Trends Plant Sci* 7, 193–195 (2002). [doi:10.1016/S1360-1385(02)02251-3](https://doi.org/10.1016/S1360-1385(02)02251-3). VIB Ghent vectors (pK7WG2).

**Trademarks.** Gateway® is a registered trademark of Thermo Fisher Scientific. SnapGene® is a trademark of Dotmatics. This is an independent tool, not affiliated with, endorsed by, or derived from any Thermo Fisher or Dotmatics software.

**Software.** No third-party code is bundled. Typefaces (IBM Plex Sans, IBM Plex Sans Condensed, IBM Plex Mono) are served by Google Fonts under the SIL Open Font License.

## License

**Free for academic and other non-commercial use** under the [PolyForm Noncommercial License 1.0.0](LICENSE).

In plain terms (the licence text is what counts): you may use, copy, modify and share this software for research, teaching, study and any other non-commercial purpose, including at universities, hospitals, non-profit research institutes and government laboratories. **Commercial use**, such as use by or for a company, in industry R&D, contract research or a paid service, is not covered and needs a separate licence. Contact [contact@lao-labtools.org](mailto:contact@lao-labtools.org).

Earlier versions of this repository were released under the MIT licence. Copies obtained under those terms keep them; the PolyForm Noncommercial licence applies from November 2026 onward.

## Disclaimer

This is an independent research tool. It is provided as is, without warranty of any kind. Cross-check key numbers against established software before you publish them. It is for research use only. It is not a medical device and must not be used for diagnostic or clinical decisions.

---

Part of **[Lao LabTools](https://lao-labtools.org)**: free, install-free browser tools for the bench, by Linjiang Lao.
