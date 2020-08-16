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
  
  



