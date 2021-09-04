# zylo
--
    import "."

zLogの拡張機能を開発するためのフレームワークです。

## Usage

```go
const (
	CW    = 0
	SSB   = 1
	FM    = 2
	AM    = 3
	RTTY  = 4
	OTHER = 5
)
```
zLogバイナリファイルの通信方式の列挙子です。

```go
const (
	M1_9  = 0
	M3_5  = 1
	M7    = 2
	M10   = 3
	M14   = 4
	M18   = 5
	M21   = 6
	M24   = 7
	M28   = 8
	M50   = 9
	M144  = 10
	M430  = 11
	M1200 = 12
	M2400 = 13
	M5600 = 14
	G10UP = 15
)
```
zLogバイナリファイルの周波数帯の列挙子です。

```go
const ResponseCapacity = 256
```
問合わせの返り値の長さの限度です。

```go
const SettingsFileName = "zlog.ini"
```
設定を保管するファイルの名前です。

```go
var CityMultiList string
```
市区町村や国や地域の番号のリストを指定します。

```go
var FileExtFilter string
```
対応済みの書式の名称と拡張子のリストを設定し、 インポート及びエクスポート機能を有効化します。

```go
var OnAssignEvent = func(contest, configs string) {}
```
得点計算の権限が移譲された場合に呼び出されます。

```go
var OnAttachEvent = func(contest, configs string) {}
```
コンテストを開いた直後に呼び出されます。

```go
var OnDeleteEvent = func(qso *QSO) {}
```
交信記録が削除された際に呼び出されます。 修正時はまず削除、次に追加が行われます。

```go
var OnDetachEvent = func(contest, configs string) {}
```
コンテストを閉じた直後に呼び出されます。

```go
var OnExportEvent = func(source, format string) error {
	return nil
}
```
zLogがエクスポートした交信記録の書式を変換します。

```go
var OnFinishEvent = func() {}
```
zLogの終了時に呼び出されます。

```go
var OnImportEvent = func(source, target string) error {
	return nil
}
```
交信記録をzLogでインポート可能な書式に変換します。

```go
var OnInsertEvent = func(qso *QSO) {}
```
交信記録が追加された際に呼び出されます。 修正時はまず削除、次に追加が行われます。

```go
var OnLaunchEvent = func() {}
```
zLogの起動時に呼び出されます。

```go
var OnPointsEvent = func(score, mults int) int {
	return score * mults
}
```
総得点を計算します。 引数は交信の合計得点と第1マルチプライヤの異なり数です。

```go
var OnVerifyEvent = func(qso *QSO) {
	rcvd := qso.GetRcvd()
	qso.SetMul1(rcvd)
	if qso.Dupe {
		qso.Score = 0
	} else {
		qso.Score = 1
	}
}
```
交信の得点やマルチプライヤを検査する時に呼び出されます。 編集中の交信記録に対し、必要なら何度でも呼び出されます。

#### func  DisplayModal

```go
func DisplayModal(msg string, args ...interface{})
```
指定された文字列を対話的に表示します。

#### func  DisplayToast

```go
func DisplayToast(msg string, args ...interface{})
```
指定された文字列を通知欄に表示します。

#### func  GetINI

```go
func GetINI(section, key string) string
```
指定された設定の内容を取得します。

#### func  HandleButton

```go
func HandleButton(name string, handler func(int))
```
指定された名前のボタンにイベントハンドラを登録します。 起動時のみ登録できます。それ以後の登録は無視されます。

#### func  HandleEditor

```go
func HandleEditor(name string, handler func(int))
```
指定された名前の記入欄にイベントハンドラを登録します。 起動時のみ登録できます。それ以後の登録は無視されます。

#### func  Query

```go
func Query(text string) string
```
指定されたクエリで問合わせを行います。

#### func  SetINI

```go
func SetINI(section, key, value string)
```
指定された設定の内容を変更します。

#### func  UnicodeToShiftJIS

```go
func UnicodeToShiftJIS(utf string) (string, error)
```
指定された文字列をSJISに変換します。

#### type BinaryData

```go
type BinaryData []byte
```


#### func (BinaryData) LoadBinaryData

```go
func (source BinaryData) LoadBinaryData(log []QSO)
```
バイト列をQSO構造体に変換します。

#### type QSO

```go
type QSO struct {
	ID   uint32
	Mode byte
	Band byte
	Pow1 byte

	Score byte

	Dupe bool

	TxID byte
	Pow2 uint32
}
```

zLogバイナリファイルのQSO構造体です。

#### func  ToQSO

```go
func ToQSO(ptr uintptr) (qso *QSO)
```
指定されたポインタからQSO構造体を読み取ります。

#### func (*QSO) Delete

```go
func (qso *QSO) Delete()
```
指定された交信記録を削除します。

#### func (*QSO) Dump

```go
func (qso *QSO) Dump(locale *time.Location) []byte
```
QSO構造体をバイト列に変換します。

#### func (*QSO) GetCall

```go
func (qso *QSO) GetCall() string
```
呼出符号を返します。

#### func (*QSO) GetMul1

```go
func (qso *QSO) GetMul1() string
```
第1マルチプライヤを返します。

#### func (*QSO) GetMul2

```go
func (qso *QSO) GetMul2() string
```
第2マルチプライヤを返します。

#### func (*QSO) GetName

```go
func (qso *QSO) GetName() string
```
運用者名を返します。

#### func (*QSO) GetNote

```go
func (qso *QSO) GetNote() string
```
備考を返します。

#### func (*QSO) GetRcvd

```go
func (qso *QSO) GetRcvd() string
```
受信した番号を返します。

#### func (*QSO) GetSent

```go
func (qso *QSO) GetSent() string
```
送信した番号を返します。

#### func (*QSO) GetTime

```go
func (qso *QSO) GetTime(zone *time.Location) time.Time
```
交信時刻を返します。

#### func (*QSO) Insert

```go
func (qso *QSO) Insert()
```
指定された交信記録を追加します。

#### func (*QSO) SetCall

```go
func (qso *QSO) SetCall(value string)
```
第2マルチプライヤを返します。

#### func (*QSO) SetMul1

```go
func (qso *QSO) SetMul1(value string)
```
第1マルチプライヤを設定します。

#### func (*QSO) SetMul2

```go
func (qso *QSO) SetMul2(value string)
```
第2マルチプライヤを設定します。

#### func (*QSO) SetName

```go
func (qso *QSO) SetName(value string)
```
運用者名を設定します。

#### func (*QSO) SetNote

```go
func (qso *QSO) SetNote(value string)
```
備考を設定します。

#### func (*QSO) SetRcvd

```go
func (qso *QSO) SetRcvd(value string)
```
受信した番号を設定します。

#### func (*QSO) SetSent

```go
func (qso *QSO) SetSent(value string)
```
送信した番号を設定します。

#### func (*QSO) Update

```go
func (qso *QSO) Update()
```
指定された交信記録を更新します。
