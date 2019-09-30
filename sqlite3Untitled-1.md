create table infobar(
    id INTEGER primary key AUTOINCREMENT,
    info VARCHAR(120),
    created_at TEXT
    );

レコードの挿入
INSERT INTO infobar VALUES(1,'YAMADA DESU.YOROSIKU',DATETIME('NOW','LOCALTIME'));
テーブル名を指定、格納したい値をカラムと同じ順番で入力し、データを追加
DATETIME関数とLOCALTIMEで現在の日付と時刻を取得しています

レコードの更新
UPDATE infobar SET info = 'aiueokakikuke' WHERE id=2;
whereで条件式を指定、カラム名と値(info)を指定してデータを更新

レコードの削除
DELETE FROM infobar WHERE id=?;
条件式にに当てはまるレコードをinfobarから削除

レコードの抽出
SELECT id,name FROM infobar;
idとnameをinfobarから抽出


