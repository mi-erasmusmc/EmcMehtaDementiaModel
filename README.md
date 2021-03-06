Dementia model validation study - Mehta Model
=============
<img src="https://img.shields.io/badge/Study%20Status-Design%20Finalized-brightgreen.svg" alt="Study Status: Design Finalized"> 

- Analytics use case(s): **Patient-Level Prediction**
- Study type: **Clinical Application**
- Tags: **EHDEN, OHDSI**
- Study lead: **Henrik John**
- Study lead forums tag: **[EHDEN:lhjohn](https://forum.ehden.eu/u/lhjohn), [ODSI:lhjohn](https://forums.ohdsi.org/u/lhjohn)**
- Study start date: **2020-10-01**
- Study end date: **2021-03-31**
- Protocol: **[mi-erasmusmc/EmcDementiaModelValidation](https://github.com/mi-erasmusmc/EmcDementiaModelValidation)**
- Publications: **Coming Soon**
- Results explorer: **Coming Soon**

The objective of this study is to perform an external validation of three prognostic dementia models the using OMOP CDM data. This package replicates the model by [Mehta](https://content.iospress.com/articles/journal-of-alzheimers-disease/jad150466).

Instructions To Install and Run Package From Github
===================

- Make sure you have PatientLevelPrediction installed (this requires having Java and the OHDSI FeatureExtraction R package installed):

```r
  # get the latest PatientLevelPrediction
  install.packages("devtools")
  devtools::install_github("OHDSI/PatientLevelPrediction", ref = 'development')
  # check the package
  PatientLevelPrediction::checkPlpInstallation()
```

- Then install the study package:
```r
  # install the network package
  devtools::install_github("mi-erasmusmc/EmcMehtaDementiaModel")
```

- Execute the study by running the code in (extras/CodeToRun.R) but make sure to edit the settings:
```r
library(EmcMehtaDementiaModel)
# USER INPUTS
#=======================
# The folder where the study intermediate and result files will be written:
outputFolder <- "./EmcMehtaDementiaModelResults"

# Details for connecting to the server:
dbms <- "you dbms"
user <- 'your username'
pw <- 'your password'
server <- 'your server'
port <- 'your port'

connectionDetails <- DatabaseConnector::createConnectionDetails(dbms = dbms,
                                                                server = server,
                                                                user = user,
                                                                password = pw,
                                                                port = port)

# Add the database containing the OMOP CDM data
cdmDatabaseSchema <- 'cdm database schema'
# Add a database with read/write access as this is where the cohorts will be generated
cohortDatabaseSchema <- 'work database schema'
# Add the database name
cdmDatabaseName <- 'friendly database name'

oracleTempSchema <- NULL

# table name where the cohorts will be generated
cohortTable <- 'EmcMehtaDementiaModelCohort'

#=== Do not edit the following settings of the CodeToRun.R file 

# TAR settings (will be ignored, refer to the study protocol for final settings)
sampleSize <- NULL
riskWindowStart <- 1
startAnchor <- 'cohort start'
riskWindowEnd <- 365
endAnchor <- 'cohort start'
firstExposureOnly <- F
removeSubjectsWithPriorOutcome <- F
priorOutcomeLookback <- 99999
requireTimeAtRisk <- F
minTimeAtRisk <- 1
includeAllOutcomes <- T


#=======================

EmcMehtaDementiaModel::execute(connectionDetails = connectionDetails,
                               cdmDatabaseSchema = cdmDatabaseSchema,
                               cdmDatabaseName = cdmDatabaseName,
                               cohortDatabaseSchema = cohortDatabaseSchema,
                               cohortTable = cohortTable,
                               sampleSize = sampleSize,
                               riskWindowStart = riskWindowStart,
                               startAnchor = startAnchor,
                               riskWindowEnd = riskWindowEnd,
                               endAnchor = endAnchor,
                               firstExposureOnly = firstExposureOnly,
                               removeSubjectsWithPriorOutcome = removeSubjectsWithPriorOutcome,
                               priorOutcomeLookback = priorOutcomeLookback,
                               requireTimeAtRisk = requireTimeAtRisk,
                               minTimeAtRisk = minTimeAtRisk,
                               includeAllOutcomes = includeAllOutcomes,
                               outputFolder = outputFolder,
                               createCohorts = T,
                               runAnalyses = T,
                               viewShiny = T,
                               packageResults = T,
                               minCellCount= 5,
                               verbosity = "INFO",
                               cdmVersion = 5)
```


# Output
After running the code go to the location you specified as `outputFolder`. Inside this location you should see one folder with a name as specified by `cdmDatabaseName`. In addition there should be a zip file inside this folder. This zipped file is the study result to be shared. We recommend that you inspect the files before sending. They will contain various csv files that can be opened and inspected.

# Development status
2021-03-01 Release v1.0