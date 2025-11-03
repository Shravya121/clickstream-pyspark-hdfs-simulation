
#  Clickstream Analysis using PySpark & HDFS Simulation (Google Colab)

This project analyzes user clickstream data using **PySpark** on **Google Colab**, simulating **HDFS storage** with **Google Drive**.  
It performs clickstream analytics and visualizes hourly activity trends using **Matplotlib**.

---

##  Project Overview
The goal of this project is to simulate real-world **big data processing** using PySpark, where we:
- Process clickstream (user activity) data stored in Google Drive.
- Perform aggregation and filtering operations in PySpark.
- Visualize user activity patterns using Matplotlib.

---

## Tools & Technologies
- **PySpark**
- **Google Colab**
- **Google Drive (HDFS Simulation)**
- **Matplotlib**
- **Pandas**

---

## ⚙️ Steps to Run

### 1️. Mount Google Drive (simulate HDFS)
```python
from google.colab import drive
drive.mount('/content/drive')
`
df = spark.read.option("header", True).csv("/content/drive/MyDrive/ecommerce_clickstream_transactions.csv", inferSchema=True)
df.show(5)

from pyspark.sql.functions import col, count, desc

# Count total records
print("Total Records:", df.count())

# Find top 5 most clicked pages
top_pages = df.groupBy("EventType").agg(count("*").alias("click_count")).orderBy(desc("click_count"))
top_pages.show(5)

# Filter by country (example)
us_clicks = df.filter(col("Outcome") == "Success")
us_clicks.show(5)

from pyspark.sql.functions import hour, count
import matplotlib.pyplot as plt

hourly_trend = (
    df.withColumn("hour", hour("Timestamp"))
      .groupBy("hour")
      .agg(count("*").alias("events_per_hour"))
      .orderBy("hour")
)

pdf = hourly_trend.toPandas()
plt.plot(pdf['hour'], pdf['events_per_hour'], marker='o')
plt.title("User Activity per Hour")
plt.xlabel("Hour of Day")
plt.ylabel("Number of Events")
plt.grid(True)
plt.show()





