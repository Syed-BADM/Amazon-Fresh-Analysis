# Amazon-Fresh-Analysis

--task 5 inserting 3 rows in products table
insert into products(product_id,product_name,category,sub_category,price_per_unit,stock_quantity,supplier_id) values
('23pkm040','Door vegetable','Vegetables','Sub-vegetables-4',50,20,'23pkm001'),
('23pkm041','We vegetable','Vegetables','Sub-vegetables-1',887,100,'23pkm002'),
('23pkm042','Rule Fruit','Fruits','Sub-Fruits-4',111,20,'23pkm001')

--task 6 update the stock quantity based on the product_id
select * from products

update products set stock_quantity=100 where product_id ='23pkm042'

-- task 7delete the supplier based on the specific city
select * from suppliers

delete from suppliers where city ='Schneidermouth'

--task 8
--check the reviews where comes under the 1 to 5 range of rating
select * from reviews

select * from reviews where rating between 1 and 5;


--assign a default command in prime members
select * from customer

ALTER TABLE customer
ALTER COLUMN prime_member
SET DEFAULT 'No';

insert into customer(customer_id,name,age,gender,city,state,country,sign_up_date)values('23pkm040','syed',21,'Male','sivakasi',
'Tamil Nadu','India','2025-01-21')

-- task 9 clauses and aggregations
--where

select * from orders where order_date > '2024-01-01'

--having

SELECT product_id, AVG(rating) AS Average_rating
FROM reviews
GROUP BY product_id
HAVING AVG(rating) > 4;

--group by 

select product_id, sum(quantity)
as Totalsales from order_details
group by product_id
order by totalsales desc;

--task 10
--Deduct stock from the Products table when a product is sold.
UPDATE products
SET stock_quantity = stock_quantity - 1
WHERE product_id = '23pkm040'

select * from products

--Insert a new row in the OrderDetails table for the sale.
--already insert in previously 

--task 11
--1
select order_details.order_id, sum(order_details.quantity * order_details.unit_price) as Total_Revenue
from order_details join orders on order_details.order_id = order_details.order_id 
group by order_details.order_id
order by Total_Revenue Desc;

--2

select orders.customer_id, count(order_id)
as Total_Orders from orders where orders.order_date between '2024-01-01' and '2025-01-01'
group by orders.customer_id
order by Total_Orders Desc;

--3

select products.supplier_id, sum(products.stock_quantity) as Total_unit_stock 
from products 
group by products.supplier_id
order by Total_unit_stock desc;

--task 12 Normalization

create table product_list(product_id text,category text,sub_category text, foreign key (product_id) references products(product_id))

INSERT INTO product_list(product_id,category,sub_category)
select distinct product_id,category,sub_category from products

Select * from product_list

--task 13 

--track the top 3 selling product based on product_id
select order_details.product_id, sum(order_details.quantity * order_details.unit_price) as Total_Revenue
from order_details join orders on order_details.product_id = order_details.product_id 
group by order_details.product_id
order by Total_Revenue Desc
limit 3

--track the non buying customers
select customer.customer_id,customer.name
from customer left join orders on customer.customer_id = orders.customer_id Where orders.order_id is null;

--task 14
--highest concentration of Prime members cities
select city, count(*)
as prime_members
from customer
where prime_member ='Yes'
group by city
order by prime_members desc
limit 10;

-- the top 3 most frequently ordered categories
select category, count(*)
as orders_count from products
group by category
order by orders_count desc
limit 3;

