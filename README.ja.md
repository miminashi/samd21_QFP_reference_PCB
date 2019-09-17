# samd21_QFP_reference_PCB

これは、SWDプログラミング(訳注: Serial Wire Debug)およびテストを簡単にするためのSAMD21のリファレンスデザインです。このレイアウトは、SAMD21 QFPパッケージで動作するはずです。QFPパッケージははんだ付けが非常に簡単で、オリジナルのAruduino 328Pとほぼ同じ本数のピンを提供します。次のSAMD21パッケージはQFPです:
- SAMD21E15 32kB フラッシュメモリ \*注
- SAMD21E16 64kB フラッシュメモリ \*注
- SAMD21E17 128kB フラッシュメモリ
- SAMD21E18 256kB フラッシュメモリ

![](https://github.com/hydronics2/samd21_QFP_reference_PCB/blob/master/PCB_top.png)

私はSWDプログラミングを試してみたかったのですが、これらはAdafruitおよびSparkfunボードよく使われているSAMD21Gの安価なバージョンです。このPCBデザインは、AdafruitおよびSparkfunのリファレンスデザインに大きく依存しています。私は、Adafruitの32pin Trinketのデザインは（私のデザインに）似ていることに気付きました。これらには、Atmelが推奨する32.768MHzのクリスタルやフェライトビーズがありません。これでなぜ動いているのかはわかりません。2つのピンPA00/PA01をクリスタルに接続する代わりに、Dotstar LEDに接続しています！

このリンクからOshpark経由でボードを注文できます: [プロジェクト](https://oshpark.com/shared_projects/EjZP7lWQ)

部品の一覧を示します:

- ATSAMD21E15B from [digikey](https://www.digikey.com/product-detail/en/microchip-technology/ATSAMD21E15B-AFT/1611-ATSAMD21E15B-AFTCT-ND/6832773). ATSAMD21E15Lを入手しないこと！ピン配列がわずかに異なります.
- 10ピンSWDヘッダ from [digikey](https://www.digikey.com/product-detail/en/microchip-technology/ATSAMD21E15L-AFT/1611-ATSAMD21E15L-AFTCT-ND/6832779)
- 一般的な5.08mm（または5mm）ピッチのスクリューヘッダ
- MOHM 固定インダクタ 10uH 500mA 300mOhm [digikey](https://www.digikey.com/product-detail/en/tdk-corporation/MLZ2012N100LT000/445-6762-1-ND/2523583)
- 3.3V Regulator (18V max input) 3.3V レギュレータ (入力最大18V) [mouser](https://www.mouser.com/ProductDetail/511-LDL1117S50R)
- T0-252-3 MOS FET (必須ではない) [mouser](https://www.mouser.com/ProductDetail/ON-Semiconductor-Fairchild/FDD8780?qs=%2fha2pyFadugI30EyIBTPkO8PumBRFL59Ls98N48NSzc%3d)
- USB micro コネクタ [digikey](https://www.digikey.com/product-detail/en/amphenol-icc-fci/10118194-0001LF/609-4618-1-ND/2785382)
- 一般的な 6mm \* 6mm タクトスイッチ [sparkfun](https://www.sparkfun.com/products/97)
- スクリューターミナル 5mm ピッチ [sparkfun](https://www.sparkfun.com/products/8432) or [aliexpress](https://www.aliexpress.com/wholesale?catId=0&initiative_id=SB_20190221221755&SearchText=pcb+screw+terminal)
- 12VとUSB入力からレギュレータの間に挿入する入力保護ダイオード. これはAdafruitの設計によります [digikey](https://www.digikey.com/product-detail/en/diodes-incorporated/B130-13-F/B130-FDICT-ND/815318)
- 32.768kHz クリスタル (必要な場合. 上記のTrinketの設計を参照のこと) [digikey](https://www.digikey.com/product-detail/en/epson/FC-135-32.7680KA-AG3/SER4086DKR-ND/6132726)
- 上記の水晶からの7pf負荷と2pfの浮遊容量を想定した水晶コンデンサC7/C8, CX1 = CX2 = 2 * (7pF - 2pF) = 10pF [digikey](https://www.digikey.com/product-detail/en/wurth-electronics-inc/885012006051/732-7793-1-ND/5454420)


SWDプログラマ/デバッガ: [Segger](https://www.digikey.com/product-detail/en/segger-microcontroller-systems/8.08.91-J-LINK-EDU-MINI/899-1061-ND/7387472)

![回路図](https://github.com/hydronics2/samd21_QFP_reference_PCB/blob/master/schematic.JPG)

ノート:
- グランドプレーンを追加する
- シルクスクリーンをSAMD21E15に変更する
- シルクスクリーンのC7/C8を15pfから10pfに変更する

## \*注

SAMD21E15には32kbのフラッシュしかありません. 8ビットアーキテクチャには適していますが、32ビットSAMD21アーキテクチャには適していません!! 約50行の単純なコードはUNOで332バイト（12％）を使用しますが、SAMD21E15、SAMD21E16、SAMD21E17などでは9,000バイト（9248バイト（56％））以上をコンパイルします. 約50行の単純なコードをUNOのバイナリにコンパイルすると332バイト(12%)を使用しますが, SAMD21E15, SAMD21E16, SAMD21E17などでは9000バイト(9248バイト(56％))以上になります.

つまり, 128または256kbのSAMD21E17またはSAMD21E18を使用する必要がありました.

[Arduino forumでwestfはこんなふうに言ってました](https://forum.arduino.cc/index.php?topic=602377.msg4091161#msg4091161)...それは思ったほど悪くない. スケッチには「固定オーバーヘッド」がかなり含まれている. したがって, あなたが見ていたものは32bit RISCのためのオーバーヘッドではなく, それ(=スケッチのオーバーヘッド)だ. あなたが残した10kB(のフラッシュの領域)にかなり有意義なスケッチを収めることができるはず...ただし、ほとんどのARMライブラリは, 多くのメモリがあることを前提としているため, 効率的が悪くなるのには目をつむるように.
