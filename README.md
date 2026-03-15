# 🕷️ Web Scraping — List of Universities in India (Wikipedia)

A Python web scraping project that extracts structured university data from Wikipedia using BeautifulSoup and requests, and exports it to a clean CSV file for analysis.

---

## 📌 Project Overview

**Business Question:** How are universities distributed across Indian states by type — Central, State, Deemed, and Private?

This project scrapes the Wikipedia page for "List of Universities in India", parses the HTML table, structures the data into a pandas DataFrame, and exports it as a CSV — demonstrating the full web scraping pipeline from raw HTML to analysis-ready data.

---

## 🔧 What I Did

### 1. Fetched the Web Page
Used `requests` to send an HTTP GET request and loaded the HTML response into BeautifulSoup for parsing.

```python
from bs4 import BeautifulSoup
import requests

url = "https://en.wikipedia.org/wiki/List_of_universities_in_India"
page = requests.get(url)
soup = BeautifulSoup(page.text, 'html')
```

### 2. Located the Target Table
Identified the correct `wikitable` from multiple tables on the page using `find_all()` with a class filter.

```python
table = soup.find_all('table', class_='wikitable')[0]
```

### 3. Extracted Column Headers
Scraped all `<th>` elements and stripped whitespace to use as DataFrame column names.

```python
state_titles = [title.text.strip() for title in table.find_all('th')]
```

### 4. Extracted Row Data
Iterated over each `<tr>`, extracted all `<td>` cell values, and appended each row to the DataFrame.

```python
for row in column_data[1:]:
    row_data = row.find_all('td')
    individual_row_data = [data.text.strip() for data in row_data]
    df.loc[len(df)] = individual_row_data
```

### 5. Exported to CSV
Saved the final DataFrame as a CSV for downstream analysis or visualisation.

```python
df.to_csv('universities_india.csv', index=False)
```

---

## 📈 Output Data

The scraped dataset contains **34 rows** (one per Indian state/territory + totals) and **6 columns**:

| Column | Description |
|---|---|
| State | Indian state or union territory |
| Central Universities | Count of centrally funded universities |
| State Universities | Count of state government universities |
| Deemed Universities | Count of deemed-to-be universities |
| Private Universities | Count of private universities |
| Total | Total universities in the state |

**Key figures from the scraped data:**
- India has **1,072 universities** across all types
- **Gujarat** leads overall with 95 universities — driven by 60 private universities
- **West Bengal** has the most state universities (37)
- **Tamil Nadu** has the most deemed universities (28)
- **Delhi** has the most central universities (7)

---

## 🛠️ Tools & Libraries

| Library | Purpose |
|---|---|
| `requests` | Fetches the raw HTML from the web page |
| `BeautifulSoup` | Parses HTML and extracts table elements |
| `pandas` | Structures the data and exports to CSV |

---

