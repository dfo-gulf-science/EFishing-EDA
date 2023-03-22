# E-Fishing Exploratory Data Analysis

This study is comparing and investigating historical data from Margaree and GNS Electrofishing datasets.

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
  * some single categories have been inputted with multiple different names:
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
* some distributions with likely outliers at the extremes:
<img src="https://user-images.githubusercontent.com/94803263/226946163-95eede52-9b62-4f11-b679-7af60e76254d.png" width="800" />
<img src="https://user-images.githubusercontent.com/94803263/226946611-6c7b8510-65d5-4171-ab78-bd542e0f019c.png" width="800" />

## Exploratory Data Analysis: Phase 2

In this phase of the assessment, the data from phase 1 was imported, and additional data exploration was completed based on plots and data from phase 1.

Note: Issues previously flagged in phase 1 may not may not be mentioned again in this summary, but are often investigated in more detail in the phase 2 jupyter notebook.

### Biological Data

* There are only M and U entries indf_gns.SEX, no entries for F. 
  * there is no indication that this is an error, but it is worth confirming
* In both df_mar and df_gns, MATURITY is almost never used, and always "P" when it is used

##### GNS

* Probable outliers:
  * df_gns.FORK_LENGTH > 500
  <div>
    <img src="https://user-images.githubusercontent.com/94803263/226979288-a3c304f8-d44d-40e2-9140-99684926b94e.png" width="400" />
    <img src="https://user-images.githubusercontent.com/94803263/226976520-f42e0ad3-088c-40c8-a2a9-e7927947b7e2.png" width="400" />
  </div>

  * (df_gns.FORK_LENGTH<20) & (df_gns.WEIGHT>3) mightly be off by 10x
    * not clear, slightly better fit: if scaling these, please proceed with caution
  <img src="https://user-images.githubusercontent.com/94803263/226978884-fc0200e9-9543-42b9-a9ad-92fb762fb97c.png" width="500" />

  * still some likely data entry issues after these fixes. either leave as is, or investigate on case by case basis.

##### Margaree

* Probable outliers:
  * df_mar.WEIGHT > 10000
  * df_mar.FORK_LENGTH > 600
  <div>
    <img src="https://user-images.githubusercontent.com/94803263/226980775-5589f850-978e-4fd5-8d5a-f2224d848471.png" width="400" />
    <img src="https://user-images.githubusercontent.com/94803263/226980810-4946b9bc-783d-4cbc-a44c-dc7d4de3b883.png" width="400" />
  </div>


### Site Data

* ELECTROFISHER_TYPE inputted with multiple different names for the same category:
  * ELECTROFISHER_TYPE == "LR24" and "LR 24"
* Probable outliers:

  * df_gsite.LENGTH_RIGHT_BANK = 194 
    * LENGTH_LEFT_BANK = 19.4 for this entry
    * Recommend scaling by 1/10x
    <img src="https://user-images.githubusercontent.com/94803263/226988427-6f3eaa05-57f4-472a-bb7b-49277c89554b.png" width="500" />

  * df_msite.WATER_PH = 11.9
* The sum of all SUB_TYPE_* columns should add to 100.
  * In most cases, this is indeed the case
  * In a few cases numbers either close to 100 or NULL occur
    * In cases with errors, they appear to be estimation uncertainty, with no obvious errors to correct.
    * In these cases, it is recommended that the values be left as is unless analysis requires properly normalised columns.

* df_msite.DEPTHA3 has a large number of low value outliers
  <div>
    <img src="https://user-images.githubusercontent.com/94803263/226984645-7f4cf10b-1730-448a-aa11-d5a735ee611b.png" width="400" />
    <img src="https://user-images.githubusercontent.com/94803263/226984864-75bb040a-b049-4d42-8a7b-7e6c6c45052f.png" width="400" />
  </div>

  * a large number of DEPTHA3 values are below 5, although they aren't 0
  <img src="https://user-images.githubusercontent.com/94803263/226985153-118288ce-a0c4-47ff-897f-2c9101173b27.png" width="600" />
  <img src="https://user-images.githubusercontent.com/94803263/226985192-b67ab07c-0830-4b07-9535-bf921443609b.png" width="600" />

  * comparing to DEPTHA1 and DEPTHA2, a cluster of these data appear to be off by a factor of 10x
  <img src="https://user-images.githubusercontent.com/94803263/226986212-350996e5-a133-488a-8fb3-6edeefcc2d55.png" width="800" />
  
  * scaling problem values by 10x leads to a much better fit with other data
  <img src="https://user-images.githubusercontent.com/94803263/226989838-57ed7087-80f1-4bfd-bcff-de90c0fdcb3b.png" width="800" />

  * NOTE: DEPTHA3 is supposed to be in CM. If these data need to be scaled up by a factor of 10, that implies that the data were accidentally inputted in decimeters, which does not seem likely. There may be a better explanation for this data issue. 
  * RECOMMENDATION: (df_msite.DEPTHA3 < 5) & (df_msite.year < 1980) should likely be scaled by 10x, but proceed with caution.

