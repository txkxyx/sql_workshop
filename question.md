# 追加問題

ある会社の社員の管理と、プロジェクトの管理を行うデータベースを作成します。
データベース名は`company`とします。

## データベース定義

- 会社(company)

    | データベース名(物理) | データベース名(論理) |
    |---|---|
    | company | 会社 |

## テーブル定義

- 部門(department)

    | 列名(物理) | 列名(論理) | データ型　| 備考 |
    |---|---|---|---|
    | dno | 部門番号 | int | PK |
    | dname | 部門名 | varchar(10) | NN UK |

- プロジェクト(project)

    | 列名(物理) | 列名(論理) | データ型　| 備考 |
    |---|---|---|---|
    | pno | プロジェクト番号 | int | PK |
    | pname | プロジェクト名 | varchar(20) | NN |

- 社員(employee)

    | 列名(物理) | 列名(論理) | データ型　| 備考 |
    |---|---|---|---|
    | eno | 社員番号 | int | Primary Key |
    | ename | 社員名 | varchar(20) | NN |
    | sex | 性別 | char(1)| CH(M:男性 W:女性) |
    | hire_date | 入社日 | date | |
    | salary | 給料 | int | |
    | address | 住所 | varchar(10) | |
    | dno | 部門番号 | int | FK:department(dno) |
    | pno | プロジェクト番号 | int | FK:project(pno) |

## データ定義

- 部門(department)

    | dno | dname |
    |---|---|
    | 10 | Develop |
    | 20 | Management |
    | 30 | Jinji |
    | 40 | Eigyo |

- プロジェクト(project)

    | pno | pname |
    | --- | --- |
    | 1 | Twitter |
    | 2 | Facebook |
    | 3 | Instagram |

- 社員(employee)

    | eno | ename | sex | hire_date | salary | address | dno | pno |
    |---|---|---|---|---|---|---|---|
    | 1 | TANAKA | M | 1990/10/23 | 400000 | OSAKA |  | |
    | 2 | KIMURA | M | 1999/6/1 | 300000 | HYOGO | 10 | 1 |
    | 3 | SUZUKI | W | 2000/2/9 | 340000 | OSAKA | 30 | 2 |
    | 4 | IKEDA | M | 2001/12/21 | 290000 | HIROSHIMA | 20 | 1 |
    | 5 | YAMADA | W | 2004/7/18 | 310000 | SHIGA | 40 | 3 |
    | 6 | SHIMADA | M | 2005/10/16 | 270000 | KYOTO | 10 | 2 |
    | 7 | AONO | W | 2007/11/26 | 290000 | KYOTO | 10 | 1 |
    | 8 | FUJIWARA | M | 2010/8/13 | 250000 | OSAKA | 10 | 3 |
    | 9 | FUJII | W | 2014/3/10 | 280000 | HYOGO | 10 | 2 |
    | 10 | YAMAMOTO | M | 2015/1/7 | 240000 | KYOTO | 20 | 3 |

## 問題

### データベース作成

1. データベースを作成せよ。

### テーブル作成

1. テーブル定義を満たすDDLを作成せよ。

### データ作成

1. データ定義に記載されているレコードを作成するDMLを作成せよ。

### ソート・絞り込み

1. 社員の中から部署に所属していない社員(社長)を検索せよ。

    | eno | ename | sex | hire_date | salary | address | dno | pno |
    |---|---|---|---|---|---|---|---|---|
    |   1 | TANAKA | M    | 1990-10-23 | 400000 | OSAKA   | NULL | NULL |

2. 社員名に「A」を含む社員の社員名と給料を検索せよ。その際、給料の高い社員順に検索せよ。

    | ename    | salary |
    | --- | ---|
    | TANAKA   | 400000 |
    | YAMADA   | 310000 |
    | KIMURA   | 300000 |
    | IKEDA    | 290000 |
    | SHIMADA  | 270000 |
    | FUJIWARA | 250000 |

3. 社員の中から給料が`280000`以外の社員の名前、入社日、給料、部門番号を検索せよ。その際、部門番号は小さい順に並び替え、同じ部門番号の場合は入社日が新しい社員から並び替えて表示せよ。

    | ename    | hire_date  | salary | dno  |
    |---|---|---|---|
    | TANAKA   | 1990-10-23 | 400000 | NULL |
    | FUJII    | 2014-03-10 | 280000 |   10 |
    | FUJIWARA | 2010-08-13 | 250000 |   10 |
    | AONO     | 2007-11-26 | 290000 |   10 |
    | SHIMADA  | 2005-10-16 | 270000 |   10 |
    | KIMURA   | 1999-06-01 | 300000 |   10 |
    | YAMAMOTO | 2015-01-07 | 240000 |   20 |
    | IKEDA    | 2001-12-21 | 290000 |   20 |
    | SUZUKI   | 2000-02-09 | 340000 |   30 |
    | YAMADA   | 2004-07-18 | 310000 |   40 |

### 関数・グループ化

1. 社員の中から、住所ごとの社員の人数を検索せよ。

    | address   | count |
    |---|---|
    | OSAKA     |     3 |
    | HYOGO     |     2 |
    | HIROSHIMA |     1 |
    | SHIGA     |     1 |
    | KYOTO     |     3 |


2. 社員表から、部門ごとの給料の合計が`500000`以下の部門番号と給料の合計を検索せよ。ただし、社長のデータは含めないこと。

    | dno  | sum    |
    |---|---|
    |   30 | 340000 |
    |   40 | 310000 |

3. 社員表から、部門が`Develop`に所属している社員データから、各住所の性別ごとの給料の合計を検索せよ。検索結果は住所ごとにまとめ、同じ住所の場合は給料の合計が高い順に並び替えよ。

    | address | sex  | sum    |
    |---|---|---|
    | HYOGO   | M    | 300000 |
    | HYOGO   | W    | 280000 |
    | KYOTO   | W    | 290000 |
    | KYOTO   | M    | 270000 |
    | OSAKA   | M    | 250000 |

### 結合

1. 社員表から社員名と、所属している部署名を検索せよ。検索結果は部署番号が高い順に表示せよ。

    | ename    | dname      |
    |---|---|
    | YAMADA   | Eigyo      |
    | SUZUKI   | Jinji      |
    | YAMAMOTO | Management |
    | IKEDA    | Management |
    | KIMURA   | Develop    |
    | FUJIWARA | Develop    |
    | SHIMADA  | Develop    |
    | FUJII    | Develop    |
    | AONO     | Develop    |

2. 社員表から社員名と、所属している部署名と、参画しているプロジェクト名を検索せよ。

    | ename    | dname      | pname     |
    |---|---|---|
    | KIMURA   | Develop    | Twitter   |
    | IKEDA    | Management | Twitter   |
    | AONO     | Develop    | Twitter   |
    | SUZUKI   | Jinji      | Facebook  |
    | SHIMADA  | Develop    | Facebook  |
    | FUJII    | Develop    | Facebook  |
    | YAMADA   | Eigyo      | Instagram |
    | FUJIWARA | Develop    | Instagram |
    | YAMAMOTO | Management | Instagram |

3. 2.の検索結果を部署番号が高い順に表示し、同じ部署の場合はプロジェクト番号が低い順に表示せよ。

    | ename    | dname      | pname     |
    |---|---|---|
    | YAMADA   | Eigyo      | Instagram |
    | SUZUKI   | Jinji      | Facebook  |
    | IKEDA    | Management | Twitter   |
    | YAMAMOTO | Management | Instagram |
    | KIMURA   | Develop    | Twitter   |
    | AONO     | Develop    | Twitter   |
    | FUJII    | Develop    | Facebook  |
    | SHIMADA  | Develop    | Facebook  |
    | FUJIWARA | Develop    | Instagram |
