#<h1 style="text-align: center;">Hospitality_Analysis </h1> 

# I completed this project and i learn from this how to perform EDA
Here’s a structured breakdown of your project steps, along with potential solutions and ideas:

### 1. **Exploratory Data Analysis (EDA)**
   - **Data Import and Data Exploration:** 
     - Load the dataset and explore the basic structure using methods like `.head()`, `.describe()`, and `.info()` to understand the number of rows, columns, and the types of data.
   - **Explore Aggregate Bookings:** 
     - Identify basic trends, patterns, and outliers using summary statistics, histograms, and boxplots.
   - **Find Unique Property IDs in Aggregate Bookings Dataset:**
     - Use `df['property_id'].nunique()` to find the number of unique properties.
   - **Total Bookings per Property ID:** 
     - Group data by `property_id` and count total bookings using `df.groupby('property_id').size()`.
   - **Bookings Greater than Capacity:**
     - Filter rows where bookings exceed capacity: `df[df['bookings'] > df['capacity']]`.
   - **Properties with Highest Capacity:** 
     - Use `df.groupby('property_id')['capacity'].max().sort_values(ascending=False)` to find the properties with the highest capacity.

### 2. **Data Cleaning**
   - **Cleaning Invalid Guests:**
     - Filter out invalid data (e.g., negative guest values) using `df[df['guests'] >= 0]`.
   - **Focusing on Room Type RT4 (Presidential Suite):**
     - Filter data only for room type `RT4` for analysis: `df[df['room_type'] == 'RT4']`.
   - **Outliers in Revenue:** 
     - After analyzing, if the max value of revenue is within an acceptable range (e.g., 45220 in a range of 50583), then no cleaning is needed for this column.
   - **Null Ratings:**
     - Since 77899 rows have null ratings, these should not be dropped or replaced with median/mean. They can be ignored or left as null.
   - **Handling Null Values in Aggregate Bookings:**
     - For columns with null values, fill missing data with appropriate substitutes (e.g., mean or median):
       ```python
       df['column_name'].fillna(df['column_name'].mean(), inplace=True)
       ```

   - **Filter Records with Bookings Exceeding Capacity:**
     - Apply a filter to keep only those records where `successful_bookings` exceeds `capacity`:
       ```python
       df[df['successful_bookings'] > df['capacity']]
       ```

### 3. **Data Transformation**
   - **Convert Values to Percentages:**
     - For example, to convert occupancy into a percentage, use:
       ```python
       df['occupancy_percentage'] = (df['successful_bookings'] / df['capacity']) * 100
       ```

### 4. **Insight Generation**
   - **Average Occupancy Rate by Room Category:**
     - If `RT1`, `RT2` don't provide insights, you can rename them to meaningful categories like Standard, Premium, etc., and calculate the average occupancy rate:
       ```python
       df.groupby('room_category')['occupancy_percentage'].mean()
       ```
   - **Average Occupancy Rate by City:**
     - Group data by city to find the average occupancy percentage:
       ```python
       df.groupby('city')['occupancy_percentage'].mean()
       ```

   - **Occupancy by Weekday vs. Weekend:**
     - You can use the `weekday()` function to categorize the data into weekdays and weekends:
       ```python
       df['is_weekend'] = df['date'].apply(lambda x: 1 if x.weekday() >= 5 else 0)
       df.groupby('is_weekend')['occupancy_percentage'].mean()
       ```

   - **June Occupancy for Different Cities:**
     - Filter for the month of June and analyze the occupancy per city:
       ```python
       june_data = df[df['month'] == 'June']
       june_data.groupby('city')['occupancy_percentage'].mean()
       ```

   - **Appending New Data for August:**
     - After loading the new data for August, append it to the existing data:
       ```python
       august_data = pd.read_csv('august_data.csv')
       df = df.append(august_data, ignore_index=True)
       ```

   - **Revenue Realized per City:**
     - Group data by city to calculate total revenue:
       ```python
       df.groupby('city')['revenue_realized'].sum()
       ```

   - **Month-by-Month Revenue:**
     - Group data by month and calculate revenue for each month:
       ```python
       df.groupby('month')['revenue_realized'].sum()
       ```

### Key Points to Remember:
- Always validate data before performing analysis, especially when dealing with large datasets.
- Visualizations (using libraries like `matplotlib` or `seaborn`) can significantly enhance EDA and insight generation.
- For missing data, consider the context before filling it. Sometimes, null values carry important information and shouldn’t be replaced blindly.
