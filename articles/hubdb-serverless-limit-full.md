---
title: "HubDB×サーバレス関数：HubSpotフォーム送信上限を実現する方法（コード付き）"
emoji: "🗄️"
type: "tech"
topics: [HubSpot, HubDB, HubSpot CMS]
published: true
published_at: 2025-04-29
---

# HubDB × サーバレス関数でHubSpotフォームの送信上限を実現する方法【コード付き】💻

2025-04-29 · Reina

## はじめに 🌿
HubSpot のフォームには **送信上限**を付けられない。この課題を解決するために、**HubDB とサーバレス関数**で実装してみました 💻

![デモイメージ](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8829%E6%97%A5%2022_53_21%20(1).png)

## 仕組み（デモ動画の動き）🎥
この仕組みでは、こんな動きをしてるよ！

サーバレス関数経由でHubDBの数値をカウントアップ

上限数に達したらフォーム非表示（メッセージ表示）

申込み期限（UTC基準）を過ぎたら自動でフォーム非表示！

https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/Screencast%20from%202025-04-29%2021-15-34%20(video-converter.com).mp4



## 前提の整理 ☕
**シナリオ**
- HubSpot フォームで申し込んでもらう
- 予約者数に制限をかけたい
- 定数超えたらフォームを自動非表示に
- ついでにフォーム送信可能な **期限**のタイミングも付けたい

**構成イメージ**
- HubDB でフォームごとの情報を管理
- CMS テンプレート内で利用
- フォーム送信時にカウントアップ
- サーバレス関数側で HubDB API を使って更新

![動作イメージ](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8829%E6%97%A5%2022_54_38%20(1).png)

## 使った技術・ファイル・ツール ⚡
- [HubL の if 文](https://developers.hubspot.jp/docs/reference/cms/hubl/if-statements)
- [HubDB](https://developers.hubspot.com/docs/guides/cms/storage/hubdb/overview#access-hubdb-data-using-hubl)（[v3 API](https://developers.hubspot.jp/docs/reference/api/cms/hubdb#post-%2Fcms%2Fv3%2Fhubdb%2Ftables%2F%7Btableidorname%7D%2Fdraft%2Fpublish)）
- [Serverless Functions](https://developers.hubspot.jp/docs/guides/cms/content/data-driven-content/serverless-functions/overview)
- JavaScript（[hbspt.forms](https://developers.hubspot.com/docs/reference/cms/forms/legacy-forms)）

## 実装コードの概要 🔧
### HubDB のテーブル（例）
| カラム名 | 用途 | データ型 |
|---|---|---|
| form_guid | 対象のフォームID（送信元の特定に使用） | text |
| current_submissions | 現在の送信数 | number |
| max_submissions | 送信可能な最大数 | number |
| deadline | 申込締切日時（UTC） | datetime |

> このテーブルは フォームごとの送信状態を管理するためのもので、テンプレート側のif文で送信数の上限や締切日をもとにフォームの表示/非表示を切り替えます

### テンプレート内の HubL
```html
{# 今この瞬間のローカル時間を取得してUNIXタイムに変換してるよ #}
{% set today = unixtimestamp(local_dt) %} 
{# HubDBのテーブルIDを指定してデータを取得！ #}
{% set table = hubdb_table_rows("HUBDB_ID_HERE") %} 
{% for row in table %}
  {% if row.form_guid  "FORM_GUID_HERE" %}
    {% set current_submissions = row.current_submissions %}
    {% set submission_limit = row.max_submissions %}
    {% set deadline_timestamp = row.deadline %}

    {% if current_submissions < submission_limit and today < deadline_timestamp %}
      {# 送信数も期限もOKならフォームを表示するよ！ #}
      <div id="my-form-wrapper">
        <div id="my-form-target"></div>
        <script src="https://js.hsforms.net/forms/v2.js"></script>
        <script>
          hbspt.forms.create({
            region: "na1",
            portalId: "YOUR_PORTAL_ID",
            formId: "FORM_GUID_HERE",
            target: '#my-form-target',
            onFormSubmit: function($form) {
              // 送信時にカウントアップするためにサーバレス関数を叩く！
              fetch('/_hcms/api/update-form-submissions', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ form_guid: 'FORM_GUID_HERE' })
              });
            }
          });
        </script>
      </div>
    {% else %}
      {# 上限達成または締切すぎたら、締め切りメッセージを出す！ #}
      <div class="form-closed-message">
        <p>このフォームは締め切りました🙇‍♀️</p>
      </div>
    {% endif %}
  {% endif %}
{% endfor %}
```

if current_submissions < submission_limit and today < deadline_timestampで、送信数も期限にも制限に達していない場合のみフォームが表示されるようになっているよ

### サーバレス関数
```javascript
const HUBSPOT_PRIVATE_APP_TOKEN = process.env.FORM_LIMIT_MANAGER; // 環境変数にトークンをセット！

exports.main = async (context = {}, sendResponse) => {
  try {
    const body = context.body || {};
    const formGuid = body.form_guid;
    const tableId = 'YOUR_TABLE_ID'; //HubDBのテーブルIDを指定

    if (!formGuid) {
      return sendResponse({ statusCode: 400, body: { message: 'form_guid がリクエストに含まれていません' } });
    }

    // HubDBの行データを取得して、該当するform_guidを探す
    const recordsResponse = await fetch(`https://api.hubapi.com/cms/v3/hubdb/tables/${tableId}/rows`, { ... });
    const records = await recordsResponse.json();
    const targetRecord = records.results.find(record => record.values.form_guid = formGuid);

    if (!targetRecord) {
      return sendResponse({ statusCode: 404, body: { message: '該当レコードが見つかりませんでした' } });
    }

    // 現在の送信数を+1する準備！
    const rowResponse = await fetch(`https://api.hubapi.com/cms/v3/hubdb/tables/${tableId}/rows/${targetRecord.id}`, { ... });
    const rowData = await rowResponse.json();
    const updatedValues = {
      ...rowData.values,
      current_submissions: (targetRecord.values.current_submissions || 0) + 1
    };

    // draftに更新して...
    await fetch(`https://api.hubapi.com/cms/v3/hubdb/tables/${tableId}/rows/${targetRecord.id}/draft`, { ... });

    // そしてpublishして確定！
    await fetch(`https://api.hubapi.com/cms/v3/hubdb/tables/${tableId}/draft/publish`, { ... });

    return sendResponse({ statusCode: 200, body: { message: '送信数を更新して公開しました', newCount: updatedValues.current_submissions } });
  } catch (error) {
    return sendResponse({ statusCode: 500, body: { message: 'エラーが発生しました', error: error.message } });
  }
};
```

### serverless.json（例）
```json
{
  "runtime": "nodejs18.x",
  "version": "1.0",
  "secrets": ["FORM_LIMIT_MANAGER"],
  "endpoints": {
    "update-form-submissions": { "method": "POST", "file": "form-limit-updater.js" }
  }
}
```
このサーバレス関数はフォーム送信がされたイベントをトリガーとしてリクエストが実行されて、HubDBの "current_submissions" 列の値を +1 します

## 注意ポイント ⚡
- `deadline` / `max_submissions` は手動管理が必要
- 複数フォームを扱う場合、テンプレート側の `form_guid` 指定の扱いに工夫が必要
- HubDB のドラフト更新→公開の流れをミスらないように注意

## まとめ ✨
HubSpotの標準機能だけでは難しかった「送信上限＆期限制御」も、 ちょっとカスタムすればここまでできる！

仕組みをしっかり作り込めば、イベントやワークショップ管理がとっても楽になるよ🌸

![動作イメージ](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8829%E6%97%A5%2023_04_04.png)

## あわせて読みたい 📚
- 前編（上限の設計と仕組みの概略）: https://blog.ryamagami.com/hubspot-limit_form_01
- Serverless Functions の概要: https://developers.hubspot.jp/docs/guides/cms/content/data-driven-content/serverless-functions/overview

