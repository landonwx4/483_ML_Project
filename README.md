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


The goal of this project is to produce a map that predicts Days 1-5 maximum binned hail size using a random forest model. This model is trained, validated, and tests on GEFSv12 Reforecast data and Storm Prediction Center hail reports from 2000-2019. From the GEFS data, surface-based CAPE and 850-250 hPa wind shear are used as environmental representations. As of mid-April, the machine learning model has not yet been ran, as wrangling data has taken a lot longer than anticipated.

GEFSv12 Reforecast Data: https://noaa-gefs-retrospective.s3.amazonaws.com/index.html

Live GEFS Forecasts: https://noaa-gefs-pds.s3.amazonaws.com/index.html

SPC Hail Reports: https://www.spc.noaa.gov/wcm/#data
