# ポケモンバトルの仕様
## 技
技名           | 威力 | タイプ   | 備考               
:--            | :--  | :--      | :--                
たいあたり     | 40   | ノーマル | -                  
きりさく       | 40   | ノーマル | 急所に当たる率が2倍
でんこうせっか | 30   | ノーマル | 先制攻撃出来る
みずでっぽう   | 30   | 水       | -                  
なみのり       | 50   | 水       | -                  
ひのこ         | 30   | 火       | 20%でやけどになる  
かえんほうしゃ | 50   | 火       | -                  
したでなめる   | 30   | ゴースト | -                  
シャドーボール | 50   | ゴースト | -                  
つきのひかり   | 50   | ノーマル | 回復技             

+ 1ポケモンにつき2つまで
+ 無限に技を繰り出せる

## ダメージ計算式
### 攻撃の計算式
(技の威力 + 攻撃 - 防御) x タイプ倍率 x 一致ボーナス x 急所 x ブレ x やけど倍率

#### 倍率
要素                           | 倍率        
:--                            | :--         
こうかばつぐん                 | x 2         
こうかはいまひとつ             | x 0.5       
こうかはない                   | x 0         
自分のタイプと技のタイプが一致 | x 1.5       
急所にあたる                   | x 2         
ブレ                           | x 0.8 〜 1.2
やけどの場合                   | x 0.5       

+ 急所発生率は10%
+ ブレは攻撃一回ずつ変動する

### 回復の計算式
技の威力 x 時間ボーナス

+ やけどは治らない

#### 倍率
要素         | 倍率
:--          | :-- 
20:00 - 4:00 | x 2 
それ以外     | x 1 

## タイプ
タイプ   | ノーマル | 水         | 火       | ゴースト
:--      | :--      | :--        | :--      | :--     
ノーマル | -        | -          | -        | 無効    
水       | -        | -          | ばつぐん | -       
火       | -        | いまひとつ | -        | -       
ゴースト | 無効     | -          | -        | ばつぐん

+ 1ポケモンにつき1タイプまで
+ 1技につき1タイプまで

## ステータス
項目   | 設定値範囲
:--    | :--       
HP     | 50 〜 200 
攻撃   | 30 〜 50  
防御   | 30 〜 50  
素早さ | 30 〜 50  

以下の要素はなし

+ レベル
+ 物理/特殊

## バトルの流れ
+ ポケモンを1匹ずつ出す
+ 双方がポケモンに指示を出す
+ 素早い方から攻撃する
  + でんこうせっかが衝突した場合、素早さ計算に基づいて行動する
+ 交代はなく、手持ちは1匹までとする
+ やけどの場合、双方行動後にHPの1/16のダメージを受ける

## 決着
+ HPが先に無くなった方が負け
+ 決着が付かない組み合わせの場合、即引き分けとする

# 試したいこと
## 単体テスト
以下の要素の単体テストをうまくやってみたい

+ 乱数
  + 倍率の数値
  + 発生確率の数値
+ 時間
+ 双方が一切ダメージを与えられなくなる状況が発生しない
  + ダメージ的に
  + タイプ相性的に
+ 技の応酬を繰り返し、HPの残数テストをしたい

## 設計実装
+ ポケモンのタイプと技のタイプをうまく書きたい
+ ダメージ計算をうまく書きたい
  + 依存するクラスが多そう
  + 乱数が影響する
