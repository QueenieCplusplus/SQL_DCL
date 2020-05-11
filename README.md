# SQL DCL
Data Control Language


* GRANT 

* REVOKE

* TRANSACTION

使用 Transaction 的話，能將多個 SQL 命令當成一個邏輯單位來處理，而交易功能和 DB 的排他控制有關。
倘若這些命令群沒有全部完成的話，就不接受使用者的要求，藉此保持資料的 Integrity!

舉 MySQL 為例子：

1. Set Transaction Isolation <Level>
  
   Transaction 可設定為 Session 亦或是 Global。
   
   關於 session http://einverne.github.io/post/2017/05/sqlalchemy-session.html
   
   參數則可設定為只讀取，即 select，其他操作則錯誤。
   
   level 參數包含四種隔離等級：
   
   - UR, Read Uncommitted == Uncommited Read (未確定的讀取)
   
     因為資料不被鎖定，只要在 commit 前都能進行變更，容易讀取到資料異動前的錯誤資料，造成髒讀，
     但優點是具備快速讀取。
   
   - CS, Read Committed == Cursor Stabilty (游標穩定性) 
   
     讀取僅參照到變更的資料。（這是大多數資料庫所採用的隔離等級！）
   
   - RS, Repeatable Read == Read Stability (讀取穩定性)
   
     保證在交易功能內，無論執行多少次檢索指令，均可獲的相同結果。
   
   - RR, Serializable == Repeatable Read (可重複讀取)
   
     在交易功能開始之前，才能參照到 commit 內容。
  
2. Begin [work]

   使用交易功能，能將多個 insert 或是 update 統合在一起，並且用 Begin 開啟交易。（某些除外，例如 Oracle 和 DB2 一直都是啟動狀態，所以沒有 begin 功能的命令）
   
   在交易開始時，update 和 insert 只有保留狀態，直到 Commit 命令執行，才能讓 SQL 命令有效，結束交易功能，而使用 RollBack 則使整筆交易無效。倘若一整串命令中，造成資料遺失或重複，則可使用 RollBack 回滾功能的資料控制命令！

3. Commit [work]

   很多用來執行 SQL 的用戶端工具，都具備自動 Commit 模式，在此模式中，執行 SQL 指令會自動執行 Commit。倘若要自己控制的話，就要關閉此預設模式，而且即使在自動 Commit 模式下，也可使用 Begin 來啟動交易功能。
   
   其他類似用戶端工具的仲介軟體也具備自動 Commit 功能，可藉由仲介軟體稱為 Connection 的物件，呼叫其方法（通常命名方式與 SQL 指令相同）， 便可讓交易功能執行 SQL 指令。

4. Rollback [work] To [savepoint] <savepoint>
  
   將資料回復到交易功能中的 A 儲存點。
   
5. Savepoint <savepoint> on rollback retain cursors
  
   使用此命令可在交易功能中定義儲存點。（作為 Begin、Commit、Rollback 等控制命令的中間點）
   
6. Select < column From table_name Where expression > For Update

7. Lock Tables <table_name> <mode>

   鎖定資料表。此鎖具有排他性，無法從別的 session 來進行所定（即此新的鎖定也會被封鎖）。

8. Unlock Tables

   解除資料表的鎖定狀態。
  



