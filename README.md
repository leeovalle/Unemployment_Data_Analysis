# 📊 U.S. Unemployment Data Analysis (Python + MySQL)

This project analyzes historical U.S. unemployment data sourced from the Bureau of Labor Statistics (BLS), stored in a MySQL database, and visualized using Python.

---

## 🧱 Tech Stack

- **MySQL** – Stores structured unemployment data
- **Python** – Performs querying, analysis, and visualization
- **Pandas** – Handles data manipulation
- **Matplotlib** – Used for plotting graphs
- **Jupyter Notebook** – Interactive development
- **Sequel Ace** – Mac-based MySQL GUI client

---

## 📥 Dataset

used the **Monthly U.S. Unemployment Rate (UNRATE)** from the Federal Reserve Economic Data (FRED):

- Source: [https://fred.stlouisfed.org/series/UNRATE](https://fred.stlouisfed.org/series/UNRATE)
- Format: CSV with columns:
  - `DATE`: YYYY-MM-DD
  - `UNRATE`: Unemployment rate (%)

---

## 🔧 Setup

### 1. Install Dependencies

Use `pip` to install all required packages from the `requirements.txt` file:

```bash
pip install -r requirements.txt

### 2. Create a MySQL Database

```sql
CREATE DATABASE bls_db;
```

### 3. Create a Table

```sql
CREATE TABLE unemployment (
  id INT AUTO_INCREMENT PRIMARY KEY,
  report_date DATE,
  unemployment_rate FLOAT
);
```

### 4. Insert Data with Python

```python
conn = mysql.connector.connect(
    host=mysql_host,
    user=mysql_user,
    password=mysql_password,
    database=mysql_database
)

print("Connected successfully!")

df = pd.read_csv('UNRATE.csv')
df.columns = ['report_date', 'unemployment_rate']
df['report_date'] = pd.to_datetime(df['report_date'])

for _, row in df.iterrows():
    cursor.execute("""
        INSERT INTO unemployment (report_date, unemployment_rate)
        VALUES (%s, %s)
    """, (row['report_date'].date(), row['unemployment_rate']))
    
conn.commit()
print("Data inserted successfully.")

---

## 📈 Analysis

### 1. Load Data

```python
query = "SELECT report_date, unemployment_rate FROM unemployment ORDER BY report_date"
df = pd.read_sql(query, conn)
df['report_date'] = pd.to_datetime(df['report_date'])
```

### 2. Plot Unemployment Over Time

```python
plt.plot(df['report_date'], df['unemployment_rate'])
```

### 3. Detect & Highlight Spikes

```python
df['change'] = df['unemployment_rate'].diff()
df['is_spike'] = df['change'] >= 0.3  # Adjust threshold as needed
spikes = df[df['is_spike']]
```

```python
plt.plot(df['report_date'], df['unemployment_rate'], label="Unemployment Rate")
plt.scatter(spikes['report_date'], spikes['unemployment_rate'], color='red', label='Spike')
plt.legend()
```

---

## 📊 Next Steps

- Export graphs as reports
- Explore other BLS datasets (e.g., inflation, wages, job openings)
- Make it a reusable notebook template

---

## 💡 Project Goals

✔️ Practice database creation and queries  
✔️ Perform basic data analysis using pandas  
✔️ Visualize trends and detect anomalies (spikes)  
✔️ Integrate SQL + Python workflows for data science  

---

## 🧑‍💻 Author

Lee Ovalle  
July 31, 2025
