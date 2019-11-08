---
description: 本文紀錄如何建立一個完整的 Rails 專案、如何使用 Rails 內建的 Scaffold 功能。
---

# 製作 Rails 網頁應用：簡易留言系統

![](../.gitbook/assets/rais.001.jpeg)

為了讓正在學習 Ruby on Rails 的自己有點成就感，所以參考[五倍紅寶石這個 Ruby on Rails 社群的教學文](https://railsbook.tw/)，先做個能與使用者互動的簡易留言系統。建議也想試做一次 Rails 網頁應用的人，可以直接閱讀教學文內的兩個章節：[環境設定](https://railsbook.tw/chapters/02-environment-setup.html)、[第一個應用程式（使用 Scaffold）](https://railsbook.tw/chapters/04-your-first-rails-application.html)。

## 建立 Rails 專案 <a id="95aa"></a>

建立網頁應用前，要先在電腦本機端設立專案資料夾，Rails 本身就有 bundle，方便使用者建立完整的 Rails 專案資料夾。

步驟如下：

1. 開啟終端機（Terminal）
2. 在終端機輸入 _`rails new hello_rails`_ ，表示以 _`rails new`_ 指令建立名稱為「hello\_rails」的 Rails 專案檔，輸入指令後，終端機會自動安裝各種 bundle，此步驟需要較久的安裝時間，系統安裝中會要求使用者密碼，所以要注意系統提示。  


   ```text
   //當出現此部分的系統提示，代表成功建立 Rails 專案。
   * For more details, please refer to the Sass blog:
     https://sass-lang.com/blog/posts/7828841run  bundle exec spring binstub --all
   /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/universal-darwin19/rbconfig.rb:229: warning: Insecure world writable dir /Users/yenjulai in PATH, mode 040777
   * bin/rake: Spring inserted
   * bin/rails: Spring inserted
   ```

3. 啟動 Rails 內建的 web server 功能進行測試  
   此為非必要步驟，主要是讓第一次使用 Rails web server 的人了解啟動程序而已，先以 _`cd hello_rails`_進入安裝好的 Rails 專案資料夾，再輸入 _`bin/rails server`_啟動 web server。  


   ```text
   $ cd hello_rails  //進入設立好的hello_rails專案資料夾
   $ bin/rails server //啟動Rails內建的web server
   /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/universal-darwin19/rbconfig.rb:229: warning: Insecure world writable dir /Users/yenjulai in PATH, mode 040777
   => Booting Puma
   => Rails 5.2.3 application starting in development 
   => Run `rails server -h` for more startup options
   Puma starting in single mode...
   * Version 3.12.1 (ruby 2.6.3-p62), codename: Llamas in Pajamas
   * Min threads: 5, max threads: 5
   * Environment: development
   * Listening on tcp://localhost:3000  //開啟瀏覽器後，在網址列輸入此處出現的localhost，例如這裡的 http://localhost:3000
   Use Ctrl-C to stop
   ```

4. 開啟瀏覽器，在網址列輸入 _`http://localhost:3030`_，出現下圖內容，代表完成～

![](https://miro.medium.com/max/60/1*xxB3z4o2owNDGRRV1gOHug.png?q=20)

> 【注意】本文使用 `bin/rails server`指令啟動 web server 功能，是因為考量到 Ruby 套件須與 Rails 專案內載入的 Gemfile 是相同的版本設定，若只使用 `rails server`指令開啟 web server，可能中間還會需要額外輸入其他指令（詳情請見[教學文內說明](https://railsbook.tw/chapters/02-environment-setup.html)），才能順利安裝。

## 規劃系統的功能（User Story） <a id="0103"></a>

此部分跟教學文相同，只想要先有基本的 2 種常見功能：

* 能夠新增使用者
* 讓每個使用者可以新增、刪除與修改發文的內容

下文將依序完成上述 2 種功能的建立。

## 開始建立功能 <a id="5dc1"></a>

### A. 使用 Rails 內建的 Scaffold，建立「新增使用者」的功能 <a id="ef79"></a>

1. 先設想好使用者的資料型態，我只想記錄使用者的名字與mail，所以這兩個資料欄位大概就像這樣  - 使用者名稱：欄位名稱 ＝ name, 資料型態 ＝ 字串（string） - 使用者的email：欄位名稱 ＝ email, 資料型態 ＝ 字串（string） 
2. 使用終端機進入前面建立好的 Rails 專案資料夾，再輸入 _`bin/rails generate scaffold User name:string email:string`_讓 Scaffold 產生需要儲存資料的檔案位置。  


   > 請特別注意 Rails 專案內多了一個檔案「20190715124353\_create\_users.rb」，它會是下個步驟要的遷移檔。

   ```text
   //開啟 20190715124353_create_users.rb 檔案，會發現內容就是個準備要產生儲存使用者資料的表格。
   class CreateUsers < ActiveRecord::Migration[5.2]
     def change
       create_table :users do |t|
         t.string :name
         t.string :emailt.timestamps
       end
     end
   end
   ```

3. 描述遷移檔（migration file）建立表格  
  
   在終端機內輸入 _`bin/rails db:migrate`_來執行遷移檔的描述（執行此步驟前，務必要確保自己仍是處在 Rails 專案資料夾 hello\_rails 內），在資料庫內建立一個名為 users 的表格，讓使用者的資料都能存在內。完成後會出現如下圖的終端機訊息，新增使用者的功能就完成了。  


   ```text
   == 20190715124353 CreateUsers: migrating ======================================
   — create_table(:users)   //建立了名為 users 的表格
      -> 0.0011s
   == 20190715124353 CreateUsers: migrated (0.0011s) =============================
   ```

4. 啟動 Rails Web Server （方式前面已提過）  使用 _`bin/rails se`_ 與 _`rails s`_ 皆可，並在瀏覽器網址列輸入 _`http://localhost:3000/users`_，注意這裡必須要進入到 users 的部分，如果沒輸入 users，只會顯示 Rails Web Server 的歡迎畫面。

![](https://miro.medium.com/max/60/1*jA7EMMX2Z6ItNn4buYUQaw.png?q=20)

左圖是新增使用者，新增後會出現右圖的提示畫面，還能多次修改使用者資訊。（但這方法發現個 bug，就是無法藉由判斷 email 的格式，要求使用者填寫正確資訊。）

到這裡，已完成兩種功能的其中一個：能夠新增使用者。  
接下來要用相同原理，製作第二個功能：新增/刪除/修改貼文。

### B. 使用 Rails 內建的 Scaffold，建立新增/刪除/修改貼文的功能 <a id="8d32"></a>

1. 先設想好貼文的資料型態  
  
   - 文章標題：欄位名稱 ＝ title, 資料型態 ＝ 字串（string）  
   - 內文：欄位名稱 ＝ content, 資料型態 ＝ 文字（text）  
   - 文章狀態（是否發布）：欄位名稱 ＝ status, 資料型態 ＝ 布林（boolean）  
   - 使用者編號：欄位名稱 ＝ user\_id, 資料型態 ＝ 數字（integer）  


   **【注意】**字串（string） 與 文字（text）不同，text 可以存放的文字比 string 多；而額外編列使用者編號資料的原因，是為了將文章與發佈的使用者連結在一起。  
   ****

2. 利用設想好的資料型態，在終端機輸入   _`bin/rails g scaffold Post title:string content:text status:boolean user:references`_  **【注意】** _`user:references`_ 的寫法，為何不像前面的方式，寫成 _`user:user_id`_? 其實寫成 _`user:user_id`_ 也行，只是使用 _`user:references`_ 的話會自動完成更多細節。 
3. 描述遷移檔（migration file）建立表格  
   輸入 `bin/rails db:migrate` ，即在專案資料夾內建立了一個名為 posts 的表格，如下所示。  


   ```text
   == 20190715132027 CreatePosts: migrating ======================================
   — create_table(:posts)  //建立了名為 posts 的表格
      -> 0.0022s
   == 20190715132027 CreatePosts: migrated (0.0023s) =============================
   ```

4. 啟動 Rails Web server 後，在瀏覽器網址列輸入 [_`http://localhost:3000/posts`_](http://localhost:3000/posts)

![](https://miro.medium.com/max/60/1*4o6RlbOxd7qf1VrLBJEjOg.png?q=20)

上面兩圖是發布貼文的後台，貼文發布後的結果，就是下圖。但此處有個 bug：User 欄位顯示的不是貼文發布者的名稱，所以後續會進行修改。

## 修正 <a id="c3d7"></a>

依照本文操作後，會發現到 2 個地方需要修正

1. 修正使用者欄位的亂碼
2. 改善檢索效能問題

### **1. 修正使用者（User）欄位的亂碼** <a id="4c87"></a>

如前面所見到，使用者欄位目前是亂碼狀態，所以要修改成此欄位能顯示發布貼文者的名稱。

首先開啟專案內路徑為 app/views/posts/index.html.erb 檔案，把第 21 行的 `post.user`改成 `post.user.name`。

![](https://miro.medium.com/max/60/1*ZdP44_XQoFRIerpYk9h9KQ.png?q=20)

圖內是已將`post.user`改成`post.user.name`的結果。

![](https://miro.medium.com/max/60/1*nkS7QCvIpt9DAMGKObNtqw.png?q=20)

再次重整 [http://localhost:3000/users](http://localhost:3000/users`) 網頁，使用者欄位就會顯示正確的貼文發布者名稱。

### 2. 改善效能問題（N + 1 Query） <a id="fa07"></a>

#### **什麼是 N + 1 Query?**

為了顯示Post 關連的使用者姓名資料，會額外做多次的資料庫查詢，這就是所謂的 N + 1 查詢（1 次 Post 查詢 + N 次的關連資料查詢）。在 Rails 裡可使用 includes 方法減少不必要的資料庫查詢次數。

所以要使用 include 方法，進入 Rails 專案資料夾內的 posts\_controller 檔案內，更改其中的 index actions。

![](https://miro.medium.com/max/60/1*hMZzr9IBn6cNWu6zMGfbdQ.png?q=20)

使用 includes 之後，SQL 查詢變成 IN 的寫法，原來的 N + 1 次也會變成 1 + 1 = 2 次了。

以上，以 Rails 完成簡易的留言系統。

