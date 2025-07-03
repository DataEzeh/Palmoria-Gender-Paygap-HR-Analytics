# KPI and DAX Measure List — Palmoria HR Analytics (Power BI)

This document contains all calculated KPIs and DAX formulas used in the Palmoria Group HR analytics dashboard.

---

## Gender & Employee Insights

### Total Employees
Total Employees = DISTINCTCOUNT(Employees[Name])

### % Female Employees
% Female = 
DIVIDE(
    CALCULATE(COUNTROWS(Employees), Employees[Gender] = "Female"),
    CALCULATE(COUNTROWS(Employees), Employees[Gender] IN {"Male", "Female", "Unspecified"})
)

---

## Salary & Pay Gap Analysis

### Average Salary
Average Salary = AVERAGE(Employees[Salary])

### Gender Pay Gap
Gender Pay Gap = 
CALCULATE(AVERAGE(Employees[Salary]), Employees[Gender] = "Male") -
CALCULATE(AVERAGE(Employees[Salary]), Employees[Gender] = "Female")

### Employees Below Minimum Wage
Below Min Wage Count = 
CALCULATE(COUNTROWS(Employees), Employees[Salary] < 90000)

---

## Bonus & Compensation

### Bonus Amount (Column)
Bonus Amount = 
VAR BonusRate = 
LOOKUPVALUE(
    BonusRules[Bonus %],
    BonusRules[Rating],
    Employees[Rating]
)
RETURN
Employees[Salary] * BonusRate

### Total Compensation (Column)
Total Compensation = Employees[Salary] + Employees[Bonus Amount]

### Total Bonus Paid (Measure)
Total Bonus = SUM(Employees[Bonus Amount])

### Total Compensation (Measure)
Total Compensation (Measure) = SUM(Employees[Total Compensation])

---

---

---

## Salary Band (Power Query Conditional Column)

This column was created using Power Query, not DAX.

### Steps:

1. Open Power BI → Click “Transform Data” to open Power Query
2. Go to "Add Column" → Select "Conditional Column"
3. Name the column: **Salary Band**
4. Add the following conditions:

| If              | Column Name | Operator     | Value             | Output        |
|-----------------|-------------|--------------|-------------------|---------------|
| If              | Salary      | is less than | 10000             | Below 10k     |
| Else if         | Salary      | is between   | 10000 and 20000   | 10k - 20k     |
| Else if         | Salary      | is between   | 21000 and 30000   | 21k - 30k     |
| Else if         | Salary      | is between   | 31000 and 40000   | 31k - 40k     |
| Else if         | Salary      | is between   | 41000 and 50000   | 41k - 50k     |
| Else if         | Salary      | is between   | 51000 and 60000   | 51k - 60k     |
| Else if         | Salary      | is between   | 61000 and 70000   | 61k - 70k     |
| Else if         | Salary      | is between   | 71000 and 80000   | 71k - 80k     |
| Else if         | Salary      | is between   | 81000 and 90000   | 81k - 90k     |
| Else            | (no condition) |            |                   | Above 90k     |

5. Click **OK**, then **Close & Apply**

This column allows you to categorize employee salaries into clear bands for reporting.  
You can now use this Salary Band in visuals like bar charts, stacked columns, or maps.


---

End of file.
