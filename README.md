# E-Fishing Exploratory Data Analysis

This study is comparing and investigating historical data from Margaree and GNS Electrofishing Data.

## EDA Phase 1

### Biological Data

* Dataframe names:
  * df_mar
  * df_gns
* There are large amounts of null data in many feature columns:

<img src="https://user-images.githubusercontent.com/94803263/226928420-a3959eba-b08d-4b66-a077-89e7b9edfd91.png" width="400" />

* There are a few columns with data inconsistent with their datatypes
  * GNS: WEIGHT
  * MAR: FORK_LENGTH, TOTAL_LENGTH
* Some columns different are between GNS and MAR spreadsheets
  * missing from GNS: 'CGNDB', 'SPECIES_ITIS_CODE', 'FORK_LENGTH_INTERVAL_WIDTH'
  * missing from MAR: 'RIVER_NAME'
* Some columns are mostly unused (or completely empty)
  * GNS (12895 total entries): 'TOTAL_LENGTH' (5 entries), 'MATURITY' (23 entries)
  * MAR (44472 total entries): 'SPECIES_ITIS_CODE' (0 entries), 'MATURITY' (36 entries)
* A few data points appear to be inputted incorrectly or inconsistently:
  * typo:
    * df_gns['WEIGHT']=='4..14'
  * ranges in place of numeric: 
    * ~df_mar['FORK_LENGTH'].astype(str).str.replace('.','',1,regex=False).str.isnumeric()
    * ~df_mar['TOTAL_LENGTH'].astype(str).str.replace('.','',1,regex=False).str.isnumeric()
  * same categories typed differently:

<img src="https://user-images.githubusercontent.com/94803263/226931204-efd68916-d986-4f55-b25a-f4b07e651d46.png" width="800" />
<img src="https://user-images.githubusercontent.com/94803263/226931428-4cebdc6f-a069-4437-89d7-e00623673c3e.png" width="800" />
<img src="(https://user-images.githubusercontent.com/94803263/226931534-f1654068-c859-46b1-9bfc-7045cd1c6ca7.png" width="800" />

  * some distributions with likely outliers at the extremes:
  
<img src="https://user-images.githubusercontent.com/94803263/226931825-8886b3f5-3364-4f36-a0a1-f8724f40d1b1.png" width="800" />

### Code Data

