# Step 1: Load packages and create folders

**Code:** install.packages(c("rentrez", "ggplot2"))
library(rentrez)
library(ggplot2)
dir.create("data", showWarnings = FALSE)
dir.create("figures", showWarnings = FALSE)
dir.create("results", showWarnings = FALSE)
packageVersion("rentrez")
packageVersion("ggplot2")
list.files()

<img width="1360" height="641" alt="step01_load_packages" src="https://github.com/user-attachments/assets/5105fb2f-e705-4315-926c-4f4de5a494bf" />

*Figure 1.*
This step installs and loads the rentrez package, which accesses NCBI, and the ggplot2 package, which makes plots. It also creates the data, figures and results folders to keep the files organized. The version checks and file listing confirm that everything loaded and the folders exist.

# Step 2: Search NCBI

**Code:** accession <- "NP_001899.1"   # change to your enzyme
hit <- entrez_search(db = "protein", term = paste0(accession, "[ACCN]"))
print(hit)
hit$ids
hit$count

<img width="1356" height="640" alt="step02_ncbi_search" src="https://github.com/user-attachments/assets/d5299bb0-00d7-4026-8cfd-f5ba2a33bb1b" />

*Figure 2.*
This step stores the protein accession number and uses entrez_search() to look it up in the NCBI Protein database. The output gives the number of matching records and the NCBI ID, which confirms the protein exists before downloading it.

# Step 3: Download the FASTA

**Code:** fasta_txt <- entrez_fetch(db = "protein", id = hit$ids,
                          rettype = "fasta", retmode = "text")
writeLines(fasta_txt, "data/CTSB_NP_001899.1.fasta")
cat(substr(fasta_txt, 1, 300), "\n")
file.exists("data/CTSB_NP_001899.1.fasta")
file.info("data/CTSB_NP_001899.1.fasta")$size

<img width="1360" height="640" alt="step03_download_fasta" src="https://github.com/user-attachments/assets/07641064-d437-4c62-bfad-4107ba6deaea" />

*Figure 3.*
This step uses entrez_fetch() to download the protein sequence in FASTA format directly from NCBI through R. The sequence is saved in the data folder with writeLines(). Printing the first lines and checking file.exists() confirm the file was downloaded and saved.

# Step 4: Read the file and get the length

**Code:** lines <- readLines("data/CTSB_NP_001899.1.fasta")
header <- lines[1]
seq <- paste(lines[-1], collapse = "")
header
nchar(seq)
substr(seq, 1, 60)

<img width="1353" height="636" alt="step04" src="https://github.com/user-attachments/assets/b09b6510-bd38-4dd2-b2e2-c46c81db128e" />

*Figure 4.*
This step reads the saved FASTA file back into R with readLines(). The first line is stored as the header, and the remaining lines are joined into one sequence string. nchar() counts the characters to give the protein length, and substr() shows the beginning of the sequence.

# Step 5: Amino acid composition

**Code:** aa <- strsplit(seq, "")[[1]]
comp <- as.data.frame(table(aa), stringsAsFactors = FALSE)
colnames(comp) <- c("AminoAcid", "Count")
comp$Percent <- round(100 * comp$Count / sum(comp$Count), 2)
comp <- comp[order(-comp$Count), ]
write.csv(comp, "results/amino_acid_composition.csv", row.names = FALSE)
print(comp)
cat("Most abundant:", comp$AminoAcid[1], "\n")
cat("Least abundant:", comp$AminoAcid[nrow(comp)], "\n")
file.exists("results/amino_acid_composition.csv")

<img width="1354" height="638" alt="step05" src="https://github.com/user-attachments/assets/59afcec5-0949-42b0-bee4-203d0d022b37" />

*Figure 5.*
This step splits the sequence into single amino acids and counts each one with table(). It calculates the percentage of each, sorts them from most to least abundant, and saves the table as a CSV file in results. The most and least abundant amino acids are printed from the top and bottom of the table.

# Step 6: Composition plot

**Code:** p1 <- ggplot(comp, aes(reorder(AminoAcid, -Count), Count)) +
  geom_col(fill = "steelblue") +
  labs(title = "Amino Acid Composition", x = "Amino acid", y = "Count") +
  theme_minimal()
print(p1)
ggsave("figures/amino_acid_composition.png", p1, width = 8, height = 4)
file.exists("figures/amino_acid_composition.png")

<img width="2400" height="1350" alt="CTSB_amino_acid_composition" src="https://github.com/user-attachments/assets/e6ebd814-8ec1-4c7a-b5cc-3ad2970c1147" />

*Figure 6.*
This step creates a bar plot showing the amino acid counts in the CTSB protein using ggplot2. Glycine (G) has the highest count, while methionine (M) has the lowest. The plot is displayed in RStudio and saved as a PNG file in the figures folder.

# Step 7: N-glycosylation motifs

**Code:** m <- gregexpr("(?=(N[^P][ST]))", seq, perl = TRUE)[[1]]
pos <- as.integer(m)
pos <- pos[pos > 0]
motifs <- data.frame(Position = pos, Motif = substring(seq, pos, pos + 2))
write.csv(motifs, "results/NGlyc_motifs.csv", row.names = FALSE)
print(motifs)
cat("Number of motifs:", nrow(motifs), "\n")
file.exists("results/NGlyc_motifs.csv")

<img width="1357" height="643" alt="step07" src="https://github.com/user-attachments/assets/e6166ac0-827b-4120-a3c4-854ad532fce7" />

*Figure 7.*
This step uses a regular expression to search the sequence for the N-X-S/T motif, where X is any amino acid except proline. It records the position and sequence of each match in a table, saves it as a CSV file in results, and prints the number of motifs found. These are potential sites where sugar groups can attach to the protein.

# Step 8: Motif plot

**Code:** p2 <- ggplot(motifs, aes(Position, 1)) +
  geom_segment(aes(x = 1, xend = nchar(seq), y = 1, yend = 1),
               linewidth = 2, color = "grey70") +
  geom_point(size = 4, color = "red") +
  geom_text(aes(label = paste0(Motif, " (", Position, ")")),
            vjust = -1.2, size = 3) +
  scale_y_continuous(limits = c(0.8, 1.5)) +
  labs(title = "Potential N-glycosylation motifs",
       x = "Residue position", y = NULL) +
  theme_minimal() + theme(axis.text.y = element_blank())
print(p2)
ggsave("figures/NGlyc_motifs.png", p2, width = 8, height = 2.5)
file.exists("figures/NGlyc_motifs.png")

<img width="2400" height="990" alt="CTSB_potential_N_glycosylation" src="https://github.com/user-attachments/assets/7d81dae5-ce33-435c-818c-fdc8fb42a085" />

*Figure 8.*
This step plots the motif positions along the full length of the protein. A grey line represents the sequence, and red points with labels mark each potential glycosylation site. The plot is displayed in R and saved as a PNG in the figures folder.

# Step 9: summary csv

**Code:** # Build the summary table (same columns as the sample)
summary_df <- data.frame(
  gene = "CTSB",
  accession = accession,
  length_aa = nchar(seq),
  potential_N_glycosylation_motifs = nrow(motifs)
)
print(summary_df)
Save it as a CSV
write.csv(summary_df, "results/CTSB_summary.csv", row.names = FALSE)
file.exists("results/CTSB_summary.csv")

<img width="1358" height="640" alt="step09" src="https://github.com/user-attachments/assets/a4c6b6b9-00cf-4d7a-a553-edb57113b5ae" />

*Figure 9.*
This step creates a summary table of the main CTSB results, with one row containing the gene symbol, NCBI accession, protein length in amino acids, and the number of potential N-glycosylation motifs. The table is saved as CTSB_summary.csv in the results folder.
