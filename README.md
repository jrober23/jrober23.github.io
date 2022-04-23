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
