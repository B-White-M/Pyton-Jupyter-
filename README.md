### [Back to Main-Projects tab](https://github.com/B-White-M/Projects/tree/main)

# Python_Jupyter: Web Scrapping.
In this project, I created a Python script that leverages the Google Trends API to analyze search trends related to the Colombian lingerie market. By connecting to Google Trends using the pytrends library, I defined a set of relevant search terms and segmented them into smaller groups to optimize data requests.

The script then retrieves data over time and by region, allowing me to identify trends in both temporal and geographic contexts which can help inform targeted marketing and business decisions.

# Python (Jupyter) Code used:
Here is an example of the Python (Jupyter) code used in the project

## Data Source details:

```python
# Importing required libraries
from pytrends.request import TrendReq
import pandas as pd

# Connect to Google Trends
pytrends = TrendReq(hl='en-US', tz=360)

# Define search terms (focused on Colombian lingerie market)
search_terms = ["sample", "sample 2", "sample 3", "sample 4", "sample 5"]

# Function to split the list into chunks
# This allows us to send multiple search terms in smaller groups to avoid data limits
def chunk_list(lst, chunk_size):
    for i in range(0, len(lst), chunk_size):
        yield lst[i:i + chunk_size]

# Download and save data as CSV
chunk_size = 3
for i, chunk in enumerate(chunk_list(search_terms, chunk_size), start=1):
    try:
        print(f"Downloading data for group {i}: {chunk}")
        
        # Build payload for each chunk of search terms
        pytrends.build_payload(chunk, cat=0, timeframe='2020-01-01 2024-01-01', geo='CR', gprop='')
        
        # Get interest over time data
        data_time = pytrends.interest_over_time()
        
        # Get data by region
        data_region = pytrends.interest_by_region(resolution='REGION', inc_low_vol=True, inc_geo_code=True)
        
        # Save data to CSV if available
        if not data_time.empty:
            filename_time = f'google_trends_data_time_part_{i}.csv'
            data_time.to_csv(filename_time)
            print(f"Time data saved to {filename_time}")
        else:
            print(f"No time data available for group {i}")
        
        if not data_region.empty:
            filename_region = f'google_trends_data_region_part_{i}.csv'
            data_region.to_csv(filename_region)
            print(f"Region data saved to {filename_region}")
        else:
            print(f"No region data available for group {i}")
    
    except Exception as e:
        print(f"Error in group {i}: {e}")
```

### [Back to Main-Projects tab](https://github.com/B-White-M/Projects/tree/main)

