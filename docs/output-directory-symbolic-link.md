# ComfyUI 出力先が変わらない問題：完全ケーススタディ  
**～なぜハマり、どう解決したのか～**
> This document describes a real troubleshooting case for **ComfyUI Windows portable**.
> The conclusion is based on actual source-code behavior, not assumptions.

## 1. やりたかったこと
- ComfyUI の生成画像を **D:\ComfyUI_output** に保存したかった  
- C ドライブの容量を節約したかった  
- portable 版なので設定ファイルで制御できると思っていた

---

## 2. どこからドツボにハマったのか

### 2-1. ComfyUI に config.json があると思い込んだ  
ネット上の情報から「config.json に output_directory を書けば反映される」と考えた。  
しかし実際には：

- portable 版には **最初から config.json は存在しない**
- 作っても **ComfyUI は config.json を読まない仕様**

これが最初の誤解だった。

---

### 2-2. config.json を作ったが、ComfyUI は完全に無視  
作成した config.json：

```json
{
  "output_directory": "D:/ComfyUI_output"
}
```

内容は正しいが、  
**ComfyUI は一切読まない**。

---

### 2-3. run_nvidia_gpu.bat の起動オプションを疑った  
portable 版の起動バッチ：

```
.\python_embeded\python.exe -s ComfyUI\main.py --windows-standalone-build
```

`--windows-standalone-build` が原因かと思い削除したが、  
**それでも出力先は C:\output のまま**。

---

### 2-4. 設定もバッチも関係なかった  
ここまでの切り分けで分かったこと：

- config.json → 読まれない  
- 起動オプション → 関係ない  
- WebUI の設定 → 変更不可  
- 出力先 → ずっと C:\output のまま  

---

## 3. 真の原因：ComfyUI の仕様だった

結論：

# **ComfyUI は出力先を設定ファイルから変更できない。  
内部コード（folder_paths.py）で C:\output が固定されている。**

つまり：

- config.json → 無視  
- 起動オプション → 無関係  
- portable 版 → 特に固定パスが強い  

**ユーザーが何をしても出力先は変わらない仕様だった。**

---

## 4. 最終的な解決策：シンボリックリンク

ComfyUI が C:\output にしか書けないなら、  
**C:\output を D:\ に偽装するのが最も正しい。**

---

## 5. 実際に行った手順（PowerShell 管理者）

### 5-1. C:\output を削除

```powershell
Remove-Item "C:\AI\ComfyUI_windows_portable_nvidia\ComfyUI_windows_portable\ComfyUI\output" -Recurse -Force
```

### 5-2. D:\ComfyUI_output を作成

```powershell
New-Item -ItemType Directory -Path "D:\ComfyUI_output"
```

### 5-3. シンボリックリンクを作成

```powershell
cmd /c mklink /D "C:\AI\ComfyUI_windows_portable_nvidia\ComfyUI_windows_portable\ComfyUI\output" "D:\ComfyUI_output"
```

成功すると：

```
output <<===>> D:\ComfyUI_output のシンボリックリンクが作成されました
```

---

## 6. 結果

- C:\output → ショートカットマーク付きのリンクに変化  
- D:\ComfyUI_output → 実体フォルダ  
- ComfyUI の出力 → **D:\ に保存されるようになった**

完全に意図通りの動作になった。

---

## 7. まとめ（他のユーザーへの教訓）

- ComfyUI portable は **出力先を設定で変更できない**  
- config.json は **存在しても無視される**  
- 出力先を変えたいなら **シンボリックリンクが唯一の正攻法**  
- これは仕様であり、ユーザーのミスではない  
- 同じ罠に落ちる人は多いので、この記録は役に立つ

---

## ライセンス（任意）

```
This document is provided as-is for educational and troubleshooting purposes.
Feel free to copy, modify, and redistribute.
```
