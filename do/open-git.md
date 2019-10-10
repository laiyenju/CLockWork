---
description: MacOS 限定，Git command line 的基本指令
---

# 正確開啟 Git 的方式

**本篇會記錄以下 Git 功能，方便日後查找**

1. 確認 Git 的版本
2. 設定 Git 使用者的名稱與信箱
3. 建立專案資料夾
4. 登記 Git 專案資料夾
5. 解除 Git 版本控制
6. 提交修改過的檔案，建立版本節點
7. 還原專案的版本

## 什麼是 Git ？ <a id="c951"></a>

> 目前市面上的許多企業協作工具，例如微軟開發的 [Team](https://products.office.com/en-US/microsoft-teams/group-chat-software)、字節跳動的 [Lark](https://www.larksuite.com/) 以及 Google 的 [G Suite](https://gsuite.google.com/)，除了能供內部員工共享與編輯文檔，也能查看共享文檔的編輯歷史紀錄，只是上述工具紀錄的是文書檔案更動紀錄，而能紀錄程式碼更動紀錄的工具，就是 Git。

Git 是分散式的版本控制系統（distributed version control system），最常看到的 Git 使用情境就在開發程式的團隊內，Git 在這裡扮演的角色就是紀錄程式碼的所有更新紀錄與內容，方便團隊協作，因此是程式設計師必學的技能了。

## **初次使用 Git 的基本設定** <a id="1ea6"></a>

> 再次提醒：以下使用的所有命令列（command line）都是基於MacOS系統

* **確認 Git 的版本** `git --version`
* **更新 Git** `brew upgrade git` _\*我使用的是_ [_Homebrew 軟體管理工具_](https://brew.sh/)_更新git，因此之前必須先在電腦安裝好 Homebrew，然後用命令列_ _`brew update`更新 Homebrew。_
* **設定 Git 使用者名稱** `git config --global user.name"myName"` _\*在myName位置輸入自己的使用者名稱。_
* **設定 Git 使用者信箱** `git config --global user.email"myEmail"` _\*在myEmail設定自己的信箱_

## 為專案資料夾登記 ＆ 解除 Git 版本控制 <a id="4af5"></a>

> 進行本次步驟前，須先熟悉[命令列的基本使用方式](http://docs.gitlab.com/ee/gitlab-basics/command-line-commands.html)，了解如何從終端機以命令列進入專案資料夾。

### 登記專案資料夾 Git 的步驟如下： <a id="2200"></a>

1. 開啟終端機（terminal）
2. 以命令列指令進入要登記為 Git 的專案資料夾
3. 進入專案資料夾內，以命令列指令輸入 `git init`
4. 確認是否登記成功，以命令列指令輸入 `ls -al`，若出現 .git 的標記，表示該資料夾已成功登記

完成上述步驟，即完成 Git 登記，此後 Git 會紀錄這個專案資料夾內任何更新的檔案內容。 要解除 Git 的登記（等同解除 Git 的版本控制），只要以**命令列指令輸入** **`rm -rf .git`**即可。

## 更新 Git 專案資料夾的內容 <a id="0ec5"></a>

![](https://miro.medium.com/max/60/1*0c69m-Cf0xL2pbr3kWfdCQ.png?q=20)

Git 版本控制區分為 3 個工作階段：Working Area（工作區）、Staging Area（暫存區）、Git Repository（儲存區）。可先透過上圖了解這些工作階段的角色，才能正確使用 Git 的更新功能。

* **檢查目前專案資料夾內是否有檔案更新** `git status`
* **將更新的檔案從「工作區」移動到「暫存區」**`git add fileName`

  _\*fileName要放入Staging Area的檔案名稱。_

* **將所有更動的檔案一次性放入「暫存區」** `git add.`
* **將檔案從「暫存區」移動到「儲存區」，並建立版本紀錄節點以及訊息** `git commit -m"描述檔案更新的目的與內容"`

![&#x4F7F;&#x7528; git add &#x8207; git commit &#x7684;&#x908F;&#x8F2F;&#x3002;](https://miro.medium.com/max/60/1*DK60Ct0sDrWpR633oN1IaQ.png?q=20)

## 還原 Git 版本 <a id="c829"></a>

> 由於編寫程式碼會使用編輯器（如：[Sublime Text](https://www.sublimetext.com/)、[Atom](https://atom.io/)、[VScode](https://code.visualstudio.com/)⋯⋯等）建議先幫 Git 設定一個開啟程式碼檔案的編輯器，再進行 Git 的版本還原。

* **設定 Git 的編輯器** `git config --global core.editor<編輯器名稱>`
* **找出要還原檔案的SHA1碼** `git log`，_\*輸入命令列指令後，會顯示SHA1碼。_

#### **還原方式 1** `git reset <SHA1碼>`

此種還原方式會保留修改過的檔案內容，但刪除該檔案在「儲存區」的版本紀錄。（如下圖所示）

![&#x4F7F;&#x7528; git reset &amp;lt;SHA1&#x78BC;&amp;gt; &#x7684;&#x6548;&#x679C;](https://miro.medium.com/max/60/1*tJq8EMLWIy-eEaLIE3Lllw.png?q=20)

#### **還原方式 2** `git revert <SHA1碼>`

此種還原方式會刪除修改過的檔案內容，但不會刪除該檔案在「儲存區」的版本紀錄，而是在「儲存區」額外新增一個該檔案的版本紀錄。（如下圖所示）

![&#x4F7F;&#x7528; git revert &amp;lt;SHA1&#x78BC;&amp;gt;&#x7684;&#x6548;&#x679C;](https://miro.medium.com/max/60/1*uiAWPuMApAfPxULNrwqjmQ.png?q=20)

值得注意的是，因爲 `git revert <SHA1碼>`比較適合在已清楚知道是某某版本紀錄的內容更新出錯，若擔心會將正確的更新內容也刪掉，使用 `git reset <SHA1碼>`搭配 `git diff`查找更新前後的檔案內容差異，是比較安全的方法。

**查詢更新前後的檔案內容差異：** `git diff`

## 該怎麼知道 Git 更新檔案紀錄了沒？ <a id="b501"></a>

可以在終端機內看到執行 git 命令列指令後的內容，綠色文字顯示新增的內容，紅色文字顯示被刪除的內容，所以當檔案內容有被修改過，修改前的內容會以紅色顯示，修改後的內容會以綠色顯示。



#### 參考資料

* [Git FAQ](https://git.wiki.kernel.org/index.php/GitFaq#What_is_Git.3F)（Git 官方網站的常見問答集）
* [Resetting, Checking Out & Reverting](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting)
* [大家一起來Git!](https://medium.com/@10446005/%E5%A4%A7%E5%AE%B6%E4%B8%80%E8%B5%B7%E4%BE%86git-f34945411bbb)（內有更詳細的 Git 操作方式，也額外介紹了 Git 介面化軟體的使用方式）

