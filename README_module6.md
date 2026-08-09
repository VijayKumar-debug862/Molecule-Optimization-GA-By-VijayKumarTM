# Evolutionary Molecule Optimization Using Genetic Algorithms

A cheminformatics research project implementing a genetic algorithm (GA) for de novo molecular optimization — evolving populations of drug-like molecules toward improved pharmacological properties, validated against a random search baseline.

**Author:** Vijay Kumar T M
**Institution:** Nrupathunga University, Bengaluru
**Domain:** Cheminformatics | Computational Drug Discovery | Evolutionary Algorithms

## Overview

Starting from a small set of known drugs (Aspirin, Ibuprofen, Paracetamol) and simple chemical fragments, this project applies biologically-inspired operators — selection, crossover, and mutation — to iteratively generate and refine candidate molecules over successive generations, without training any predictive model.

Fitness is evaluated using a multi-objective function combining:
- **QED** (Quantitative Estimate of Drug-likeness)
- **LogP** within a favorable range for oral bioavailability
- A **synthetic accessibility proxy** penalizing overly complex structures

## Methods

- **Molecular representation:** RDKit `Mol` objects, manipulated via `RWMol` for atom-level edits
- **Mutation:** Random atom swaps and fragment additions from a small chemical fragment library
- **Crossover:** BRICS-based fragment recombination between two parent molecules
- **Selection:** Tournament selection with elitism (top performers preserved each generation)
- **Diversity tracking:** Tanimoto dissimilarity across the population, monitored each generation to detect premature convergence
- **Validation:** GA performance compared against a random search baseline under an identical evaluation budget, across 3 independent runs, with mean/standard deviation reported for statistical grounding

## Results

| Method | Mean Best Fitness | Std Dev |
|---|---|---|
| Genetic Algorithm | 0.938 | ± 0.022 |
| Random Search | 0.916 | ± 0.013 |

The GA outperformed random search in all 3 independent runs, indicating a consistent, real advantage from selection pressure and crossover rather than favorable random chance.

## Limitations

Population diversity dropped to 0.000 (near-identical molecules) by generation 1 in every run — a case of **premature convergence**, a well-documented failure mode in genetic algorithms. This likely stems from a small mutation fragment library and BRICS crossover recombining an already-narrow set of structural motifs, compounded by elitism anchoring the population to early high-scorers. Despite this, the GA still meaningfully outperformed random search, suggesting selection pressure contributed real optimization power even with limited structural exploration. Addressing this in future work would involve a larger, more chemically diverse fragment library and explicit diversity-preservation mechanisms (e.g. niching or island models).

## Notebook structure

1. Setup & seed molecules
2. Fitness function (QED-based, then extended to multi-objective)
3. Genetic operators — mutation and crossover
4. Evolutionary loop (single-objective)
5. Multi-objective fitness with diversity tracking
6. Baseline comparison — GA vs. random search (single run)
7. Multi-run statistical comparison
8. Results, limitations, and discussion

## How to run

Open `Molecule_Optimization_GA_VijayKumarTM.ipynb` in Google Colab and run all cells in order. RDKit is installed in the first code cell — no other setup required.

## Relevance

This approach mirrors real-world computational drug design techniques used in de novo molecule generation and lead optimization, complementing predictive ADMET models (as developed in my earlier [ADMET prediction project](../admet-property-prediction)) by generating new candidate structures rather than only scoring existing ones.
