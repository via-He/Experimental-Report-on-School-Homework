一、程序理解题：请仔细阅读下列PL/SQL程序块，写出各程序实现的功能。（4小题，每小题5分，共20分）
1．程序1（5分）
declare
  my_toy_price toys.toyprice%type;
  cursor toy_cur is
    select toyprice from toys
    where toyprice<250;
begin
  open toy_cur;
  loop
    fetch toy_cur into my_toy_price;
    exit when toy_cur%notfound;
    dbms_output.put_line (toy_cur%rowcount || '. 玩具单价:' || my_toy_price);
  end loop;
  close toy_cur;
end;
/

2．程序2（5分）
declare 
e_2292 exception;
e_no_rows exception;
pragma exception_init(e_2292, -2292)
begin
     delete from dept where deptno=&dno;
     if sql%notfound then
       raise e_no_rows;
     end if;
     exception
       when e_2292 then
         dbms_output.put_line(‘There are employees in this department’);
       when e_no_rows then
         dbms_output.put_line(‘no this department’);
end;
/

3．程序3（5分）
declare
type worker_record_type is record 
(
     id number(3),
     name varchar2(20)
);
  worker_record worker_record_type;
begin
  worker_record.id:=10;
  worker_record.name:='jack';
  dbms_output.put_line(worker_record.id || ':' || worker_record.name);
end;

4．程序4（5分）
create or replace trigger trg_add_emp
before insert on emp
begin
if to_char(sysdate, ‘DY’, ‘nls_date_language=american’) in (‘SAT’, ‘SUN’) then
    raise_application_error(-20001, ‘only add emp on work days’);
end if;
if to_char(sysdate, ‘HH24’) not between 9 and 17 then
    raise_application_error(-20002, ‘only add emp on work time’);
end if;
end;
/


二、问答题（4小题，每小题5分，共20分）
1. 简述Oracle系统的体系结构。（5分）

2. 简述Tablespace和Datafile之间的关系。（5分）

3. Oracle数据库提倡以空间换取性能，试举2个例子说明该特性。（5分）

4. Oracle的物理备份是什么？逻辑备份是什么？二者有什么区别？（5分）


三、编程题（4小题，每小题15分，共60分）
现有表emp和dept，结构如下所示：
emp
字段含义	字段名	字段类型	空值	约束
员工编号	eno	number(4)	not null	PK
员工姓名	ename	varchar2(20)	not null	
性别	sex	varchar2(1)		
工资	sal	number(7,2)	not null	
职位	job	varchar2(40)	not null	
部门编号	dno	number(2)	not null	FK

dept
字段含义	字段名	字段类型	空值	约束
部门编号	dno	number(2)	not null	PK
部门名称	dname	varchar2(40)	not null	
所在地址	loc	varchar2(20)		

1、设计一个函数，功能是根据指定的员工编号获取该员工的姓名，要有异常处理。（15分）

2、设计一个存储过程，为表emp实现新增数据的功能，在数据插入前需进行判断，若工资<1000则不允许插入，并给出异常提示信息。（15分）

3、设计一个触发器。当删除dept的数据时，应先删除emp表中对应部门的员工信息，才能进行dept的数据删除。（15分）

4、公司将根据员工的职位为每个员工加薪，设计一个程序块实现该功能。（15分）
Job        Raise
-------------------------
Clerk       500
Salesman    1000
Analyst      1500
其他职位    800
