二、问答题（4小题，每小题5分，共20分）
1. 简述Oracle系统的体系结构。（5分）


2. 简述Tablespace和Datafile之间的关系。（5分）
一个tablespace可以有一个或多个datafile,每个datafile只能在一个tablespace内,

　　table中的数据,通过hash算法分布在tablespace中的各个datafile中,

　　tablespace是逻辑上的概念,datafile则在物理上储存了数据库的种种对象。

3. Oracle数据库提倡以空间换取性能，试举2个例子说明该特性。（5分）
典型应用一：控制用户所占用的表空间配额

在一些大型的数据库应用中，我们需要控制某个用户或者某一组用户其所占用的磁盘空间。这就好像在文件服务器中，需要为每个用户设置磁盘配额一样，以防止硬盘空间耗竭。所以，在数据库中，我们也需要限制用户所可以使用的磁盘空间大小。为了达到这个目的，我们就可以通过表空间来实现。

我们可以在Oracle数据库中，建立不同的表空间，为其设置最大的存储容量，然后把用户归属于这个表空间。如此的话，这个用户的存储容量，就受到这个表空间大小的限制。

典型应用二：控制数据库所占用的磁盘空间

有时候，在Oracle数据库服务器中，可能运行的不止一个服务。除了数据库服务器外，可能还有邮件服务器等应用系统服务器。为此，就需要先对Oracle数据库的磁盘空间作个规划，否则，当多个应用程序服务所占用的磁盘空间都无限增加时，最后可能导致各个服务都因为硬盘空间的耗竭而停止。所以，在同一台服务器上使用多个应用程序服务，我们往往需要先给他们进行磁盘空间的规划和分配。各个服务都不能够超过我们分配给他的最大限额，或者超过后及时的提醒我们。只有这样，才能够避免因为磁盘空间的耗竭而导致各种应用服务的崩溃。

典型应用三：灵活放置表空间，提高数据库的输入输出性能

数据库管理员还可以将不同类型的数据放置到不同的表空间中，这样可以明显提高数据库输入输出性能，有利于数据的备份与恢复等管理工作。因为我们数据库管理员在备份或者恢复数据的时候，可以按表空间来备份数据。如在设计一个大型的分销系统后台数据库的时候，我们可以按省份建立表空间。与浙江省相关的数据文件放置在浙江省的表空间中，北京发生业务记录，则记录在北京这个表空间中。如此，当浙江省的业务数据出现错误的时候，则直接还原浙江省的表空间即可。很明显，这样设计，当某个表空间中的数据出现错误需要恢复的时候，可以避免对其他表空间的影响。

另外，还可以对表空间进行独立备份。当数据库容量比较大的时候，若一下子对整个数据库进行备份，显然会占用比较多的时间。虽然说Oracle数据库支持热备份，但是在备份期间，会占用比较多的系统资源，从而造成数据库性能的下降。为此，当数据库容量比较大的时候，我们就需要进行设置多个表空间，然后规划各个表空间的备份时间，从而可以提高整个数据库的备份效率，降低备份对于数据库正常运行的影响。

典型应用四：大表的排序操作

我们都知道，当表中的记录比较多的时候，对他们进行查询，速度会比较慢。第一次查询成功后，若再对其进行第二次重新排序，仍然需要这么多的时间。为此，我们在数据库设计的时候，针对这种容量比较大的表对象，往往把它放在一个独立的表空间，以提高数据库的性能。

典型应用五：日志文件与数据文件分开放，提高数据库安全性

默认情况下，日志文件与数据文件存放在同一表空间。但是，这对于数据库安全方面来说，不是很好。所以，我们在数据库设计的过程中，往往喜欢把日志文件，特别是重做日志文件，放在一个独立的表空间中，然后把它存放在另外一块硬盘上。如此的话，当存放数据文件的硬盘出现故障时，能够马上通过存放在另一个表空间的重做日志文件，对数据库进行修复，以减少企业因为数据丢失所带来的损失。

4. Oracle的物理备份是什么？逻辑备份是什么？二者有什么区别？（5分）
物理备份是指将数据库的所有物理文件完整拷贝到备份位置的一个过程。物理备份是所有物理文件的一个副本，例如，数据文件、控制文件、归档日志等。该副本能被存储在本地磁盘或磁带。物理备份是备份或恢复的基础，包括冷备份（非归档模式）和热备份（归档模式）。物理备份既可以在数据库打开的状态下进行也可在数据库关闭的状态下进行，但是逻辑备份和恢复则只能在数据库打开的状态下进行。
逻辑备份是指使用工具exp或expdp将数据库对象的结构和数据导出到二进制文件的过程。当数据库对象被误操作而损坏后就可以使用工具imp或impdp利用备份的文件把数据对象导入到数据库中进行恢复。逻辑备份是物理备份方式的一种补充，多用于数据迁移。

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

1、 设计一个函数，功能是根据指定的员工编号获取该员工的姓名，要有异常处理。（15分）
CREATE OR REPLACE PROCEDURE test(aaa emp.empno%type) is
v_emp emp%rowtype;
begin
begin
select ename, job
into v_emp.ename, v_emp.job
from emp
where empno = aaa;
exception
when no_data_found then
raise_application_error(-20005, '没有该编号人员');
when others then
raise_application_error(-20005, '执行失败');
end;
dbms_output.put_line(v_emp.ename || v_emp.job);
end;

2、设计一个存储过程，为表emp实现新增数据的功能，在数据插入前需进行判断，若工资<1000则不允许插入，并给出异常提示信息。（15分）
(1)declare 
v_sal emp.sal%type := '&v_sal';
begin
	   if(v_sal<1000)
dbms_output.put_line(v_sal); 
exception 
when v_sal>1000then 
raise_application_error(-20005, '工资大于等于1000不允许插入 '); 
end	;
/	
(2)declare 
v_sal emp.sal%type := '&v_sal';
begin
	   if(v_sal<1000)
dbms_output.put_line(v_sal); 
else
dbms_output.put_line(‘出错’);
end	;
/	
。
3、设计一个触发器。当删除dept的数据时，应先删除emp表中对应部门的员工信息，才能进行dept的数据删除。（15分）
create or replace trigger mytrigger
	after delete on dept
		for each row
	begin
	delete * from emp where dno=:old.dno;
	end;
4、公司将根据员工的职位为每个员工加薪，设计一个程序块实现该功能。（15分）
Job        Raise
-------------------------
Clerk       500
Salesman    1000
Analyst      1500
其他职位    800
declare 
  cursor cemp is select eno,job from emp;
  eno emp.eno%type;
  job emp.job%type;
begin
  rollback;  
  open cemp;  
  loop
       fetch cemp into eno , job;
       exit when cemp%notfound;
       if job = 'Clerk' then 
update emp 
set sal=sal+1000 
where eno=emp.eno;
        else if  
job = ‘Salesman' then 
update emp 
set sal=sal+1000 
where eno=emp.eno;
else if  
job =’Analyst ' then 
update emp 
set sal=sal+1500 
where eno=emp.eno;
          else 
update emp 
set sal=sal+800 
where eno=emp.eno;
       end if;
end loop;
close cemp;
  commit;
   dbms_output.put_line('完成');
end;
/

