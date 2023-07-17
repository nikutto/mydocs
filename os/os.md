# OS

## Linux

### OSの調べ方

`cat /etc/*releases | grep PRETTY_NAME`でversionもわかる。

### Debian系

package managerはaptだったり、dpkgだったり。

#### Debian


#### Ubuntu


### Redhat系

#### Package manager

package managerはrpm, yum, dnfあたり。
rpmのリッチ版がyum。yumの後発がdnf（dandified yum）。
コンテナのimageとしてはUBI minimalというものが使われることがあり、
UBI minimalではmicrodnfというパッケージマネジャーを使う。

rpmでは.rpmファイルをinstallできるが依存性解決がついていない。
`rpm -i <URL>`でinstallできる（URLはローカルのパスでもnetのURLでもいい）。
yumでは依存性解決もしてくれる。`yum install <package name>`でOK。
dnfとyumはパフォーマンス面と依存性解決くらいが違うらしく、併用してもどちらを使っても動くらしい。

#### 個別のOSたち

##### RHEL

Red Hat Enterprise Linux。Red Hat社が開発するOS。

##### Fedora

RedHatに支援されるコミュニティ。RHELの先行版にあたるらしい。

##### CentOS

RHELのOSS版。2024にサポート終了するらしい。

##### Rocky linux

RHELと100%互換を持つように作られている。
CentOSのサポート終了を受けて、初期のCentOSの共同創設者がプロジェクトを立ち上げた。

### Arch linux

軽量を目標にしているらしい。
パッケージマネジャーはpacmanなど。
ローリングアップデートなので互換性を切るタイプのupdateがない。major versionのような概念がない。

### Alpine

超軽量なやつ。Dockerで使われがち。
パッケージマネジャーはapkなど。
