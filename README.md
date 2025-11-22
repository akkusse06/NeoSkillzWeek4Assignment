# Awesome Chocolates Sales Analytics Dashboard
## Overview
This repository contains a Power BI dashboard for analyzing sales data from Awesome Chocolates, a fictional chocolate company. The dashboard provides insights into sales performance, product profitability, regional trends, and salesperson effectiveness.

The dashboard includes interactive visualizations to explore:
- Overall sales, costs, profits, and margins.
- Geographic and regional breakdowns.
- Product performance by category, margin, and volume.
- Salesperson and team rankings.
- Time-based trends.

Used field parameters for map and trends so that we can understand how sales,costs,profit,boxes,shipments affect them

# Features
- Multi-Page Dashboard:
  - Overview Page: High-level KPIs (e.g., Total Sales: $34M, Profit: $21M, Margin: 60.29%), geographic map with drill-down, regional bar chart, trend line for monthly performance, top products by profit margin, and top salespeople by profit.
  - Products Page: Product-specific KPIs (e.g., Total Products: 22, Avg Cost per Box: $6.65), detailed table with boxes, costs, profits, and margins; bubble chart for multi-dimensional analysis; donut chart for shipments by category; bar charts for sales and margins.
  - Salespersons Page: Salesperson KPIs (e.g., Total Salespersons: 25, Avg Revenue: $1.36M, Best Performer: Kelci Walkden), profile cards with photos, bar charts for team profits/boxes, line charts for monthly sales trends, and a detailed performance table.
  - Filters Page: Global slicers for date range, geography, team, category, region, and product to enable dynamic filtering across pages.
- Interactivity: Slicers, drill-downs, cross-filtering, and tooltips for deeper exploration.
- Visual Theme: Orange and brown color scheme inspired by chocolate, with icons for engagement.
- Bookmarks are provided for slicer panel and pages
- Calculations:
   - Total Sales = SUM(shipments[Sales])
   - Total Costs = SUM(shipments[Costs])
   - Total Boxes = SUM(shipments[Boxes])
   - Shipments = COUNTROWS(shipments)
   - Profit = [Total Sales] - [Total Costs]
   - Profit Margin % = DIVIDE([Profit], [Total Sales], 0)
   - Average Revenue per Person = AVERAGEX(VALUES( 'shipments'[Sales Person] ),[Total Sales])
   - Avg Cost Per Box = AVERAGE(products[Cost per box])
   - Label Placeholder = 0
   - Bar chart labels = 
    VAR v_type = SELECTEDVALUE(people[Sales person])                                                                         
    VAR profit = FORMAT([Profit],"#,##0,.0K")                                                                                
    VAR profit_margin = FORMAT([Profit Margin %],"0.00%")                                                                
RETURN                                                                                     
    v_type & " "& "|" & " " & profit & " "& "|" & " "& profit_margin
   - Best Performer Name =                                                                                         
VAR RankingTable = ADDCOLUMNS(VALUES( 'ShipmentS'[Sales Person] ),"@Revenue", [Total Sales])                                              
VAR TopPerson =  TOPN(1,RankingTable[@Revenue], DESC)                                                                   
RETURN MAXX( TopPerson, 'shipments'[Sales Person] )
   - Best Performer Photo URL =                                                                                  
VAR RankingTable = ADDCOLUMNS(VALUES( 'shipments'[Sales Person] ),"@Revenue", [Total Sales])                                        
VAR TopPerson = TOPN(1,RankingTable,[@Revenue], DESC,'shipments'[Sales Person], ASC)                                           
VAR TopSalesPersonName = MAXX( TopPerson, 'shipments'[Sales Person])                                                 
RETURN                                                             
    CALCULATE(SELECTEDVALUE( 'people'[Picture] ),'people'[Sales person] = TopSalesPersonName)                            
  - Highest Margin Product = 
CALCULATE(SELECTEDVALUE(products[Product]),TOPN(1,products,DIVIDE([Profit],[Total Sales],0),DESC))
  - Lowest Margin Product = 
CALCULATE(SELECTEDVALUE(products[Product]),TOPN(1,products,DIVIDE([Profit],[Total Sales],0),ASC))
  - Max Team With Profit = 
CALCULATE( SELECTEDVALUE(people[Team]), TOPN(1,people,CALCULATE([Profit]),DESC))
  - Total SalesPerson = DISTINCTCOUNT(people[Sales person])
