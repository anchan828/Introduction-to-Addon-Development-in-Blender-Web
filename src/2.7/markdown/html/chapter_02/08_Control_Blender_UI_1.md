---
pagetitle: 2-8. BlenderのUIを制御する①
subtitle: 2-8. BlenderのUIを制御する①
---

BlenderのUIを自由に変えたいと思ったことはありませんか？
実はBlenderのUIの大半はPythonで記載されているため、Pythonを理解できていればある程度自由にBlenderのUIを変更することができます。
本節は、アドオンの開発はしないけど自分の好みのUIへ改造したい、という方にも参考になると思います。


# BlenderのUIはPythonで制御できる

BlenderのUIを変更するためには大変な労力が必要だと思っている方が多いのではないでしょうか。
しかし **BlenderのUIの大半はPythonのソースコードで書かれていますし**、本書をここまで読まれた方であれば、UIを制御するための基本的な知識が身についているはずです。
BlenderのUIがPythonのソースコードで書かれていることを確認するために、[3Dビュー] エリアのメニューを変更してみましょう。

以下の手順で、[3Dビュー] エリアのメニューを制御するソースコードを修正します。


<div class="work"></div>

|||
|---|---|
|1|[3Dビュー] エリアのメニューにある、[現在の視点をOpenGLレンダリング] ボタンにマウスカーソルを置いて [右クリック] します。<br>![](../../images/chapter_02/08_Control_Blender_UI_1/control_UI_on_View3D_1.png "3Dビューエリアのメニューを修正する 手順1")|
|2|[ソース編集] をクリックします。<br>![](../../images/chapter_02/08_Control_Blender_UI_1/control_UI_on_View3D_2.png "3Dビューエリアのメニューを修正する 手順2")|
|3|[テキストエディター] エリアにソースコードが表示されます。また、[現在の視点をOpenGLレンダリング] ボタンを表示するためのコードにカーソルが自動的に移動しています。<br>![](../../images/chapter_02/08_Control_Blender_UI_1/control_UI_on_View3D_3.png "3Dビューエリアのメニューを修正する 手順3")|
|4|カーソルが示している行をコメントアウトします。<br>![](../../images/chapter_02/08_Control_Blender_UI_1/control_UI_on_View3D_4.png "3Dビューエリアのメニューを修正する 手順4")|
|5|[テキストエディター] エリアのメニュー [テキスト] > [保存] を実行し、上書き保存します。<br>![](../../images/chapter_02/08_Control_Blender_UI_1/control_UI_on_View3D_5.png "3Dビューエリアのメニューを修正する 手順5")|
|6|[1-4節](../chapter_01/04_Understand_Install_Uninstall_Update_Add-on.html) で紹介した『Reload Scripts』機能を用いてアップデートすると、[3Dビュー] エリアのメニューから [現在の視点をOpenGLレンダリング] ボタンが消えます。<br>![](../../images/chapter_02/08_Control_Blender_UI_1/control_UI_on_View3D_6.png "3Dビューエリアのメニューを修正する 手順6")|


元の [3Dビュー] エリアのメニューに戻すためには、先ほどコメントアウトした行のコメントを外して保存し、『Reload Scripts』機能でアップデートすることで元に戻すことができます。

ここまでの説明で、BlenderのUIはPythonで制御されていることが理解できたのではないでしょうか。以降の説明では、BlenderのUIをPythonから構築する時に知っておくと良いことなどをサンプルを用いて説明します。


# 作成するアドオンの仕様

* 以下のようなタブを [3Dビュー] エリアの [ツール・シェルフ] に追加する
  * 追加したタブは、[オブジェクトモード] の時かつ、最低1つでもオブジェクトが選択されている時のみ表示される

![](../../images/chapter_02/08_Control_Blender_UI_1/add-on_spec.png "アドオンの仕様")

* [3Dビュー] エリアのメニューである [オブジェクト] の先頭に [項目1]、末尾に [項目2] を追加する


# アドオンを作成する

[1-5節](../chapter_01/05_Install_own_Add-on.html) を参考にして以下のソースコードを入力し、ファイル名を `sample_2_8.py` として保存してください。

[@include-source pattern="full" filepath="chapter_02/sample_2_8.py"]


# アドオンを使用する

## アドオンを有効化する

[1-5節](../chapter_01/05_Install_own_Add-on.html) を参考に作成したアドオンを有効化すると、コンソールウィンドウに以下の文字列が出力されます。

```
サンプル2-8: アドオン「サンプル2-8」が有効化されました。
```

さらに、[3Dビュー] エリアの [ツール・シェルフ] にタブ [カスタムメニュー] が追加されます。
タブは [オブジェクトモード] 時かつオブジェクトが1つでも選択されている時のみ表示されることに注意が必要です。
[エディットモード]、または [オブジェクトモード] でもオブジェクトが1つも選択されていない場合は、タブが表示されません。

また、[3Dビュー] エリアのメニュー [オブジェクト] の先頭に [項目1]、末尾に [項目2] が追加されます。


## アドオンの機能を使用する

[3Dビュー] エリアの [ツール・シェルフ] のタブ [カスタムメニュー] をクリックすると、カスタムメニューのメニューが表示されます。


## アドオンを無効化する

[1-5節](../chapter_01/05_Install_own_Add-on.html) を参考に有効化したアドオンを無効化すると、コンソールウィンドウに以下の文字列が出力されます。

```
サンプル2-8: アドオン「サンプル2-8」が無効化されました。
```


# ソースコードの解説


## ツール・シェルフのタブを追加する

本節のサンプルでは、ツール・シェルフにタブを追加しています。
アドオンの機能をタブとしてまとめることで、ユーザはアドオンがどのような機能を持っているかを一目でわかるようになります。
また普段よく使う機能をタブにまとめ、独自のUIを構築してBlenderの利便性を高めるのも良いかもしれません。

ツール・シェルフのタブに追加するためには、以下に示すような `bpy.types.Panel` クラスを継承した **パネルクラスを作成する** 必要があります。

[@include-source pattern="partial" filepath="chapter_02/sample_2_8.py" block="panel_cls"]


本節のサンプルでは、`bpy.types.Panel` クラスを継承した `VIEW3D_PT_CustomMenu` クラスを作成しています。
クラス名は基本的に自由につけることができますが、Blender本体では `<エリア名>_<タイプ>_<クラスを表す適切な名前>` と命名されていることが多いようですので、本節のサンプルではそれに合わせてみました。
ここで `<タイプ>` には、継承する型に応じて以下のような文字列が入ります。

|文字列|型|
|---|---|
|`MT`|`bpy.types.Menu`|
|`PT`|`bpy.types.Panel`|

クラス `VIEW3D_PT_CustomMenu` には、4つのクラス変数が定義されています。

|クラス変数|値の意味|
|---|---|
|`bl_category`|パネルに登録時にタイトルとして表示される文字列<br>ツール・シェルフに登録する場合はタブに表示される文字列となる|
|`bl_space_type`|登録先のエリア|
|`bl_region_type`|登録先のリージョン|
|`bl_label`|ツール・シェルフのタブを開いたときに表示されるメニューのヘッダーに表示される文字列|


`bl_label` には、ツール・シェルフのタブに表示される文字列を指定します。
クラス変数 `bl_space_type` には登録先のエリアを指定します。
本節のサンプルでは、[3Dビュー] エリアに登録することを考えて `VIEW_3D` を指定しています。

他にもクラス変数 `bl_space_type` には、Blenderのエリアに対応した以下のような値を指定することができます。


|設定値|エリア|
|---|---|
|`VIEW_3D`|3Dビュー|
|`TIME_LINE`|タイムライン|
|`GRAPH_EDITOR`|グラフエディター|
|`DOPESHEET_EDITOR`|ドープシート|
|`NLA_EDITOR`|NLAエディター|
|`IMAGE_EDITOR`|UV/画像エディター|
|`SEQUENCE_EDITOR`|ビデオシーケンスエディター|
|`CLIP_EDITOR`|動画クリップエディター|
|`TEXT_EDITOR`|テキストエディター|
|`NODE_EDITOR`|ノードエディター|
|`LOGIC_EDITOR`|ロジックエディター|
|`PROPERTIES`|プロパティ|
|`OUTLINER`|アウトライナー|
|`USER_PREFERENCES`|ユーザ設定|
|`INFO`|情報|
|`FILE_BROWSER`|ファイルブラウザー|
|`CONSOLE`|Pythonコンソール|


クラス変数 `bl_region_type` には登録先のリージョンを指定します。
本節のサンプルではツール・シェルフに登録するため、`TOOLS` を指定しています。

クラス変数 `bl_region_type` には、他にも以下のような値を設定することが可能です。


|設定値|値の意味|
|---|---|
|`UI`|プロパティパネル（[N] キーを押したときにエリアの右側に表示される領域）|
|`TOOLS`|ツール・シェルフ（[T] キーを押したときにエリアの左側に表示される上側部分の領域）|
|`TOOL_PROPS`|ツール・シェルフのプロパティ（[T] キーを押したときにエリアの左側に表示される下側部分の領域）|
|`WINDOW`|ウィンドウ（エリア中央の領域で常に表示される）|
|`HEADER`|ヘッダ（エリア上部または下部に表示されるバーで常に表示されている）|


`VIEW3D_PT_CustomMenu` クラスには、`poll` メソッド、`draw_header` メソッド、`draw` メソッドが定義されています。
各メソッドの処理を以下に示します。定義したメソッドの詳細については後ほど説明します。

|メソッド|処理|
|---|---|
|`poll`|本メソッドが定義されたクラスの処理が実行可能な状態にあるかを判定するメソッド<br>`True` を返すと実行可能と判断し、`False` を返すと実行不可能であると判断する<br>実行不可能と判断されると、本メソッドが定義されたクラスで定義したいかなる処理も無効化される|
|`draw_header`|ヘッダーを描画するメソッド|
|`draw`|メニューを描画するメソッド<br>本メソッドでは、ヘッダー部のUIを変更することはできない|


## タブが表示される条件を設定する

本節のサンプルでは、[オブジェクトモード] 時かつオブジェクトが選択されている時のみタブが表示される仕様としていました。
特定の状況下でのみメニューを表示したり、処理を実行させるようにしたりするためには `poll` メソッドとクラス変数 `bl_context` 活用します。

`bl_context` はパネルクラス時に指定することが可能なクラス変数で、**指定したコンテキストである時のみパネルの描画処理を実行します。**
`bl_context` には以下のような値を設定することができます。

|値|説明|
|---|---|
|`objectmode`|オブジェクトモード時のみ描画する|
|`mesh_edit`|エディットモード時のみ描画する|

本節のサンプルではタブをオブジェクトモード時のみ描画するため、`bl_context = "objectmode"` としています。

また、`poll` メソッドでは、オブジェクトが選択されている時にのみ描画する処理を追加しています。

[@include-source pattern="partial" filepath="chapter_02/sample_2_8.py" block="poll"]

`poll` メソッドはクラス単位の処理となるため、クラスメソッドとして定義する必要があります。
このため、メソッドの前にデコレータ `@classmethod` をつける必要があります。

`poll` メソッドに渡されてくる引数は以下の通りです。

|引数|型|意味|
|---|---|
|`cls`|`bpy.types.RNAMeta`|`poll` メソッドを実装したクラス|
|`context`|`bpy_types.Context`|`poll` メソッド実行時のコンテキスト|

`poll` メソッド内では `bpy.data.objects` によりオブジェクトを全て取得し、選択されているオブジェクトが存在する場合は `True` を返しています。
この処理により、選択されているオブジェクトが1つでもあればタブが表示されます。


## ヘッダーの UI を変更する

タブに追加したメニューのヘッダーのUIを変更するためには、パネルクラスの `draw_header` メソッドを定義します。
`draw` メソッドでは、ヘッダーのUIを変更できない点に注意が必要です。

[@include-source pattern="partial" filepath="chapter_02/sample_2_8.py" block="draw_header"]


`draw_header` メソッドの引数は、以下の通りです。

|引数|型|値の説明|
|---|---|---|
|`self`|呼びだされた `draw_header` メソッドを定義しているクラス|オペレータクラスのインスタンス|
|`context`|`bpy_types.Context`|`draw_header` メソッド実行時のコンテキスト|

`draw_header` メソッドでは、メニューのヘッダーに表示される文字列の左にアイコンを追加する処理を追加しています。

`layout.label` の引数を以下に示します。

|引数|値の意味|
|---|---|
|`text`|表示する文字列|
|`icon`|表示するアイコン|

本節のサンプルでは文字列を追加しないため、引数 `text` に空の文字列、引数 `icon` にプラグインのアイコンIDを指定しています。


## メニューへ項目を追加する順番を制御する

[2-1節](01_Basic_of_Add-on_Development.html) では、`bpy.types.INFO_MT_mesh_add.append` 関数を用いてメニューの末尾へ項目を追加していました。

本節のサンプルでは、`bpy.types.VIEW3D_MT_object.prepend` 関数を用いてメニューの先頭にも項目を追加しています。

[@include-source pattern="partial" filepath="chapter_02/sample_2_8.py" block="append_item_to_menu"]


# まとめ

本節では、BlenderのUIをPythonから制御できることを確認しました。
また、[3Dビュー] エリアの []ツール・シェルフ] へタブを追加する方法について説明し、メニューへ項目を追加する方法を説明しました。

BlenderのUIを変更すると聞くと難しそうに思えますが、Pythonさえ理解できていればなんとかなりそうだと、ここまでの説明を読んだ方であれば思えてきたのではないでしょうか？
なお、UIの説明を行っている [2-9節](09_Control_Blender_UI_2.html) と [2-10節](10_Control_Blender_UI_3.html) の内容も一緒に理解すれば、Blenderで実現可能な大半のUIを自由に扱えるようになったと言ってよいと思います。

次節では、ボタンやメニューなどのUI部品を制御する方法について解説します。


## ポイント

* BlenderはボタンやメニューなどのUI部品を追加するためのAPIを用意しているため、APIを活用することで独自のUIを構築できる
* ツール・シェルフのタブを追加するためには、`bpy.types.Panel` クラスを継承したパネルクラスを作成し、クラス変数 `bl_region_type` に `TOOLS` を指定する必要がある
* 特定の状況下でのみメニューを表示したり処理を実行を禁止したりと、定義した処理に制限をかける場合は `poll` メソッドを使用する
* パネルクラスのクラス変数 `bl_context` を宣言することで、描画するコンテキストを指定することができる
* ツール・シェルフに追加したタブのヘッダーのUIを変更するためには、パネルクラスに `draw_header` メソッドを定義する
* ツール・シェルフに追加したタブのメニューのUIを変更するためには、パネルクラスに `draw` メソッドを定義する