# SQL
## WAREHOUSE FULFILLMENT SUMMARY

Wrote a complex query to join an Order Summary table and a Warehouse table to create a Warehouse Fulfillment Summary table to track the number of total orders completed by each warehouse. There were 9999 total orders and 7 warehouses. The warehouse with the lowest total orders was located in Knoxville, TN while the warehouse with the highest total was located in Lansing, MI. 

```
SELECT 
   Warehouse.warehouse_id,
   CONCAT(Warehouse.state, ': ', Warehouse.warehouse_alias) AS warehouse_name,
   COUNT(Orders.order_id) AS number_of_orders,
   (SELECT 
     COUNT(*)
    FROM warhouse_orders.Orders Orders)
   AS total_orders,
   CASE 
    WHEN COUNT(Orders.order_id)/ (SELECT  COUNT(*) FROM warhouse_orders.Orders Orders) <= 0.20
    THEN "fulfilled 0-20% of Orders"
    WHEN COUNT(Orders.order_id)/ (SELECT  COUNT(*) FROM warhouse_orders.Orders Orders) > 0.20
    AND COUNT(Orders.order_id)/ (SELECT  COUNT(*) FROM warhouse_orders.Orders Orders) <= 0.60
    THEN "Fulfilled 21-60% of Orders"
   ELSE "Fulfilled more than 60% of Orders"
   END AS fulfillment_summary
FROM warhouse_orders.Warehouse AS Warehouse
LEFT JOIN  warhouse_orders.Orders AS Orders
  ON Orders.warehouse_id = Warehouse.warehouse_id
GROUP BY 
  Warehouse.warehouse_id,
  warehouse_name
HAVING
  COUNT(Orders.order_id) > 0
```  
# Tableau

You can view Tableau projects below.

[Just the Data - World Happiness](https://public.tableau.com/views/JusttheData-WorldHappiness_16454917106770/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link)

[WrestleMania Attendance Stats](https://public.tableau.com/views/WrestleManiaAttendanceStats/Sheet1?:language=en-US&:display_count=n&:origin=viz_share_link)

[Global CO2 Emissions](https://public.tableau.com/views/GlobalCO2Emissions_16454879666530/Sheet1?:language=en-US&:display_count=n&:origin=viz_share_link)

# R/Python
## Palmer Pengiuns

Created a visualization comparing the body mass and flipper length of three species of penguins: Adelie, Chinstrap, Gentoo. The males and females of each species were also compared. The males of each species were larger than their respective females. However, the Gentoo females were larger than both the Adelie, and Chinstrap males. The Gentoo was the overall larger species. 

```
install.packages("ggplot2")
install.packages("palmerpenguins")

library(ggplot2)
library(palmerpenguins)

data("penguins")
View(penguins)

ggplot(data = penguins)+
  geom_smooth(mapping = aes(x=flipper_length_mm, y=body_mass_g, linetype=species), color="purple")

ggplot(data = penguins)+geom_point(mapping = aes(x=flipper_length_mm, y=body_mass_g, alpha=species), color="purple")+geom_smooth(mapping = aes(x=flipper_length_mm, y=body_mass_g))
ggplot(data = penguins)+geom_jitter(mapping = aes(x=flipper_length_mm, y=body_mass_g, alpha=species), color="purple")


ggplot(data = diamonds)+
  geom_bar(mapping = aes(x=cut, fill=clarity))

ggplot(data = penguins)+geom_point(mapping = aes(x=flipper_length_mm, y=body_mass_g, alpha=species), color="purple")+facet_wrap(~species)

ggplot(data = diamonds)+
  geom_bar(mapping = aes(x=color, fill=cut))+facet_wrap(~cut)

ggplot(data = penguins)+geom_point(mapping = aes(x=flipper_length_mm, y=body_mass_g, color=species))+facet_grid(~sex)

ggsave("Three Penguin Species.png")
```
![Three Penguin Species](https://user-images.githubusercontent.com/11672093/164946552-69bbdb2f-f767-4998-94c4-9b513515b694.png)
