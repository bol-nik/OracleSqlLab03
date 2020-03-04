1.	Вывести в таблицу  n данные о 5 сотрудниках с самыми высокими комиссионными.
DECLARE
cursor c1 is select office, sp_name, comm
from sperson, office where sperson.of_id=office.of_id
order by 3 desc;
BEGIN
delete from n;
for a in c1
LOOP
insert into n (office, sp_name, comm) values(a.office, a.sp_name,a.comm);
exit when c1%rowcount=5;
END LOOP;
end ;

2. Заполнить таблицу temp_table расчетными данными по формуле n1/(n2+n3) для каждого эксперимента. Исходные данные находятся в таблице data_table (id – номер эксперимента, 
n1,n2,n3 – показатели эксперимента).
DECLARE
cursor c1 is select id, n1,n2,n3 from data_table order by 1;
res temp_table.result%type;
BEGIN
delete from temp_table;
for i in c1
loop
res:=i.n1/(i.n2+i.n3);
insert into temp_table values(i.id, res);
end loop;
end ;

3.
create or replace procedure of_count(p_office in office.office%type) is
v_count integer;
e_office exception;
BEGIN
select count(*) into v_count from office where office=p_office;
if v_count=0 then raise e_office; end if;
select count(*) into v_count from sperson where of_id=(select of_id from office where office=p_office);
dbms_output.put_line('In ' || p_office ||' - '|| to_char(v_count));
EXCEPTION
when e_office then raise_application_error(-20001, 'No such office!!!');
end of_count;

Лабораторная работа № 7
1.	Задана следующая схема БД:
EMP		DEPT
e_no
e_name
date_hire
salary
address
d_no	
d_no
d_name
location

Создать подпрограммы, которые будут включать в себя следующие операции:
а) создание нового отдела;
б) прием на работу нового сотрудника;
в) увольнение с работы сотрудника по заданному номеру или по фамилии;
г) изменение зарплаты указанному сотруднику на заданную величину;
д) в заданном отделе определить число сотрудников получающих более 5000 в год;
е) определить название отдела по его номеру;
ж) подсчет общего жалования сотрудников для заданного отдела;
з) подсчет общего количества сотрудников для заданного отдела;
и) Реструктуризация фирмы - отделы с названием Sales перевести Dallas, остальные в New York.

