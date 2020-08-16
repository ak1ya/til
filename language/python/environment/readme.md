# 環境構築

## 構築手順
- 仮想環境を作って、その中でPythonの実行やライブラリの追加をする

### pyenvのインストール
- bashの場合
```
# pyenvをインストール
$ brew install pyenv

# インストール中に表示された注意（Caveats）に従い、.bash_profileに2つのコマンドを追記する
# 参考：https://qiita.com/crankcube@github/items/15f06b32ec56736fc43a
$ ls -l ~/.bash_profile

$ cat << 'EOS' >> ~/.bash_profile
# pyenvさんに~/.pyenvではなく、/usr/loca/var/pyenvを使うようにお願いする
export PYENV_ROOT=/usr/local/var/pyenv

# pyenvさんに自動補完機能を提供してもらう
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi

EOS

# ~/.bash_profileに書いた内容を有効にする
$ source ~/.bash_profile

# インストールできたか確認
$ brew list | grep pyenv
$ pyenv --version
```

- zshの場合(差分)
```
cat << 'EOS' >> ~/.zshrc
# pyenvさんに~/.pyenvではなく、/usr/loca/var/pyenvを使うようにお願いする
export PYENV_ROOT=/usr/local/var/pyenv

# pyenvさんに自動補完機能を提供してもらう
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
EOS
# shellをリロードする
zsh -l
```

### Pythonのインストール

```
# インストールできるPythonの一覧を確認する
$ pyenv install -l
 
# 3.8.5をインストールする
$ pyenv install 3.8.5
 
# いま使っているPythonのバージョンを確認する
$ pyenv versions
 
# いまインストールした3.8.5を使うように設定する
$ pyenv global 3.8.5
 
# 確認
$ pyenv versions
$ python
```

- 実行結果
```	
$ pyenv versions
  system
* 3.8.5 (set by /usr/local/var/pyenv/version)
$ python
Python 3.8.5 (default, Aug 17 2020, 00:51:41) 
[Clang 11.0.3 (clang-1103.0.32.62)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 
$ 
```

#### バージョンが切り替わらない時
-  [参考](https://qiita.com/TheHiro/items/88d885ef6a4d25ec3020)
```
$ pyenv init
# Load pyenv automatically by appending
# the following to ~/.bash_profile:

eval "$(pyenv init -)"
```

- `~/.bash_profile`に`eval "$(pyenv init -)"`を追記して、`source ~/.bash_profile`を実行

```
$ python -V
Python 3.8.5
```

### venv(仮想環境の作成)
  
```
# 自分のユーザ名を確認する（これ以降の(ユーザ名)はこの実行結果で置き換えて読み進める）
$ whoami
 
# 例えば、/Users/(ユーザ名)/python_env というディレクトリを作ってそこに移動する
$ cd /Users/(ユーザ名)/; pwd
$ mkdir -p python_env
$ cd python_env; pwd
 
# Python 3.8.5に付いているvenvコマンドを使って、/Users/(ユーザ名)/python_env というディレクトリの中に、py3envという名前の仮想環境を作成する
$ python -m venv py3env
```

### 作成した仮想環境の有効化・無効化
- これからPythonを使うときは、必ず仮想環境py3envを有効化してから使う

```
# 仮想環境py3envを有効化する
$ cd /Users/(ユーザ名)/python_env; pwd
$ source py3env/bin/activate
 
# Pythonを使う
 
 
# 仮想環境py3envを無効化する
(py3env) $ deactivate 
```

## その他
### どのPythonを使っていたか忘れてしまった
```
# バージョン切り替えツール「pyenv」を使っているのを思い出した
$ which python
/usr/local/var/pyenv/shims/python
 
# pyenvを使って、3.8.5をインストールしたんだった
$ pyenv versions
  system
* 3.8.5 (set by /usr/local/var/pyenv/version)
$ 
 
# なんかディレクトリを作ってその中に仮想環境を作ったんだった
$ cd /Users/(ユーザ名)/python_env; pwd
 
# 有効化しよう
$ source py3env/bin/activate
```

### 仮想環境の中にライブラリをインストールしたい
```
# 行列・線形代数
(py3env) $ pip install numpy
 
# 数式処理
(py3env) $ pip install scipy
 
# グラフなどの描画
(py3env) $ pip install matplotlib
 
# データ操作
(py3env) $ pip install pandas
 
# 機械学習
(py3env) $ pip install scikit-learn
```


### 何のライブラリを入れたか忘れた
```
# 確認
(py3env) $ pip freeze -l
APScheduler==3.5.3
certifi==2018.8.24
chardet==3.0.4
ez-setup==0.9
idna==2.7
mysql-connector-python==8.0.12
protobuf==3.6.1
pytz==2018.5
requests==2.19.1
six==1.11.0
tzlocal==1.5.1
urllib3==1.23
(py3env) $ 
 
# 結果を、requirements.txtという名前のテキストファイルに保存しておく
(py3env) $ pip freeze -l > requirements.txt
```

### インストールしたライブラリをアップデートしたい
```
# 更新可能なパッケージ一覧を表示
(py3env) $ pip list -o
 
# 全部最新にしてよければ以下を実行
(py3env) $ pip list -o | awk '{print $1}' | xargs pip install -U
 
# 依存関係が壊れていないか確認
(py3env) $ pip check
```

### Pythonをアップデートしたい

- 例：3.6.6から3.7.xにあげたい
- いままでの仮想環境py3envはそのまま残して、新しい仮想環境py37envでPython3.7.xを使えるように
```
# py3envの中で、requirements.txtを作っていなければ作る
(py3env) $ cd /Users/(ユーザ名)/python_env/; pwd
(py3env) $ pip freeze -l > py3env/requirements.txt
 
# 仮想環境py3envを無効化しておきます
(py3env) $ deactivate
 
# pyenvで3.7.yを新たにインストール
$ pyenv install -l
$ pyenv install 3.7.y
$ pyenv versions
$ pyenv global 3.7.y
$ python
 
# venvで仮想環境py37envの作成・有効化
$ cd /Users/(ユーザ名)/python_env; pwd
$ python -m venv py37env
$ source py37env/bin/activate
(py37env) $
 
# 仮想環境py3envで作った requirements.txt を仮想環境py37envのディレクトリにコピー
(py37env) $ cp -p py3env/requirements.txt py37env/requirements.txt
 
# 仮想環境py3envでインストールしていたライブラリを py37envにそのままインストール
(py37env) $ pip install -r py37env/requirements.txt
```

### 失敗したのでアンインストールして最初からやり直したい
- 仮想環境は、そのディレクトリを消すだけでOK
- venvを使って新しく仮想環境を作り直す

```
# 仮想環境py3envを無効化しておきます。
(py3env) $ deactivate
 
# 仮想環境py3envディレクトリを消せばOK
$ cd /Users/(ユーザ名)/python_env; pwd
$ rm -r py3env
 
# 新しく仮想環境を作ります
$ python -m venv py3envnew
$ source py3envnew/bin/activate
(py3envnew) $
```

- pyenvでインストールしたPythonをアンインストールする
```	
# pyenvでインストールしたPythonを確認してアンインストール
$ pyenv versions
$ pyenv uninstall 3.6.6
```
