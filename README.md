# Visual Traffic Regulation Reasoning Dataset

A benchmark for **image-based traffic-regulation applicability inference** using a traffic-scene taxonomy and manually annotated real-world driving images. The benchmark is associated with the paper *Image-Based Traffic Regulation Applicability Inference via Taxonomy-Guided Visual-Semantic Alignment and Learnable Prerequisite Aggregation*.

## Overview

- **600** forward-facing driving images selected from three public autonomous-driving datasets: **DAIR-V2X (134)**, **ONCE (179)**, and **SODA10M (287)**.
- A hierarchical traffic-scene taxonomy with **119 leaf-level scene elements** and an English-language semantic prompt for each element.
- Per-image scene-element annotations and metadata linking examples to their source datasets and original sequences.
- Group-aware dataset partition information for evaluation, stored in the metadata spreadsheet.
- A separate declaration of source-derived mutual-exclusion groups and related semantic constraints.
- The associated study considers **198 regulation entries**. The rule inventory and regulation-to-element prerequisite mappings should be distinguished from the image annotations and taxonomy; see the **Regulation knowledge-base availability** note below.

The repository **does not redistribute any original images**. It supplies research annotations and metadata referring to images obtainable from the original providers.

## Repository files

| File | Purpose | Notes |
| --- | --- | --- |
| [`metadata.xlsx`](metadata.xlsx) | Dataset metadata, source dataset information, original sequence/session identifiers, and dataset grouping/partition information. | Use this file to relate samples to their source sequences and to inspect the supplied evaluation groups or splits. Consult the workbook's actual column headers for field names. |
| [`original_data-EN.json`](original_data-EN.json) | Per-image annotation data. | Contains the image-level annotation records. Consult the JSON object structure for specific keys and label representation; original image pixels are not included. |
| [`runtime_prompts_119-EN.json`](runtime_prompts_119-EN.json) | English-language semantic prompts for the 119 scene elements. | A JSON mapping from canonical English scene-element names to their descriptive text prompts. |
| [`tag_tree-EN.json`](tag_tree-EN.json) | Complete hierarchical traffic-scene taxonomy. | Defines the tree/category structure, including the leaf-level element names used in the benchmark. |
| [`mutual_exclusion_groups.json`](mutual_exclusion_groups.json) | Source-derived mutually exclusive element groups and related conditions. | Includes stable group identifiers, canonical English labels when present in the 119-element prompt vocabulary, original Chinese source labels, and separately described conditional/hierarchical constraints. |
| [`README.md`](README.md) | Dataset overview, file descriptions, and usage/license notes. | This document. |

## How the files relate

1. **Identify the image and its provenance** using `metadata.xlsx`. The source dataset and sequence/group identifiers are important when constructing partitions that avoid overlap between related frames.
2. **Read the image annotations** in `original_data-EN.json` for the image-level scene-element labels.
3. **Interpret the annotation vocabulary** with `tag_tree-EN.json`, which provides the hierarchy, and `runtime_prompts_119-EN.json`, which provides prompts for the 119 English scene-element names.
4. **Inspect semantic constraints** using `mutual_exclusion_groups.json`. Its `mutually_exclusive_groups` entries list the groups configured in the supplied reasoning script. Conditional absence evidence and nested predicates are placed separately under `related_conditional_and_hierarchical_constraints` rather than being incorrectly described as flat mutual-exclusion groups.

### Important note on group semantics

The mutual-exclusion declaration was extracted from the source script's probability-refinement logic and translated to the English label vocabulary where possible. Some source-script members, notably specific numerical speed-limit sign classes, are **not present as separate nodes in the published 119-element prompt vocabulary**. Such members are marked `source_only_not_in_119` rather than being treated as additional public labels. Some configured groups can also contain concepts that coexist in real road scenes; consequently, group membership should be understood as **a modeling/configuration choice**, not a universal statement of legal or semantic incompatibility.

`mutual_exclusion_groups.json` documents group membership and relationships; it is **not an executable implementation** of the complete perception or aggregation pipeline.

## Annotation and evaluation context

Three graduate researchers specializing in traffic-law research independently annotated regulation prerequisites and image-level scene elements, with disagreements resolved through discussion; difficult legal interpretation questions were further checked with traffic-safety experts. In the associated study, image-level regulation-applicability labels were derived from consensus scene-element labels and predefined regulation–element mappings. Dataset images were selected to cover varied road segments and scenarios, and severely occluded images that prevented reliable annotation were excluded.

The experimental protocol uses group-aware cross-validation to reduce leakage between images originating from the same sequence/session. **Use the supplied group identifiers rather than randomly splitting individual frames.** More detailed annotation and evaluation procedures are described in the accompanying paper.

## Regulation knowledge-base availability

The accompanying study uses **198 regulation entries** based on Chinese traffic regulations applicable in mainland China, including the Road Traffic Safety Law of the People's Republic of China, its Implementation Regulations, and the GB 5768 Road Traffic Signs and Markings standards. The taxonomy, scene-element annotations, and semantic prompts in this repository are **not themselves a substitute for the complete regulation list or the explicit regulation–element prerequisite mapping table**.

The current repository root does **not** list a standalone file containing the full regulation inventory or the regulation–element prerequisite mappings. If these resources are already embedded within `original_data-EN.json`, please document their exact keys here; otherwise, they must be released separately to enable independent reconstruction of regulation-applicability labels. This README does not claim that `mutual_exclusion_groups.json` provides these mappings.

## Obtaining source images and licensing

The benchmark refers to images from **DAIR-V2X, ONCE, and SODA10M**. The original images remain subject to their respective providers' terms. This repository distributes source identifiers/references and additional annotation assets, **not** copies of the source images. Obtain the images from the official providers and comply with the relevant conditions before using or redistributing them.

- **DAIR-V2X:** [Official dataset page](https://air.tsinghua.edu.cn/DAIR-V2X/english/index.html). Consult the provider's current data-use agreement.
- **ONCE:** [Official dataset/download page](https://once-for-auto-driving.github.io/download.html#downloads) and [terms of use](https://once-for-auto-driving.github.io/terms_of_use.html).
- **SODA10M:** [Official dataset/download page](https://soda-2d.github.io/download.html#instructions) and [dataset information](https://soda-2d.github.io/about.html).

The previous repository documentation describes ONCE and SODA10M as using **CC BY-NC-SA 4.0** with additional dataset-specific restrictions, including limitations on distributing the original images. Users should consult each provider's current terms; this README does not grant any rights over the original image datasets. No independent blanket license for the newly contributed repository files is implied by these third-party dataset licenses.

## Citation

If using this benchmark, please cite the accompanying manuscript when its bibliographic details are available. Also cite the source dataset(s) whose images you use, in accordance with their terms and citation instructions.
