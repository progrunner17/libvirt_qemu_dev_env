# libvirt_qemu_dev_env

libvirtとQEMUを自前でビルドしてテストするための環境自動構築用リポジトリ。

`vagrant up`すると、自動でlibvirt, qemuをclone&ビルドして、vagrant(vagrant-libvirt)のセットアップまでしてくれる。

## requisites
* ansible
  * [community.general collection](https://github.com/ansible-collections/community.general) のインストールも必要
* vagrant
  * [vagrant-libvirt](https://github.com/vagrant-libvirt/vagrant-libvirt) (plugin)
    * libvirt
    * QEMU/KVM (nested 有効化)

## 使用例
#### ホスト
```bash
$ git clone git@github.com:progrunner17/libvirt_qemu_dev_env.git
$ cd libvirt_qemu_dev_env
$ vagrant up
$ vagrant ssh
```

#### ゲスト(L1)
ホストでの `vagrant ssh` の後の例
```bash
$ mkdir myvm
$ cd myvm
$ vagrant init generic/ubuntu2004
$ vagrant up
$ vagrant ssh
```

