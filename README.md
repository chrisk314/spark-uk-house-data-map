# Spark UK House Data Map
This project demonstrates how to use ESRI shapefiles to generate a map of the UK (actually the UK
minus Northern Ireland due to data availability) using `pyspark` with `pyshp`, `pyproj` and
`matplotlib`. Shapefiles which contain the geometry of the administrative districts of England,
Wales and Scotland with coordinates specified in OSGB36 format are available in the Code-Point Open
data set from Ordnance Survey (OS). The districts of England and Wales are coloured based on the
mean sales price for a given year. House sale price data for England and Wales is available from 
The Office for National Statistics (ONS) in the Price Paid dataset. Data for Scotland and Northern
Ireland is not available in this dataset.

The Price Paid dataset turned out to be too large (it's ~4GB) to load in to a Jupyter notebook with
Pandas on my laptop (16GB memory). I decided it would be nice to demonstrate using the Spark
DataFrame API for the data aggregations as Spark can be run in "standalone" mode on a single machine
and it tends to handle memory a lot better than Pandas in my experience.

Anyway, here's the map. Take a look at the notebook to see how it's made.

![map-mean-price-anim-small](https://user-images.githubusercontent.com/2366658/47806707-4a552f00-dd32-11e8-84bf-2b39dd6ce2d0.gif)

## Data sources
Below is a table of data sources used in this project and where to find them (they are all
freely available). To recreate the map these data sources need to be available with there 
locations set in the `.env` file.

| Dataset | Description | Available from | Env var |
|---|---|---|---|
| OS Boundary Line | Contains shapefiles specifying map geometry | [link](https://www.ordnancesurvey.co.uk/business-and-government/products/boundaryline.html) | DATA_BDLINE |
| OS Code-Point Open | Contains data about postcode locations etc | [link](https://www.ordnancesurvey.co.uk/business-and-government/products/code-point-open.html) | DATA_CODEPO |
| ONS Price Paid | Historic house sale price data | [link](https://www.gov.uk/government/statistical-data-sets/price-paid-data-downloads) | DATA_PP_CSV |
| ONS London Postcodes | ...London Postcodes | [link](https://data.london.gov.uk/dataset/postcode-directory-for-london) | DATA_LDN_POSTCODES_CSV |
| OS RPI | Historical Retail Prices Index (RPI) data | [link](https://www.ons.gov.uk/economy/inflationandpriceindices/timeseries/czbh/mm23) | DATA_RPI_CSV |
| OS GDP | Historical Gross Domestic Product (GDP) data | [link](https://www.ons.gov.uk/economy/grossdomesticproductgdp/timeseries/abmi/ukea) | DATA_GDP_CSV |
| OS Population | Historical population data. This is in `.xls` format. I manually exported the data of interest in `.csv` format | [link](https://www.ons.gov.uk/peoplepopulationandcommunity/populationandmigration/populationestimates/adhocs/004356ukpopulationestimates1851to2014) | DATA_POP_CSV   |
| OS Labour Market Survey | Historical employment related data | [link](https://www.ons.gov.uk/employmentandlabourmarket/peopleinwork/employmentandemployeetypes/datasets/labourmarketstatistics) | DATA_LMS_CSV |

## Install

### Installing Spark standalone
Spark must be installed in order to run the notebook. Spark requires java. Currently Spark 2.3.2
only works with java version 8. To install java 8 on ubuntu

```
sudo apt-get install openjdk-8-jdk openjdk-8-jre
```

Then add these lines to your `~/.profile`

```
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre
export PATH=${PATH}:${JAVA_HOME}/bin
```

Download a precompiled binary version of Spark with Hadoop

```
cd /opt
wget http://apache.claz.org/spark/spark-2.3.2/spark-2.3.2-bin-hadoop2.7.tgz
tar -xzf spark-2.3.2-bin-hadoop2.7.tgz
``` 

Add the following lines to your `~/.profile`

```
export SPARK_HOME=/opt/spark-2.3.2-bin-hadoop2.7
export PATH=${PATH}:${SPARK_HOME}/bin
export PYTHONPATH=${SPARK_HOME}/python:${PYTHONPATH}
```

### Installing project dependencies

```bash
git clone git@github.com:chrisk314/spark-uk-house-data-map.git
cd spark-uk-house-data-map
virtualenv -p python3 venv
source .env
python -m pip install -r requirements.in
```

## Run
The house price map is generated interactively by running the Jupyter notebook. To start the
notebook server using `pyspark` run the below commands

```
source .env
pyspark
```

