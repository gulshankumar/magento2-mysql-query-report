# magento2-mysql-query-report

# query to get report of average fulfilment time by product
` SELECT `si`.sku, YEAR (`o`.created_at) 'Order Year',  COUNT(`si`.entity_id) 'item_shipments', 
CONCAT(FLOOR(SUM(TIMESTAMPDIFF(DAY, `o`.created_at, `os`.created_at))/COUNT(`si`.entity_id)), " day ", FLOOR(MOD(SUM(TIMESTAMPDIFF(HOUR, `o`.created_at, `os`.created_at))/COUNT(`si`.entity_id), 24)), ' hour') 'fulfilment_time'
FROM sales_shipment_item as `si` 
LEFT JOIN sales_shipment as `os` ON (`os`.entity_id = `si`.parent_id)
LEFT JOIN sales_order_item as `oi` ON (`oi`.item_id = `si`.order_item_id)
LEFT JOIN sales_order as `o` ON (`o`.entity_id = `oi`.order_id)
WHERE `o`.state = 'complete'
GROUP BY YEAR(`o`.created_at), `si`.product_id
Order by `si`.product_id, `o`.created_at; `
