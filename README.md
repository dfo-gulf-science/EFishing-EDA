# E-Fishing Exploratory Data Analysis

This study is comparing and investigating historical data from Margaree and GNS Electrofishing Data.

## Exploratory Data Analysis: Phase 1

In this phase of the assessment, the data was imported, plotted, explored, and datatype inconsistencies were cleaned and flagged. The formatted and flagged dataframes were saved to be investigated and cleaned further in phase 2 of the EDA.

Note: In cases when data has been imputed and flagged, it is recommended that methods be considered in detail. For example, a large number of numerical data points have been inputted as a range of values. Imputation asssumed that an average is appropriate. However, minimum or maximum values may be more appropriate, depending on the calculations to be performed with the data. Alternatively, separating the data into separate features could preserve more nuance, at the expense of a more complex dataset.

### Biological Data

* Dataframe names:
  * df_gns
  * df_mar
* There are large amounts of null data in many feature columns:
<img src="https://user-images.githubusercontent.com/94803263/226928420-a3959eba-b08d-4b66-a077-89e7b9edfd91.png" width="350" />

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
    * converted to 4.14 and flagged
  * ranges in place of numeric: 
    * ~df_mar['FORK_LENGTH'].astype(str).str.replace('.','',1,regex=False).str.isnumeric()
    * ~df_mar['TOTAL_LENGTH'].astype(str).str.replace('.','',1,regex=False).str.isnumeric()
    * converted to averages and flagged
  * same categories typed differently:
<img src="https://user-images.githubusercontent.com/94803263/226931204-efd68916-d986-4f55-b25a-f4b07e651d46.png" width="800" />
<img src="https://user-images.githubusercontent.com/94803263/226931428-4cebdc6f-a069-4437-89d7-e00623673c3e.png" width="800" />
<img src="(https://user-images.githubusercontent.com/94803263/226931534-f1654068-c859-46b1-9bfc-7045cd1c6ca7.png" width="800" />

  * some distributions with likely outliers at the extremes:
<img src="https://user-images.githubusercontent.com/94803263/226931825-8886b3f5-3364-4f36-a0a1-f8724f40d1b1.png" width="800" />

### Code Data

* Margaree and GNS spreadsheets have the exact same data:
  * in 'Code' and 'Code, species' sheets, respectively
* There are many duplicated codes and descriptions
  * e.g., the Label "1113000 - MIRAMICHI RIVER" occurs twice, once in 'River' and once in 'Site Data'
  * even more duplicate if alternate spellings and subtle wording changes are considered
  * significant cleanup is possible, but implementation will depend on how the data are used in analysis and reporting

### Site Data

* Dataframe names:
  * df_gsite
  * df_msite
* There are large numbers of completely unused columns:
<img src="https://user-images.githubusercontent.com/94803263/226938598-c390342e-5ef3-4163-a459-1b0937b561b1.png" width="400" />

* Many data points appear to be inputted incorrectly or inconsistently:
  * GNS:
    * numeric column inputted as ranges:
      * ELECTROFISHER_CURRENT
      * data flagged and averages imputed 
    * WIDTH_LOWER == '~6'
      * flagged and imputed as 6
    * SUB_TYPE_FINES == 'tr', SUB_TYPE_FINES == 'present -100'
      * flagged and imputed as 0
    * many numeric columns with '.' entry:
      * DEPTHA1, DEPTHA2, DEPTHA3, DEPTHB1, DEPTHB2, DEPTHB3, DEPTHC1, DEPTHC2, DEPTHC3, ELECTROFISHER_CURRENT, SUB_TYPE_BEDROCK, SUB_TYPE_BOULDER, SUB_TYPE_COBBLE, SUB_TYPE_FINES, SUB_TYPE_GRAVEL, SUB_TYPE_PEBBLE, SUB_TYPE_ROCKS, SUB_TYPE_SAND, TOS1, TOS2, TOS3, TOS4, WATER_PH, WATER_TEMPERATURE_ARRIVAL, WIDTH_LOWER, WIDTH_MIDDLE
      * data flagged and '.' converted to NULL
  * Margaree:
    * numeric columns inputted as ranges:
      * AIR_TEMPERATURE, WATER_TEMPERATURE_ARRIVAL, WATER_PH, WATER_CONDUCTIVITY, ELECTROFISHER_CURRENT, ELECTROFISHER_VOLTAGE
      * data flagged and averages imputed 
    * R_BK_OVERHANGING_VEG == 'upstream 80%'
      * flagged and imputed as 80
    * MAX_OVERHANG_R_BK == ',,5'
      * flagged and converted to NULL
    
