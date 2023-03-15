# E-Fishing Exploratory Data Analysis

### Preliminary Findings
* some numerical data stored as ranges

##### Biological Data
* a few datatypes incorrectly imported due to inconsistent data
  * GNS: WEIGHT
  * MAR: FORK_LENGTH, TOTAL_LENGTH
* some columns different between GNS and MAR
  * missing from GNS: 'CGNDB', 'SPECIES_ITIS_CODE', 'FORK_LENGTH_INTERVAL_WIDTH'
  * missing from MAR: 'RIVER_NAME'
* some columns mostly unused (or completely empty)
  * GNS (12895 total entries): 'TOTAL_LENGTH' (5), 'MATURITY' (23)
  * MAR (44472 total entries): 'SPECIES_ITIS_CODE' (0), 'MATURITY' (36)
