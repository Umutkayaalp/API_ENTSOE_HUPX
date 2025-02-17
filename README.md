# API_ENTSOE_HUPX
https://newtransparency.entsoe.eu/market/energyPrices?appState=%7B%22sa%22%3A%5B%22BZN%7C10YHU-MAVIR----U%22%5D%2C%22st%22%3A%22BZN%22%2C%22mm%22%3Atrue%2C%22ma%22%3Afalse%2C%22sp%22%3A%22HALF%22%2C%22dt%22%3A%22TABLE%22%2C%22df%22%3A%222025-02-17%22%2C%22tz%22%3A%22CET%22%7D
This repository contains code for downloading day-ahead energy prices for Hungary from the ENTSO-E API, saving the data as CSV files, and converting the hourly energy price data to 15-minute intervals by duplicating the hourly values.How to Use
Set Your ENTSO-E API Key
Before running the script, you'll need to create an ENTSO-E API account and get your API key. You can do this at ENTSO-E Transparency Platform.
Replace YOUR_API_KEY with your actual API key in the code.

Run the Script
The script consists of two main parts:

Downloading Energy Prices: The code fetches day-ahead energy prices for Hungary (country code 10YHU-MAVIR----U) between a specified start_dt and end_dt.
Resampling Data: After downloading the hourly energy prices, the data is resampled to 15-minute intervals by duplicating the hourly values using forward-fill (ffill()).
Files Generated:

hungary_energy_prices.csv: Contains the hourly energy prices for the selected time period.
hungary_energy_prices_15min.csv: Contains the 15-minute interval energy prices (duplicated from the hourly values).
Customization:

You can modify the start_dt and end_dt variables to change the time period for which you want to download data.
If you want to change the country or area, replace the country_code in the query method with the appropriate ENTSO-E area code.

from entsoe import EntsoePandasClient
import pandas as pd

# Set ENTSO-E API Key (Replace with your API key)
api_key = "YOUR_API_KEY"
client = EntsoePandasClient(api_key=api_key)

# Define Time Period
start_dt = pd.Timestamp('2025-01-01', tz='Europe/Rome')
end_dt = pd.Timestamp('2025-02-01', tz='Europe/Rome')

# Download Energy Prices for Hungary (Hourly data)
energy_prices = client.query_day_ahead_prices(country_code='10YHU-MAVIR----U', start=start_dt, end=end_dt)

# Save Hourly Energy Prices to CSV
file_name = "hungary_energy_prices.csv"
energy_prices.to_csv(file_name, index=True)
print(f"Energy prices saved as {file_name}")

# Resample Hourly Data to 15-minute Intervals (Duplicating the hourly values)
energy_prices_15min = energy_prices.resample('15T').ffill()

# Save Resampled Data to CSV
file_name = "hungary_energy_prices_15min.csv"
energy_prices_15min.to_csv(file_name, index=True)
print(f"15-minute energy prices saved as {file_name}")
