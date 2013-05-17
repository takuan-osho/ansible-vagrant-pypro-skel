ansible-vagrant-pypro-skelとは?
===============

Pythonプロフェッショナルプログラミング_ 内で使われているUbuntuの開発環境を Vagrant_ と Ansible_ を使って VirtualBox_ に自動で構築できるplaybookの詰め合わせ。

あくまで個人用に作ったので、作りは荒いです。

目的
----

以下箇条書き。

- 個人的に Pythonプロフェッショナルプログラミング_ をまだ学習しきってないので、すぐに仮想環境を用意できるようにすること
- Chef_ でも Puppet_ でも Ansible_ でも何でもいいので、自動でサーバーを構成できる楽しさをすぐに体験できるようにすること

実際に Pythonプロフェッショナルプログラミング_ で作っていく環境と違う点
-------------------------------------------------------------------------

色々ありますが、現時点で気づいているところだけ書いていきます。

- VirtualBoxに入るOSがubuntu-11.1-server-amd64ではない
    - 入るのはUbuntu precise 64 (Vagrantが `公式に上げているbox <http://files.vagrantup.com/precise64.box>`_ を利用)
    - Vagrantに入れるboxを作るのが面倒・分からない人向けに自動で環境を作れる様にしたかったからこういう仕様にしています。Ubuntu Serverじゃないのは、Vagrant公式のbox(=中身がおそらく安全なbox)の中でUbuntu Serverが見当たらなかったからです。
    - 新しくboxをVagrantの中に入れたくない人や、既にUbuntu Serverのboxを作ってキャッシュしている人はVagrantfileを書き換えてください。

- 本には記載されているけど入ってないミドルウェアやソフトウェアがある
    - Python2.6
        - Ubuntuのバージョン・種類が違うためか、apt-get searchで出てこなかったので入れてません。
    - Redmine
        - 入れるのが(おそらく非常に)面倒なので入れてません。私もまだAnsible初心者なので無理です。
    - Jenkins
        - Redmine程ではないにしろ、複数コマンドを行う必要があり、まだ私のAnsibleの理解だと上手く行かなそうなので外しました。
    - openjdk-6-jre-headless
        - openjdk-7-jre-headlessがあったので、そちらを入れています。
        - OpenJDKは Pythonプロフェッショナルプログラミング_ の中ではJenkinsをインストールするためだけに入れていると思われるので、もしopenjdk-7-jre-headlessだと動かない場合は入れ換えて下さい。
    - あと細かいのがあるかもしれませんが忘れました。


使い方
------

とりあえず使えるようにする手順を紹介。

動作確認しているのは以下の環境だけなので、他の環境だと問題が発生するかも。

:OS:
    OS X Mountain Lion 10.8.3
:Python:
    2.7.2 (OS X 標準のPython)
:VirtualBox:
    4.2.12 r84980
:Vagrant:
    1.2.2
:Ansible:
    1.1

まず予め次のものをインストールしておく

- VirtualBox_
- Vagrant_

ここでは VirtualBox_ や Vagrant_ の使い方は省略。

Ansibleも最初にインストールしておく。

::

    pip install ansible

    もしくは

    easy_install ansible

pipやdistributeを入れていない場合は、`ymotongpoo <https://github.com/ymotongpoo>`_ さんの `こちらの記事 <http://ymotongpoo.hatenablog.com/entry/2012/10/18/144352>`_ を参考にするといいと思います。

次にやることは、適当な場所でansible-vagrant-pypro-skelをcloneした後、Vagrantを使ってVirtualBoxに仮想環境を作ることです。

::

    git clone git@github.com:takuan-osho/ansible-vagrant-pypro-skel.git
    cd ansible-vagrant-pypro-skel
    vagrant up

これでVirtualBox内に仮想環境が構築されるので、Ansibleを使う準備をします。

::

    cd playbooks
    ansible-playbook main.yml -i hosts -k

こうすると、

::

    SSH password:

と表示されてパスワードを要求されるので、vagrant と打ち込んでやればAnsibleが動いてmain.ymlに記述された処理が実行され、各種パッケージがインストールされるはずです。

プロンプトにはこんな感じで出力されると思います。

::

    PLAY [pypro] *********************

    GATHERING FACTS *********************
    ok: [192.168.33.10]

    TASK: [Install basic server pkgs] *********************
    changed: [192.168.33.10] => (item=build-essential,libsqlite3-dev,libreadline6-dev,libgdbm-dev,zlib1g-dev,libbz2-dev,sqlite3,tk-dev,zip,python-dev,python-pip,python-setuptools,python3.2,subversion,openjdk-7-jre-headless,nginx)

    TASK: [Install vim] *********************
    changed: [192.168.33.10] => (item=vim)

    TASK: [Pip install basic server pkgs] *********************
    ok: [192.168.33.10] => (item=distribute)
    changed: [192.168.33.10] => (item=virtualenv)
    changed: [192.168.33.10] => (item=virtualenvwrapper)
    changed: [192.168.33.10] => (item=IPython)
    changed: [192.168.33.10] => (item=pep8)
    changed: [192.168.33.10] => (item=pyflakes)
    changed: [192.168.33.10] => (item=Flask)
    changed: [192.168.33.10] => (item=trac)
    changed: [192.168.33.10] => (item=sphinx)
    changed: [192.168.33.10] => (item=mercurial)
    changed: [192.168.33.10] => (item=nose)
    changed: [192.168.33.10] => (item=mock)
    changed: [192.168.33.10] => (item=webtest)
    changed: [192.168.33.10] => (item=django)
    changed: [192.168.33.10] => (item=unittest-xml-reporting)
    changed: [192.168.33.10] => (item=coverage)
    changed: [192.168.33.10] => (item=fabric)
    changed: [192.168.33.10] => (item=gunicorn)
    changed: [192.168.33.10] => (item=South)
    changed: [192.168.33.10] => (item=bpmappers)
    ok: [192.168.33.10] => (item=chardet)
    changed: [192.168.33.10] => (item=feedparser)
    changed: [192.168.33.10] => (item=pillow)
    ok: [192.168.33.10] => (item=pycrypto)
    changed: [192.168.33.10] => (item=tweepy)

    PLAY RECAP *********************
    192.168.33.10                  : ok=4    changed=3    unreachable=0    failed=0

上記の操作をした直後、また同じようにansible-playbookをすると以下のようになります。

出力が少し変わっているのが分かるかと思います。

::

    PLAY [pypro] *********************

    GATHERING FACTS *********************
    ok: [192.168.33.10]

    TASK: [Install basic server pkgs] *********************
    ok: [192.168.33.10] => (item=build-essential,libsqlite3-dev,libreadline6-dev,libgdbm-dev,zlib1g-dev,libbz2-dev,sqlite3,tk-dev,zip,python-dev,python-pip,python-setuptools,python3.2,subversion,openjdk-7-jre-headless,nginx)

    TASK: [Install vim] *********************
    ok: [192.168.33.10] => (item=vim)

    TASK: [Pip install basic server pkgs] *********************
    ok: [192.168.33.10] => (item=distribute)
    ok: [192.168.33.10] => (item=virtualenv)
    ok: [192.168.33.10] => (item=virtualenvwrapper)
    ok: [192.168.33.10] => (item=IPython)
    ok: [192.168.33.10] => (item=pep8)
    ok: [192.168.33.10] => (item=pyflakes)
    ok: [192.168.33.10] => (item=Flask)
    ok: [192.168.33.10] => (item=trac)
    ok: [192.168.33.10] => (item=sphinx)
    ok: [192.168.33.10] => (item=mercurial)
    ok: [192.168.33.10] => (item=nose)
    ok: [192.168.33.10] => (item=mock)
    ok: [192.168.33.10] => (item=webtest)
    ok: [192.168.33.10] => (item=django)
    ok: [192.168.33.10] => (item=unittest-xml-reporting)
    ok: [192.168.33.10] => (item=coverage)
    ok: [192.168.33.10] => (item=fabric)
    ok: [192.168.33.10] => (item=gunicorn)
    ok: [192.168.33.10] => (item=South)
    ok: [192.168.33.10] => (item=bpmappers)
    ok: [192.168.33.10] => (item=chardet)
    ok: [192.168.33.10] => (item=feedparser)
    ok: [192.168.33.10] => (item=pillow)
    ok: [192.168.33.10] => (item=pycrypto)
    ok: [192.168.33.10] => (item=tweepy)

    PLAY RECAP *********************
    192.168.33.10                  : ok=4    changed=0    unreachable=0    failed=0

.. _Pythonプロフェッショナルプログラミング: http://www.shuwasystem.co.jp/products/7980html/3294.html
.. _VirtualBox: https://www.virtualbox.org/
.. _Vagrant: http://www.vagrantup.com/
.. _Ansible: http://ansible.cc
.. _Chef: http://www.opscode.com/chef/
.. _Puppet: http://puppetlabs.com/puppet/what-is-puppet/
