# Exploring Lysosomal Enzymes Using NCBI and R

# Protein Overview

- **Gene Symbol:** CTSB
- **Enzyme Name:** Cathepsin B (lysosomal cysteine protease)
- **NCBI Protein Record:** NP_001899.1
- **Brief Function Description:** Cathepsin B is a lysosomal enzyme that breaks down proteins and peptides into smaller components for recycling. It is produced as an inactive precursor and processed into its active form, which contains heavy and light chains linked by a disulfide bond. It also participates in processing other proteins, including amyloid precursor protein, and has been implicated in the cleavage of the SARS-CoV-2 spike protein during viral entry.

# Figures and Description

# - Amino Acid Composition

**Figure 1:** [View CTSB Amino Acid Composition Plot](figures/CTSB_amino_acid_composition.png)

**Description:** A bar plot illustrating the distribution and frequency of each amino acid in the CTSB protein sequence. Glycine (G) is the most abundant amino acid, followed by serine (S), while methionine (M) is the least abundant.

# - Potential N-Glycosylation Motifs

**Figure 2:** [View CTSB Potential N-Glycosylation Motifs](figures/CTSB_potential_N_glycosylation.png)

**Description:** A plot showing the positions of three potential N-glycosylation motifs in the CTSB protein sequence. These motifs follow the consensus sequence N-X-S/T, where X can be any amino acid except proline. The motifs are located approximately near amino acid positions 38, 192, and 289. These sites may serve as locations for N-linked glycosylation, a process in which carbohydrate groups are attached to a protein and can influence its folding, stability, and function.

# Questions

**1. What does your enzyme normally break down inside lysosomes?**
Cathepsin B breaks down proteins and peptides into smaller components inside lysosomes. It acts as both an endopeptidase, which cuts peptide bonds within protein chains, and an exopeptidase, which can remove small peptides from the ends. This helps the cell recycle proteins and remove unwanted or damaged proteins. 

**2. What is the total length of your protein, and which amino acids are most common?**
According to the NCBI protein record NP_001899.1, the CTSB preproprotein contains 339 amino acids. Based on my amino acid composition plot, glycine (G) is the most abundant amino acid, with approximately 35 residues, followed by serine (S), with approximately 27 residues. Methionine (M) is the least abundant, with approximately 7 residues. 

**3. How many potential N-glycosylation motifs did you find? Where are they located?**
I identified three potential N-glycosylation motifs in the CTSB protein sequence. Based on my plot, they are located approximately at amino acid positions 38, 192, and 289. These motifs follow the N-X-S/T pattern, where X can be any amino acid except proline. These positions are potential glycosylation sites, but the sequence analysis alone cannot confirm that sugars are attached to them.

**4. Why can glycosylation matter for lysosomal proteins? Does finding a motif prove the protein is glycosylated?**
Glycosylation can help with protein folding, stability, and transport to the correct cellular location. For many soluble lysosomal enzymes, carbohydrate groups can also be modified with mannose-6-phosphate, which helps direct them to lysosomes. However, finding an N-X-S/T motif does not prove that glycosylation occurs at that site. It also does not confirm the presence of a mannose-6-phosphate tag. Experimental evidence is needed to verify these modifications. 

**5. What could happen in the cell if your enzyme did not work properly?**
If cathepsin B did not work properly, the breakdown and recycling of proteins inside lysosomes could be affected. This may contribute to the accumulation of materials that are not properly degraded and interfere with normal cellular processes. Cathepsin B also participates in processing other proteins, so reduced or altered activity may affect these processes. However, the specific effects would depend on the cause and extent of the enzyme's dysfunction.
