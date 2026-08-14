##  Dashboard Preview
<p align="center">
  <img src="./vaccination.png" alt="Vaccination Dashboard" width="100%">
</p>


  # 📊 Project Overview
An interactive Vaccination Campaign Dashboard developed using Power BI to track and monitor a hospital vaccination campaign, showing registered employees, external entities, vaccine stock, and overall campaign progress in real time.

The dashboard provides a comprehensive, live view of campaign completion and helps stakeholders monitor vaccine supply against the number of people vaccinated.

## 🎯 Project Objectives
The main objectives of this dashboard are to:
- Monitor the total number of people registered for vaccination.
- Track campaign completion progress against a fixed target.
- Monitor remaining vaccine stock relative to people vaccinated.
- Track the remaining days left in the vaccination campaign.
- Compare vaccinated hospital employees against external entities.
- Provide a clear, real-time, visually engaging status of the campaign.
- Reinforce awareness through an animated messaging component.

## 📌 Dashboard KPIs
The dashboard includes key performance indicators focused on campaign progress, including:
- Total Peoples (Employees + External Entities)
- Campaign Completion % (Vaccinations Progress Bar)
- Remaining Vaccine Stock %
- Remaining Campaign Days

## 📈 Dashboard Analysis

### 👥 Registered People
The dashboard analyzes people registered for vaccination based on:
- Hospital employees (specialty, department, vaccination date, entry date)
- External entities (health status, entity name, vaccination date, entry date)

This breakdown helps identify how many people from each group have been processed.

### 💉 Vaccine Stock
The dashboard tracks:
- Quantity of vaccines received
- Quantity of vaccines dispensed
- Vaccine location
- Remaining stock relative to number of people vaccinated

This helps ensure supply keeps pace with campaign progress and flags shortages early.

### ⏳ Campaign Timeline
The dashboard tracks:
- Campaign start date and end date
- Days passed vs. days remaining
- Overall time-based progress percentage

This helps stakeholders understand how much of the campaign timeline remains.

### 📈 Daily Entry Statistics
The dashboard tracks day-by-day and weekly entry statistics, including:
- Number of people entered per day
- Target number required
- Completion rate per period

This allows monitoring of daily campaign pace against targets.

## 🧮 DAX Measures
The dashboard uses DAX measures to calculate campaign KPIs, and combines them with custom HTML/CSS to render animated, color-coded progress indicators instead of Power BI's default visuals.

**Total_Peoples**
```dax
COUNT('موظفي المستشفى'[الرقم]) + COUNT('الجهات الخارجية'[الرقم])
```
Counts all registered people by combining hospital employees and external entities into a single total.

**vaccinations (Campaign Completion Progress Bar)**
```dax
VAR TotalValue = 21901
VAR CurrentValue = [total_peoples]
VAR Percentage = DIVIDE(CurrentValue, TotalValue, 0) * 100

VAR BarColor =
    SWITCH(
        TRUE(),
        Percentage < 50, "#e63946",
        Percentage < 75, "#f6b400",
        "#2e7d32"
    )

RETURN
"<div>...animated progress bar HTML...</div>"
```
Calculates the percentage of people vaccinated against a fixed campaign target (21,901), and renders an animated, color-coded progress bar (red / yellow / green) via HTML/CSS.

**Progress_bar (Remaining Vaccine Stock)**
```dax
VAR TotalValue = Sum('اللقاحات'[كمية اللقاح الوارد])
VAR CurrentValue = Sum('اللقاحات'[كمية اللقاح الوارد]) - [total_peoples]
VAR Percentage = DIVIDE(CurrentValue, TotalValue, 0) * 100
...
```
Calculates the percentage of vaccine stock still remaining (total received minus people already vaccinated), using the same animated progress bar styling.

**Remaining_Days_bar**
```dax
VAR StartDate = DATE(2025, 9, 14)
VAR EndDate   = DATE(2026, 4, 4)
VAR TodayDate = TODAY()

VAR TotalDays = EndDate - StartDate
VAR PassedDays = TodayDate - StartDate
VAR Percentage = DIVIDE(PassedDays, TotalDays, 0) * 100
VAR RemainingDays = EndDate - TodayDate
...
```
Calculates the total campaign duration, days passed, and days remaining relative to today's date (`TODAY()`), displayed as an animated progress bar.

**تطعيمك (Awareness Message)**
```dax
"<div>... 💚 تطعيمك أمانك وأمان عائلتك .. لا تأجّل ...</div>"
```
A static, non-numeric measure that returns an animated (fade-in/fade-out) HTML awareness banner encouraging people not to delay vaccination.

## 🎨 Custom HTML/CSS Visualizations (Technique)
Instead of using Power BI's default visuals, this dashboard renders fully custom animated components through DAX + the **HTML Content** visual:
1. Each measure first calculates the required value using standard DAX (`DIVIDE`, `SUM`, `COUNT`, date functions).
2. A `SWITCH(TRUE(), ...)` statement dynamically selects a bar color (red/yellow/green) based on the calculated percentage.
3. The measure concatenates the calculated values into a complete HTML string (`<div>` with inline CSS: width, color, border-radius, box-shadow) using `&`, so the markup updates automatically with the data.
4. A `<style>` block with `@keyframes` animates the progress fill and the awareness message.
5. The resulting HTML/CSS string is rendered live inside the report using the HTML Content visual.

## 💡 Key Insights
- The dashboard gives a real-time, single-glance view of vaccination campaign progress.
- Comparing vaccine stock against people vaccinated helps anticipate supply shortages before they happen.
- Tracking remaining campaign days alongside completion percentage highlights whether the campaign is on pace to finish on time.
- Splitting registered people between hospital employees and external entities enables group-level monitoring.
- Custom HTML/CSS visuals provide a more engaging, branded presentation than Power BI's default indicators.

## 🛠️ Tools & Technologies
- Power BI
- DAX
- HTML/CSS
- Data Modeling
- Data Analysis
- Data Visualization
- Power Query
- Interactive Dashboard Development
