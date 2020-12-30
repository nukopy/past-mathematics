# 作業ログ：TeX Live 2019 -> 2020 へのアップグレード

tlmgr でパッケージを入れようとしたら TeX Live のアップグレードが必要そうだったので公式の「[Upgrade from TeX Live 2019 -> 2020](https://tug.org/texlive/upgrade.html)」を見ながらやった．

環境壊れそうで怖いから作業ログを残す．

## （おまけ）環境構築

本記事には関係ないが，唐突にインストールしたときのメモをしたくなった．また環境を構築できるようにお祈り（本当は Docker でやった方が楽なんだろうな）．

薄い記憶を辿ると多分 [TeX Wiki の macOS のインストールのページ](https://texwiki.texjp.org/?TeX%20Live%2FMac#texlive-install-brew) を見ながらインストールしたと思う．

- インストールコマンド

```sh
$ brew cask install mactex-no-gui
```

ちなみに現在の TeX Live の存在を確認．

```sh
$ brew cask list | grep tex
mactex-no-gui
$ ls /usr/local/texlive
2019 texmf-local  # 2019 年版がある
```

## TeX Live 2019 -> 2020 へのアップグレード

アップグレードする．

### アップグレードをすることになったきっかけ

最近は pLaTeX で書いてる．文書中に下線を引いたときの改行が `\underline` だと出来なくて，それを解決するために `ulinej` というパッケージを入れようとしたのがきっかけ．

```sh
$ sudo tlmgr install ulinej

tlmgr: Remote repository is newer than local (2019 < 2020)
Cross release updates are only supported with
update-tlmgr-latest(.sh/.exe) --update
Please see https://tug.org/texlive/upgrade.html for details.
```

- 参考：[tex の下線部を改行しましょう．](https://mekanical.hatenadiary.org/entry/20110324/1300918062)

### アップグレード

公式の「[Upgrade from TeX Live 2019 -> 2020](https://tug.org/texlive/upgrade.html)」を見ながらアップグレード作業を行う．

Unix

1. Find the parent directory of the current installation; it's /usr/local/texlive by default.
2. Copy the whole directory 2019 to 2020, preserving symbolic links; for example:
   `cp -a 2019 2020`
   If you don't understand this, stop here and do a regular installation.
3. To save some space, you can exclude tlpkg/backups/\* or remove them from the 2020/ directory afterwards. (Theoretically, you could rename 2019 to 2020, but we strongly advise against this, since you may lose your existing installation with no good way back.)
4. If you installed symlinks in system directories (via the installer option or tlmgr path add), remove them now with tlmgr path remove.
5. As needed, adjust your PATH in your startup files to point to .../2020/bin/platform instead of .../2019/....
6. Log out and log in, and confirm that your PATH now has the 2020 directory. This is crucial. Your PATH must use the new location.
7. cd to your top-level .../2020 directory.
8. Download the latest update-tlmgr-latest.sh and run it like this:
   `sh update-tlmgr-latest.sh -- --upgrade`
9. (The extra options are to try to prevent the upgrade from happening unintentionally.)
   If you don't want to use the default repository (that is, not the automatic CTAN redirection) for downloading the new files, run (as usual):
   `tlmgr option repository yourrepo`
10. Run (with patience, it will be downloading all the new material):
    `tlmgr update --self --all`
11. Remake the lualatex/fontspec cache:
    luaotfload-tool -fu
12. If you want symlinks in system directories (not recommended), run tlmgr path add.
13. When you are happy with how the new TL is working, if you wish you can remove the old installation by running `.../2019/.../tlmgr uninstall` (full tlmgr doc).Not recommended, since you might always find a document that doesn't work with the new version, just at the wrong time …

Good luck, and to reiterate from the top, don't do any of this if it doesn't make sense to you. Just do a fresh installation.
