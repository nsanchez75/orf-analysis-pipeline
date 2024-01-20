# ORF Analysis Pipeline

## Purpose of the Pipeline

<!-- TODO: add explanation and goal of pipeline -->
<!-- perform ETL on sequence ORF data -->

## Dealing with Dependencies

<!-- TODO: add details for installing/running conda and setting up the environment -->

## Process 0: Input Data Validation

<!-- TODO: work on this -->

## Process 1a: Produce BLASTp Data

### Input(s)

- ORF FASTA - file
- AF-Uniprot FASTA - file
- Species Name - string

### Output

- BLASTp output file (tabularized)

### Procedure

1. Run `makeblastdb` on ORF file to produce a BLAST database
2. Run `blastp`. The argument specifics are as follows:
    - Query File (-query): AF-Uniprot file
    - Database (-db): BLAST database created in Step 1
    - Output (-out): `{species name}_AF-on-ORF_blastp-output.txt`

## Process 1b: Append Other Identifiers (NCBI, Kyle's Naming Method, etc.)

### Input(s)

- BLASTp table - procedure 1a's output file
- ID(s) - file(s)

### Output

- overwritten BLASTp output file with IDs appended to it

### Procedure

1. Repeat following steps for each ID file
2. Convert the ID file into column(s) and tabularize the BLASTp file
3. Identify a primary key that will help associate new IDs to the BLASTp output (manually identify?)
4. Append the ID column(s) to the BLASTp table on the right side using the identified primary key to sort the rows
5. After iterating through each file, update the output file

## Process 2aa: Run ORFs on EffectorO

### Input(s)

- ORF FASTA - file

### Output

- EffectorO FASTA result - file
  <!-- - TODO: identify where the FASTA result is created -->

### Procedure

1. Run EffectorO's `predict_effectors.py` on the input ORF file

### Dependencies

- Python 3.6 (this is to ensure that pickle works properly)
- pickle
- scikit-learn
<!-- - TODO: continue adding dependencies -->

## Process 2ab: Identify Known Effectors + Non-Effectors

### Input(s)

- ORF FASTA - file
- Category HMMs - directory containing .hmm files

### Output

- 'seq_categorization_results' - directory containing list of sequences per categorization

### Procedure

1. Run each .hmm file from the Category HMMs directory on the following steps
2. Run `hmmsearch` using the provided .hmm file onto the input FASTA file
3. The output of `hmmsearch` will produce the list of sequences from the FASTA file that are predicted to be of the specified .hmm file's categorization
4. The list will be stored in a text file underneath the 'seq_categorization_results' directory
<!-- TODO: add species name to 'seq_...' directory -->

### Dependencies

- HMMer (ideally v3.1b2+)

### Extra Notes

- Processes 2aa and 2ab will run concurrently
  - Process 2b waits for both to complete

## Process 2b: Performing Analysis on EffectorO

<!-- TODO: refactor R script into something more useful -->

## Notes

- Processes 1, 2, and 3 can be run concurrently
  - Process 4 waits for all of them to complete
