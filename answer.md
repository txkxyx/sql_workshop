# 追加問題回答

## データベース作成

1. データベースを作成せよ。

    ```sql
    create database company;
    use company
    ```

## テーブル作成

1. テーブル定義を満たすDDLを作成せよ。

    ```sql
    create table department (
        dno int primary key,
        dname varchar(10) unique not null
    );

    create table project(
        pno int primary key,
        pname varchar(20) not null
    );

    create table employee(
        eno int primary key,
        ename varchar(20) not null,
        sex char(1) check(sex in ('W','M')),
        hire_date date,
        salary int,
        address varchar(10),
        dno int ,
        pno int ,
        foreign key (dno) references department(dno),
        foreign key (pno) references project(pno)
    );
    ```

## データ作成

1. データ定義に記載されているレコードを作成するDMLを作成せよ。

    ```sql
    insert into department(dno,dname) values (10,'Develop');
    insert into department(dno,dname) values (20,'Management');
    insert into department(dno,dname) values (30,'Jinji');
    insert into department(dno,dname) values (40,'Eigyo');

    insert into project(pno,pname) values(1,'Twitter');
    insert into project(pno,pname) values(2,'Facebook');
    insert into project(pno,pname) values(3,'Instagram');

    insert into employee(eno,ename,sex,hire_date,salary,address,dno,pno) values (1,'TANAKA','M','1990/10/23',400000,'OSAKA',null,null);
    insert into employee(eno,ename,sex,hire_date,salary,address,dno,pno) values (2,'KIMURA','M','1999/6/1',300000,'HYOGO',10,1);
    insert into employee(eno,ename,sex,hire_date,salary,address,dno,pno) values (3,'SUZUKI','W','2000/2/9',340000,'OSAKA',30,2);
    insert into employee(eno,ename,sex,hire_date,salary,address,dno,pno) values (4,'IKEDA','M','2001/12/21',290000,'HIROSHIMA',20,1);
    insert into employee(eno,ename,sex,hire_date,salary,address,dno,pno) values (5,'YAMADA','W','2004/7/18',310000,'SHIGA',40,3);
    insert into employee(eno,ename,sex,hire_date,salary,address,dno,pno) values (6,'SHIMADA','M','2005/10/16',270000,'KYOTO',10,2);
    insert into employee(eno,ename,sex,hire_date,salary,address,dno,pno) values (7,'AONO','W','2007/11/26',290000,'KYOTO',10,1);
    insert into employee(eno,ename,sex,hire_date,salary,address,dno,pno) values (8,'FUJIWARA','M','2010/8/13',250000,'OSAKA',10,3);
    insert into employee(eno,ename,sex,hire_date,salary,address,dno,pno) values (9,'FUJII','W','2014/3/10',280000,'HYOGO',10,2);
    insert into employee(eno,ename,sex,hire_date,salary,address,dno,pno) values (10,'YAMAMOTO','M','2015/1/7',240000,'KYOTO',20,3);
    ```

## ソート・絞り込み

1. 社員の中から部署に所属していない社員(社長)を検索せよ。

    ```sql
    select * from employee where dno is null;
    ```

2. 社員名に`A`を含む社員の社員名と給料を検索せよ。その際、給料の高い社員順に検索せよ。

    ```sql
    select ename, salary from employee where ename like '%A' order by salary desc;
    ```

3. 社員の中から給料が`280000`以外の社員の名前、入社日、給料、部門番号を検索せよ。その際、部門番号は小さい順に並び替え、同じ部門番号の場合は入社日が新しい社員から並び替えて表示せよ。

    ```sql
    select ename, hire_date, salary, dno from employee order by dno asc, hire_date desc;
    ```

## 関数・グループ化

1. 社員の中から、住所ごとの社員の人数を検索せよ。

    ```sql
    select address, count(*) as count from employee group by address;
    ```

2. 社員表から、部門ごとの給料の合計が`500000`以下の部門番号と給料の合計を検索せよ。ただし、社長のデータは含めないこと。

    ```sql
    select dno, sum(salary) as sum from employee group by dno having sum <= 500000 and dno is not null;
    ```

3. 社員表から、部門が`Develop`に所属している社員データから、各住所の性別ごとの給料の合計を検索せよ。検索結果は住所ごとにまとめ、同じ住所の場合は給料の合計が高い順に並び替えよ。

    ```sql
    select address, sex, sum(salary) as sum from employee where dno = 10 group by address, sex order by address, sum desc;
    ```

## 結合

1. 社員表から社員名と、所属している部署名を検索せよ。検索結果は部署番号が高い順に表示せよ。

    ```sql
    select ename, dname from employee left join department on employee.dno = department.dno order by employee.dno desc;
    ```

2. 社員表から社員名と、所属している部署名と、参画しているプロジェクト名を検索せよ。ただし社長は検索対象から外すこと。

    ```sql
    select ename, dname, pname from employee inner join department on employee.dno = department.dno inner join project on employee.pno = project.pno;
    ```

3. 2.の検索結果を部署番号が高い順に表示し、同じ部署の場合はプロジェクト番号が低い順に表示せよ。

    ```sql
    select ename, dname, pname from employee inner join department on employee.dno = department.dno inner join project on employee.pno = project.pno order by employee.dno desc, employee.pno asc;
    ```

## 副問い合わせ

1. 社員表の中から、`SHIMADA`さん(社員番号が`6`)よりも前に入社した社員の、社員名と入社日を検索せよ。

    ```sql
    select ename, hire_date from employee where hire_date < (select hire_date from employee where eno = 6);
    ```

2. 社員表の中から、給料が全社員の平均給料よりも高い社員の人数を検索せよ。

    ```sql
    select count(*) as sum from employee where salary > (select avg(salary) from employee);
    ```