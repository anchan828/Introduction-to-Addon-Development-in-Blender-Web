---
pagetitle: 3-2. キーボードのイベントを扱う
subtitle: 3-2. キーボードのイベントを扱う
---

[3-1節](01_Handle_Mouse_Event.html) では、マウスのイベントを扱う方法を説明しました。
本節はこの流れで、キーボードのイベントを扱う方法を説明します。


# 作成するアドオンの仕様

キーボードのイベントを扱う方法を理解するため、次のような機能を持つサンプルアドオンを紹介します。

* *[3Dビューポート]* スペースの *[Sidebar]* にあるタブ *[Sample 3-2]* に配置されたパネル *[入力キーを表示]* に、入力キーの表示を開始するためのボタンを配置する
* 入力キーを表示中は、キーボードから入力されたアルファベットのキーをテキストオブジェクトとして表示する


# アドオンを作成する

[1-5節](../chapter_01/05_Install_own_Add-on.html) を参考にして次のソースコードを入力し、ファイル名を `sample_3-2.py` として保存してください。

[@include-source pattern="full" filepath="chapter_03/sample_3-2.py"]


# アドオンを使用する

## アドオンを有効化する

[1-5節](../chapter_01/05_Install_own_Add-on.html) を参考に作成したアドオンを有効化すると、コンソールウィンドウに次の文字列が出力されます。

```
サンプル3-2: アドオン『サンプル3-2』が有効化されました。
```

*[Sidebar]* を表示し、タブ *[Sample 3-2]* が追加されていることを確認します。

![](../../images/chapter_03/02_Handle_Keyboard_Event/enable_add-on.png "サンプル3-2 アドオン有効化")


## アドオンの機能を使用する

有効化したアドオンの機能を使い、動作を確認します。


<div class="work"></div>

|||
|---|---|
|1|*[3Dビューポート]* スペースの *[Sidebar]* から、*[Sample 3-2]* > *[入力キーを表示]* > *[開始]* ボタンをクリックすると、入力キーを表示するモードに移行します。このとき、コンソールウィンドウに次のメッセージが出力されます。<br>![](../../images/chapter_03/02_Handle_Keyboard_Event/use_add-on_1.png "サンプル3-2 アドオンの使用 手順1")<br>`サンプル3-2: 入力キーの表示処理を開始しました。`|
|2|入力キーを表示するモードでは、入力したキーボードのキーをテキストオブジェクトとして表示します。<br>![](../../images/chapter_03/02_Handle_Keyboard_Event/use_add-on_2.png "サンプル3-2 アドオンの使用 手順2")|
|3|*[3Dビューポート]* スペースの *[Sidebar]* から、*[Sample 3-2]* > *[入力キーを表示]* > *[終了]* ボタンをクリックすると、入力キーを表示するモードに移行します。また、コンソールウィンドウに次のメッセージが表示されます。<br>`サンプル3-2: 入力キーの表示処理を終了しました。`<br>![](../../images/chapter_03/02_Handle_Keyboard_Event/use_add-on_3.png "サンプル3-2 アドオンの使用 手順3")|


## アドオンを無効化する

[1-5節](../chapter_01/05_Install_own_Add-on.html) を参考にして有効化したアドオンを無効化すると、コンソールウィンドウに次のような文字列が出力されます。

```
サンプル3-2: アドオン『サンプル3-2』が無効化されました。
```


# ソースコードの解説

ソースコードをみるとわかると思いますが、変数名などの細かい部分やオペレータクラスの `invoke` メソッドと `modal` メソッドの処理を除いて [3-1節](01_Handle_Mouse_Event.html) で説明した内容とほとんど同じです。
このため本節では、オペレータクラス `SAMPLE32_OT_ShowInputKey` の `invoke` メソッドと `modal` メソッドについて説明します。

[3-1節](01_Handle_Mouse_Event.html) では、オブジェクトが回転中であることとモーダルモード中であることが対応していましたが、本節のサンプルアドオンでは入力キーの表示中がモーダルモード中に対応します。
以降本節では、モーダルモードと書かれていたら入力キーの表示中であるとして読み進めて問題ありません。


## invokeメソッド

オペレータクラス `SAMPLE32_OT_ShowInputKey` の `invoke` メソッドでは、モーダルモード中ではない場合（クラスメソッド `is_running` が `False` を返した場合）に、`bpy.ops.object.text_add` 関数を呼び出して、キーを表示するためのテキストオブジェクトを作成します。
このとき、テキストオブジェクトで表示するテキストは空文字列とします。
また、あとで `modal` メソッドがからテキストオブジェクトにアクセスするために、クラス変数 `__text_object_name` に作成したテキストオブジェクトのオブジェクト名を保存しておきます。


## modalメソッド

`modal` メソッドの最初の処理である *[3Dビューポート]* スペースの更新処理は、[3-1節](01_Handle_Mouse_Event.html) と同様です。
ここでは、それ以降の処理について説明します。


### モーダルモード終了処理

クラスメソッド `is_running` が `False` を返すとき、モーダルモードを終了させます。
このとき、モーダルモード開始時に作成したテキストオブジェクトを削除する必要があります。
クラス変数 `__text_object_name` に保存しておいたテキストオブジェクトのオブジェクト名を利用し、削除対象のオブジェクトを `bpy.data.objects` から削除します。
`bpy.data.objects.remove` メソッドの引数に、削除対象のオブジェクトのオブジェクト名を指定することで、オブジェクトを削除できます。
オブジェクト削除後、`modal` メソッドは `{'FINISHED'}` を返して、モーダルモードを終了します。

[@include-source pattern="partial" filepath="chapter_02/sample_3-2.py" block="finish_modal_mode"]


### テキストオブジェクトのテキスト更新処理

入力されたキーにしたがって、テキストオブジェクトのテキストを更新します。
テキストオブジェクトのテキストを更新するためには、オブジェクトのメンバ変数 `data.body` に表示するテキストを設定する必要があります。

入力されたキーは、イベント情報である引数 `event` のメンバ変数 `type` に識別子が保存されている（[3-1節](01_Handle_Mouse_Event.html) 参照）ため、これを利用します。
`ALPHABET_LIST` は大文字のアルファベット `'A'` から `'Z'` までが保存されたリストになっていて、`event.type` がリスト内のいずれかの要素に一致する場合に、テキストオブジェクトのテキストを更新しています。
もしリスト中のいずれの要素にも一致しない場合、`modal` メソッドは `{'PASS_THROUGH'}` を返してしまうため、テキストオブジェクトを更新しません。

[@include-source pattern="partial" filepath="chapter_02/sample_3-2.py" block="change_text_object_text"]

なお、大文字のアルファベットのリストは、次のコードで作成できます。

[@include-source pattern="partial" filepath="chapter_02/sample_3-2.py" block="make_alphabet_list"]


# まとめ

本節では、キーボードのキーイベントを扱う方法を紹介しました。
[3-1節](01_Handle_Mouse_Event.html) のサンプルと比較すると、`modal` メソッドまたは `invoke` メソッドの引数に渡されてくるイベント情報を用いる点で、マウスのイベントとキーボードのイベントの扱い方が、ほとんど同じであることが理解できたかと思います。

ユーザからの入力イベントを扱ってインタラクティブな機能を提供することは、これまでに紹介してきた `execute` メソッドを単に実行するだけの処理と比べて、処理が複雑になりがちでバグも発生しやすくなります。
しかし、キーボードやマウスのイベントを適切に扱うことができるようになると、アドオンで実現できることが広がるため、より高度なアドオンを作ることができるようになるでしょう。


## ポイント

* `modal` メソッドや `invoke` メソッドの引数に渡されてくるイベント情報を用いることにより、キーボードのキーイベントを扱うことができる
* ユーザからの入力イベントを扱うことで、インタラクティブな機能を提供できる反面、機能を実現するための処理が複雑化する傾向がある