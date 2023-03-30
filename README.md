# SQL performance tuning and query optimisation

## 1. Figure out what and why you need to optimise.<br><br>

## 2. Specify the fields that are needed instead of *
Instead of 
<pre>
select 
*
from table_name
</pre>

Use

<pre>
select 
col_1,col_2...
from table_name
</pre><br><br>

## 3. If you are using cross join and where filter, instead use inner join.
Instead of 
<pre>
select 
*
from table_1 a,table_2 b
where a.id=b.id
</pre>

Use

<pre>
select 
col_1,col_2...
from table_1 a
inner join table_2 b on a.id=b.id
</pre><br><br>

## 4. Create indexes to run queries with filters and aggregations faster.
If few set of columns are used regularly as filter or in join columns then create index in them.

<pre>
create index index_name
on table_name (col_1,col2..)
</pre>

Note that using indexes on production write tables might hamper you write response time.<br><br>

## 5. When creating complex queries always try and modularise your code into multiple CTE.
This would help you optimise individual logical parts and also narrow down the issues quickly when debugging.
<pre>
with sample_cte as 
(
    select 
    col_1,col2,sum(value) as sum_value
    from test_table
    group by 1,2
)
,
sample_cte_2 as 
(
    select 
    col_1,col2,sum(value) as sum_value
    from test_table
    group by 1,2
)
select 
*
from sample_cte a
inner join sample_cte_2 b on a.col_1=b.col_2
</pre>


## 6. Avoid using computation heavy view which are run frequently and turn them into a table.<br><br>

## 7. Make the very primary table without any duplicates, so that you never have to use `select distinct ... from ....` or `select col_1,col_2 ... from .... group by 1,2` type of statements.<br><br>

## 8. Avoid joins based on string columns rather use integer columns to join e.g. id columns.<br><br>

## 9. If you joining based on datetime or date columns, store another column as the `unix_epoch` of the datetime/date column.
<pre>
select 
date_part('epoch',current_date) 
;
</pre>

<pre>
1680134400
</pre><br><br>

## 10. Use `EXPLAIN ANALYZE` to time and analyse various version of you code.<br><br>

## 11. If data allows, set you tables to incremental update rather than truncate and load.<br><br>

## 12. If odering does not matter, don't use `oder by` clause.<br><br>

## 13. Maintain and update documentation of tables, views, procedure and data pipelines.<br><br>

## 14. Do regular catch up with peers and have a healthy knowledge sharing environment where everyone knows a common most optimised way of quering a particular data point.