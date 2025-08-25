# 🏥 Healthcare Dashboard

![Dashboard Overview](https://github.com/sourav1174/Healthcare-Dashboard/blob/main/images/image1.PNG)  
![Detailed View](https://github.com/sourav1174/Healthcare-Dashboard/blob/main/images/image2.PNG)

## 📌 Project Objective
The main objective of this project is to build an **interactive Power BI Healthcare Dashboard** that helps track and analyze patient waiting lists across **Inpatient** and **Outpatient** categories.  

### Goals
- Track **current status of patient waiting list**
- Analyze **historical monthly trend** of waiting lists (2018–2021)
- Perform **detailed specialty-level & age profile analysis**
- Provide **summary view** for management and **detailed view** for granular analysis

---

## 📊 Data Scope
- **Time Period:** 2018 – 2021  
- **Categories:** Inpatient & Outpatient  
- **Key Metrics:**
  - Average & Median Waiting List
  - Current Total Wait List

---

## 🔗 Data Sources
Power BI supports 200+ connectors, but for this project we used:  

- **Excel / CSV (Folder Connector)** → Centralized folder hosting Inpatient & Outpatient data  
- Other possible connectors (not used here):  
  - SQL Server / Databases  
  - Power BI Services  
  - Azure, AWS, GCP  
  - Salesforce, SAP, SharePoint  
  - Web or JSON APIs  

---

## ⚙️ Data Collection & Import
1. Open **Power BI Desktop** → `Get Data` → Select **Folder**  
2. Browse and select the folder containing **Inpatient data** → Click **Combine & Load**  
3. Repeat the same process for **Outpatient data**  
4. Both datasets are now ready for transformation  

---

## 🛠️ Data Transformation (Power Query)
Steps performed in **Power Query Editor**:
- **Renaming Columns** → Align column names between Inpatient & Outpatient (e.g., `Specialty_Name` vs `Specialty`)  
- **Rearranging Columns** → Match order of columns between both datasets  
- **Adding Columns** → Created `Case_Type` column for Outpatient  
- **Appending Tables** → Combined Inpatient & Outpatient into single table `All_Data`  
- **Data Cleaning** → Replaced redundant values (e.g., `"18+ months"` vs `"18 month +"`) and trimmed blanks  

---

## 📐 Data Modelling
- Created relationships between `All_Data` and a **Specialty Mapping table**  
- Used **Specialty_Name** as the key for mapping  
- Hid the original inpatient/outpatient tables and used **All_Data** for reporting  

---

## 🎨 Visualization Blueprint
- **Summary Page:** High-level KPIs & trends  
- **Detailed Page:** Granular specialty & age-level breakdown  

---

## 📊 Key Measures (DAX)
```DAX
Latest Month Wait List =
CALCULATE(SUM(All_Data[Total]), All_Data[Archive_Date] = MAX(All_Data[Archive_Date])) + 0

PY Latest Month Wait List =
CALCULATE(SUM(All_Data[Total]), All_Data[Archive_Date] = EDATE(MAX(All_Data[Archive_Date]), -12)) + 0

Median Wait List = MEDIAN(All_Data[Total])
Average Wait List = AVERAGE(All_Data[Total])

Avg/Med Wait List =
SWITCH(
   VALUES('Calculation Method'[Calc Method]),
   "Average", [Average Wait List],
   "Median", [Median Wait List]
)
