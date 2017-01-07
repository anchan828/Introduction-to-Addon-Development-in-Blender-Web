<div id="sect_title_img_3_2"></div>

<div id="sect_title_text"></div>

# キーボードのキーイベントを扱う

<div id="preface"></div>

###### [3-1節](../01_Handle_Mouse_Click_Event.md) ではマウスのイベントを扱う方法を説明しました。本節ではキーボードのイベントを扱う方法を、サンプルを交えて紹介します。

## 作成するアドオンの仕様

* *3Dビュー* エリアのプロパティパネルの *オブジェクト並進移動モード* から、オブジェクト並進移動モードを開始するためのボタンを配置する
* キーボードのキー *Q* が押された時に、オブジェクト並進移動モードを終了する
* オブジェクト並進移動モードでは以下のキーボードのキー入力に応じて、オブジェクトを並進移動する

|キー|処理|
|---|---|
|*X*|X軸正方向に平行移動|
|*Shift* + *X*|X軸負方向に平行移動|
|*Y*|Y軸正方向に平行移動|
|*Shift* + *Y*|Y軸負方向に平行移動|
|*Z*|Z軸正方向に平行移動|
|*Shift* + *Z*|Z軸負方向に平行移動|


## アドオンを作成する

[1-5節](../chapter_01/05_Install_own_Add-on.md) を参考にして以下のソースコードを入力し、ファイル名を ```sample_3-2.py``` として保存してください。


[import](../../sample/src/chapter_03/sample_3-2.py)

## アドオンを使用する

### アドオンを有効化する

[1-5節](../chapter_01/05_Install_own_Add-on.md) を参考に、作成したアドオンを有効化するとコンソールウィンドウに以下の文字列が出力されます。

```sh
サンプル3-2: アドオン「サンプル3-2」が有効化されました。
```

<div id="sidebyside"></div>

|また、プロパティパネルの *オブジェクト並進移動モード* にボタンが配置されます。|![3-2節 アドオン有効化](https://dl.dropboxusercontent.com/s/sgjvumciomz3uh4/enable_add-on.png "3-2節 アドオン有効化")|
|---|---|


### アドオンの機能を使用する

<div id="process_title"></div>

##### Work

<div id="process"></div>

|<div id="box">1</div>|*3Dビュー* エリアのプロパティパネル上の *オブジェクト並進移動モード* に配置されている *開始* ボタンをクリックすると、オブジェクト並進移動モードに移行します。この時、コンソールウィンドウに以下のメッセージが出力されます。|![3-2節 アドオンの使用 手順1](https://dl.dropboxusercontent.com/s/io4co0ap9qnphb5/use_add-on_1.png "3-2節 アドオンの使用 手順1")|
|---|---|---|

```sh
サンプル3-2: オブジェクト並進移動モードへ移行しました。
```


<div id="process_sep"></div>

---


<div id="process"></div>

|<div id="box">2</div>|本節のサンプルの仕様で書いたように、オブジェクト並進移動モードではキーボードのキーの組み合わせによりオブジェクトを並進移動することができます。|![3-2節 アドオンの使用 手順2](https://dl.dropboxusercontent.com/s/7ygmch24qzmfcb0/use_add-on_2.png "3-2節 アドオンの使用 手順2")|
|---|---|---|



<div id="process_noimg"></div>

|<div id="box">3</div>|キーボードの *Q* キーを押し、オブジェクト並進移動モードを終了します。オブジェクト並進移動モードを終了すると、コンソールウィンドウに以下のメッセージが表示されます。|
|---|---|

```sh
サンプル3-2: 通常モードへ移行しました。
```


<div id="process_start_end"></div>

---


### アドオンを無効化する

[1-5節](../chapter_01/05_Install_own_Add-on.md) を参考に有効化したアドオンを無効化すると、コンソールウィンドウに以下の文字列が出力されます。

```sh
サンプル3-2: アドオン「サンプル3-2」が無効化されました。
```


## ソースコードの解説

変数名などの細かい部分が異なるところはありますが、オペレータクラスの ```modal()``` メソッドの処理を除いて [3-1節](01_Handle_Mouse_Click_Event.md) で説明した内容とほとんど同じです。このため本節では、オペレータクラス ```SpecialObjectEditMode``` の ```modal()``` メソッドの処理内容について説明します。

### modal()メソッド

```modal()``` メソッドの最初の処理である *3Dビュー* エリアの更新処理までは、[3-1節](01_Handle_Mouse_Click_Event.md) と同様です。

#### モーダルモード終了処理

本節のサンプルでは、モーダルモードを終了するためにキーボードの *Q* キーを押す必要があります。この処理を実現するために、キーボードのキーイベントが発生した時に ```modal()``` メソッドの引数 ```event``` のイベント情報を利用します。*Q* キーが押された時にモーダルモードを終了するためのコードを以下に示します。

[import:"exit_modal_mode", unindent:"true"](../../sample_raw/src/chapter_03/sample_3-2.py)

キーイベント発生時の変数 ```event``` には、以下のメンバ変数が格納されています。

|メンバ変数|意味|
|---|---|
|```type```|イベントが発生したキーの識別子|
|```value```|イベントの内容|

本節のサンプルでは、*Q* キーが押されたイベントが発生した場合、```event.type``` には ```Q``` 、```event.value``` に ```PRESS``` が格納されていることを利用しています。オブジェクト並進移動モード時に ```True``` を設定する ```props.running``` を ```False``` にして ```{'FINISHED'}``` を返すことでモーダルモードを終了し、オブジェクト並進移動モードを終了します。

#### オブジェクト並進移動モード時のキー入力状態の確認

オブジェクト並進移動モードでは、利用するキーの状態を確認しオブジェクトを並進移動する必要があります。次に示すコードにより、キーの入力を確認してオブジェクトを並進移動します。

[import:"check_key_state", unindent:"true"](../../sample_raw/src/chapter_03/sample_3-2.py)

基本的にはモーダルモード終了処理と同様に、```event``` のイベント情報を利用しています。ただし、*Shift* キーを押した時に反対方向へ並進移動するため、*Shift* キーが押されていることを判定する必要があります。*Shift* キーが押されたか否かを判定するためには、```event.shift``` 変数を参照します。*Shift* キーが押されていると ```event.shift``` に ```True``` が設定されることを利用し、```event.shift``` が ```True``` か ```False``` かで、移動量を ```1.0``` または ```-1.0``` を設定します。

実は ```event.type``` には、 ```'LEFT_SHIFT'``` や ```'RIGHT_SHIFT'```といった *Shift* キーのイベントを扱うイベント情報が存在します。しかし本節のサンプルではあえて ```event.shift``` 変数を利用しています。これは ```modal()``` メソッドが呼ばれてくるのが *Shift* キーを押したり離したりした時であり、かつ ```modal()``` メソッドが1回の呼び出しで1つのキーのイベントしか扱えないからです。例えば *Shift* キー + *X* キーを押す場合、*Shift* キーを押した後に *X* キーを押すことになるので、*Shift* キーのイベントが発生した後に *X* キーのイベントが発生することになります。このため、仮に ```event.type``` を用いて *Shift* キーが押されている状態を判定しようとすると、独自に処理を作る必要があります。このように、キーが押されていることを判定するための変数は他にもあります。以下にその変数一覧を示します。

|変数|キー|
|---|---|
|```event.alt```|*Alt*|
|```event.ctrl```|*Ctrl*|
|```event.oskey```|(Macの) *Command*|
|```event.shift```|*Shift*|


#### オブジェクトの並進移動

本節のサンプルでは、オブジェクトの並進移動のために ```bpy.ops.transform.translate()``` 関数を利用しています。引数 ```value``` に並進移動量を渡すことで選択中のオブジェクトを並進移動します。```bpy.ops.transform.translate()``` 関数には他にも色々な引数を渡すことができますが、ここでは割愛します。

[import:"translate_object", unindent:"true"](../../sample_raw/src/chapter_03/sample_3-2.py)

#### modal()メソッドの戻り値

最後に、```modal()``` メソッドの戻り値を ```{'RUNNING_MODAL'}``` で返していますが、これは特殊オブジェクトモード時にキーを押した時にBlenderへ処理を伝搬しないようにするためです。仮にこの部分を ```{'PASS_THROUGH'}``` としてしまうと、Blenderにもキーイベントが発生してしまいます。例えば、Blenderがオブジェクトの削除機能に割り当てている *X* キーを押した場合、Blenderの削除機能が起動してしまい期待した動作になりません。

## まとめ

本節では、キーボードのキーイベントを扱う方法を紹介しました。[3-1節](01_Handle_Mouse_Click_Event.md) のサンプルと比較すると、マウスのイベントとキーボードのイベントを扱うためには ```modal()``` メソッド、または ```invoke()``` メソッドに渡されてくるイベント情報が格納された引数を用いる点で同じであることを理解していただけたのではないでしょうか。

通常のアドオンの処理と異なり、ユーザからのイベントを扱ってインタラクティブな機能を提供することは、これまでに紹介してきた ```execute()``` メソッドを単に実行する処理と比べて処理が複雑になりがちでバグも発生しやすくなります。しかしキーボードやマウスのイベントを適切に扱うことができれば、アドオンで実現できることがかなり広がります。


<div id="point"></div>

### ポイント

<div id="point_item"></div>

* ```modal()``` メソッドや ```invoke()``` メソッドの引数に渡されてくるイベント情報を用いることにより、キーボードのキーイベントを扱うことができる
* 一部のキー(*Alt* キー、*Ctrl* キー、*Command* キー、*Shift* キー)には、押されている状態を判定するためのイベント情報が存在する
* ユーザのイベントを扱うことでインタラクティブな機能を提供できる反面、機能を実現するための処理が複雑化する傾向がある