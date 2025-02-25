---
aliases:
- /ja/monitors/monitor_types/integration
- /ja/monitors/create/types/integration/
description: 特定のインテグレーションのメトリクス値または健全性ステータスを監視する
further_reading:
- link: /monitors/notify/
  tag: ドキュメント
  text: モニター通知の設定
- link: /monitors/downtimes/
  tag: ドキュメント
  text: モニターをミュートするダウンタイムのスケジュール
- link: /monitors/manage/status/
  tag: ドキュメント
  text: モニターステータスを確認
title: インテグレーションモニター
---

## 概要

インテグレーションモニターを使用して、インストールした[インテグレーション][1]が実行されているか確認します。さらに詳しいモニタリングでは、メトリクスモニターを使用してインテグレーションに関する特定の情報を測定できます。

## モニターの作成

Datadog で[インテグレーションモニター][2]を作成するには

1. メインナビゲーションで、*Monitors --> New Monitor --> Integration* の順に選択します。
2. インテグレーションを検索するか、一覧または画像から選択します。

### インテグレーションのメトリクス

[メトリクスモニター][3]ドキュメントの手順に従って、インテグレーションメトリクスモニターを作成します。モニタータイプにインテグレーションメトリクスを選択すると、[モニターの管理][4] ページで、確実にインテグレーションモニタータイプのファセットでモニターを選択できるようになります。

**Note**: To configure an integration monitor, ensure that the integration submits metrics or service checks.

#### チェックを選択する

インテグレーションに含まれるチェックが 1 つしかない場合、選択する必要はありません。それ以外の場合は、モニターのチェックを選択します。

#### モニターのスコープを選択

ホスト名、タグ、または `All Monitored Hosts` を選択して、監視するスコープを決定します。特定のホストを除外する必要がある場合は、2 番目のフィールドに名前やタグをリストアップします。

* インクルードフィールドでは `AND` ロジックを使用します。リストアップされたすべてのホスト名とタグが存在するホストがスコープに含まれます。
* エクスクルードフィールドでは `OR` ロジックを使用します。リストアップされたホスト名やタグを持つホストはスコープから除外されます。

#### アラートの条件を設定する

このセクションで、**Check Alert** または **Cluster Alert** を選択します。

{{< tabs >}}
{{% tab "Check Alert" %}}

チェックアラートは、各チェックグループにつき、送信されたステータスを連続的にトラックし、しきい値と比較します。

チェックアラートをセットアップする

1. チェックレポートを送信する各 `<グループ>` に対し、アラートを個別にトリガーします。

    チェックグループは既存のグループリストから指定するか、独自に指定します。インテグレーションモニターでは、チェックごとのグループを明確にします。たとえば Postgres インテグレーションなら、`db`、`host`、`port` でタグ付けします。

2. 何回連続して失敗したらアラートをトリガーするか、回数 `<数値>` を選択します。

    各チェックは `OK`、`WARN`、`CRITICAL`、`UNKNOWN` のいずれか 1 つのステータスを送信します。`CRITICAL` ステータスが連続して何回送信されたら通知をトリガーするか選択します。たとえば、データベースで接続に失敗する異常が 1 回発生したとします。値を `> 1` に設定した場合、この異常は無視されますが、2 回以上連続で失敗した場合は通知をトリガーします。

3. インテグレーションチェックで `UNKNOWN` ステータスが報告された場合、Unknown ステータスに対して `Do not notify` または `Notify` を選択してください。

   有効にした場合、`UNKNOWN` への状態遷移は通知をトリガーします。[モニターステータスページ][1]では、`UNKNOWN` 状態のグループのステータスバーには `NODATA` のグレーが表示されます。モニターの全体的なステータスは `OK` のままです。

4. 何回連続して成功したらアラートを解決するか、回数 `<数値>` を選択します。

    何回連続して `OK` ステータスが送信されたらアラートを解決するか、回数を選択します。


[1]: /ja/monitors/manage/status
{{% /tab %}}
{{% tab "Cluster Alert" %}}

クラスターアラートは、既定のステータスでチェックの割合を計算し、しきい値と比較します。

クラスターアラートをセットアップする

1. タグによりチェックをグループ化するかどうか決定します。`Ungrouped` はすべてのソースでステータスのパーセンテージを計算します。`Grouped` は各グループごとのステータスのパーセンテージを計算します。

2. アラートのしきい値となるパーセンテージを選択します。

タグの個別の組み合わせでタグ付けされた各チェックは、クラスター内の個別のチェックと見なされます。タグの各組み合わせの最後のチェックのステータスのみが、クラスターのパーセンテージの計算で考慮されます。

{{< img src="monitors/monitor_types/process_check/cluster_check_thresholds.png" alt="クラスターチェックのしきい値" style="width:90%;">}}

たとえば、環境ごとにグループ化されたクラスターチェックモニターは、いずれかの環境のチェックの 70% 以上が `CRITICAL` ステータスを送信した場合にアラートし、いずれかの環境のチェックの 50% 以上が `WARN` ステータスを送信した場合に警告できます。
{{% /tab %}}
{{< /tabs >}}

#### 高度なアラート条件

[データなし][6]、[自動解決][7]、[新しいグループ遅延][8]の各オプションに関する情報は、[モニターコンフィギュレーション][5]ドキュメントを参照してください。

#### 通知

For detailed instructions on the **Configure notifications and automations** section, see the [Notifications][9] page.

## その他の参考資料

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/integrations/
[2]: https://app.datadoghq.com/monitors#create/integration
[3]: /ja/monitors/types/metric/
[4]: https://app.datadoghq.com/monitors/manage
[5]: /ja/monitors/configuration/#advanced-alert-conditions
[6]: /ja/monitors/configuration/#no-data
[7]: /ja/monitors/configuration/#auto-resolve
[8]: /ja/monitors/configuration/#new-group-delay
[9]: /ja/monitors/notify/