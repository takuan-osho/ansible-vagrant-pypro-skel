About pypro-skel
================

Pythonプロフェッショナルプログラミング_ 内で使われているUbuntuの開発環境を Vagrant_ と Ansible_ を使って VirtualBox_ に自動で構築できるplaybookの詰め合わせ。

Aim
===

以下箇条書き。

- 個人的に Pythonプロフェッショナルプログラミング_ をまだ学習しきってないので、すぐに仮想環境を用意できるようにする
- Chef_ でも Puppet_ でも Ansible_ でも何でもいいので、自動でサーバーを構成できる楽しさをすぐに体験できるようにする

Usage
-----

詳しくは後で書く

::

   pip install ansible

::

   ansible-playbook -i hosts main.yml -u vagrant -k

SSHのパスワードは公式のものと一緒。

.. _Pythonプロフェッショナルプログラミング: http://www.shuwasystem.co.jp/products/7980html/3294.html
.. _VirtualBox: https://www.virtualbox.org/
.. _Vagrant: http://www.vagrantup.com/
.. _Ansible: http://ansible.cc
.. _Chef: http://www.opscode.com/chef/
.. _Puppet: http://puppetlabs.com/puppet/what-is-puppet/
