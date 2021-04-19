# Benchmark internal transfer prices for an Electricity Authority consultation paper on internal transfer prices and gross profitability.

20 April 2021

### PURPOSE: 

This readme file briefly describes R code and data files used to compute benchmark internal transfer prices (ITPs).
The benchmark ITPs were reported in an April 2021 consultation paper, available at: 
https://www.ea.govt.nz/assets/dms-assets/28/Internal-transfer-prices-and-segmented-profitability-reporting-Consultation-paper.pdf
See in particular the blue shaded regions in figure 2 on page 7.

See https://www.r-project.org/ for the R statistical computing software (R is released under the GNU General Public License (GPL), version 2 
or version 3.)


### BACKGROUND:

ITPs determine the allocation of profits and costs between the generation and retail arms of integrated retailers. 
The benchmark ITPs are used to assess whether the ITPs of five large integrated generator-retailers, reported to the Authority in 2020, 
broadly accord with the methodologies that were also reported. 

This code cannot be run without additional proprietary data owned by the ASX, which cannot be disseminated by the Electricity Authority. 
The code does, however, illustrate the logic underpinning the derivation of the benchmarks, and external parties may also have access to 
the ASX futures price data required to run the Code.

### DATA:

Amongst other data, the methodologies used to compute the benchmark ITPs are based on a CSV file of monthly futures prices that have been 
aggregated from daily futures prices. 

As mentioned above, the daily futures prices are proprietary data owned by the ASX and cannot be provided by the Electricity Authority. 
Examples of the underlying futures prices can be found here: https://www.asxenergy.com.au/products/data_centre/daily-files .

The ASX's historical futures price data have been redacted from an example of the CSV file of the underlying monthly-frequency futures data. 
This redacted file has been included to inform the R code.


### FILES IN THE REPOSITORY:

    1. README - Benchmark ITPs.txt
    2. csLoadAnalysisITP-v2.R
    3. csBenchmarkPricesQuarterly-v3.R
    4. csFTRanalysis-v1.R
    5. injection-offtake-all-participants.csv
    6. REDACTED-Futures Markets Prices 2020-07-09 Settlement price summary.csv
    7. Spot Prices Monthly at FTR Nodes.csv


### DESCRIPTION OF FILES

    1. README - Benchmark ITPs.txt (Text file)
    The readme file you are reading.

### R-SCRIPTS

    2. csLoadAnalysisITP-v2.R (Text file)
Computes a matrix of load-based weights (by regional zone and month) based on different retailer group's load. 
The regional zones are Upper North Island, Central North Island, Lower North Island, Upper South Island, Lower South Island. 
To derive the load-weights for figure 2 in the consultation paper we used the average load profile for the five large integrated generator retailers -- 
this average load profile approximates New Zealand's average load profile.

    3. csBenchmarkPricesQuarterly-v3.R
This is the main file for computing the benchmark internal transfer prices. It uses weights derived from csLoadAnalysisITP-v2.R and 
uses them to aggregate 'averaged' futures prices, adjusted for location and monthly seasonality, to compute an internal transfer price for a given financial year. 
There are multiple benchmarks primarily to reflect the fact that different integrated retailers use different methodologies to average futures prices.
The program has four main steps to compute the internal transfer price benchmarks
	a. Average futures prices at the Otahuhu node
	b. Add a price adjustment reflecting average locational differences in prices
	c. Interpolate seasonal (monthly) variation in prices
	d. Use weights derived from load (volumes) to compute a weighted average of the prices for a given financial year. 

    4. csFTRanalysis-v1.R  (Text file)
This file (un-used) was used to consider how locational differences (basis risk) priced by FTRs might affect internal transfer prices. 
To simplify the benchmark ITP analysis average differences in locational spot prices were used instead of FTR prices. (See file 7. below.)
On average FTR prices should broadly accord with average spot price differences.
Another alternative would be to use locational factor data from https://www.emi.ea.govt.nz/Wholesale/Reports/ALYRQB?_si=tg|prices,v|3  
Note: FTR_Register.csv can be downloaded by joining and logging into the FTR manager's portal at www.ftr.co.nz.

### DATA FILES (Comma-delimited text files)

    5. injection-offtake-all-participants.csv  (Comma-delimited text file)
This CSV file contains reconcilaition data for participants aggregated by zone. (See csLoadAnalysisITP-v2.R.)
The underlying reconciliation data at finer granularity can be found here: https://www.emi.ea.govt.nz/Wholesale/Datasets/Volumes/Reconciliation

    6. REDACTED-Futures Markets Prices 2020-07-09 Settlement price summary.csv (Comma-delimited text file)
This redacted data file illustrates the columns of futures data that underpinned the benchmark ITPs.

    7. Spot Prices Monthly at FTR Nodes.csv (Comma-delimited text file)
This file contains spot price data from the FTR nodes which were used to approximate zonal spot prices. 
