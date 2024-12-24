![image](https://www.scudata.com/images/data-analysis/0.png)

<div align="center">
    <h2>Novel grid-style programming</h2>
</div> 

<div align="center">
    <h3>Excel-like Grid-style programming</h3>
</div> 

![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/1.png)

---
 

![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/2.gif)

---

<div align="center">
    <h3>High Interactivity for Exploratory Analysis</h3>
</div>  

![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/4.gif)

---

<div align="center">
    <h3>XLL Plugin Helps Excel</h3>
</div>  


Write SPL code in Excel directly

> **Finding periods during which stocks have risen consecutively for more than 5 days**

```
=spl("=E(?1).sort(CODE,DT).group@i(CODE!=CODE[-1]||CL<CL[-1]).select(~.len()>=5).conj()",A1:D253)
```
 
<div align="center"> 
    <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/5.png" width="60%" />
</div>
 
---

<div align="center">
    <h2>Concise and Powerful Code</h2>
</div> 

<div align="center">
    <h3>Comprehensive and Simple Operations</h3>
</div> 

![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/6.png)

---

<div align="center">
    <h3>Unique Set and Ordered Operations</h3>
</div> 

> **calculate the longest consecutive rising days for each stock**

#### SQL

```sql
SELECT CODE, MAX(con_rise) AS longest_up_days
FROM (
    SELECT CODE, COUNT(*) AS con_rise
    FROM (
        SELECT CODE, DT,  SUM(updown_flag) OVER (PARTITION BY CODE ORDER BY CODE, DT) AS no_up_days
        FROM (
            SELECT CODE, DT, 
                    CASE WHEN CL > LAG(CL) OVER (PARTITION BY CODE ORDER BY CODE, DT)  THEN 0
                    ELSE 1 END AS updown_flag
            FROM stock
        )
    )
    GROUP BY CODE, no_up_days
)
GROUP BY CODE
```
#### Python
```Python
import pandas as pd
stock_file = "StockRecords.txt"
stock_info = pd.read_csv(stock_file,sep="\t")
stock_info.sort_values(by=['CODE','DT'],inplace=True)
stock_group = stock_info.groupby(by='CODE')
stock_info['label'] = stock_info.groupby('CODE')['CL'].diff().fillna(0).le(0).astype(int).cumsum()
max_increase_days = {}
for code, group in stock_info.groupby('CODE'):
    max_increase_days[code] = group.groupby('label').size().max() – 1
max_rise_df = pd.DataFrame(list(max_increase_days.items()), columns=['CODE', 'max_increase_days'])
```

#### SPL
 

|   | A |
| --- | --- |
| 1 | StockRecords.xlsx |
| 2 | =T(A1).sort(DT) |
| 3 | =A2.group(CODE;\~.group@i(CL< CL[-1]).max(~.len()):max_increase_days) |

**Especially skilled at complex scenarios such as order-related operations, sliding windows, and cross-row computations, much simpler than SQL or Python**


[What to use for data analysis programming: SPL,Python or SPL?](https://www.scudata.com/langding-page-scientist/)

---

<div align="center">
    <h3>Easy Big Data and Parallel Support</h3>
</div> 

![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/8.png)


---

<div align="center">
    <h3>Lightweight and Portable</h3>
</div>  

<div align="center"> 
    <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/9.png" width="60%" />
</div>

---

# Resource

- [Read the e-book "SPL Programming"](https://www.scudata.com/html/SPL-programming-book.html)
- [Watch the video "SPL video course"](https://www.scudata.com/html/SPL-programming-course.html)
- [Download esProc Desktop](https://www.scudata.com/download-desktop/)
- [Community - c.scudata.com](https://c.scudata.com/)
