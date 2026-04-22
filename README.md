# Predicting Days 1-5 Maximum Hail Size with GEFS: A Machine Learning Approach

Samantha Harrison and Landon Moeller

EAE 483

Spring 2026

-------------------------------------------------------------------------------------------------------------------------------------------

## Geoscience Problem

Out of the three perils associated with severe thunderstorms (tornadoes, hail, and damaging winds), hail is by far the costliest. Each year, hail losses often exceed $10 billion in the Contiguous United States (CONUS), accounting for more than half of total severe weather losses. Recent analyses indicate that hail can represent 50–80% of insured losses from severe convective storms in the CONUS, with annual severe convective storm insured losses frequently exceeding $50 billion in recent years (Cape Analytics 2023; Insurance Information Institute 2026). Large hail events have also become more frequent over time in the CONUS, aligning with an increase in insured losses (Battaglioli et al. 2026).

Hail forms in thunderstorms when raindrops or small ice embryos are lofted by strong updrafts into a favorable -10°C to -30°C region of the cloud, called the hail growth zone (HGZ), where they accrete layers of supercooled water. Hailstone growth continues if the updraft can support the hailstone against gravity, provided there is sufficient residence time in the HGZ with adequate liquid water content (Allen et al. 2020). Stones with longer residence time tend to be larger than those with shorter residence time. Hailstones fall out of thunderstorms once they either become too heavy for the updraft or are ejected out of the HGZ/updraft.

Environmental conditions are important in determining whether hail is favored to form and how large it may become. At a basic level, high convective available potential energy (CAPE), supports stronger and wider updrafts capable of suspending hailstones in a growth regime for longer periods. Another important environmental consideration is vertical wind shear, as it plays a key role in organizing thunderstorms. These organized thunderstorms can sometimes rotate and thus be considered supercells instead of transient or messy storms, also supporting longer residence times and a larger hail embryo source region (Dennis and Kumjian 2017).

The prediction of hail size remains a challenge at both the short range/storm scale and medium range/synoptic scale. While short-term hail probability products exist, particularly with the use of machine learning (Hill et al. 2020), there currently does not exist a tool to estimate the potential hail size days in advance on a broad scale. Visualization of hail occurrence probability is already useful but could be further paired with a maximum hail size product to highlight the possible upper-end impact. With the largest hail events causing the most damage, having such a tool could offer early signs for areas that have the potential for large hail (≥1 in./25.4 mm) to very large hail (>2 in./50.8 mm) to occur. 

## Data and Methods

The goal of this project is to produce an eastern CONUS map that shows smoothed max hail size bins, highlighting where the largest hail may occur. This product will not necessarily consider potential realization of convective storms but rather will be environmentally based with thermodynamic and kinematic fields. This tool will be designed to only be run in the spring and summer. In an operational setting, areas that stand out can be examined to assess potential for such a scenario to unfold in a region. The data that will be used to train the ML model is the Global Ensemble Forecast System Version 12 (GEFSv12) reforecast data paired with the historical hail reports archive from the Storm Prediction Center (SPC). GEFSv12 reforecast data is stored on AWS with 4x daily runs and 5 member (one control member and four perturbed members) from 2000-2019 (AWS). The GEFSv12 reforecast runs that we will be choosing will be the 00 UTC cycle only, May 1 00 UTC – June 1 00 UTC, as the eventual live data will also be chosen from the 00 UTC cycle. Reforecast data contains basic atmospheric variables (u wind, v wind, vertical velocity, temperature, height, and specific humidity) at 25 pressure levels from 1000 hPa up to 1 hPa. The reforecast data is stored in two separate files for each run, one for variables below 700 hPa and another for variables above 500 hPa. Horizontal resolution below 700 hPa is 0.25° and above 700 hPa is 0.50°. The GEFS data was converted to much smaller daily .nc files and regionally confined to the eastern CONUS. Hail report data is stored in .csv files on the SPC website (SPC). These exist in individual files for years and decades, but also in one single zip file for all years back to 1950. For simplicity, we downloaded the full file and then filtered to the period we choose to match with our GEFSv12 reforecast data. The data that the tool will run on in real-time will be the GEFS model, also stored on AWS. This version of the GEFS contains 31 members (one control member and 30 perturbed members), as well as mean values across all members. In the mean data files (formatted as grib2), there are 85 parameters available to choose from at different pressure levels and heights. The GEFSv12 reforecast data are downloaded via a custom script while live GEFS data is obtained via the Herbie package. Pandas was used to open the hail report database. Cartopy and matplotlib are used to build output maps. Scikit-learn will be used to train an ML model, most likely a random forest classification model. This may be the best option as it works well with gridded data and can output binned data. To provide an environment to the ML model, we downloaded and computed the following parameters:
	Surface-based convective available potential energy (SBCAPE)
	850-250 hPa bulk wind difference (BWD)

  
The ML model will likely be trained on 2000-2015 data and validated/tested on 2016-2019 data from the GEFSv12 reforecast and SPC hail reports. 

## Feasibility and Ambition

This type of project has been done before, except for general severe and hail probabilities instead of maximum potential hail size (Mazurek at al. 2025). Our team generally has the required geoscience knowledge of spatial data, analysis methods, data visualization, and cartography. Our team is mostly comfortable with Python. A lot of data is required for this project. For the training alone, 620 runs of the GEFSv12 reforecast were read in. Each run has 3-hour time increments, which means 40 timesteps total for each variable (F00-F120). As for the hail data, tens of thousands of hail reports will likely be downloaded. For computing, CPU will be needed to process and train the data. Our general understanding of all parts of this project is that a lot of data will need to be read in and processed in python, that a random forest model will most likely be used to train with the data, that testing/validation will need to be done with some of the remaining data, and that the output will be in a concise map form for viewer digestion.

## Potential Issues

The main area where we could envision issues arising is the collection of reforecast data and the performance of parameter computations on that data. It would require a lot of niche coding and code run time. If we go with more of a simple approach (ie. less parameters), the fear is that the model would not perform so well, but at least we know we would be able to produce a product with the time that is left in the semester. As of the late semester, the GEFSv12 reforecast data took far longer to download than anticipated.

## Results

Results of the machine learning model are yet to come. This section will be populated before the final draft is due. Will add citations to contextualize the results as well. As mentioned, the GEFSv12 reforecast data took far longer than anticipated to download, convert, and calculate shear from. The hail data is downloaded and subset to the 2000-2019 period. We nearly have our training, testing, and validation data subsets ready to go.

## Summary

The motivation for this project is to improve the detection of large hail risk at the short and medium range with a daily maximum hail size map. Because the largest contribution of insured losses from severe storms is associated with damaging hail, it is of interest to highlight areas where this peril may exist. Predicting maximum potential hail size for short-fused severe thunderstorm or tornado warnings is difficult, let alone for the forecast days in advance. The goal is to make some headway on this with the use of a machine learning model. The methods of this project are fairly simple. We use 620 days of GEFSv12 reforecast data and thousands of SPC hail reports as data for training, validation, and testing of a random forest classifier model. The machine learning model is to understand relationships between environmental conditions (SBCAPE and 850-250 hPa shear) and large reported hailstones. Then, these relationships are used to predict max hail size maps (binned) across the eastern CONUS by using forecast SBCAPE and wind shear. Again, we will have more on the results of this shortly.

## Works Cited

Allen, J. T., I. M. Giammanco, M. R. Kumjian, H. Jurgen Punge, Q. Zhang, P. Groenemeijer, M. Kunz, and K. Ortega, 2020: Understanding Hail in the Earth System. Reviews of Geophysics, 58, e2019RG000665, https://doi.org/10.1029/2019RG000665.

Battaglioli, F., M. Taszarek, P. Groenemeijer, T. Púčik, and A. Rädler, 2026: Contrasting trends in very large hail events and related economic losses across the globe. Nat. Geosci., 19, 52–58, https://doi.org/10.1038/s41561-025-01868-0.

Dennis, E. J., and M. R. Kumjian, 2017: The Impact of Vertical Wind Shear on Hail Growth in Simulated Supercells. Journal of the Atmospheric Sciences, 74, 641–663, https://doi.org/10.1175/JAS-D-16-0066.1.

Hail Risk: The Growing Threat for Property Insurers,. CAPE Analytics. Accessed 17 April 2026, https://capeanalytics.com/blog/hail-risk-for-property-insurers/.

Hill, A. J., G. R. Herman, and R. S. Schumacher, 2020: Forecasting Severe Weather with Random Forests. Monthly Weather Review, 148, 2135–2161, https://doi.org/10.1175/MWR-D-19-0344.1.

Mazurek, A. C., A. J. Hill, R. S. Schumacher, and H. J. McDaniel, 2025: Can Ingredients-Based Forecasting Be Learned? Disentangling a Random Forest’s Severe Weather Predictions. Weather and Forecasting, 40, 237–258, https://doi.org/10.1175/WAF-D-23-0193.1.

Triple-I: Severe Convective Storms Generate More Than $50B in Insured Losses for Third Consecutive Year | III,. Accessed 17 April 2026, https://www.iii.org/press-release/triple-i-severe-convective-storms-generate-more-than-50b-in-insured-losses-for-third-consecutive-year-041326.

-------------------------------------------------------------------------------------------------------------------------------------------

PR-01.A: Download GEFSv12 Reforecast Data (grib2 files)
Priority: High
Sprint: 1
Assigned to: Landon
As a developer of a machine learning model, I need to gather an analysis dataset so I have data that I can train my model on.
Acceptance Criteria:
Downloaded files must be in grib2 format and be subset to 00z initialized runs between May 1, 2000 and June 1, 2019.
Status: Completed

PR-01.B: Download SPC Hail Reports (csv files)
Priority: High
Sprint: 1
Assigned to: Samantha
As a developer of a machine learning model, I need to gather a secondary analysis dataset so I have data that I can train my model on.
Acceptance Criteria:
Downloaded files must be in csv format and be subset to 2000-2019.
Status: Completed

PR-02: Subset GEFSv12 Data Regionally into netCDF Files
Priority: High
Sprint: 1
Assigned to: Landon
As a developer of a machine learning model, I need to reduce the size of data required and make it easier to work with.
Acceptance Criteria:
•	Dataset must be output as a .nc file and stored in the EAE Triton scratch folder.
•	Dataset must be a significantly smaller size than when in .grib2 form.
•	Dataset must be centered over the eastern Contiguous United States.

Automatic Test: Check the size of output files and plot a spatial map of a variable to make sure it is over the correct area.
Status: Completed

PR-03: Calculate Variables from GEFSv12 Reforecast Data with MetPy
Priority: High
Sprint: 2
Assigned to: Both Members
As a developer of a machine learning model, I need to calculate new variables from my dataset to serve as proper model input.
Acceptance Criteria:
•	Scripts must be able to calculate variables with 3-dimensional data on pressure surfaces.
Automatic Test: Plot a map of the computed variables to make sure they were computed at every grid point and timestep.
Status: Revised… no MetPy necessary

PR-04: Create Data Subsets for Training, Validating, and Testing
Priority: High
Sprint: 2
Assigned to: Samantha 
As a developer of a machine learning model, we need to create separate datasets from the original hail data to train the model and verify and test its function to ensure it produces accurate results. 
Acceptance Criteria:
•	Hail dataset must be split into three separate datasets based on years.
•	Each individual dataset must be appropriately scaled in terms of size, such as the 70/10/20 rule. Model will be trained on earlier data and tested on more recent data. 
•	Datasets should roughly match overall distribution to confirm legitimacy and avoid biases
Automatic Test: Create a script that will generate statistics on each of the three datasets. If statistics do not match well enough, we will revise. 
Status: In Progress

PR-05: Format GEFSv12 Reforecast Data and SPC Hail Reports for RF-Classification model
Priority: Medium
Sprint: 3
Assigned to: Both Members
As a developer of a machine learning model, I need to format the data in order to develop and train my model.
Acceptance Criteria:
•	Reforecast Data and SPC hail reports must be reduced to only what is necessary
•	We will keep only the years and variables that are needed for the model
•	Data should be formatted into a csv file that can be inputted into the scikit-learn RF framework 
Automatic Test: Create a script that reads the csv file and checks if all fields/data are present. If not, generate an error showing what is missing.
Status: In Progress

PR-06: Train & Validate RF-Classification model
Priority: High
Sprint: 3
Assigned to: Both Members
As a developer of a machine learning model, we need to train the model using the hail report data and variables so we can predict maximum hail sizes based on weather conditions. 
Acceptance Criteria:
•	RF model should use scikit-learn
•	Model output should be formatted in such a way that the model will display its predicted hail sizes as atmospheric data is entered. 
Automatic Test: Create a script that tests model output for desired output (i.e. predicted hail size, affected areas, etc.)
Status: To Be Completed

PR-07: Test RF-Classification model
Priority: Medium
Sprint: 3
Assigned to: Both Members
As a developer of a machine learning model, I need to test the model using a fraction of my reforecast data, so I know how well my model does at predicting max hail size at different lead times.
Acceptance Criteria:
•	Model output must be plotted on a map
•	SPC hail reports should be plotted on the same map
Automatic Test: Write a script that plots model output with SPC hail reports overlaid to check for spatial accuracy.
Status: To Be Completed

------------------------------------------------------------------------------------------------------------------------------------------

GEFSv12 Reforecast Data: https://noaa-gefs-retrospective.s3.amazonaws.com/index.html

Live GEFS Forecasts: https://noaa-gefs-pds.s3.amazonaws.com/index.html

SPC Hail Reports: https://www.spc.noaa.gov/wcm/#data
