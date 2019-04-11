# 追加問題回答

## データベース作成

1. データベースを作成せよ。

    ```sql
    create database development;
    use development
    ```

## テーブル作成

1. テーブル定義を満たすDDLを作成せよ。

    ```sql
    create table m_role (
        rno int primary key,
        rname varchar(50) unique not null
    );

    create table m_project(
        pno int primary key,
        pname varchar(50) not null
    );

    create table m_employee(
        eno int primary key,
        ename varchar(20) not null,
        hire_date date,
        salary int,
        address varchar(20)
    );

    create table t_project(
        id int primary key auto_increment,
        pno int,
        eno int,
        rno int,
        foreign key (pno) references m_project(pno),
        foreign key (eno) references m_employee(eno),
        foreign key (rno) references m_role(rno)
    );
    ```

## データ作成

1. データ定義に記載されているレコードを作成するDMLを作成せよ。

    ```sql
    insert into m_role(rno,rname) values (10,'Project Manager');
    insert into m_role(rno,rname) values (20,'Project Leader');
    insert into m_role(rno,rname) values (30,'System Engineer');
    insert into m_role(rno,rname) values (40,'Programmer');

    insert into m_project(pno,pname) values(1,'Web Application');
    insert into m_project(pno,pname) values(2,'iOS Application');
    insert into m_project(pno,pname) values(3,'Android Application');

    insert into m_employee(eno,ename,hire_date,salary,address) values (1,'TANAKA','1990/10/23',400000,'Osaka');
    insert into m_employee(eno,ename,hire_date,salary,address) values (2,'KIMURA','1999/6/1',300000,'Hyogo');
    insert into m_employee(eno,ename,hire_date,salary,address) values (3,'SUZUKI','2000/2/9',340000,'Osaka');
    insert into m_employee(eno,ename,hire_date,salary,address) values (4,'IKEDA','2001/12/21',290000,'Okayama');
    insert into m_employee(eno,ename,hire_date,salary,address) values (5,'YAMADA','2004/7/18',310000,'Kyoto');
    insert into m_employee(eno,ename,hire_date,salary,address) values (6,'SHIMADA','2005/10/16',270000,'Hyogo');
    insert into m_employee(eno,ename,hire_date,salary,address) values (7,'AONO','2007/11/26',290000,'Kyoto');
    insert into m_employee(eno,ename,hire_date,salary,address) values (8,'FUJIWARA','2010/8/13',250000,'Osaka');
    insert into m_employee(eno,ename,hire_date,salary,address) values (9,'FUJII','2014/3/10',280000,'Okayama');
    insert into m_employee(eno,ename,hire_date,salary,address) values (10,'YAMAMOTO','2015/1/7',240000,'Kyoto');

    insert into t_project(pno,eno,rno) values (1,1,10);
    insert into t_project(pno,eno,rno) values (1,3,20);
    insert into t_project(pno,eno,rno) values (1,5,30);
    insert into t_project(pno,eno,rno) values (1,9,40);
    insert into t_project(pno,eno,rno) values (1,10,40);
    insert into t_project(pno,eno,rno) values (2,2,10);
    insert into t_project(pno,eno,rno) values (2,4,20);
    insert into t_project(pno,eno,rno) values (2,6,30);
    insert into t_project(pno,eno,rno) values (2,8,40);
    insert into t_project(pno,eno,rno) values (3,1,10);
    insert into t_project(pno,eno,rno) values (3,2,20);
    insert into t_project(pno,eno,rno) values (3,7,30);
    insert into t_project(pno,eno,rno) values (3,10,40);
    ```

## ソート・絞り込み

1. 社員の中から2000年代に入社した社員を検索せよ。

    ```sql
    select * from m_employee where hire_date >= '2000/1/1';
    ```

2. 社員名に`A`を含む社員の社員名と給料を検索せよ。その際、給料の高い社員順に検索せよ。

    ```sql
    select ename, salary from m_employee where ename like '%A' order by salary desc;
    ```

3. 社員の中から給料が`280000`以上の社員の名前、入社日、給料を検索せよ。その際、給料は昇順で並び替え、同じ給料の場合は入社日が新しい社員から並び替えて表示せよ。

    ```sql
    select ename, hire_date, salary from m_employee where salary >= 280000 order by salary asc, hire_date desc;
    ```

## 関数・グループ化

1. 社員の中から、2000年代に入社した社員の平均給料を検索せよ。

    ```sql
    select avg(salary) as average from m_employee where hire_date >= '2000/1/1';
    ```

2. 社員表から、住所ごとの給料の合計が`600000`以上の住所と給料の合計を検索せよ。

    ```sql
    select address, sum(salary) as sum_salary from m_employee group by address having sum_salary > 600000;
    ```

## 結合

1. 社員表とプロジェクト管理表から、プロジェクト番号と社員名の一覧を検索せよ。またプロジェクト番号が高い順に表示せよ。

    ```sql
    select pno, ename from m_employee inner join t_project on m_employee.eno = t_project.eno order by pno desc;
    ```

2. 1.の結果から、プロジェクト番号をプロジェクト名に変えて表示せよ。

    ```sql
    select pname, ename from t_project inner join m_employee on t_project.eno = m_employee.eno inner join m_project on t_project.pno = m_project.pno order by t_project.pno desc;
    ```

3. 全てのプロジェクトにおいて、プロジェクトごとの社員名とその役割名の一覧を検索せよ。さらに、プロジェクトの並び順はプロジェクト番号が高い順で、役割の並び順は役割番号が低い順に表示せよ。

    ```sql
    select pname, ename, rname from t_project join m_employee on t_project.eno = m_employee.eno inner join m_project on t_project.pno = m_project.pno inner join m_role on t_project.rno = m_role.rno order by t_project.pno desc, t_project.rno asc;
    ```

## 副問い合わせ

1. 社員表の中から、`SHIMADA`さん(社員番号が`6`)よりも前に入社した社員の、社員名と入社日を検索せよ。

    ```sql
    select ename, hire_date from m_employee where hire_date < (select hire_date from m_employee where eno = 6);
    ```

2. 社員表の中から、給料が全社員の平均給料よりも高い社員の人数を検索せよ。

    ```sql
    select count(*) as sum from m_employee where salary > (select avg(salary) from m_employee);
    ```