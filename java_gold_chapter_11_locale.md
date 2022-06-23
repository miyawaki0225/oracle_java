# ローカライズとフォーマット

## ロケール

|言語|言語コード（ISO639）|国|国コード（ISO3166）|
|---|---|---|---|
|日本語|ja|日本|JP|
|英語|en|米国|US|
|イタリア語|it|イタリア|IT|
|フランス語|fr|フランス|FR|

言語コードには国コードが設定されていない。
```java
import java.util.Locale;

public class Main {
	public static void main(String[] args) {
		print("デフォルト     ",Locale.getDefault());
		print("new Locale()  ",new Locale("ja","JP"));
		print("JAPAN         ",Locale.JAPAN);
		print("JAPANESE      ",Locale.JAPANESE);
	}

	static void print(String msg, Locale locale) {
		System.out.println(msg + ":locale     "+locale);
		System.out.println(msg + ":language   "+locale.getLanguage());
		System.out.println(msg + ":country    "+locale.getCountry());
	}
}
```
```java
Locale aLocale = new Locale.Builder()
				.setLanguage("sr")
				.setScript("Latn")
				.setRegion("RS")
				.build();
```

## リソースバンドル
アプリのユーザーインターフェースのロケールによって表示を自動的に切り替える。  
利用法：
- ListResorceBundleクラスを継承したpublicなクラスを作成する。
- getContents()メソッドをオーバーライドし、配列でリソースのリストを作成する。
- リソースは、キーと値を要素とする配列として作成する。


|クラス名|説明|
|---|---|
|ListResourceBuncel|リソースバンドルをリソースのリストとして管理するクラス|
|PropertyResourceBundle|リソースバンドルをプロパティファイルで管理するクラス|
※同一アプリケーション内に混在可能

命名ルール
- MyResources:規定名のみの場合は、デフォルトロケールの場合に読み込まれる。
- MyResources_en:規定名と言語コードを「\_」でつなぐ。言語コードを指定したロケールの場合に読み込まれる。
- MyResources_en_US:規定名と言語コード、国コードを「\_」でつなぐ。言語コード及び国コードを指定した場合読み込まれる。


```java
//Object[][]をオーバーライドする
public class MyResources_en_US extends ListResourceBundle {
	@Override
	protected Object[][] getContents() {
		Object[][] contents = { { "send", "send" }, { "cancel", "cancel" } };
		return contents;
	}
}
```

- プロパティファイルを読み込ませる場合、デフォルトだと「src」の直下にある。

## フォーマット
