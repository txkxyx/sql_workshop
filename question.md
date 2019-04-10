# 追加問題

ある会社の開発部の社員の管理と、プロジェクトの管理を行うデータベースを作成します。
データベース名は`development`とします。

## データベース定義

- 会社(development)

    | データベース名(物理) | データベース名(論理) |
    |---|---|
    | development | 開発部 |

## テーブル定義

- 役割(m_role)

    | 列名(物理) | 列名(論理) | データ型　| 備考 |
    |---|---|---|---|
    | rno | 役割番号 | int | PK |
    | rname | 役割名 | varchar(10) | NN UK |

- プロジェクト(m_project)

    | 列名(物理) | 列名(論理) | データ型　| 備考 |
    |---|---|---|---|
    | pno | プロジェクト番号 | int | PK |
    | pname | プロジェクト名 | varchar(20) | NN |

- 社員(m_employee)

    | 列名(物理) | 列名(論理) | データ型　| 備考 |
    |---|---|---|---|
    | eno | 社員番号 | int | Primary Key |
    | ename | 社員名 | varchar(20) | NN |
    | hire_date | 入社日 | date | |
    | salary | 給料 | int | |
    | address | 住所 | varchar(20) | |

- プロジェクト管理(t_project)

    | 列名(物理) | 列名(論理) | データ型　| 備考 |
    |---|---|---|---|
    | id | 管理ID | int | PK 自動連番 |
    | pno | プロジェクト番号 | int | FK:m_project(pno) |
    | eno | 社員番号 | int | FK:m_employee(eno) |
    | rno | 役割番号 | int | FK:m_role(rno) |

## データ定義

- 役職(m_role)

    | rno | rname |
    |---|---|
    | 10 | Project Manager |
    | 20 | Project Leader |
    | 30 | System Engineer |
    | 40 | Programmer |

- プロジェクト(m_project)

    | pno | pname |
    | --- | --- |
    | 1 | Web Application |
    | 2 | iOS Application |
    | 3 | Android Application |

- 社員(m_employee)

    | eno | ename | hire_date | salary | address |
    |---|---|---|---|---|
    | 1 | TANAKA | 1990/10/23 | 400000 | Osaka |
    | 2 | KIMURA | 1999/6/1 | 300000 | Hyogo |
    | 3 | SUZUKI | 2000/2/9 | 340000 | Osaka |
    | 4 | IKEDA | 2001/12/21 | 290000 | Okayama |
    | 5 | YAMADA | 2004/7/18 | 310000 | Kyoto |
    | 6 | SHIMADA | 2005/10/16 | 270000 | Hyogo |
    | 7 | AONO | 2007/11/26 | 290000 | Kyoto |
    | 8 | FUJIWARA | 2010/8/13 | 250000 | Osaka |
    | 9 | FUJII | 2014/3/10 | 280000 | Okayama |
    | 10 | YAMAMOTO | 2015/1/7 | 240000 | Kyoto |

- プロジェクト管理(t_project)
    | id |pno | eno | rno |
    |---|---|---|---|
    | 1 | 1 | 1 | 10 |
    | 2 | 1 | 3 | 20 |
    | 3 | 1 | 5 | 30 |
    | 4 | 1 | 9 | 40 |
    | 5 | 1 | 10 | 40 |
    | 6 | 2 | 2 | 10 |
    | 7 | 2 | 4 | 20 |
    | 8 | 2 | 6 | 30 |
    | 9 | 2 | 8 | 40 |
    | 10 | 3 | 1 | 10 |
    | 11 | 3 | 2 | 20 |
    | 12 | 3 | 7 | 30 |
    | 13 | 3 | 10 | 40 |

## 問題

### データベース作成

1. データベースを作成せよ。

### テーブル作成

1. テーブル定義を満たすDDLを作成せよ。

### データ作成

1. データ定義に記載されているレコードを作成するDMLを作成せよ。

### ソート・絞り込み

1. 社員の中から2000年代に入社した社員を検索せよ。

    | eno | ename    | hire_date  | salary | address |
    |---|---|---|---|
    |   3 | SUZUKI   | 2000-02-09 | 340000 | Osaka   |
    |   4 | IKEDA    | 2001-12-21 | 290000 | Okayama |
    |   5 | YAMADA   | 2004-07-18 | 310000 | Kyoto   |
    |   6 | SHIMADA  | 2005-10-16 | 270000 | Hyogo   |
    |   7 | AONO     | 2007-11-26 | 290000 | Kyoto   |
    |   8 | FUJIWARA | 2010-08-13 | 250000 | Osaka   |
    |   9 | FUJII    | 2014-03-10 | 280000 | Okayama |
    |  10 | YAMAMOTO | 2015-01-07 | 240000 | Kyoto   |

2. 社員名に「A」を含む社員の社員名と給料を検索せよ。その際、給料の高い社員順に検索せよ。

    | ename    | salary |
    | --- | ---|
    | TANAKA   | 400000 |
    | YAMADA   | 310000 |
    | KIMURA   | 300000 |
    | IKEDA    | 290000 |
    | SHIMADA  | 270000 |
    | FUJIWARA | 250000 |

3. 社員の中から給料が`280000`以上の社員の名前、入社日、給料を検索せよ。その際、給料は昇順で並び替え、同じ給料の場合は入社日が新しい社員から並び替えて表示せよ。

    | ename    | hire_date  | salary |
    |---|---|---|
    | FUJII  | 2014-03-10 | 280000 |
    | AONO   | 2007-11-26 | 290000 |
    | IKEDA  | 2001-12-21 | 290000 |
    | KIMURA | 1999-06-01 | 300000 |
    | YAMADA | 2004-07-18 | 310000 |
    | SUZUKI | 2000-02-09 | 340000 |
    | TANAKA | 1990-10-23 | 400000 |

### 関数・グループ化

1. 社員の中から、2000年代に入社した社員の平均給料を検索せよ。

    | average     |
    |---|
    | 283750.0000 |

2. 社員表から、住所ごとの給料の合計が`600000`以上の住所と給料の合計を検索せよ。

    | address | sum_salary |
    |---|---|
    | Kyoto   |     840000 |
    | Osaka   |     990000 |

### 結合

1. 社員表とプロジェクト管理表から、プロジェクト番号と社員名の一覧を検索せよ。またプロジェクト番号が高い順に表示せよ。

    | pno  | ename    |
    |---|---|
    |    3 | AONO     |
    |    3 | TANAKA   |
    |    3 | YAMAMOTO |
    |    3 | KIMURA   |
    |    2 | SHIMADA  |
    |    2 | IKEDA    |
    |    2 | FUJIWARA |
    |    2 | KIMURA   |
    |    1 | TANAKA   |
    |    1 | YAMAMOTO |
    |    1 | SUZUKI   |
    |    1 | YAMADA   |
    |    1 | FUJII    |

2. 1.の結果から、プロジェクト番号をプロジェクト名に変えて表示せよ。

    | pname               | ename    |
    |---|---|
    | Android Application | TANAKA   |
    | Android Application | YAMAMOTO |
    | Android Application | AONO     |
    | Android Application | KIMURA   |
    | iOS Application     | FUJIWARA |
    | iOS Application     | SHIMADA  |
    | iOS Application     | IKEDA    |
    | iOS Application     | KIMURA   |
    | Web Application     | SUZUKI   |
    | Web Application     | TANAKA   |
    | Web Application     | YAMAMOTO |
    | Web Application     | FUJII    |
    | Web Application     | YAMADA   |

3. 全てのプロジェクトにおいて、プロジェクトごとの社員名とその役割名の一覧を検索せよ。さらに、プロジェクトの並び順はプロジェクト番号が高い順で、役割の並び順は役割番号が低い順に表示せよ。

    | pname               | ename    | rname           |
    |---|---|---|
    | Android Application | TANAKA   | Project Manager |
    | Android Application | KIMURA   | Project Leader  |
    | Android Application | AONO     | System Engineer |
    | Android Application | YAMAMOTO | Programmer      |
    | iOS Application     | KIMURA   | Project Manager |
    | iOS Application     | IKEDA    | Project Leader  |
    | iOS Application     | SHIMADA  | System Engineer |
    | iOS Application     | FUJIWARA | Programmer      |
    | Web Application     | TANAKA   | Project Manager |
    | Web Application     | SUZUKI   | Project Leader  |
    | Web Application     | YAMADA   | System Engineer |
    | Web Application     | YAMAMOTO | Programmer      |
    | Web Application     | FUJII    | Programmer      |

### 副問い合わせ

1. 社員表の中から、`SHIMADA`さん(社員番号が`6`)よりも前に入社した社員の、社員名と入社日を検索せよ。

    | ename  | hire_date  |
    |---|---|
    | TANAKA | 1990-10-23 |
    | KIMURA | 1999-06-01 |
    | SUZUKI | 2000-02-09 |
    | IKEDA  | 2001-12-21 |
    | YAMADA | 2004-07-18 |

2. 社員表の中から、給料が全社員の平均給料よりも高い社員の人数を検索せよ。

    | sum |
    |---|
    |   4 |

