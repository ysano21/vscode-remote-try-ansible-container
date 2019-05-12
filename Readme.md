# Ansible開発環境 for VSCodeユーザ

VSCodeでAnsible開発環境を構築するもっともスマートな方法を紹介したいと思います（特にWindowsユーザ）<br/>
結論から言うと先日発表され話題になったVSCode拡張機能「[Remote Development](https://code.visualstudio.com/blogs/2019/05/02/remote-development)」を用います

# 手順

以下の手順はDocker＋VSCodeになっています
Dockerを用いる理由としては環境を汚さないのと、複数環境の同居が容易

1. Dockerをインストールし、起動する<br/>
    共有ドライブを有効にすること！（Windowsの場合は**必ずCドライブも追加**）
2. [VSCode Insiders](https://code.visualstudio.com/insiders/)をインストール<br/>
    該当の拡張機能はまだプレビュー版のため、VSCodeのInsiders版を導入する必要があります<br/>
    ちょっとだけ試してみたい方はZip版がおすすめ
3. [こちらのサンプルソース](https://github.com/comefigo/vscode-remote-try-ansible-container)をローカルにクローン
4. VSCodeを起動し、拡張機能「[Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)」をインストールする
5. `.devcontainer/requirements.txt`にansible実行時に必要なpythonライブラリを追加することで、コンテナイメージをビルドする際に取り込まれます
6. 「Remote-Containers: Open Folder in Container...」で前述のサンプルソースの「.devcontainer」フォルダがあるディレクトリを選択
![vscode-remote-open-folder.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/30522/0d6aa028-8bf6-3ef4-5b40-a260a527c4a6.png)

7. コンテナイメージの自動ビルド、コンテナの作成、拡張機能の自動インストールが行われる
8. ターミナルを「bash」に切り替えて、ansibleコマンドを使ってみましょう！
![vscode-remote-open-terminal.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/30522/f6228eac-2cf5-595e-0028-657b2e5c75c5.png)

    ```shell
    > ansible-playbook -i localhost, -c local playbook.yml
    ```

9. **enjoy ansible!**

## 環境の再構築

.devcontainer配下の定義を変更したら以下のように環境を再構築

![vscode-remote-rebuild-container.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/30522/364e1d0c-c978-0a35-bde2-727d9d99e857.png)


# 説明

## Docker

.devcontainerフォルダ配下のDockerfileとdevcontainer.jsonによってコンテナの作成と実行が定義されている
Dockerfileも既存のまま利用できるので使わない手はないと思います
コンテナの起動後に`vscode-server-linux`がコンテナ内に導入され、このようなリモート接続が実現されているようです
![vscode-remote-vscode-server.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/30522/bcc13f2a-bddb-9e4b-a520-4e787e199850.png)


## 拡張機能

devcontainer.jsonのextensionsで定義されている拡張機能は作成された**コンテナ内にのみ**導入されているので、環境に応じて必要最低限の拡張機能で済む
![vscode-remote-installed-extensions.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/30522/bded32e1-335f-ddaf-4fe2-affdb9e247a7.png)
また、拡張機能の導入も含めてInfrastracture as a Codeとして管理できるので、手順書の量も減らせられる
ちなみにextensionsの名前は拡張機能の以下の箇所で調べることができる（一部導入できないものもある）
![vscode-remote-extension-name.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/30522/81d5df9f-0e87-657d-c7b6-fbffaaf6165d.png)


## リファレンス

- [devcontainer.json](https://code.visualstudio.com/docs/remote/containers#_devcontainerjson-reference)

# まとめ

「Remote Development」で何が変わるのかを一言でいうとより**環境を意識する必要がなくなった**と思います
今まではDocker＋Ansible構成でansibleが使えるまでにdockerコマンドが度々登場していたが、
上記の手順のようにdockerコマンドは一切出てこない。つまり今までDockerで開発環境を作ったのはいいが、Dockerを知らない人からするとよくわからないから敬遠されがちだった
このように**タダのリモート環境**として扱えることで抵抗が減るのではないかと思います。もちろん利便性も向上するので、Dockerに慣れている人もどんどん使っていくべきだと思いました

# 参考

- 「[Dockerで立ち上げた開発環境をVS Codeで開く!](https://qiita.com/yoskeoka/items/01c52c069123e0298660)」
- https://github.com/Microsoft/vscode-remote-try-python