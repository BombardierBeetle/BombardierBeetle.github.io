#GPD pocket setup memo

初代GPDpocketを再活用するため、Linuxをインストール
プリインのWin10はストレージ負荷が重すぎた

## 端末の特徴

7" FHD Screen
USB-C Powered
    若干不安定
トラックポイント
バッテリー容量
8GB RAM
128GB SSD
 ただしeMMCのためIO性能や換装等に過度な期待不可
アルミ筐体

## WiFiリージョン

国内在住の方は日本仕様にすること


## Windows環境のディスクイメージについて

何も気にせず内蔵eMMCのすべてを上書きするか、
万が一将来Windows環境に戻したくなった時の為に、回復/OEMパーティションはそのまま残す運用のうち後者を
デュアルブートは、WindowsBootMgrの暴走が怖いので避けておく

事前にWindowsでログインして念のため回復ディスクを作成

installウィザードで、UFIパーティション（多分/dev/mmcblk0p2）をEFIに
メインのボリューム(たぶん/dev/mmcblk0p4)を/としてインストール先に指定

## Biosについて

当時の公式Ubuntuイメージは、旧BIOSの適用が推奨されていたが、
新しめのカーネルを仕様する場合は最新Biosで問題ないらしい

## OSイメージ

ありがたいことにUbuntu MateがGPDpocket専用イメージを配布してくれているため、isoファイルをDL、
balenaEtcher,rufus,Win32DiskImager,unetbootln等のライティングソフトでUSBメモリに焼く

## setup

そのままセットアップ
上記のボリュームの取り扱いに注意
また、ロケールは日本語で問題ないが、キーボードはUSキーボード(104)を指定すること

セットアップ後、MozC-fcitxアイコンとデスクトップのフリーズが繰り返される場合は
~/.config/fcitxディレクトリを削除し再起動


## セキュリティ

専門的なことはわからないので、不要なポートが空いてないか確認

    ss -natp

631 IPP 印刷関連？ GoogleChromeについてきた可能性
34264 chromoting関連か？



## toDo

パスワード共有
共有フォルダ

バッテリーの満充電対策
    ThinkpadはTLPで可能らしいが、機種固有のパラメータのため絶望的


## issue

起動時のBluetoothが毎回ON→バッテリー的に若干不利
デフォルトOFF、あるいはWin/Macのようにシャットダウン前のステータスを引き継ぎたい
以下の2箇所の設定を変更することで無効化が可能

本環境（Ubuntu Mate 19.04）ではBluemanが管理している模様
以下手順より、Bluemanトレイからプラグイン＞PowerManager＞設定でAutoPowerOnをオフにする
https://winaero.com/blog/disable-bluetooth-auto-power-blueman/

さらに以下の設定を編集
```/etc/bluetooth/main.conf
    [Policy]
    AutoEnable=false
```

WiFiのアンテナピクトの強度が低く表示される
日本仕様WiFiの出力が弱いのが原因？


ブラウザ H/Wサポート


## memo

デスクトップ（Mate)のスケーリング設定


ランレベル（死語？）の「一時的な」切替
systemdのターゲット？詳しい内容や違いをしらべておく

CUI
    Ctrl+Alt+F2
    Ctrl+Alt+F3
GUI
    Ctrl+Alt+F7



chrome remote desktopの設定

ホームディレクトリに .chrome-remote-desktop-session を作成
exec /usr/sbin/lightdm-session "<YOUR_EXEC_COMMAND>"

execの部分にはcinnnamon-session-cinnamon？
これで待ち受けが可能になるが、セッションは失敗するので要修正


chromeリモートデスクトップ、Mateデスクトップ環境にログインは簡単だが、Cinnamonはなんか色々対策が必要っぽい
ホームディレクトリに.chrome-remote-desktop-sessionを置いただけじゃムリなんかな

```~/.bashrc
    case $TERM in
        linux) LANG=C ;;
        *)       LANG=ja_JP.UTF-8;;
    esac
```