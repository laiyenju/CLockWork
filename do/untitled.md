---
description: 為了將 Python 升級為 Python3，因此使用 Homebrew 安裝，結果開始了修復 Homebrew 的作業，將過程記錄下來，以防下次再度發生。
---

# OMG！我的 Homebrew 🍺 壞掉了

![](../.gitbook/assets/homebrew.png.001.jpeg)

兵荒馬亂的起始點，就在終端機輸入 `brew install python3` 後，出現了錯誤訊息：

```bash
Error: An unexpected error occurred during the `brew link` step
The formula built, but is not symlinked into /usr/localPermission denied @ dir_s_mkdir - /usr/local/FrameworksError: Permission denied @ dir_s_mkdir - /usr/local/Frameworks
```

因此，在終端機輸入 `brew doctor` 診斷發生什麼事、該如何處理，結果獲得，主要是缺少了某些資料夾，導致 python 與 node 的安裝包都有問題：

```bash
Warning: The following directories do not exist:/usr/local/sbinYou should create these directories and change their ownership to your account.  sudo mkdir -p /usr/local/sbin
  sudo chown -R $(whoami) /usr/local/sbinWarning: You have unlinked kegs in your Cellar.Leaving kegs unlinked can lead to build-trouble and cause brews that depend onthose kegs to fail to run properly once built. Run `brew link` on these:pythonWarning: Some installed formulae are missing dependencies.
You should `brew install` the missing dependencies:brew install node
```

因此依照顯示的步驟一一在終端機執行

* `sudo mkdir -p/user/local/sbin`
* `sudo chown -R $(whoami) /usr/local/sbin`
* `brew link --overwrite node`
* `brew link --overwrite python`

再次執行 `brew doctor` 查看修復狀況，會得到

`Your system is ready to brew.` 撒花～🎉

## 後記 <a id="e431"></a>

因為 brew 執行下去後，螢幕顯示結果很長，最後才出現 warning 訊息，導致沒遇過 Homebrew 的我有點緊張，不知如何處理，只好到 Stackoverflow 查，透過這兩則貼文的回覆才解決問題

* [Permissions issue when linking python3 \#19286](https://github.com/Homebrew/homebrew-core/issues/19286)
* [“Unlinked kegs in your Cellar”. How do I remove them?](https://stackoverflow.com/questions/30976312/unlinked-kegs-in-your-cellar-how-do-i-remove-them)

結果 Stakoverflow 中有不少人回覆要仔細看終端機回傳的內容，回頭再查看自己終端機內的內容，才了解只要照著做就能解決，根本不需要上網查🤣

