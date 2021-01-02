criar tabela seller_type
```sql
create table seller_type (
	id int(11),
    name varchar(30)
);
```
populando tabela seller_type
```sql
insert into seller_type (id, name) values (1,  'consumer');
insert into seller_type (id, name) values (2,  'seller');
```

criando tabela sellers
```sql
create table sellers (
	id int(11) primary key AUTO_INCREMENT,
    name varchar(30),
    dt_birthday date,
    dt_join datetime,
    seller_type_id int(11)
);
```

populando tabela sellers
```sql
insert into sellers (name, dt_birthday, dt_join, seller_type_id) values ('Gabriel', '1992-06-10', now(), 2 );
```

recuperando seller nascido em 1992
```sql
select * from sellers where year(dt_birthday)=1992;
```

```sql
SELECT CONCAT('2015-',FLOOR(1+RAND()*12),'-02');
```

```sql
SELECT FLOOR(RAND()*(2020-1990)+1990);
```

```sql
insert into sellers (name, dt_birthday, dt_join, seller_type_id) values (
	CONCAT('Teste -', 1 ), 
    CONCAT(FLOOR(RAND()*(2020-1990)+1990),'-',LPAD(FLOOR(1+RAND()*12),2,'0'),'-',FLOOR(RAND()*(28-1)+1)), 
    now(), 
    2 );
```

```sql
    drop procedure if exists load_foo_test_data;
delimiter #
create procedure load_foo_test_data()
begin
declare v_max int unsigned default 600;
declare v_counter int unsigned default 0;
  start transaction;
  while v_counter < v_max do
    insert into sellers (name, dt_birthday, dt_join, seller_type_id) values (
	CONCAT('Teste -', 1 ), 
    CONCAT(FLOOR(RAND()*(2020-1990)+1990),'-',LPAD(FLOOR(1+RAND()*12),2,'0'),'-',FLOOR(RAND()*(28-1)+1)), 
    now(), 
    2 );
    set v_counter=v_counter+1;
  end while;
  commit;
end #
delimiter ;
call load_foo_test_data();
```
puxar pelo signo - Áries
```sql
select concat('1990-', LPAD(month(dt_birthday), 2, '0'),'-',LPAD(day(dt_birthday), 2, '0')) from sellers 
	where  concat('1990-', LPAD(month(dt_birthday), 2, '0'),'-',LPAD(day(dt_birthday), 2, '0')) 
    between '1990-03-21' and '1990-04-20' ;
```
agrupar por signos
```sql
select 
	count(*) as qtd,
    case 
		WHEN concat('1990-', LPAD(month(dt_birthday), 2, '0'),'-',LPAD(day(dt_birthday), 2, '0')) between '1990-03-21' and '1990-04-20' THEN 'Aries' 
		WHEN concat('1990-', LPAD(month(dt_birthday), 2, '0'),'-',LPAD(day(dt_birthday), 2, '0')) between '1990-04-21' and '1990-05-20' THEN 'Touro'
		WHEN concat('1990-', LPAD(month(dt_birthday), 2, '0'),'-',LPAD(day(dt_birthday), 2, '0')) between '1990-05-21' and '1990-06-20' THEN 'Gemeos'
	end as signo
    from sellers
    group by signo;
```
