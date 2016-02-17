# Introduction #

Now have now installed and tested the GDataSource package. You now want to add your own DataSources to query.


# Adding your DataSources #

The schema.sql file creates a table (gdatasources). In order to use your own datasources you just need to insert new ones in this table. For example, for a list of employees names:

```

insert into gdatasources values ('emp-names','select first_name, last_name from hr.employees');
commit;
grant select on hr.employees to scott;
```


---

NOTE: OraGoods doesn't really care of what you insert, it can be any query with inline views or whatever, but the outer select clause is parsed, so it must complain with these rules:

1) if two or more columns have the same name, use the AS keyword to name them uniquely: select a.id AS myfirstid, b.id AS mysecondid ... ;

2) if your query contains functions then use an inline view to wrap them, ej: select col1, mysum2 from (select col1, sum(col2) AS mysum2 ... );

---


Don't forget to grant the proper privileges if the user your are using is not the owner of the tables.

Then just call the datasource by it's name: http://your-db-server:8080/your_dad/gdatasource.get_json?p_datasource_id=emp-names

You can also filter using the Google Query Language: http://your-db-server:8080/your_dad/gdatasource.get_json?p_datasource_id=emp-names&tq=select%20first_name%20where%20last_name%20%3D%20%22tiger%22

Here we encoded tq with the encodeURIComponent() javascript function. tq actually is: <b>select first_name where last_name = "tiger"</b>