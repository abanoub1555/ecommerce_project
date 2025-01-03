# ecommerce_project
Order_item_fact 
CREATE TABLE order_item_sales_fact (
    order_id VARCHAR(50),
    order_item_id int,
    product_id VARCHAR(50),
    seller_id VARCHAR(50),
    pickup_limit_date_id int,
    pickup_limit_time_id int,
    price int,
    shipping_cost int
);


Time_Dim
CREATE TABLE time_dim (
    time_id int,
    time time,
    hour  int,
    Interval_1 varchar(50)
)








Order_fact
CREATE TABLE orders_fact (
    order_id VARCHAR(50),
    user_id int,
    order_status VARCHAR(50),
    order_date_id int,
    order_time_id int,
    order_approved_date_id int,
    order_approved_time_id int,
    pickup_date_id int,
    pickup_time_id int,
    delivered_date_id int,
    delivered_time_id int,
    estimated_time_delivery_id int
);

Feedback_fact
CREATE TABLE feedback_fact (
    order_id VARCHAR(50),
    feedback_id VARCHAR(50),
    feedback_score INT,
    feedback_form_sent_date_id INT,
    feedback_form_sent_time_id INT,
    feedback_answer_date_id INT,
    feedback_answer_time_id INT
);



Payment_fact
CREATE TABLE payment_fact (
    order_id VARCHAR(50),
    payment_sequential int,
    payment_type VARCHAR(50),
    payment_installments int,
    payment_value float
)






















What is the peak season?
Monthly 
select count(test.order_fact.order_id)as OrderCount_monthly , test.date_dim.month_desc 
from 
	test.order_fact 
join 
	test.date_dim
on test.order_fact.order_date_id = test.date_dim.date_id
group by 
	test.date_dim.month_desc
the result
OrderCount_monthly	month
4305	September
4959	October
5674	December
8069	January
8508	February
9893	March
9343	April
10573	May
9412	June
10318	July






quarterly

select count(test.order_fact.order_id)as OrderCount_quarterly , test.date_dim.quarter_desc 
from 
	test.order_fact 
join 
	test.date_dim
on test.order_fact.order_date_id = test.date_dim.date_id
group by 
	test.date_dim.quarter_desc


result
OrderCount_quarterly	querter
25466	Qtr 03
18177	Qtr 04
26470	Qtr 01
29328	Qtr 02









i.	What time users are most likely make an order or using the ecommerce app?
select count(test.order_fact.order_id)as hourlyorders_count , test.time_dim_n.hour 
from 
	test.order_fact
join 
	test.time_dim_n
on
	test.order_fact.order_time_id = test.time_dim_n.time_id
group by 
	test.time_dim_n.hour

result
hourlyorders_count	The hour 
2394	0
1170	1
510	2
272	3
206	4
188	5
502	6
1231	7
2967	8
4785	9
6177	10
6578	11
5995	12
6518	13
6569	14
6454	15
6675	16
6150	17
5769	18
5982	19
6193	20
6217	21
5816	22
4123	23

























i.	What is the preferred way to pay in the ecommerce?

select count(test.payment_fact.payment_type) , test.payment_fact.payment_type
from 
	test.payment_fact
group by 
	test.payment_fact.payment_type

result


count(test.payment_fact.payment_type)	payment_type
76795	credit_card
19784	blipay
5775	voucher
1529	debit_card
3	not_defined











i.	How many installment is usually done when paying in the ecommerce?
select count(test.payment_fact.payment_installments) , test.payment_fact.payment_installments
from 
	test.payment_fact
group by 
	test.payment_fact.payment_installments

result
Count(payment_installment)	Payment_installments
4268	8
52546	1
12413	2
10461	3
3920	6
5239	5
7098	4
5328	10
1626	7
133	12
644	9
16	13
74	15
18	24
23	11
27	18
15	14
17	20
3	21
8	17
1	22
2	0
5	16
1	23



















i.	What is the average spending time for user for our ecommerce?

ii.	What is the average spending time for user for our ecommerce?
select count(test.order_fact.order_id) , test.user_dim_n.customer_state
from 
	test.order_fact
join 
	test.user_dim_n
on 
	test.order_fact.user_id=test.user_dim_n.user_id
group by 
	 test.user_dim_n.customer_state


result
count(test.order_fact.order_id)	Customer_
8586	JAWA TENGAH
12825	JAWA BARAT
21200	BANTEN
12536	DKI JAKARTA
1688	KALIMANTAN TIMUR
2377	SULAWESI SELATAN
8446	JAWA TIMUR
1085	JAMBI
1561	RIAU
1801	PAPUA
2150	SUMATERA SELATAN
3928	SUMATERA UTARA
484	KALIMANTAN UTARA
1565	KALIMANTAN BARAT
1228	SULAWESI UTARA
891	SULAWESI TENGGARA
1064	KALIMANTAN TENGAH
1675	LAMPUNG
1758	DI YOGYAKARTA
1256	NUSA TENGGARA TIMUR
487	MALUKU
1420	BALI
988	SULAWESI TENGAH
1521	KALIMANTAN SELATAN
1886	SUMATERA BARAT
712	PAPUA BARAT
726	KEPULAUAN RIAU
470	SULAWESI BARAT
827	ACEH
529	MALUKU UTARA
577	BENGKULU
377	NUSA TENGGARA BARAT
479	GORONTALO
338	KEPULAUAN BANGKA BELITUNG



i.	Which logistic route that have heavy traffic in our ecommerce?


SELECT AVG(DATEDIFF(
    STR_TO_DATE(test.order_fact.delivered_date_id, '%Y%m%d'),
    STR_TO_DATE(test.order_fact.pickup_date_id, '%Y%m%d'))
) AS date_diff,
test.user_dim_n.customer_state 
FROM test.order_fact
join test.user_dim_n
on test.order_fact.user_id=test.user_dim_n.user_id
group by test.user_dim_n.customer_state

result
date_diff	Customer_state
5.9919	BANTEN
7.8554	RIAU
8.3295	KALIMANTAN TIMUR
9.4088	JAWA TENGAH
9.4259	SULAWESI SELATAN
9.4576	DI YOGYAKARTA
9.4604	GORONTALO
9.5799	JAWA TIMUR
9.6094	KALIMANTAN BARAT
9.6617	KALIMANTAN SELATAN
9.6899	JAWA BARAT
10.2744	SUMATERA BARAT
10.4181	SULAWESI TENGAH
10.4477	SUMATERA UTARA
10.5041	DKI JAKARTA
10.5707	BENGKULU
10.6859	KALIMANTAN UTARA
10.9650	MALUKU UTARA
10.9760	SUMATERA SELATAN
10.9895	MALUKU
11.1277	JAMBI
11.1939	KALIMANTAN TENGAH
11.2867	PAPUA BARAT
11.4390	NUSA TENGGARA BARAT
11.4752	ACEH
11.5654	LAMPUNG
11.6003	PAPUA
11.6908	SULAWESI UTARA
11.8101	NUSA TENGGARA TIMUR
11.8312	SULAWESI TENGGARA
11.8345	KEPULAUAN RIAU
11.9628	SULAWESI BARAT
12.3219	BALI
12.9697	KEPULAUAN BANGKA BELITUNG




i.	How many late delivered order in our ecommerce? Are late order affecting the customer satisfaction?

SELECT DATEDIFF(
    STR_TO_DATE(test.order_fact.estimated_time_delivery_id, '%Y%m%d'),
    STR_TO_DATE(test.order_fact.delivered_date_id, '%Y%m%d')
) AS date_diff,
test.order_fact.order_id ,  test.order_fact.user_id ,test.feedback_fact.feedback_score 
FROM test.order_fact
join test.feedback_fact
on test.order_fact.order_id=test.feedback_fact.order_id
having date_diff < 0


and this (if I need to know the number of late orders)
SELECT 
    COUNT(*) AS row_count
FROM 
    test.order_fact
JOIN 
    test.feedback_fact 
    ON test.order_fact.order_id = test.feedback_fact.order_id
WHERE 
    DATEDIFF(
        STR_TO_DATE(test.order_fact.estimated_time_delivery_id, '%Y%m%d'),
        STR_TO_DATE(test.order_fact.delivered_date_id, '%Y%m%d')
    ) < 0;

Result
Count_of_lateorders
6563

And this measuring the average score fore the late orders 
SELECT 
    AVG(test.feedback_fact.feedback_score) AS avg_feedback_score
FROM 
    test.order_fact
JOIN 
    test.feedback_fact 
    ON test.order_fact.order_id = test.feedback_fact.order_id
WHERE 
    DATEDIFF(
        STR_TO_DATE(test.order_fact.estimated_time_delivery_id, '%Y%m%d'),
        STR_TO_DATE(test.order_fact.delivered_date_id, '%Y%m%d')
    ) < 0;

Result
avg_feedback_score
2.2560


