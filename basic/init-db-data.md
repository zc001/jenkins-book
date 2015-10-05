# 測試資料與資料庫

進行規格運行前，或是功能開發告一段落，很多時後需要建立一些資料才能進行測試，比如說當需要測試使用者刪除時，需要先建立 user 在進行使用者刪除的功能操作，但是使用者建立並不是我們需要測試的功能，如果每次測試使者刪除都需要經過建立的步驟，未免太浪費時間。

因此，如果我們可以在開始運行規格前先將測試資料準備好，那就可以省略建立 user 的過程，直接對目標功能進行測試，如此一來將可以更聚焦於要運行的規格上。

不同的語言大部份都會實作 ORM 的技術，除了操作資料庫方便以外，還會提供自動根據 model 的定義，建立好對應的 database schema，從程式碼的角度，讓開發人員不需要分心去對資料庫進行操作，某方面來說，也是希望每次調整 model 的屬性時，對應得資料庫欄位可以自動建立。

有了基本認識後，要為大家介紹三種資料庫初始的方式：

## drap

-	會將會將資料庫卸載（drop）在重新建立。
-	不會保留資料庫的資料。
-	會根據 model 的定義再次產生對應的 schema。
-	用於 development mode
-	完成資料庫初始速度快

## update

-	不會將會將資料庫卸載（drop）在重新建立。
-	會保留資料庫的資料
-	會根據 model 的定義再次產生對應的 schema。
-	用於 development mode，需要驗證 production 資料時
-	完成資料庫初始速度慢

## safe / none

-	不會將會將資料庫卸載（drop）在重新建立。
-	會保留資料庫的資料
-	不會根據 model 的定義再次產生對應的 schema。
-	用於 production mode
-	不需要資料庫初始時間

對於沒有 ORM support 或正要進入 TDD 的開發而言，應該會很想像 drop 模式，每次都把資料庫卸載，該怎麼進行測試？很簡單，只要在每次開始運行規格前透過 model 的操作將需要的測試資料建立完成即可，感覺資料庫裡先建好測試資料就好，為什麼需要再用程式來建立資料？

讓我們在想像一個情境，可運行的規格是需要定時運行的，以確保程式碼的邏輯與規格一致，假設我們需要確認使用者刪除功能，如果只使用資料庫的資料，而不是程式進行建立，每運行一次使用者刪除規格，該使用者被刪除後，下次要在運行規格時就會無法通過，因為沒有使用者資料存在。

換另外一個角度，如果我們是實機測試，每次 QA 的過程結束後資料一定會跟初始的資料庫有出入，要再進行下次 QA 時又要再還原一次，這樣的流程將會讓你浪費時間在不必要的操作上。

根據 development/production mode 的不同，選定 database 的初始方式，將是開始 TDD 的第一步。