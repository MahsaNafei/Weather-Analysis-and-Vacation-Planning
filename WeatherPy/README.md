## WeatherPy Analysis

### Introduction

WeatherPy is a Python script that retrieves weather data for a list of cities based on their geographic coordinates and analyzes the relationship between weather variables and latitude. This analysis aims to understand how weather variables like temperature, humidity, cloudiness, and wind speed vary with latitude in different parts of the world.

### Data Retrieval

The script uses the OpenWeatherMap API to fetch weather data for each city in the list. It collects data such as maximum temperature, humidity, cloudiness, wind speed, country, and date. The data is stored in a Pandas DataFrame for analysis.

**API Key Setup:**

Before retrieving weather data, you import the necessary libraries and set up your API key. The API key is stored in a separate file, api_keys.py, and imported into your script using from api_keys import weather_api_key.

**Generate Random Geographic Coordinates and City List:**

- To create a list of cities for which you want to retrieve weather data, you follow these steps:

- Create empty lists, lat_lngs for latitude and longitude combinations, and cities for city names.

- Define the latitude and longitude ranges for random coordinate generation, lat_range (-90 to 90) and lng_range (-180 to 180).

- Generate random latitude and longitude pairs using NumPy's np.random.uniform() function.

- Use the citipy library to identify the nearest city for each latitude and longitude pair. The city names are stored in the cities list.

- Ensure that only unique cities are added to the list.

**API Data Retrieval Loop:**

The main part of data retrieval involves making API requests to OpenWeatherMap for each city in the cities list. This is done in a loop for each city:

- Create a base URL for the OpenWeatherMap API with the desired units and your API key.

- Set up counters record_count and set_count to keep track of the API requests. These are used for logging purposes.

- Loop through each city and its index in the cities list.

- Group cities into sets of 50 for logging purposes. When i is a multiple of 50 (e.g., 50, 100, 150...), increment set_count and reset record_count to 0.

- Create an endpoint URL for the specific city.

- Log the URL, record number, and set number to track progress.

- Attempt to make an API request for the city's weather data using requests.get().

- Parse the JSON response to extract relevant weather data such as latitude, longitude, temperature, humidity, cloudiness, wind speed, country, and date.

- Append this data to the city_data list as a dictionary.

**Data Loading Complete:**

After completing the loop for all cities, you print a message indicating that data loading is complete.

**Data Conversion to DataFrame:**

Convert the collected weather data in city_data into a Pandas DataFrame named city_data_df. This DataFrame will be used for analysis and visualization.

**Data Export:**

Export the city_data_df to a CSV file named "cities.csv" with "City_ID" as the index label. This CSV file can be used for future reference or analysis.

### Data Visualization

The analysis creates scatter plots to showcase the relationship between weather variables and latitude for the cities. The following plots are generated:
1. Latitude vs. Max Temperature
2. Latitude vs. Humidity
3. Latitude vs. Cloudiness
4. Latitude vs. Wind Speed

Each plot is labeled and saved as a PNG file in the "output_data" directory.

### Linear Regression

Linear regression analysis is performed to assess the relationships between latitude and weather variables in both the Northern and Southern Hemispheres. The following relationships are analyzed:
1. Temperature vs. Latitude
2. Humidity vs. Latitude
3. Cloudiness vs. Latitude
4. Wind Speed vs. Latitude

The linear regression plots, including regression lines and equations, are displayed for each relationship. The r-value is also calculated to quantify the strength of the relationship.


### Instructions

To use this script:

1. Ensure you have the required libraries installed (matplotlib, pandas, numpy, requests, citipy).

2. Replace `weather_api_key` in the code with your OpenWeatherMap API key.

3. Run the script using Python.

4. The analysis results, including scatter plots and regression analysis, will be displayed.

5. The script also saves the scatter plots as image files in the "output_data" folder and exports the city weather data to a CSV file named "cities.csv" in the same folder.



# VacationPy Analysis

This project uses data from the WeatherPy analysis to find ideal vacation spots based on specific weather conditions. It includes creating a map with city markers representing humidity, narrowing down cities that meet ideal weather criteria, finding nearby hotels using the Geoapify API, and displaying the hotels on the map.

### Data Sources and Dependencies

The data for this analysis is obtained from the WeatherPy analysis, where weather data for various cities was retrieved from the OpenWeatherMap API.

### City Map with Humidity

The analysis begins by creating a map that displays a point for every city in the `city_data_df` DataFrame. The size of each point on the map corresponds to the humidity level in each city. This map provides a visual representation of cities and their humidity levels.

### Ideal Weather Conditions

The `city_data_df` DataFrame is narrowed down to find cities that match specific ideal weather conditions. The criteria for ideal weather conditions include:
- Maximum temperature between 22°C (71.6°F) and 35°C (95°F).
- Wind speed less than 10 meters per second.
- Cloudiness less than 15%.
- Humidity between 60% and 80%.

The resulting DataFrame, named `ideal_city`, contains cities that meet these criteria while also excluding any rows with null values.

### Hotel Data

A new DataFrame named `hotel_df` is created to store information about hotels in the selected cities. This DataFrame includes the city, country, coordinates, humidity, and an empty column for the hotel name.

### Finding Hotels

The Geoapify API is used to search for hotels located within 10,000 meters of the coordinates of each city in the `hotel_df` DataFrame. A loop iterates through the DataFrame, making API requests for each city. The API searches for hotels near the city's coordinates and returns the name of the nearest hotel, which is then added to the `hotel_df` DataFrame.

If no hotel is found within the specified radius, the hotel name is set as "No hotel found."

### Map with Hotel Information

A final map is created with city markers that include information about the nearest hotel and the country. The size of each marker still represents the humidity level in the city. This map provides an overview of ideal vacation spots with nearby hotels.

#### Instructions

To use this script:

1. Ensure you have the following Python libraries installed: hvplot.pandas, pandas, and requests.
2. Obtain an API key from Geoapify and save it in a file named `api_keys.py`.
3. Run the provided code in a Python environment.
4. The analysis results, including the map with hotel information, will be displayed.


