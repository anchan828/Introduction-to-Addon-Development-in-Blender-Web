---
pagetitle: 3-10. ユーザー・プリファレンスを活用する
subtitle: 3-10. ユーザー・プリファレンスを活用する
---

ユーザは、アドオンごとに提供される設定を、ユーザー・プリファレンスから変更することができます。
例えば、アドオンで提供する機能に割り当てるキーボードのキーやUIのフォントなど、ユーザに設定させたい情報をユーザー・プリファレンスに配置することができます。
ユーザー・プリファレンスを活用することで、プロパティパネルをはじめとした既存のUIの邪魔にならないように、アドオンの設定手段をユーザへ提供できます。


# ユーザー・プリファレンス


## ユーザー・プリファレンスにおけるアドオン設定

ユーザー・プリファレンスとは、[情報] エリアのメニュー [ファイル] > [ユーザー設定...] から開くことができるウィンドウのことを指します。
これまでの節では、ユーザー・プリファレンスを使ってBlenderのUIを日本語化したり、アドオンの有効/無効化を行ったりしてきました。
これらのBlenderの設定に加えて、アドオンの設定情報を配置するための場所が、ユーザー・プリファレンスに存在します。
ユーザは、ここに配置されたアドオンの設定情報をもとに、ユーザの好みに合わせてアドオンの設定を行います。

ユーザー・プリファレンス上にアドオンの設定情報を配置する方法を説明する前に、アドオンの設定情報が配置される場所を知っておく必要があります。
ここでは、筆者が作成したアドオン『Magic UV』を使って確認します。アドオン『Magic UV』は、[https://github.com/nutti/Magic-UV/releases/download/v4.0/uv_magic_uv.zip](https://github.com/nutti/Magic-UV/releases/download/v4.0/uv_magic_uv.zip) からダウンロードすることができます。


<div class="work"></div>

|||
|---|---|
|1|[1-4節](../chapter_01/04_Understand_Install_Uninstall_Update_Add-on.html) のインストール方法2に従って、アドオンをインストールします。|
|2|[情報] エリアのメニュー [ファイル] > [ユーザ設定...] を実行して表示されるユーザー・プリファレンスで、[アドオン] タブを選択し、『Magic UV』を有効化した状態で左の矢印をクリックして、アドオンの詳細情報を表示します。<br>![](../../images/chapter_03/10_Use_User_Preference/user_preferences_1.png "3-10節 ユーザー・プリファレンス 手順1")|
|3|[ユーザ設定:] と書かれているところが、本節で説明するアドオンの設定情報の配置先になります。<br>![](../../images/chapter_03/10_Use_User_Preference/user_preferences_2.png "3-10節 ユーザー・プリファレンス 手順2")|


## ユーザー・プリファレンスに追加すべきアドオン設定

これまで、アドオンの動作を変えるUIの配置先として、ツール・シェルフやプロパティパネルを紹介しました。
本節ではさらに、ユーザー・プリファレンスと呼ばれる新たな配置先が登場しました。
さて、このユーザー・プリファレンスですが、どのように活用したら良いのでしょうか。
極端なことを言えば、ツール・シェルフやプロパティパネルにもアドオンの設定情報を配置することができます。
しかし、全てのアドオンの設定をツール・シェルフやプロパティパネルに配置してしまうと、UIが煩雑になってしまいます。
このため、ツール・シェルフやプロパティパネルに配置する設定情報は、アドオン使用中に頻繁に変更する設定のみとするのがよさそうです。
そして、その他の設定情報は、ユーザー・プリファレンスに配置してUIが煩雑になることを防ぎます。
ユーザー・プリファレンスに配置する情報として、適切だと考えられるものを次に示します。

* アドオンの機能に割り当てる、キーボードのキーやマウスのボタン
* フォントなどのアドオンのUI設定
* 文字数の関係上、`bl_info` に記載できなかったアドオンの詳細情報（特に、`location` や `description` は長文になることが多い）
* アドオン内の機能の有効化/無効化（アドオンが複数の機能で構成される場合）
* アドオン全体で共有したい設定（アドオンが複数の機能で構成される場有）

本節のサンプルでは、一番上の用途でユーザー・プリファレンスに設定情報のUIを配置します。


# 作成するアドオンの仕様

本節では、アドオンの機能に割り当てるキーボードのキーを、ユーザー・プリファレンスから変更できるようにするため、次のような仕様のアドオンを作成します。

* [3-2節](02_Handle_Keyboard_Key_Event.html) のサンプルを改造し、オブジェクトの操作に使用するキーボードのキーを、ユーザー・プリファレンスから選択できるようにする
* 指定可能なキーは [X]、[Y]、[Z] の3個のみ
* 使用するキーを重複して設定することができる
  * 例：X軸とY軸に [X] キーを割り当てると、[X] キーを押した時にX軸とY軸の正の方向へ同時に移動する


## アドオンを作成する

[1-5節](../chapter_01/05_Install_own_Add-on.html) を参考にして以下のソースコードを入力し、ファイル名 `sample_3_10.py` として保存してください。

[@include-source pattern="full" filepath="chapter_03/sample_3_10.py"]


# アドオンを使用する

## アドオンを有効化する

[1-5節](../chapter_01/05_Install_own_Add-on.html) を参考にして作成したアドオンを有効化すると、コンソールウィンドウに文字列が出力されます。

```
サンプル3-10: アドオン「サンプル3-10」が有効化されました。
```

アドオンを有効化し、右図のようにアドオンの設定が [ユーザー設定] に表示されることを確認します。

![](../../images/chapter_03/10_Use_User_Preference/enable_add-on.png "3-10節 アドオン有効化")


## アドオンの機能を使用する

有効化したアドオンの機能を使い、動作を確認します。


<div class="work"></div>

|||
|---|---|
|1|[ユーザー設定] から、機能に割り当てられているキーボードのキーを変更します。<br>![](../../images/chapter_03/10_Use_User_Preference/use_add-on_1.png "3-10節 アドオンの使用 手順1")|
|2|[3-2節](02_Handle_Keyboard_Key_Event.html) の機能が、手順1で割り当てたキーで動作することを確認します。キーを重複して登録した場合、登録した軸の正方向へ同時に移動することができます。<br>![](../../images/chapter_03/10_Use_User_Preference/use_add-on_2.png "3-10節 アドオンの使用 手順2")|
|3|[Q] キーを押すと、オブジェクト並進移動モードが終了します。|


## アドオンを無効化する

[1-5節](../chapter_01/05_Install_own_Add-on.html) を参考にして有効化したアドオンを無効化すると、コンソールウィンドウに文字列が出力されます。

```
サンプル3-10: アドオン「サンプル3-10」が無効化されました。
```


# ソースコードの解説

本節のサンプルは、[3-2節](02_Handle_Keyboard_Key_Event.html) のサンプルを改造したものです。
このため、ユーザー・プリファレンスにアドオンの設定情報を追加し、アドオンの動作に反映させる方法のみを説明します。
その他の処理については、[3-2節](02_Handle_Keyboard_Key_Event.html) を参照してください。
なお、サンプルでポイントとなる処理は次の通りです。

* アドオン設定クラスの定義
* アドオン設定クラスに定義した設定情報の取得


## アドオン設定クラスの定義

ユーザー・プリファレンスにアドオンの設定情報を追加するためには、次に示す `bpy.types.AddonPreferences` クラスを継承したクラス（アドオン設定クラスと呼ぶことにします）を定義する必要があります。

[@include-source pattern="partial" filepath="chapter_03/sample_3_10.py" block="addon_prefs"]


アドオン設定クラスのクラス変数 `bl_idname` には、ユーザー・プリファレンスにアドオンの設定情報を追加する、パッケージ名やモジュール名を指定します。
アドオンが単一のファイルで構成される場合は、自身のモジュール名を表す `__name__` を指定します。
一方、アドオンが複数のファイルで構成される場合は、パッケージ名を指定する必要がありますが、指定方法について注意する必要があります。
アドオン設定クラスが `__init__.py` に定義されている場合は、`__name__` がパッケージ名を表すため `__name__` を指定すれば問題ありません（`__package__` を指定してもよいです）。
しかし、`__init__.py` 以外に定義されている場合は、パッケージ名を表す `__package__` を指定する必要があります。
モジュール名を表す `__name__` を指定するのは正しくありません。


<div class="column">
アドオン設定クラスのクラス変数 `bl_idname` には、すでにインストール済みの他のアドオンのパッケージ名やモジュール名を指定することができます。
この場合、アドオンを有効化すると、クラス変数 `bl_idname` に指定したパッケージ名やモジュール名を持つアドオンの設定情報に、アドオン設定クラスで定義した設定情報が表示されます。（クラス変数bl_idnameに指定した表示先のアドオンと、アドオン設定クラスを定義したアドオンの両方が有効化されている必要があることに注意が必要です。）  
例えば、Blenderに標準搭載されている（サポートレベルがOfficialである）アドオンである「UV Layout」（パッケージ名io_mesh_uv_layout）に設定情報を表示するためには、`bl_idname = 'io_mesh_uv_layout'` のように指定します。
</div>


ユーザー・プリファレンスに追加するアドオンの設定情報は、プロパティクラスとしてクラス変数に定義します。
そして、アドオン設定クラスの `draw` メソッドに、ユーザー・プリファレンスに追加する設定情報のUIを構築するための処理を追加します。
`draw` メソッドで記述する処理は、これまでパネルクラスやツール・シェルフのオプション上でUIを構築するために、オペレータクラスに実装してきた `draw` メソッドと書き方が基本的に同じです。
しかし、`row.prop` 関数などのUIを追加する関数の第1引数には、表示したい設定情報（プロパティクラス）を定義したインスタンスを指定する必要があることに注意が必要です。
サンプルでは、`draw` メソッドが呼び出されたインスタンスとプロパティクラスのクラス変数を定義しているインスタンスが同じであることから、`self` を指定します。

ここまでの処理で、ユーザー・プリファレンスにアドオンの設定情報が追加できます。
続いて、ユーザー・プリファレンスに追加したアドオンの設定情報を、アドオンの処理内で受け取る方法を説明します。


## アドオン設定クラスに定義した設定情報の取得

ユーザー・プリファレンスに追加したアドオンの設定情報は、次のコードにより取得できます。

[@include-source pattern="partial" filepath="chapter_03/sample_3_10.py" block="get_prefs"]


本節のサンプルで追加したアドオンの設定情報に限らず、ユーザー・プリファレンスに追加されている全てのアドオンの設定情報は、パッケージ名やモジュール名をキーとした辞書型で `context.user_preferences.addons` に保存されています。
そして、サンプルで追加した設定情報は、そのインスタンス変数である `preferences` から参照することができます。

本節のサンプルは単一ファイルで構成され、かつ自分自身のアドオンの設定情報にアクセスします。
このため、`context.user_preferences.addons[__name__].preferences` でアドオンの設定情報にアクセスします。
アドオンが複数のファイルで構成されている場合は、前述の `bl_idname` に指定する値と同様に、`__name` または `__package__` を指定します。
`__init__.py` でアドオンの設定情報にアクセスする場合は `context.user_preferences.addons[__name__].preferences`、それ以外の場合は `context.user_preferences.addons[__package__].preferences` でアドオンの設定情報にアクセスできます。

これで、ユーザー・プリファレンスに追加されたアドオンの設定情報にアクセスできるようになりました。
例えばここで、アドオン設定クラスで定義したクラス変数 `x_axis` の値を取得したいのであれば、`context.user_preferences.addons[__name__].preferences.x_axis` を参照します。
本節のサンプルでは、ソースコードの記載量を減らすために `context.user_preferences.addons[__name__].preferences` を `prefs` に代入しているため、`prefs.x_axis` で値を取得できます。

最後に、アドオンの設定情報を使ってオブジェクトを移動する処理を次に示します。

[@include-source pattern="partial" filepath="chapter_03/sample_3_10.py" block="refer_prefs"]


`event.type` でどのボタンが押されたのかを判定するために、`'X'` などの文字列の代わりにアドオンの設定情報の値を用いることを除けば、[3-2節](02_Handle_Keyboard_Key_Event.html) と同じですので、説明は割愛します。


# まとめ

ユーザー・プリファレンスにアドオンの設定情報を配置し、設定された情報をユーザから取得する方法を紹介しました。
ユーザー・プリファレンスを活用することで、プロパティやツール・シェルフに多くの設定情報を追加することで発生する、UIが煩雑になる問題を解消することができます。
ユーザー・プリファレンスに配置する設定情報を選択する基準を決めるのはなかなか難しいところがありますが、設定情報を頻繁に設定するか否かが1つの基準になるかと思います。

本節までで、Blenderのフレームワークの中でUIを構築する方法が何度か出てきました。
幸いなことに、BlenderのUIは対象がプロパティパネルであるかユーザー・プリファレンスであるかに関わらず、基本的には同じような方法でUIを構築することができます。
1度覚えたことを他の場面でも活用できるのは、BlenderのUIが一貫した方法で構築されているからです。
実現したいことがあるたびに新たに覚えなおす必要がないため、アドオン開発者としては非常に助かります。
なお、UIの構築については、[2-8節](../chapter_02/08_Control_Blender_UI_1.html) から [2-10節](../chapter_02/10_Control_Blender_UI_3.html) の3節に渡って説明しているため、参考にしてください。

長くなりましたが、サンプルを使ってBlenderのAPIを紹介するのは、本節で最後になります。
これまで様々なアドオンのサンプルを紹介してきましたが、ここまでで紹介したBlenderのAPIを組み合わせることで、いろいろなことが実現できると思いませんか？
実際、APIは単独で使うのではなく、組み合わせて使うことでより面白く便利な機能を提供することができます。
このことを理解してもらえるように、[5章](../chapter_05/index.html) では、これまで紹介してきたAPIを組み合わせて作ったサンプルをいくつか紹介しますので、アドオンを開発するときの参考にしてみてください。

実質最後の章にあたる [4章](../chapter_04/index.html) では、アドオンの開発・公開時に参考となる情報を紹介します。
筆者のこれまでのアドオン開発の経験から、アドオン開発者に知っておいて欲しいことをまとめていますので、ぜひ最後まで読んでみてください。


## ポイント

* アドオンごとに設定や詳細情報を記述することができるUIが、ユーザー・プリファレンスに用意されている
* 頻繁に変更する設定をツール・シェルフやプロパティパネルに配置し、その他はユーザー・プリファレンスに配置するなど、設定内容に応じてアドオンの設定情報を配置する場所を決めよう
* ユーザー・プリファレンスにアドオンの設定情報を配置するためには、`bpy.types.AddonPreferences` クラスを継承したアドオン設定クラスを定義する必要がある
* アドオンの設定情報は、アドオン設定クラスのクラス変数にプロパティクラスの変数として定義し、ユーザー・プリファレンスに配置するアドオンの設定情報のUIは `draw` メソッドで定義する
* ユーザー・プリファレンスのアドオンの設定情報は、モジュール名やパッケージ名を `context.user_preferences.addons` のキーに指定することで参照できる