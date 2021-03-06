---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-25"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}



# アノテーションを使用した Ingress のカスタマイズ
{: #ingress_annotation}

Ingress アプリケーション・ロード・バランサー (ALB) に機能を追加するため、Ingress リソースにメタデータとしてアノテーションを指定できます。
{: shortdesc}

**重要**: アノテーションを使用する前に、『[Ingress を使用してアプリを公開する](cs_ingress.html)』の手順に従い、Ingress サービス構成を適切にセットアップしてください。 基本構成で Ingress ALB をセットアップしたら、Ingress リソース・ファイルにアノテーションを追加して機能を拡張できます。

<table>
<caption>一般的なアノテーション</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>一般的なアノテーション</th>
 <th>名前</th>
 <th>説明</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-external-service">外部サービス</a></td>
 <td><code>proxy-external-service</code></td>
 <td>{{site.data.keyword.Bluemix_notm}} でホストされるサービスなどの外部サービスへのパス定義を追加します。</td>
 </tr>
 <tr>
 <td><a href="#location-modifier">ロケーション修飾子</a></td>
 <td><code>location-modifier</code></td>
 <td>ALB が要求 URI とアプリ・パスを突き合わせる方法を変更します。</td>
 </tr>
 <tr>
 <td><a href="#location-snippets">ロケーション・スニペット</a></td>
 <td><code>location-snippets</code></td>
 <td>サービスのロケーション・ブロックのカスタム構成を追加します。</td>
 </tr>
 <tr>
 <td><a href="#alb-id">プライベート ALB のルーティング</a></td>
 <td><code>ALB-ID</code></td>
 <td>プライベート ALB によって着信要求をアプリにルーティングします。</td>
 </tr>
 <tr>
 <td><a href="#rewrite-path">再書き込みパス</a></td>
 <td><code>rewrite-path</code></td>
 <td>着信ネットワーク・トラフィックを、バックエンド・アプリが listen する別のパスにルーティングします。</td>
 </tr>
 <tr>
 <td><a href="#server-snippets">サーバー・スニペット</a></td>
 <td><code>server-snippets</code></td>
 <td>サーバー・ブロックのカスタム構成を追加します。</td>
 </tr>
 <tr>
 <td><a href="#tcp-ports">TCP ポート</a></td>
 <td><code>tcp-ports</code></td>
 <td>標準以外の TCP ポートを介してアプリにアクセスします。</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>接続アノテーション</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>接続アノテーション</th>
 <th>名前</th>
 <th>説明</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">接続タイムアウトおよび読み取りタイムアウトのカスタマイズ</a></td>
  <td><code>proxy-connect-timeout、proxy-read-timeout</code></td>
  <td>ALB がバックエンド・アプリへの接続とそこからの読み取りを待機する時間を設定します。この時間を超えると、そのバックエンド・アプリは使用不可と見なされます。</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">キープアライブ要求数</a></td>
  <td><code>keepalive-requests</code></td>
  <td>1 つのキープアライブ接続で処理できる要求の最大数を設定します。</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">キープアライブ・タイムアウト</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>サーバー上でキープアライブ接続を開いた状態で保つ最大時間を設定します。</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">次のアップストリームにプロキシー</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>ALB が要求を次のアップストリーム・サーバーに渡すことができる場合に設定します。</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">Cookie によるセッション・アフィニティー</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>スティッキー Cookie を使用して、着信ネットワーク・トラフィックを常に同じアップストリーム・サーバーにルーティングします。</td>
  </tr>
  <tr>
  <td><a href="#upstream-fail-timeout">アップストリーム・エラーのタイムアウト</a></td>
  <td><code>upstream-fail-timeout</code></td>
  <td>サーバーを使用不可と見なす前に、ALB がサーバーへの接続を試行する時間の長さを設定できます。</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">アップストリーム・キープアライブ</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>アップストリーム・サーバーのアイドル・キープアライブ接続の最大数を設定します。</td>
  </tr>
  <tr>
  <td><a href="#upstream-max-fails">アップストリーム失敗最大回数</a></td>
  <td><code>upstream-max-fails</code></td>
  <td>サーバーを使用不可と見なす前に、サーバーとの通信の試行に失敗できる最大回数を設定します。</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>HTTPS および TLS/SSL 認証アノテーション</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>HTTPS および TLS/SSL 認証アノテーション</th>
  <th>名前</th>
  <th>説明</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#appid-auth">{{site.data.keyword.appid_short}} 認証</a></td>
  <td><code>appid-auth</code></td>
  <td>{{site.data.keyword.appid_full_notm}} を使用して、アプリの認証を行います。</td>
  </tr>
  <tr>
  <td><a href="#custom-port">カスタムの HTTP および HTTPS ポート</a></td>
  <td><code>custom-port</code></td>
  <td>HTTP (ポート 80) および HTTPS (ポート 443) ネットワーク・トラフィックのデフォルト・ポートを変更します。</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">HTTPS への HTTP リダイレクト</a></td>
  <td><code>redirect-to-https</code></td>
  <td>ドメイン上の非セキュアな HTTP 要求を HTTPS にリダイレクトします。</td>
  </tr>
  <tr>
  <td><a href="#hsts">HTTP Strict Transport Security (HSTS)</a></td>
  <td><code>hsts</code></td>
  <td>ドメインへのアクセスに HTTPS のみ使用するようにブラウザーを設定します。</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">相互認証</a></td>
  <td><code>mutual-auth</code></td>
  <td>ALB の相互認証を構成します。</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">SSL サービス・サポート</a></td>
  <td><code>ssl-services</code></td>
  <td>SSL サービス・サポートで、HTTPS が必要なアップストリーム・アプリへのトラフィックを暗号化できるようにします。</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>Istio アノテーション</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Istio アノテーション</th>
<th>名前</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td><a href="#istio-services">Istio サービス</a></td>
<td><code>istio-services</code></td>
<td>Istio 管理対象サービスにトラフィックを転送します。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>プロキシー・バッファー・アノテーション</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>プロキシー・バッファー・アノテーション</th>
 <th>名前</th>
 <th>説明</th>
 </thead>
 <tbody>
 <tr>
 <td><a href="#proxy-buffering">クライアント応答データのバッファリング</a></td>
 <td><code>proxy-buffering</code></td>
 <td>クライアントに応答を送信する間、ALB でのクライアント応答バッファリングを無効にします。</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">プロキシー・バッファー</a></td>
 <td><code>proxy-buffers</code></td>
 <td>プロキシー・サーバーからの単一の接続に対して、応答を読み取るバッファーの数とサイズを設定します。</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">プロキシー・バッファー・サイズ</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>プロキシー・サーバーから受け取る応答の、最初の部分を読み取るバッファーのサイズを設定します。</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">プロキシー・ビジー・バッファー・サイズ</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>ビジー状態にすることができるプロキシー・バッファーのサイズを設定します。</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>要求/応答アノテーション</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>要求/応答アノテーション</th>
<th>名前</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td><a href="#add-host-port">ホスト・ヘッダーへのサーバー・ポートの追加</a></td>
<td><code>add-host-port</code></td>
<td>要求を転送するために、ホストにサーバー・ポートを追加します。</td>
</tr>
<tr>
<td><a href="#client-max-body-size">クライアント要求本体のサイズ</a></td>
<td><code>client-max-body-size</code></td>
<td>クライアントが要求の一部として送信できる本体の最大サイズを設定します。</td>
</tr>
<tr>
<td><a href="#large-client-header-buffers">ラージ・クライアント・ヘッダー・バッファー</a></td>
<td><code>large-client-header-buffers</code></td>
<td>ラージ・クライアント要求ヘッダーを読み取るバッファーの最大数と最大サイズを設定します。</td>
</tr>
<tr>
<td><a href="#proxy-add-headers">クライアント要求またはクライアント応答の追加ヘッダー</a></td>
<td><code>proxy-add-headers、response-add-headers</code></td>
<td>クライアント要求をバックエンド・アプリに転送する前に要求にヘッダー情報を追加します。または、クライアント応答をクライアントに送信する前に応答にヘッダー情報を追加します。</td>
</tr>
<tr>
<td><a href="#response-remove-headers">クライアント応答ヘッダーの削除</a></td>
<td><code>response-remove-headers</code></td>
<td>クライアント応答をクライアントに転送する前に、応答からヘッダー情報を削除します。</td>
</tr>
</tbody></table>

<br>

<table>
<caption>サービス制限アノテーション</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>サービス制限アノテーション</th>
<th>名前</th>
<th>説明</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">グローバルな速度制限</a></td>
<td><code>global-rate-limit</code></td>
<td>すべてのサービスに対して、定義済みキーあたりの要求処理速度と接続数を制限します。</td>
</tr>
<tr>
<td><a href="#service-rate-limit">サービスの速度制限</a></td>
<td><code>service-rate-limit</code></td>
<td>特定のサービスに対して、定義済みキーあたりの要求処理速度と接続数を制限します。</td>
</tr>
</tbody></table>

<br>



## 一般的なアノテーション
{: #general}

### 外部サービス (proxy-external-service)
{: #proxy-external-service}

{{site.data.keyword.Bluemix_notm}} でホストされるサービスなどの外部サービスへのパス定義を追加します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>外部サービスへのパス定義を追加します。 このアノテーションは、バックエンド・サービスの代わりに外部サービスでアプリを実行する場合にのみ使用します。 このアノテーションを使用して外部サービス経路を作成する場合、併用できるのは、`client-max-body-size`、`proxy-read-timeout`、`proxy-connect-timeout`、`proxy-buffering` の各アノテーションのみです。 その他のアノテーションは、`proxy-external-service` と併用できません。<br><br><strong>注</strong>: 単一のサービスとパスに、複数のホストを指定することはできません。
</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: cafe-ingress
  annotations:
    ingress.bluemix.net/proxy-external-service: "path=&lt;mypath&gt; external-svc=https:&lt;external_service&gt; host=&lt;mydomain&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>path</code></td>
 <td><code>&lt;<em>mypath</em>&gt;</code> を、外部サービスが listen するパスに置き換えます。</td>
 </tr>
 <tr>
 <td><code>external-svc</code></td>
 <td><code>&lt;<em>external_service</em>&gt;</code> を、呼び出される外部サービスに置き換えます。 例えば、<code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code> を入力します。</td>
 </tr>
 <tr>
 <td><code>host</code></td>
 <td><code>&lt;<em>mydomain</em>&gt;</code> を、外部サービスのホスト・ドメインに置き換えます。</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### ロケーション修飾子 (location-modifier)
{: #location-modifier}

ALB が要求 URI とアプリ・パスを突き合わせる方法を変更します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>デフォルトでは、ALB はアプリが listen するパスを接頭部として処理します。 ALB がアプリへの要求を受け取ると、ALB は、要求 URI の先頭に一致するパス (接頭部として指定) があるか Ingress リソースを検査します。 一致するものが見つかった場合、要求は、アプリがデプロイされているポッドの IP アドレスに転送されます。<br><br>`location-modifier` アノテーションは、ロケーション・ブロックの構成を変更することによって、ALB が一致を検索する方法を変更します。 ロケーション・ブロックは、アプリ・パスに対する要求の処理方法を決定します。<br><br><strong>注</strong>: 正規表現 (regex) パスを処理するには、このアノテーションが必要です。</dd>

<dt>サポートされる修飾子</dt>
<dd>

<table>
<caption>サポートされる修飾子</caption>
 <col width="10%">
 <col width="90%">
 <thead>
 <th>修飾子</th>
 <th>説明</th>
 </thead>
 <tbody>
 <tr>
 <td><code>=</code></td>
 <td>等号修飾子を使用すると、ALB は完全一致のみを選択します。 完全一致が検出されると、検索は停止し、一致したパスが選択されます。<br>例えば、アプリが <code>/tea</code> で listen する場合、ALB はアプリへの要求を突き合わせるときに、厳密に一致する <code>/tea</code> パスのみを選択します。</td>
 </tr>
 <tr>
 <td><code>~</code></td>
 <td>波形記号修飾子を使用すると、マッチング時に ALB はパスを大/小文字を区別する正規表現パスとして処理します。<br>例えば、アプリが <code>/coffee</code> で listen する場合、ALB はアプリへの要求を突き合わせるときに、<code>/ab/coffee</code> パスまたは <code>/123/coffee</code> パスを選択できます (これらのパスがアプリ用に明示的に設定されていなくてもかまいません)。</td>
 </tr>
 <tr>
 <td><code>~\*</code></td>
 <td>波形記号の後にアスタリスクが続く修飾子では、マッチング時に ALB はパスを大/小文字を区別しない正規表現パスとして処理します。<br>例えば、アプリが <code>/coffee</code> で listen する場合、ALB はアプリへの要求を突き合わせるときに、<code>/ab/Coffee</code> パスまたは <code>/123/COFFEE</code> パスを選択できます (これらのパスがアプリ用に明示的に設定されていなくてもかまいません)。</td>
 </tr>
 <tr>
 <td><code>^~</code></td>
 <td>脱字記号の後に波形記号が続く修飾子では、ALB は正規表現パスではなく、非正規表現による最適な一致を選択します。</td>
 </tr>
 </tbody>
</table>

</dd>

<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/location-modifier: "modifier='&lt;location_modifier&gt;' serviceName=&lt;myservice1&gt;;modifier='&lt;location_modifier&gt;' serviceName=&lt;myservice2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: &lt;myservice&gt;
          servicePort: 80</code></pre>

 <table>
 <caption>アノテーションの構成要素について</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>modifier</code></td>
  <td><code>&lt;<em>location_modifier</em>&gt;</code> を、パスに使用するロケーション修飾子に置き換えてください。 サポートされる修飾子は、<code>「=」</code>、<code>「~」</code>、<code>「~\*」</code>、および<code>「^~」</code>です。 修飾子は単一引用符で囲む必要があります。</td>
  </tr>
  <tr>
  <td><code>serviceName</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
  </tr>
  </tbody></table>
  </dd>
  </dl>

<br />


### ロケーション・スニペット (location-snippets)
{: #location-snippets}

サービスのロケーション・ブロックのカスタム構成を追加します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>サーバー・ブロックは、ALB 仮想サーバーの構成を定義する nginx ディレクティブです。 ロケーション・ブロックは、サーバー・ブロック内で定義される nginx ディレクティブです。 ロケーション・ブロックは、要求 URI、またはドメイン名または IP アドレスおよびポートの後の要求の一部を Ingress がどのように処理するかを定義します。<br><br>サーバー・ブロックが要求を受け取ると、ロケーション・ブロックが URI をパスと突き合わせ、要求がアプリのデプロイ先のポッドの IP アドレスに転送されます。 <code>location-snippets</code> アノテーションを使用することで、ロケーション・ブロックが要求を特定のサービスに転送する方法を変更できます。<br><br>代わりにサーバー・ブロック全体を変更する方法については、<a href="#server-snippets">server-snippets</a> アノテーションを参照してください。</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/location-snippets: |
    serviceName=&lt;myservice&gt;
    # Example location snippet
    proxy_request_buffering off;
    rewrite_log on;
    proxy_set_header "x-additional-test-header" "location-snippet-header";
    &lt;EOS&gt;
spec:
tls:
- hosts:
  - mydomain
  secretName: mytlssecret
rules:
- host: mydomain
  http:
    paths:
    - path: /
      backend:
        serviceName: &lt;myservice&gt;
        servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成したサービスの名前に置き換えます。</td>
</tr>
<tr>
<td>ロケーション・スニペット</td>
<td>指定したサービスに使用する構成スニペットを指定します。 このサンプル・スニペットは、プロキシー要求バッファリングをオフにして、ログの再書き込みをオンにし、要求を <code>myservice</code> サービスに転送するときに追加ヘッダーを設定するように、ロケーション・ブロックを構成しています。</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### プライベート ALB のルーティング (ALB-ID)
{: #alb-id}

プライベート ALB によって着信要求をアプリにルーティングします。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
パブリック ALB の代わりに着信要求を転送するプライベート ALB を選択します。</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/ALB-ID: "&lt;private_ALB_ID&gt;"
spec:
tls:
- hosts:
  - mydomain
  secretName: mytlssecret
rules:
- host: mydomain
  http:
    paths:
    - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>プライベート ALB の ID。 プライベート ALB の ID を検索するには、<code>ibmcloud ks albs --cluster &lt;my_cluster&gt;</code> を実行します。<p>
有効な状態のプライベート ALB が複数含まれる複数ゾーン・クラスターがある場合、<code>;</code> で区切って ALB ID のリストを指定できます。 例: <code>ingress.bluemix.net/ALB-ID: &lt;private_ALB_ID_1&gt;;&lt;private_ALB_ID_2&gt;;&lt;private_ALB_ID_3&gt</code></p>
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### 再書き込みパス (rewrite-path)
{: #rewrite-path}

ALB ドメイン・パスへの着信ネットワーク・トラフィックを、バックエンド・アプリケーションが listen する別のパスにルーティングできます。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>Ingress ALB ドメインは、<code>mykubecluster.us-south.containers.appdomain.cloud/beans</code> への着信ネットワーク・トラフィックをアプリにルーティングします。 アプリは、<code>/beans</code> ではなく <code>/coffee</code> で listen します。 着信ネットワーク・トラフィックをアプリに転送するには、Ingress リソース構成ファイルに再書き込みアノテーションを追加します。 再書き込みアノテーションにより、<code>/beans</code> 上の着信ネットワーク・トラフィックは <code>/coffee</code> パスを使用してアプリに転送されるようになります。 複数のサービスを含める場合は、セミコロン (;) のみを使用して区切ってください。</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/rewrite-path: "serviceName=&lt;myservice1&gt; rewrite=&lt;target_path1&gt;;serviceName=&lt;myservice2&gt; rewrite=&lt;target_path2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /beans
        backend:
          serviceName: myservice1
          servicePort: 80
</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td><code>&lt;<em>target_path</em>&gt;</code> を、アプリが listen するパスに置き換えます。 ALB ドメイン上の着信ネットワーク・トラフィックは、このパスを使用して Kubernetes サービスに転送されます。 大多数のアプリは特定のパスで listen するのではなく、ルート・パスと特定のポートを使用します。 上記の例では、再書き込みパスは <code>/coffee</code> として定義されました。</td>
</tr>
</tbody></table>

</dd></dl>

<br />


### サーバー・スニペット (server-snippets)
{: #server-snippets}

サーバー・ブロックのカスタム構成を追加します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>サーバー・ブロックは、ALB 仮想サーバーの構成を定義する nginx ディレクティブです。 <code>server-snippets</code> アノテーションを使用してカスタム構成スニペットを提供することにより、ALB が要求を処理する方法を変更できます。</dd>

<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/server-snippets: |
    location = /health {
    return 200 'Healthy';
    add_header Content-Type text/plain;
    }
spec:
tls:
- hosts:
  - mydomain
  secretName: mytlssecret
rules:
- host: mydomain
  http:
    paths:
    - path: /
      backend:
        serviceName: &lt;myservice&gt;
        servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td>サーバー・スニペット</td>
<td>使用する構成スニペットを指定します。 このサンプル・スニペットは、<code>/health</code> 要求を処理するロケーション・ブロックを指定しています。 ロケーション・ブロックは、正常という応答を返し、要求を転送するときにヘッダーを追加するよう構成されています。</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### アプリケーション・ロード・バランサー用 TCP ポート (tcp-ports)
{: #tcp-ports}

標準以外の TCP ポートを介してアプリにアクセスします。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
このアノテーションは、TCP ストリーム・ワークロードを実行するアプリに使用します。

<p>**注:** ALB はパススルー・モードで動作し、トラフィックをバックエンド・アプリに転送します。 この場合、SSL 終端はサポートされません。 TLS 接続は終了せず、何もしないで通過します。</p>
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/tcp-ports: "serviceName=&lt;myservice&gt; ingressPort=&lt;ingress_port&gt; [servicePort=&lt;service_port&gt;]"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: &lt;myservice&gt;
          servicePort: 80</code></pre>

 <table>
 <caption>アノテーションの構成要素について</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code> を、標準以外の TCP ポートでアクセスする Kubernetes サービスの名前に置き換えます。</td>
  </tr>
  <tr>
  <td><code>ingressPort</code></td>
  <td><code>&lt;<em>ingress_port</em>&gt;</code> を、アプリにアクセスするための TCP ポートに置き換えます。</td>
  </tr>
  <tr>
  <td><code>servicePort</code></td>
  <td>このパラメーターはオプションです。 指定した場合、トラフィックがバックエンド・アプリに送信される前に、ポートはこの値に置き換えられます。 指定しない場合、ポートは Ingress ポートと同じままです。</td>
  </tr>
  </tbody></table>

 </dd>
 <dt>使用法</dt>
 <dd><ol><li>ALB 用の開かれたポートを確認します。
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 出力は、以下のようになります。
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>ALB の構成マップを開きます。
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>TCP ポートを構成マップに追加します。 <code>&lt;port&gt;</code> を、開く TCP ポートに置き換えます。 <b>注</b>: デフォルトでは、ポート 80 とポート 443 が開いています。 80 と 443 を開いたままにしておく場合は、指定する他の TCP ポートに加えて、それらのポートも `public-ports` フィールドに含める必要があります。 プライベート ALB を有効にした場合は、開いたままにしておくポートも `private-ports` フィールドで指定する必要があります。 詳しくは、<a href="cs_ingress.html#opening_ingress_ports">Ingress ALB でポートを開く</a>を参照してください。
<pre class="codeblock">
<code>apiVersion: v1
kind: ConfigMap
data:
 public-ports: 80;443;&lt;port1&gt;;&lt;port2&gt;
metadata:
 creationTimestamp: 2017-08-22T19:06:51Z
 name: ibm-cloud-provider-ingress-cm
 namespace: kube-system
 resourceVersion: "1320"
 selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
 uid: &lt;uid&gt;</code></pre></li>
 <li>ALB が TCP ポートによって再構成されたことを確認します。
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 出力は、以下のようになります。
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>標準以外の TCP ポートを介してアプリにアクセスするように Ingress を構成します。 この解説ではサンプル YAML ファイルを使用します。 </li>
<li>ALB リソースを作成するか、既存の ALB 構成を更新します。
<pre class="pre">
<code>kubectl apply -f myingress.yaml</code></pre>
</li>
<li>Ingress サブドメインに対して curl を実行して、アプリにアクセスします。 例: <code>curl &lt;domain&gt;:&lt;ingressPort&gt;</code></li></ol></dd></dl>

<br />


## 接続アノテーション
{: #connection}

### 接続タイムアウトおよび読み取りタイムアウトのカスタマイズ (proxy-connect-timeout、proxy-read-timeout)
{: #proxy-connect-timeout}

ALB がバックエンド・アプリへの接続とそこからの読み取りを待機する時間を設定します。この時間を超えると、そのバックエンド・アプリは使用不可と見なされます。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>クライアント要求が Ingress ALB に送信されると、ALB はバックエンド・アプリへの接続を開きます。 デフォルトでは、ALB はバックエンド・アプリからの応答を受信するまで 60 秒待機します。 バックエンド・アプリが 60 秒以内に応答しない場合は、接続要求が中止され、バックエンド・アプリは使用不可と見なされます。

</br></br>
ALB がバックエンド・アプリに接続されると、応答データは ALB によってバックエンド・アプリから読み取られます。 この読み取り操作では、ALB はバックエンド・アプリからのデータを受け取るために、2 回の読み取り操作の間に最大 60 秒間待機します。 バックエンド・アプリが 60 秒以内にデータを送信しない場合、バックエンド・アプリへの接続が閉じられ、アプリは使用不可と見なされます。
</br></br>
60 秒の接続タイムアウトと読み取りタイムアウトは、プロキシー上のデフォルトのタイムアウトであり、通常は変更すべきではありません。
</br></br>
アプリの可用性が安定していない場合や、ワークロードが多いためにアプリの応答が遅い場合は、接続タイムアウトまたは読み取りタイムアウトを引き上げることもできます。 タイムアウトを引き上げると、タイムアウトになるまでバックエンド・アプリへの接続を開いている必要があるため、ALB のパフォーマンスに影響が及ぶことに注意してください。
</br></br>
一方、タイムアウトを下げると、ALB のパフォーマンスは向上します。 ワークロードが多いときでも、指定したタイムアウト内でバックエンド・アプリが要求を処理できるようにしてください。</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-connect-timeout: "serviceName=&lt;myservice&gt; timeout=&lt;connect_timeout&gt;"
   ingress.bluemix.net/proxy-read-timeout: "serviceName=&lt;myservice&gt; timeout=&lt;read_timeout&gt;"
spec:
 tls:
 - hosts:
   - mydomain
  secretName: mytlssecret
rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;connect_timeout&gt;</code></td>
 <td>バックエンド・アプリに接続されるのを待機する秒数または分数 (例: <code>65s</code> または <code>1m</code>)。 <strong>注:</strong> 接続タイムアウトは、75 秒より長くできません。</td>
 </tr>
 <tr>
 <td><code>&lt;read_timeout&gt;</code></td>
 <td>バックエンド・アプリが読み取られるまで待機する秒数または分数 (例: <code>65s</code> または <code>2m</code>)。
 </tr>
 </tbody></table>

 </dd></dl>

<br />



### キープアライブ要求数 (keepalive-requests)
{: #keepalive-requests}

1 つのキープアライブ接続で処理できる要求の最大数を設定します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
1 つのキープアライブ接続で処理できる要求の最大数を設定します。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/keepalive-requests: "serviceName=&lt;myservice&gt; requests=&lt;max_requests&gt;"
spec:
tls:
- hosts:
  - mydomain
  secretName: mytlssecret
rules:
- host: mydomain
  http:
    paths:
    - path: /
      backend:
        serviceName: &lt;myservice&gt;
        servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。 このパラメーターはオプションです。 特定のサービスが指定されていなければ、Ingress ホスト内のすべてのサービスにこの構成が適用されます。 このパラメーターを指定した場合は、所定のサービスに対してキープアライブ要求が設定されます。 パラメーターを指定しない場合は、キープアライブ要求が構成されていないすべてのサービスに対して、サーバー・レベルの <code>nginx.conf</code> でキープアライブ要求が設定されます。</td>
</tr>
<tr>
<td><code>requests</code></td>
<td><code>&lt;<em>max_requests</em>&gt;</code> を、1 つのキープアライブ接続で処理できる要求の最大数に置き換えます。</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### キープアライブ・タイムアウト (keepalive-timeout)
{: #keepalive-timeout}

サーバー側でキープアライブ接続を開いた状態に保つ最大時間を設定します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
サーバー上でキープアライブ接続を開いた状態に保つ最大時間を設定します。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/keepalive-timeout: "serviceName=&lt;myservice&gt; timeout=&lt;time&gt;s"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。 このパラメーターはオプションです。 このパラメーターを指定した場合は、所定のサービスに対してキープアライブ・タイムアウトが設定されます。 パラメーターを指定しない場合は、キープアライブ・タイムアウトが構成されていないすべてのサービスに対して、サーバー・レベルの <code>nginx.conf</code> でキープアライブ・タイムアウトが設定されます。</td>
 </tr>
 <tr>
 <td><code>timeout</code></td>
 <td><code>&lt;<em>time</em>&gt;</code> を時間の長さ (秒単位) に置き換えます。 例: <code>timeout=20s</code>。 値をゼロに設定すると、キープアライブ・クライアント接続が無効になります。</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />


### 次のアップストリームにプロキシー (proxy-next-upstream-config)
{: #proxy-next-upstream-config}

ALB が要求を次のアップストリーム・サーバーに渡すことができる場合に設定します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
Ingress ALB は、クライアント・アプリとユーザー・アプリの間のプロキシーとして機能します。 一部のアプリの構成では、ALB からの着信クライアント要求を処理する複数のアップストリーム・サーバーが必要です。 ALB で使用するプロキシー・サーバーが、アプリで使用するアップストリーム・サーバーとの接続を確立できない場合があります。 その場合、ALB は次のアップストリーム・サーバーとの接続を確立して、要求を代わりに渡すことができます。 `proxy-next-upstream-config` アノテーションを使用すると、ALB が要求を次のアップストリーム・サーバーに渡そうと試行するケース、期間、回数を設定できます。<br><br><strong>注</strong>: `proxy-next-upstream-config` を使用する場合は必ずタイムアウトが構成されるため、このアノテーションに `timeout=true` は追加しないでください。
</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-next-upstream-config: "serviceName=&lt;myservice1&gt; retries=&lt;tries&gt; timeout=&lt;time&gt; error=true http_502=true; serviceName=&lt;myservice2&gt; http_403=true non_idempotent=true"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 80
</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
</tr>
<tr>
<td><code>retries</code></td>
<td><code>&lt;<em>tries</em>&gt;</code> を、ALB が要求を次のアップストリーム・サーバーに渡そうとする最大試行回数に置き換えます。 この回数には元の要求が含まれます。 この制限を無効にするには、<code>0</code> を使用します。値を指定しなかった場合は、デフォルト値 <code>0</code> が使用されます。
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td><code>&lt;<em>time</em>&gt;</code> を、ALB が要求を次のアップストリーム・サーバーに渡そうとする最大試行時間 (秒) に置き換えます。 例えば、時間を 30 秒に設定するには、<code>30s</code> と入力します。 この制限を無効にするには、<code>0</code> を使用します。値を指定しなかった場合は、デフォルト値 <code>0</code> が使用されます。
</td>
</tr>
<tr>
<td><code>error</code></td>
<td><code>true</code> に設定すると、最初のアップストリーム・サーバーに接続を確立するとき、要求を渡すとき、応答ヘッダーを読み取るときにエラーが発生した場合に、ALB は次のアップストリーム・サーバーに要求を渡します。
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td><code>true</code> に設定すると、最初のアップストリーム・サーバーが空の応答または無効な応答を返したときに、ALB は次のアップストリーム・サーバーに要求を渡します。
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td><code>true</code> に設定すると、最初のアップストリーム・サーバーがコード 502 の応答を返したときに、ALB は次のアップストリーム・サーバーに要求を渡します。 指定できる HTTP 応答コードは、<code>500</code>、<code>502</code>、<code>503</code>、<code>504</code>、<code>403</code>、<code>404</code>、<code>429</code> です。
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td><code>true</code> に設定すると、ALB は非べき等型メソッドを含む要求を次のアップストリーム・サーバーに渡すことができます。 デフォルトでは、ALB はこれらの要求を次のアップストリーム・サーバーに渡しません。
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>ALB が要求を次のアップストリーム・サーバーに渡さないようにするには、<code>true</code> に設定します。
</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### Cookie によるセッション・アフィニティー (sticky-cookie-services)
{: #sticky-cookie-services}

スティッキー Cookie のアノテーションを使用すると、セッション・アフィニティーを ALB に追加し、常に着信ネットワーク・トラフィックを同じアップストリーム・サーバーにルーティングできます。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>高可用性にするためには、アプリのセットアップによっては、着信クライアント要求を処理する複数のアップストリーム・サーバーをデプロイする必要があります。 クライアントがバックエンド・アプリに接続したら、セッション期間中またはタスクが完了するまでの間、1 つのクライアントに同じアップストリーム・サーバーがサービスを提供するように、セッション・アフィニティーを使用することができます。 着信ネットワーク・トラフィックを常に同じアップストリーム・サーバーにルーティングしてセッション・アフィニティーを保つように、ALB を構成することができます。

</br></br>
バックエンド・アプリに接続した各クライアントは、ALB によって、使用可能なアップストリーム・サーバーのいずれかに割り当てられます。 ALB は、クライアントのアプリに保管されるセッション Cookie を作成します。そのセッション Cookie は、ALB とクライアントの間でやり取りされるすべての要求のヘッダー情報に含められます。 この Cookie の情報により、同一セッションのすべての要求を同じアップストリーム・サーバーで処理することができます。

</br></br>
複数のサービスを含める場合は、セミコロン (;) を使用して区切ってください。</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/sticky-cookie-services: "serviceName=&lt;myservice1&gt; name=&lt;cookie_name1&gt; expires=&lt;expiration_time1&gt; path=&lt;cookie_path1&gt; hash=&lt;hash_algorithm1&gt;;serviceName=&lt;myservice2&gt; name=&lt;cookie_name2&gt; expires=&lt;expiration_time2&gt; path=&lt;cookie_path2&gt; hash=&lt;hash_algorithm2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: &lt;myservice1&gt;
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: &lt;myservice2&gt;
          servicePort: 80</code></pre>

  <table>
  <caption>アノテーションの構成要素について</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>serviceName</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
  </tr>
  <tr>
  <td><code>name</code></td>
  <td><code>&lt;<em>cookie_name</em>&gt;</code> を、セッション中に作成されたスティッキー Cookie の名前に置き換えます。</td>
  </tr>
  <tr>
  <td><code>expires</code></td>
  <td><code>&lt;<em>expiration_time</em>&gt;</code> を、スティッキー Cookie が期限切れになるまでの時間 (単位は秒 (s)、分 (m)、または時間 (h)) に置き換えます。 この時間は、ユーザー・アクティビティーとは無関係です。 期限が切れた Cookie は、クライアント Web ブラウザーによって削除され、ALB に送信されなくなります。 例えば、有効期間を 1 秒、1 分、または 1 時間に設定するには、<code>1s</code>、<code>1m</code>、または <code>1h</code> と入力します。</td>
  </tr>
  <tr>
  <td><code>path</code></td>
  <td><code>&lt;<em>cookie_path</em>&gt;</code> を、Ingress サブドメインに付加されるパスに置き換えます。このパスは、Cookie を ALB に送信する対象となるドメインとサブドメインを示すものです。 例えば、Ingress ドメインが <code>www.myingress.com</code> である場合に、すべてのクライアント要求で Cookie を送信するには、<code>path=/</code> を設定する必要があります。 <code>www.myingress.com/myapp</code> とそのすべてのサブドメインについてにのみ Cookie を送信するには、<code>path=/myapp</code> を設定する必要があります。</td>
  </tr>
  <tr>
  <td><code>hash</code></td>
  <td><code>&lt;<em>hash_algorithm</em>&gt;</code> を、Cookie の情報を保護するハッシュ・アルゴリズムに置き換えます。 <code>sha1</code> のみサポートされます。 SHA1 は、Cookie の情報に基づいてハッシュ合計を生成し、そのハッシュ合計を Cookie に付加します。 サーバーは Cookie の情報を暗号化解除し、データ保全性を検証します。</td>
  </tr>
  </tbody></table>

 </dd></dl>

<br />


### アップストリーム失敗タイムアウト (upstream-fail-timeout)
{: #upstream-fail-timeout}

ALB がサーバーへの接続を試行する時間の長さを設定します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
サーバーを使用不可と見なす前に、ALB がサーバーへの接続を試行する時間の長さを設定できます。 設定した時間内に、<a href="#upstream-max-fails"><code>upstream-max-fails</code> アノテーション</a>で設定した接続試行の最大失敗回数に ALB が達すると、サーバーは使用不可と見なされます。 また、この時間の長さによって、サーバーが使用不可と見なされる時間の長さも決まります。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-fail-timeout: "serviceName=&lt;myservice&gt; fail-timeout=&lt;fail_timeout&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>serviceName(オプション)</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
</tr>
<tr>
<td><code>fail-timeout</code></td>
<td><code>&lt;<em>fail_timeout</em>&gt;</code> を、サーバーを使用不可と見なす前に ALB がサーバーへの接続を試行する時間の長さに置き換えます。 デフォルトは <code>10s</code> です。 時間は秒単位にする必要があります。</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### アップストリーム・キープアライブ (upstream-keepalive)
{: #upstream-keepalive}

アップストリーム・サーバーのアイドル・キープアライブ接続の最大数を設定します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
所定のサービスのアップストリーム・サーバーへのアイドル・キープアライブ接続の最大数を設定します。 デフォルトでは、64 個のアイドル・キープアライブ接続がアップストリーム・サーバーに設定されています。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-keepalive: "serviceName=&lt;myservice&gt; keepalive=&lt;max_connections&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
</tr>
<tr>
<td><code>keepalive</code></td>
<td><code>&lt;<em>max_connections</em>&gt;</code> を、アップストリーム・サーバーへのアイドル・キープアライブ接続の最大数に置き換えます。 デフォルトは <code>64</code> です。 値を <code>0</code> に設定すると、所定のサービスのアップストリーム・キープアライブ接続が無効になります。</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


### アップストリーム失敗最大回数 (upstream-max-fails)
{: #upstream-max-fails}

サーバーとの通信の試行に失敗できる最大回数を設定します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
サーバーを使用不可と見なす前に、ALB がサーバーへの接続に失敗できる最大回数を設定します。 <a href="#upstream-fail-timeout"><code>upstream-fail-timeout</code> アノテーション</a>で設定した時間内に、ALB が最大回数に達すると、サーバーは使用不可と見なされます。 サーバーが使用不可と見なされる時間の長さも、<code>upstream-fail-timeout</code> アノテーションによって設定されます。</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-max-fails: "serviceName=&lt;myservice&gt; max-fails=&lt;max_fails&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>serviceName(オプション)</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
</tr>
<tr>
<td><code>max-fails</code></td>
<td><code>&lt;<em>max_fails</em>&gt;</code> を、ALB がサーバーとの通信の試行に失敗できる最大回数に置き換えます。 デフォルトは <code>1</code> です。値を <code>0</code> にするとアノテーションが無効になります。</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />


## HTTPS および TLS/SSL 認証アノテーション
{: #https-auth}

### {{site.data.keyword.appid_short_notm}} 認証 (appid-auth)
{: #appid-auth}

{{site.data.keyword.appid_full_notm}} を使用して、アプリケーションの認証を行います。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
{{site.data.keyword.appid_short_notm}} を使用して、Web または API の HTTP /HTTPS 要求を認証します。

<p>要求タイプを <code>web</code> に設定すると、{{site.data.keyword.appid_short_notm}} アクセス・トークンを含む Web 要求が検証されます。 トークンの検証が失敗すると、Web 要求は拒否されます。 要求にアクセス・トークンが含まれていない場合、要求は {{site.data.keyword.appid_short_notm}} ログイン・ページにリダイレクトされます。 <strong>注</strong>: {{site.data.keyword.appid_short_notm}} Web 認証が機能するためには、ユーザーのブラウザーで Cookie を有効にする必要があります。</p>

<p>要求タイプを <code>api</code> に設定すると、{{site.data.keyword.appid_short_notm}} アクセス・トークンを含む API 要求が検証されます。 要求にアクセス・トークンが含まれていない場合、「<code>401 : Unauthorized</code>」というエラー・メッセージがユーザーに返されます。</p>

<p>**注**: セキュリティー上の理由から、{{site.data.keyword.appid_short_notm}} 認証は、TLS/SSL が有効化されているバックエンドのみをサポートします。</p>
</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/appid-auth: "bindSecret=&lt;bind_secret&gt; namespace=&lt;namespace&gt; requestType=&lt;request_type&gt; serviceName=&lt;myservice&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>bindSecret</code></td>
<td><em><code>&lt;bind_secret&gt;</code></em> を、{{site.data.keyword.appid_short_notm}} サービス・インスタンス用のバインド・シークレットが保管されている Kubernetes シークレットに置き換えてください。</td>
</tr>
<tr>
<td><code>namespace</code></td>
<td><em><code>&lt;namespace&gt;</code></em> をバインド・シークレットの名前空間に置き換えてください。 このフィールドのデフォルトは、`default` 名前空間です。</td>
</tr>
<tr>
<td><code>requestType</code></td>
<td><code><em>&lt;request_type&gt;</em></code> を、{{site.data.keyword.appid_short_notm}} に送信する要求のタイプに置き換えてください。 受け入れられる値は `web` または `api` です。 デフォルトは `api` です。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td><code><em>&lt;myservice&gt;</em></code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。 このフィールドは必須です。 サービス名を指定しない場合、アノテーションはすべてのサービスに対して有効になります。  サービス名を指定した場合、アノテーションはそのサービスに対してのみ有効になります。 複数のサービスは、コンマ (,) で区切ります。</td>
</tr>
</tbody></table>
</dd>
<dt>使用法</dt></dl>

アプリケーションは認証に {{site.data.keyword.appid_short_notm}} を使用するため、{{site.data.keyword.appid_short_notm}} インスタンスをプロビジョンし、有効なリダイレクト URI をインスタンスに構成し、インスタンスをクラスターにバインドしてバインド・シークレットを生成する必要があります。

1. 既存の {{site.data.keyword.appid_short_notm}} インスタンスを選択するか、新しいインスタンスを作成します。
    * 既存のインスタンスを使用する場合、サービス・インスタンス名にスペースが含まれていないことを確認します。 スペースを削除する場合、サービス・インスタンス名の隣にある「オプション (詳細)」メニューを選択し、「**サービスの名前変更**」を選びます。
    * [新しい {{site.data.keyword.appid_short_notm}} インスタンスをプロビジョンするには、以下のようにします。](https://console.bluemix.net/catalog/services/app-id)
        1. 自動入力による**サービス名**を、サービス・インスタンスを表す固有の名前に置き換えます。
            **重要**: サービス・インスタンス名にスペースを含めることはできません。
        2. クラスターのデプロイ先と同じ地域を選択します。
        3. **「作成」**をクリックします。
2. アプリのリダイレクト URL を追加します。 リダイレクト URL は、アプリのコールバック・エンドポイントです。 フィッシング攻撃を防止するため、アプリ ID では、リダイレクト URL のホワイトリストを使用して要求 URL が検証されます。
    1. {{site.data.keyword.appid_short_notm}} 管理コンソールで、**「ID プロバイダー」 > 「管理」**とナビゲートします。
    2. ID プロバイダーが選択されていることを確認します。 ID プロバイダーが選択されていない場合、ユーザーは認証されませんが、アプリへの匿名アクセス用のアクセス・トークンが発行されます。
    3. 「**Web リダイレクト URL の追加**」フィールドで、`http://<hostname>/<app_path>/appid_callback` または `https://<hostname>/<app_path>/appid_callback` という形式でアプリのリダイレクト URL を追加します。
        * 例えば、IBM Ingress サブドメインに登録されているアプリの場合は、`https://mycluster.us-south.containers.appdomain.cloud/myapp1path/appid_callback` などになります。
        * カスタム・ドメインに登録されているアプリの場合は、`http://mydomain.net/myapp2path/appid_callback` などです。

3. {{site.data.keyword.appid_short_notm}} サービス・インスタンスをクラスターにバインドします。
    ```
    ibmcloud ks cluster-service-bind <cluster_name_or_ID> <namespace> <service_instance_name>
    ```
    {: pre}
    サービスがクラスターに正常に追加されると、サービス・インスタンスの資格情報を保持するクラスター・シークレットが作成されます。 CLI 出力例:
    ```
    ibmcloud ks cluster-service-bind mycluster mynamespace appid1
    Binding service instance to namespace...
    OK
    Namespace:    mynamespace
    Secret name:  binding-<service_instance_name>
    ```
    {: screen}

4. クラスター名前空間に作成されたシークレットを取得します。
    ```
    kubectl get secrets --namespace=<namespace>
    ```
    {: pre}

5. バインド・シークレットとクラスター名前空間を使用して、`appid-auth` アノテーションを Ingress リソースに追加します。

<br />



### カスタムの HTTP ポートおよび HTTPS ポート (custom-port)
{: #custom-port}

HTTP (ポート 80) および HTTPS (ポート 443) ネットワーク・トラフィックのデフォルト・ポートを変更します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>デフォルトで、Ingress ALB は、ポート 80 上の着信 HTTP ネットワーク・トラフィックとポート 443 上の着信 HTTPS ネットワーク・トラフィックを listen するように構成されています。 ALB ドメインのセキュリティーを強化するため、または HTTPS ポートだけを有効にするために、デフォルト・ポートを変更できます。<p><strong>注</strong>: ポートでの相互認証を有効にするには、[有効なポートを開くように ALB を構成し](cs_ingress.html#opening_ingress_ports)、そのポートを [`mutual-auth` アノテーション](#mutual-auth)で指定します。 相互認証のポートを指定するために `custom-port` アノテーションを使用しないでください。</p></dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/custom-port: "protocol=&lt;protocol1&gt; port=&lt;port1&gt;;protocol=&lt;protocol2&gt; port=&lt;port2&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>着信 HTTP または HTTPS ネットワーク・トラフィックのデフォルト・ポートを変更するには、<code>http</code> または <code>https</code> を入力します。</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>着信 HTTP または HTTPS ネットワーク・トラフィックに使用するポート番号を入力します。  <p><strong>注:</strong> HTTP または HTTPS 用にカスタム・ポートを指定した場合、デフォルト・ポートは、HTTP と HTTPS のどちらに対しても有効ではなくなります。 例えば、HTTPS のデフォルト・ポートを 8443 に変更し、HTTP ではデフォルト・ポートを使用する場合、それらの両方に対して次のようにカスタム・ポートを設定する必要があります。<code>custom-port: "protocol=http port=80; protocol=https port=8443"</code></p></td>
 </tr>
 </tbody></table>

 </dd>
 <dt>使用法</dt>
 <dd><ol><li>ALB 用の開かれたポートを確認します。
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 出力は、以下のようになります。
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d</code></pre></li>
<li>ALB の構成マップを開きます。
<pre class="pre">
<code>kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system</code></pre></li>
<li>非デフォルトの HTTP および HTTPS ポートを構成マップに追加します。 &lt;port&gt; を、開く HTTP または HTTPS のポートに置き換えます。 <b>注</b>: デフォルトでは、ポート 80 とポート 443 が開いています。 80 と 443 を開いたままにしておく場合は、指定する他の TCP ポートに加えて、それらのポートも `public-ports` フィールドに含める必要があります。 プライベート ALB を有効にした場合は、開いたままにしておくポートも `private-ports` フィールドで指定する必要があります。 詳しくは、<a href="cs_ingress.html#opening_ingress_ports">Ingress ALB でポートを開く</a>を参照してください。
<pre class="codeblock">
<code>apiVersion: v1
kind: ConfigMap
data:
 public-ports: &lt;port1&gt;;&lt;port2&gt;
metadata:
 creationTimestamp: 2017-08-22T19:06:51Z
 name: ibm-cloud-provider-ingress-cm
 namespace: kube-system
 resourceVersion: "1320"
 selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
 uid: &lt;uid&gt;</code></pre></li>
 <li>ALB が非デフォルトのポートによって再構成されたことを確認します。
<pre class="pre">
<code>kubectl get service -n kube-system</code></pre>
CLI 出力は、以下のようになります。
<pre class="screen">
<code>NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                      AGE
public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx  169.xx.xxx.xxx &lt;port1&gt;:30776/TCP,&lt;port2&gt;:30412/TCP   109d</code></pre></li>
<li>着信ネットワーク・トラフィックをサービスにルーティングする際に、非デフォルトのポートを使用するように Ingress を構成します。 この解説ではサンプル YAML ファイルを使用します。 </li>
<li>ALB 構成を更新します。
<pre class="pre">
<code>kubectl apply -f myingress.yaml</code></pre>
</li>
<li>任意の Web ブラウザーを開いてアプリにアクセスします。 例: <code>https://&lt;ibmdomain&gt;:&lt;port&gt;/&lt;service_path&gt;/</code></li></ol></dd></dl>

<br />



### HTTPS への HTTP リダイレクト (redirect-to-https)
{: #redirect-to-https}

非セキュア HTTP クライアント要求を HTTPS に変換します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>Ingress ALB は、IBM 提供の TLS 証明書またはカスタム TLS 証明書を使用してドメインを保護するようにセットアップされます。 一部のユーザーが、ALB ドメインへの非セキュアな <code>HTTP</code> 要求、例えば、<code>https</code> ではなく <code>http://www.myingress.com</code> を使用してアプリにアクセスしようとする可能性があります。 リダイレクト・アノテーションを使用すると、非セキュアな HTTP 要求を常に HTTPS に変換できます。 このアノテーションを使用しない場合、デフォルトでは非セキュアな HTTP 要求は HTTPS 要求に変換されないので、暗号化されていない機密情報が公開されてしまうおそれがあります。

</br></br>
HTTP 要求の HTTPS へのリダイレクトはデフォルトで無効です。</dd>

<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/redirect-to-https: "True"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

</dd>

</dl>

<br />


### HTTP Strict Transport Security (hsts)
{: #hsts}

<dl>
<dt>説明</dt>
<dd>
HSTS は、ドメインへのアクセスに HTTPS のみ使用するようブラウザーに指示します。 ユーザーがプレーンな HTTP リンクを入力したりアクセスしたりしても、ブラウザーが接続を HTTPS に強制的にアップグレードします。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/hsts: enabled=true maxAge=&lt;31536000&gt; includeSubdomains=true
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          </code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>enabled</code></td>
  <td>HSTS を有効にするには、<code>true</code> を使用します。</td>
  </tr>
    <tr>
  <td><code>maxAge</code></td>
  <td><code>&lt;<em>31536000</em>&gt;</code> を、HTTPS に直接送信される要求をブラウザーがキャッシュに入れる秒数を表す整数に置き換えてください。 デフォルトは <code>31536000</code>で、これは 1 年に相当します。</td>
  </tr>
  <tr>
  <td><code>includeSubdomains</code></td>
  <td>HSTS ポリシーが現行ドメインのすべてのサブドメインにも適用されることをブラウザーに通知するには、<code>true</code> を使用します。 デフォルトは <code>true</code> です。 </td>
  </tr>
  </tbody></table>

  </dd>
  </dl>


<br />


### 相互認証 (mutual-auth)
{: #mutual-auth}

ALB の相互認証を構成します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
Ingress ALB のダウンストリーム・トラフィックの相互認証を構成します。 外部クライアントはサーバーを認証し、サーバーもクライアントを認証します。どちらも証明書が使用されます。 相互認証は、証明書ベース認証とも双方向認証とも呼ばれます。</br></br>
クライアントと Ingress ALB との間の SSL 終端には、`mutual-auth` アノテーションを使用します。Ingress ALB とバックエンド・アプリケーションとの間の SSL 終端には、[`ssl-services` アノテーション](#ssl-services)を使用します。
</dd>

<dt>前提条件</dt>
<dd>
<ul>
<li>必要な <code>ca.crt</code> が含まれた有効な相互認証シークレットが必要です。 相互認証シークレットを作成するには、このセクションの終わりに記載されているステップを参照してください。</li>
<li>443 以外のポートでの相互認証を有効にするには、[有効なポートを開くように ALB を構成し](cs_ingress.html#opening_ingress_ports)、そのポートをこのアノテーションで指定します。 相互認証のポートを指定するために `custom-port` アノテーションを使用しないでください。</li>
</ul>
</dd>

<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
name: myingress
annotations:
  ingress.bluemix.net/mutual-auth: "secretName=&lt;mysecret&gt; port=&lt;port&gt; serviceName=&lt;servicename1&gt;,&lt;servicename2&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td><code>&lt;<em>mysecret</em>&gt;</code> をシークレット・リソースの名前に置き換えます。</td>
</tr>
<tr>
<td><code>port</code></td>
<td><code>&lt;<em>port</em>&gt;</code> を ALB ポート番号に置き換えます。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>servicename</em>&gt;</code> を 1 つ以上の Ingress リソースの名前に置き換えます。 このパラメーターはオプションです。</td>
</tr>
</tbody></table>

</dd>
</dl>

**相互認証シークレットを作成するには、次のようにします。**

1. 次のいずれかの方法で鍵と証明書を生成します。
    * 証明書プロバイダーから認証局 (CA) の証明書と鍵を生成します。 独自のドメインがある場合は、ご使用のドメインの正式な TLS 証明書を購入してください。
      **重要**: 証明書ごとに異なる [CN ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://support.dnsimple.com/articles/what-is-common-name/) を使用してください。
    * テストのために、OpenSSL を使用して自己署名証明書を作成することができます。 詳しくは、この[自己署名 SSL 証明書 チュートリアル ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.akadia.com/services/ssh_test_certificate.html) を参照してください。
        1. `ca.key` を作成します。
            ```
            openssl genrsa -out ca.key 1024
            ```
            {: pre}
        2. 鍵を使用して、`ca.crt` を作成します。
            ```
            openssl req -new -x509 -key ca.key -out ca.crt
            ```
            {: pre}
        3. `ca.crt` を使用して、自己署名証明書を作成します。
            ```
            openssl x509 -req -in example.org.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out example.org.crt
            ```
            {: pre}
2. [証明書を base-64 に変換します ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.base64encode.org/)。
3. 証明書を使用して、シークレット YAML ファイルを作成します。
     ```
     apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       ca.crt: <ca_certificate>
     ```
     {: codeblock}
4. 証明書を Kubernetes シークレットとして作成します。
     ```
     kubectl create -f ssl-my-test
     ```
     {: pre}

<br />



### SSL サービス・サポート (ssl-services)
{: #ssl-services}

HTTPS 要求を許可し、アップストリーム・アプリへのトラフィックを暗号化します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
Ingress リソース構成に TLS セクションが含まれている場合、Ingress ALB はアプリに対する HTTPS で保護された URL 要求を処理できます。 ただし、ALB は、TLS 終端を処理し、アプリにトラフィックを転送する前に要求を復号します。 HTTPS プロトコルを必要とするアプリがあり、トラフィックの暗号化を維持する必要がある場合は、`ssl-services` アノテーションを使用して、ALB のデフォルトの TLS 終端を無効にします。 ALB は、TLS 接続を終了し、バックエンド・アプリにトラフィックを送信する前に SSL を再暗号化します。</br></br>
また、バックエンド・アプリで TLS を処理することができ、さらにセキュリティーを追加する場合は、シークレットに含まれる証明書を提供することによって片方向認証または相互認証を追加できます。</br></br>
Ingress ALB とバックエンド・アプリケーションとの間の SSL 終端には、`ssl-services` アノテーションを使用します。クライアントと Ingress ALB との間の SSL 終端には、[`mutual-auth` アノテーション](#mutual-auth)を使用します。</dd>

<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: &lt;myingressname&gt;
  annotations:
    ingress.bluemix.net/ssl-services: |
      ssl-service=&lt;myservice1&gt; ssl-secret=&lt;service1-ssl-secret&gt;;
      ssl-service=&lt;myservice2&gt; ssl-secret=&lt;service2-ssl-secret&gt;
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          </code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>ssl-service</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code> を、HTTPS を必要とするサービスの名前に置き換えます。 トラフィックは暗号化されて ALB からこのアプリのサービスに送信されます。</td>
  </tr>
  <tr>
  <td><code>ssl-secret</code></td>
  <td>バックエンド・アプリで TLS を処理することができ、さらにセキュリティーを追加する場合は、<code>&lt;<em>service-ssl-secret</em>&gt;</code> をサービスの片方向認証または相互認証のシークレットに置き換えます。<ul><li>片方向認証シークレットを提供する場合、値にはアップストリーム・サーバーの <code>trusted.crt</code> が含まれている必要があります。 片方向シークレットを作成するには、このセクションの終わりに記載されているステップを参照してください。</li><li>相互認証シークレットを提供する場合、値にはアプリが必要とするクライアントの必須 <code>ca.crt</code> および <code>ca.key</code> が含まれている必要があります。 相互認証シークレットを作成するには、このセクションの終わりに記載されているステップを参照してください。</li></ul><strong>警告</strong>: シークレットを提供しない場合、非セキュアな接続が許可されます。 接続のテスト中で、証明書の準備ができていない場合や、証明書が期限切れになっていて、非セキュアな接続を許可する場合には、シークレットの省略を選択する可能性があります。</td>
  </tr>
  </tbody></table>

  </dd>
  </dl>

**片方向認証シークレットを作成するには、以下のようにします。**

1. 認証局 (CA) 鍵と証明書をアップストリーム・サーバーから入手します。
2. [証明書を base-64 に変換します ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.base64encode.org/)。
3. 証明書を使用して、シークレット YAML ファイルを作成します。
     ```
     apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       trusted.crt: <ca_certificate>
     ```
     {: codeblock}
     **注**: アップストリーム・トラフィックの相互認証も実施する場合は、データ・セクションに `trusted.crt` に加えて `client.crt` と `client.key` を指定できます。
4. 証明書を Kubernetes シークレットとして作成します。
     ```
     kubectl create -f ssl-my-test
     ```
     {: pre}

</br>
**相互認証シークレットを作成するには、次のようにします。**

1. 次のいずれかの方法で鍵と証明書を生成します。
    * 証明書プロバイダーから認証局 (CA) の証明書と鍵を生成します。 独自のドメインがある場合は、ご使用のドメインの正式な TLS 証明書を購入してください。
      **重要**: 証明書ごとに異なる [CN ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://support.dnsimple.com/articles/what-is-common-name/) を使用してください。
    * テストのために、OpenSSL を使用して自己署名証明書を作成することができます。 詳しくは、この[自己署名 SSL 証明書 チュートリアル ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.akadia.com/services/ssh_test_certificate.html) を参照してください。
        1. `ca.key` を作成します。
            ```
            openssl genrsa -out ca.key 1024
            ```
            {: pre}
        2. 鍵を使用して、`ca.crt` を作成します。
            ```
            openssl req -new -x509 -key ca.key -out ca.crt
            ```
            {: pre}
        3. `ca.crt` を使用して、自己署名証明書を作成します。
            ```
            openssl x509 -req -in example.org.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out example.org.crt
            ```
            {: pre}
2. [証明書を base-64 に変換します ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.base64encode.org/)。
3. 証明書を使用して、シークレット YAML ファイルを作成します。
     ```
     apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       ca.crt: <ca_certificate>
     ```
     {: codeblock}
4. 証明書を Kubernetes シークレットとして作成します。
     ```
     kubectl create -f ssl-my-test
     ```
     {: pre}

<br />


## Istio アノテーション
{: #istio-annotations}

### Istio サービス (istio-services)
{: #istio-services}

Istio 管理対象サービスにトラフィックを転送します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
<strong>注</strong>: このアノテーションが機能するのは、Istio 0.7 以前の場合のみです。
<br>Istio 管理対象サービスがある場合は、クラスター ALB を使用して HTTP/HTTPS 要求を Istio Ingress コントローラーに転送できます。 Istio Ingress コントローラーは、要求をアプリ・サービスに転送します。 トラフィックを転送するには、クラスター ALB と Istio Ingress コントローラーの両方の Ingress リソースを変更する必要があります。
<br><br>クラスター ALB の Ingress リソースで、以下を行う必要があります。
  <ul>
    <li>`istio-services` アノテーションを指定する</li>
    <li>サービス・パスを、アプリが listen する実際のパスとして定義する</li>
    <li>サービス・ポートを Istio Ingress コントローラーのポートとして定義する</li>
  </ul>
<br>Istio Ingress コントローラーの Ingress リソースで、以下を行う必要があります。
  <ul>
    <li>サービス・パスを、アプリが listen する実際のパスとして定義する</li>
    <li>サービス・ポートを、Istio Ingress コントローラーで公開するアプリ・サービスの HTTP/HTTPS ポートとして定義する</li>
</ul>
</dd>

<dt>クラスター ALB のサンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/istio-services: "enable=true serviceName=&lt;myservice1&gt; istioServiceNamespace=&lt;istio-namespace&gt; istioServiceName=&lt;istio-ingress-service&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: &lt;/myapp1&gt;
          backend:
            serviceName: &lt;myservice1&gt;
            servicePort: &lt;istio_ingress_port&gt;
      - path: &lt;/myapp2&gt;
          backend:
            serviceName: &lt;myservice2&gt;
            servicePort: &lt;istio_ingress_port&gt;</code></pre>

<table>
<caption>YAML ファイルの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/> YAML ファイルの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>enable</code></td>
  <td>Istio 管理サービスへのトラフィック転送を有効にするには、<code>True</code> に設定します。</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td><code><em>&lt;myservice1&gt;</em></code> を、Istio 管理対象アプリ用に作成した Kubernetes サービスの名前に置き換えます。 複数のサービスは、セミコロン (,) で区切ります。 このフィールドはオプションです。 サービス名を指定しなかった場合は、すべての Istio 管理対象サービスのトラフィック転送が有効化されます。</td>
</tr>
<tr>
<td><code>istioServiceNamespace</code></td>
<td><code><em>&lt;istio-namespace&gt;</em></code> を、Istio がインストールされている Kubernetes 名前空間に置き換えます。 このフィールドはオプションです。 名前空間を指定しなかった場合は、<code>istio-system</code> 名前空間が使用されます。</td>
</tr>
<tr>
<td><code>istioServiceName</code></td>
<td><code><em>&lt;istio-ingress-service&gt;</em></code> を Istio Ingress サービスの名前に置き換えます。 このフィールドはオプションです。 Istio Ingress サービス名を指定しなかった場合は、サービス名 <code>istio-ingress</code> が使用されます。</td>
</tr>
<tr>
<td><code>path</code></td>
  <td>トラフィックを転送する Istio 管理対象サービスごとに、<code><em>&lt;/myapp1&gt;</em></code> を、Istio 管理対象サービスが listen するバックエンド・パスに置き換えます。 このパスは、Istio Ingress リソースで定義したパスに対応している必要があります。</td>
</tr>
<tr>
<td><code>servicePort</code></td>
<td>トラフィックを転送する Istio 管理対象サービスごとに、<code><em>&lt;istio_ingress_port&gt;</em></code> を、Istio Ingress コントローラーのポートに置き換えます。</td>
</tr>
</tbody></table>
</dd>

<dt>使用法</dt></dl>

1. アプリをデプロイします。 以下の手順に示しているサンプル・リソースでは、[BookInfo ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://archive.istio.io/v0.7/docs/guides/bookinfo.html) というサンプル・アプリを使用しています。このサンプル・アプリは、`istio-0.7.1/samples/bookinfo/kube` リポジトリーにあります。
   ```
   kubectl apply -f bookinfo.yaml -n istio-system
   ```
   {: pre}

2. アプリの Istio ルーティング・ルールをセットアップします。 例えば、BookInfo という Istio サンプル・アプリでは、[各マイクロサービスのルーティング・ルール ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://archive.istio.io/v0.7/docs/tasks/traffic-management/request-routing.html)が、`route-rule-all-v1.yaml` ファイルに定義されています。

3. Istio Ingress リソースを作成して、Istio Ingress コントローラーにアプリを公開します。 このリソースによって、モニタリングやルーティング・ルールなどの Istio 機能を、クラスターに着信するトラフィックに適用できます。
    例えば、BookInfo アプリの以下のリソースが `bookinfo.yaml` ファイルに事前定義されています。
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: istio-ingress-resource
      annotations:
        kubernetes.io/ingress.class: "istio"
    spec:
      rules:
      - http:
          paths:
          - path: /productpage
            backend:
              serviceName: productpage
              servicePort: 9080
          - path: /login
            backend:
              serviceName: productpage
              servicePort: 9080
          - path: /logout
            backend:
              serviceName: productpage
              servicePort: 9080
          - path: /api/v1/products.*
            backend:
              serviceName: productpage
              servicePort: 9080
    ```
    {: codeblock}

4. Istio Ingress リソースを作成します。
    ```
    kubectl create -f istio-ingress-resource.yaml -n istio-system
    ```
    {: pre}
    アプリが Istio Ingress コントローラーに接続されます。

5. クラスターの IBM **Ingress サブドメイン**と **Ingress シークレット**を取得します。 サブドメインとシークレットは、クラスター用に事前登録され、アプリの固有のパブリック URL として使用されます。
    ```
    ibmcloud ks cluster-get <cluster_name_or_ID>
    ```
    {: pre}

6. IBM Ingress リソースを作成し、クラスターの IBM Ingress ALB に Istio Ingress コントローラーを接続します。
    BookInfo アプリの例を以下に示します。
    ```
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: ibm-ingress-resource
      annotations:
        ingress.bluemix.net/istio-services: "enabled=true serviceName=productpage istioServiceName=istio-ingress-resource"
    spec:
      tls:
      - hosts:
        - mycluster-459249.us-south.containers.mybluemix.net
        secretName: mycluster-459249
      rules:
      - host: mycluster-459249.us-south.containers.mybluemix.net
        http:
          paths:
          - path: /productpage
            backend:
              serviceName: productpage
              servicePort: 9080
          - path: /login
            backend:
              serviceName: productpage
              servicePort: 9080
          - path: /logout
            backend:
              serviceName: productpage
              servicePort: 9080
          - path: /api/v1/products.*
            backend:
              serviceName: productpage
              servicePort: 9080
    ```
    {: codeblock}

7. IBM ALB Ingress リソースを作成します。
    ```
    kubectl apply -f ibm-ingress-resource.yaml -n istio-system
    ```
    {: pre}

8. ブラウザーで、`https://<hostname>/frontend` に移動し、アプリの Web ページを表示します。

<br />


## プロキシー・バッファー・アノテーション
{: #proxy-buffer}


### クライアント応答データのバッファリング (proxy-buffering)
{: #proxy-buffering}

バッファー・アノテーションを使用すると、クライアントにデータを送信する間、ALB での応答データの保存を無効にできます。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>Ingress ALB は、バックエンド・アプリとクライアント Web ブラウザーの間のプロキシーとして機能します。 応答がバックエンド・アプリからクライアントに送信されると、デフォルトでは、応答データが ALB のバッファーに入れられます。 ALB は、クライアント応答をプロキシー処理し、クライアントのペースでクライアントに応答を送信し始めます。 バックエンド・アプリからのすべてのデータを ALB が受信すると、バックエンド・アプリへの接続は閉じられます。 ALB からクライアントへの接続は、クライアントがすべてのデータを受信するまで開いたままです。

</br></br>
ALB での応答データのバッファリングを無効にすると、データは即時に ALB からクライアントに送信されます。 クライアントが、ALB のペースで着信データを処理できなければなりません。 クライアントの処理速度が遅すぎる場合は、データが失われる可能性があります。
</br></br>
ALB での応答データのバッファリングは、デフォルトでは有効です。</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-buffering: "enabled=&lt;false&gt; serviceName=&lt;myservice1&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>enabled</code></td>
   <td>ALB での応答データ・バッファリングを無効にするには、<code>false</code> に設定します。</td>
 </tr>
 <tr>
 <td><code>serviceName</code></td>
 <td><code><em>&lt;myservice1&gt;</em></code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。 複数のサービスは、セミコロン (,) で区切ります。 このフィールドはオプションです。 サービス名を指定しない場合は、すべてのサービスがこのアノテーションを使用します。</td>
 </tr>
 </tbody></table>
 </dd>
 </dl>

<br />



### プロキシー・バッファー (proxy-buffers)
{: #proxy-buffers}

ALB 用のプロキシー・バッファーの数とサイズを構成します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
プロキシー・サーバーからの単一の接続に対して、応答を読み取るバッファーの数とサイズを設定します。 特定のサービスが指定されていなければ、Ingress ホスト内のすべてのサービスにこの構成が適用されます。 例えば、<code>serviceName=SERVICE number=2 size=1k</code> という構成を指定した場合は、サービスに 1k が適用されます。 <code>number=2 size=1k</code> という構成を指定した場合は、Ingress ホスト内のすべてのサービスに 1k が適用されます。
</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffers: "serviceName=&lt;myservice&gt; number=&lt;number_of_buffers&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td><code>&lt;<em>myservice</em>&gt;</code> を、プロキシー・バッファーを適用するサービスの名前に置き換えます。</td>
 </tr>
 <tr>
 <td><code>number</code></td>
 <td><code>&lt;<em>number_of_buffers</em>&gt;</code> を、<em>2</em> などの数値に置き換えます。</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td><code>&lt;<em>size</em>&gt;</code> を、各バッファーのサイズをキロバイト (k または K) 単位に置き換えます (例えば <em>1K</em>)。</td>
 </tr>
 </tbody>
 </table>
 </dd></dl>

<br />


### プロキシー・バッファー・サイズ (proxy-buffer-size)
{: #proxy-buffer-size}

応答の最初の部分を読み取るプロキシー・バッファーのサイズを構成します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
プロキシー・サーバーから受け取る応答の、最初の部分を読み取るバッファーのサイズを設定します。 応答のこの部分には通常、短い応答ヘッダーが含まれています。 特定のサービスが指定されていなければ、Ingress ホスト内のすべてのサービスにこの構成が適用されます。 例えば、<code>serviceName=SERVICE size=1k</code> という構成を指定した場合は、サービスに 1k が適用されます。 <code>size=1k</code> という構成を指定した場合は、Ingress ホスト内のすべてのサービスに 1k が適用されます。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffer-size: "serviceName=&lt;myservice&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 80
</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>serviceName</code></td>
 <td><code>&lt;<em>myservice</em>&gt;</code> を、proxy-buffers-size を適用するサービスの名前に置き換えます。</td>
 </tr>
 <tr>
 <td><code>size</code></td>
 <td><code>&lt;<em>size</em>&gt;</code> を、各バッファーのサイズをキロバイト (k または K) 単位に置き換えます (例えば <em>1K</em>)。</td>
 </tr>
 </tbody></table>

 </dd>
 </dl>

<br />



### プロキシー・ビジー・バッファー・サイズ (proxy-busy-buffers-size)
{: #proxy-busy-buffers-size}

ビジー状態にすることができるプロキシー・バッファーのサイズを構成します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
クライアントへの応答がまだ全部読み取られていない間、その応答を送信中のバッファー群のサイズを制限します。 その間、残りのバッファーでは応答を読み取ることができます。さらに必要に応じて、応答の一部を一時ファイルにバッファリングできます。 特定のサービスが指定されていなければ、Ingress ホスト内のすべてのサービスにこの構成が適用されます。 例えば、<code>serviceName=SERVICE size=1k</code> という構成を指定した場合は、サービスに 1k が適用されます。 <code>size=1k</code> という構成を指定した場合は、Ingress ホスト内のすべてのサービスに 1k が適用されます。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-busy-buffers-size: "serviceName=&lt;myservice&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code> を、proxy-busy-buffers-size を適用するサービスの名前に置き換えます。</td>
</tr>
<tr>
<td><code>size</code></td>
<td><code>&lt;<em>size</em>&gt;</code> を、各バッファーのサイズをキロバイト (k または K) 単位に置き換えます (例えば <em>1K</em>)。</td>
</tr>
</tbody></table>

 </dd>
 </dl>

<br />



## 要求/応答アノテーション
{: #request-response}

### ホスト・ヘッダーへのサーバー・ポートの追加 (add-host-port)
{: #add-host-port}

<dl>
<dt>説明</dt>
<dd>要求をバックエンド・アプリに転送する前に、クライアント要求のホスト・ヘッダーに `:server_port` を追加します。

<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/add-host-port: "enabled=&lt;true&gt; serviceName=&lt;myservice&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>enabled</code></td>
   <td>ホストの server_port 設定を有効にするには、<code>true</code> に設定します。</td>
 </tr>
 <tr>
 <td><code>serviceName</code></td>
 <td><code><em>&lt;myservice&gt;</em></code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。 複数のサービスは、セミコロン (,) で区切ります。 このフィールドはオプションです。 サービス名を指定しない場合は、すべてのサービスがこのアノテーションを使用します。</td>
 </tr>
 </tbody></table>
 </dd>
 </dl>

<br />


### クライアント要求またはクライアント応答の追加ヘッダー (proxy-add-headers, response-add-headers)
{: #proxy-add-headers}

クライアント要求をバックエンド・アプリに送信する前に要求にさらにヘッダー情報を追加します。または、クライアント応答をクライアントに送信する前に応答にさらにヘッダー情報を追加します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>Ingress ALB は、クライアント・アプリとバックエンド・アプリの間のプロキシーとして機能します。 ALB に送信されたクライアント要求は、処理 (プロキシー処理) され、新しい要求に入れられた後に、バックエンド・アプリに送信されます。 同様に、ALB に送信されたバックエンド・アプリ応答も処理 (プロキシー処理) され、新しい応答に入れられた後に、クライアントに送信されます。 要求または応答のプロキシー処理によって、クライアントまたはバックエンド・アプリから最初に送信された HTTP ヘッダー情報 (ユーザー名など) は削除されます。

<br><br>
バックエンド・アプリに HTTP ヘッダー情報が必要な場合は、<code>proxy-add-headers</code> アノテーションを使用して、ALB がクライアント要求をバックエンド・アプリに転送する前に、ヘッダー情報をクライアント要求に追加できます。

<br>
<ul><li>例えば、要求がアプリに転送される前に、次の X-Forward ヘッダー情報を要求に追加する必要があるとします。

<pre class="screen">
<code>proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;</code></pre>

</li>

<li>X-Forward ヘッダー情報をアプリに送信される要求に追加するには、`proxy-add-headers` アノテーションを以下のように使用します。

<pre class="screen">
<code>ingress.bluemix.net/proxy-add-headers: |
  serviceName=&lt;myservice1&gt; {
  Host $host;
  X-Real-IP $remote_addr;
  X-Forwarded-Proto $scheme;
  X-Forwarded-For $proxy_add_x_forwarded_for;
  }</code></pre>

</li></ul><br>

クライアント Web アプリに HTTP ヘッダー情報が必要な場合は、<code>response-add-headers</code> アノテーションを使用して、ALB が応答をクライアント Web アプリに転送する前に、ヘッダー情報を応答に追加できます。</dd>

<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-add-headers: |
      serviceName=&lt;myservice1&gt; {
      &lt;header1&gt; &lt;value1&gt;;
      &lt;header2&gt; &lt;value2&gt;;
      }
      serviceName=&lt;myservice2&gt; {
      &lt;header3&gt; &lt;value3&gt;;
      }
    ingress.bluemix.net/response-add-headers: |
      serviceName=&lt;myservice1&gt; {
      &lt;header1&gt;: &lt;value1&gt;;
      &lt;header2&gt;: &lt;value2&gt;;
      }
      serviceName=&lt;myservice2&gt; {
      &lt;header3&gt;: &lt;value3&gt;;
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: &lt;myservice1&gt;
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: &lt;myservice2&gt;
          servicePort: 80</code></pre>

 <table>
 <caption>アノテーションの構成要素について</caption>
  <thead>
  <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
  </thead>
  <tbody>
  <tr>
  <td><code>service_name</code></td>
  <td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
  </tr>
  <tr>
  <td><code>&lt;header&gt;</code></td>
  <td>クライアント要求またはクライアント応答に追加するヘッダー情報のキー。</td>
  </tr>
  <tr>
  <td><code>&lt;value&gt;</code></td>
  <td>クライアント要求またはクライアント応答に追加するヘッダー情報の値。</td>
  </tr>
  </tbody></table>
 </dd></dl>

<br />



### クライアント応答ヘッダーの削除 (response-remove-headers)
{: #response-remove-headers}

バックエンド・アプリからのクライアント応答に含まれているヘッダー情報を削除してから、クライアントに応答を送信します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>Ingress ALB は、バックエンド・アプリとクライアント Web ブラウザーの間のプロキシーとして機能します。 ALB に送信されたバックエンド・アプリからのクライアント応答は、処理 (プロキシー処理) され、新しい応答に入れられた後に、ALB からクライアント Web ブラウザーに送信されます。 応答のプロキシー処理によって、バックエンド・アプリから最初に送信された HTTP ヘッダー情報は削除されますが、バックエンド・アプリに固有のヘッダーが削除されずに残る場合があります。 ALB からクライアント Web ブラウザーにクライアント応答を転送する前に、応答からヘッダー情報を削除します。</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>
<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/response-remove-headers: |
      serviceName=&lt;myservice1&gt; {
      "&lt;header1&gt;";
      "&lt;header2&gt;";
      }
      serviceName=&lt;myservice2&gt; {
      "&lt;header3&gt;";
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: &lt;myservice1&gt;
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: &lt;myservice2&gt;
          servicePort: 80</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code> を、アプリ用に作成した Kubernetes サービスの名前に置き換えます。</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>クライアント応答から削除するヘッダーのキー。</td>
</tr>
</tbody></table>
</dd></dl>

<br />


### クライアント要求本体のサイズ (client-max-body-size)
{: #client-max-body-size}

クライアントが要求の一部として送信できる本体の最大サイズを設定します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>必要なパフォーマンスを維持するため、クライアント要求本体の最大サイズは 1 M バイトに設定されています。 本体サイズがこの制限を超えるクライアント要求が Ingress ALB に送信された場合、データを分割することをクライアントが許可していなければ、ALB は HTTP 応答 413 (要求エンティティーが大き過ぎる) をクライアントに返します。 要求本体のサイズが小さくなるまで、クライアントと ALB の間の接続は確立されません。 データを複数のチャンクに分割することをクライアントが許可した場合は、データが 1 M バイトの複数のパッケージに分割されて、ALB に送信されます。

</br></br>
本体サイズが 1 M バイトを超えるクライアント要求が想定されるために、最大本体サイズを引き上げたい場合があります。 例えば、クライアントが大きなファイルをアップロードできるようにしたい場合があります。 要求本体の最大サイズを引き上げると、要求を受信するまでクライアントへの接続を開いておかなければならないため、ALB のパフォーマンスに影響を及ぼす可能性があります。
</br></br>
<strong>注:</strong> 一部のクライアント Web ブラウザーは、HTTP 応答 413 のメッセージを正しく表示できません。</dd>
<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/client-max-body-size: size=&lt;size&gt;
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>クライアント応答本体の最大サイズ。 例えば、最大サイズを 200 M バイトに設定するには、<code>200m</code> と定義します。  <strong>注:</strong> サイズを 0 に設定すると、クライアント要求の本体サイズの検査を無効にすることができます。</td>
 </tr>
 </tbody></table>

 </dd></dl>

<br />


### ラージ・クライアント・ヘッダー・バッファー (large-client-header-buffers)
{: #large-client-header-buffers}

ラージ・クライアント要求ヘッダーを読み取るバッファーの最大数と最大サイズを設定します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>ラージ・クライアント要求ヘッダーを読み取るバッファーは、必要な場合にのみ割り振られます。要求終了処理の後に接続がキープアライブ状態に遷移すると、これらのバッファーは解放されます。 バッファー・サイズは、デフォルトでは <code>8K</code> バイトです。 要求行が 1 つのバッファーの設定された最大サイズを超えると、「<code>414 Request-URI Too Large</code>」というエラーがクライアントに返されます。 また、要求ヘッダー・フィールドが 1 つのバッファーの設定された最大サイズを超えると、「<code>400 Bad Request</code>」というエラーがクライアントに返されます。 ラージ・クライアント要求ヘッダーの読み取りに使用されるバッファーの最大数と最大サイズを調整することができます。

<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/large-client-header-buffers: "number=&lt;number&gt; size=&lt;size&gt;"
spec:
 tls:
 - hosts:
   - mydomain
    secretName: mytlssecret
  rules:
 - host: mydomain
   http:
     paths:
     - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>ラージ・クライアント要求ヘッダーを読み取るために割り振る必要があるバッファーの最大数。 例えば、4 に設定するには、<code>4</code> と定義します。</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>ラージ・クライアント要求ヘッダーを読み取るバッファーの最大サイズ。 例えば、16 キロバイトに設定するには、<code>16k</code> と定義します。
   <strong>注:</strong> サイズの末尾は、キロバイトの場合は <code>k</code>、メガバイトの場合は <code>m</code> でなければなりません。</td>
 </tr>
</tbody></table>
</dd>
</dl>

<br />


## サービス制限アノテーション
{: #service-limit}


### グローバルな速度制限 (global-rate-limit)
{: #global-rate-limit}

すべてのサービスに対して、定義済みキーあたりの要求処理速度と接続数を制限します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>
すべてのサービスに対して、選択したバックエンドのすべてのパスに使用される単一の IP アドレスから渡される、定義済みキーあたりの要求処理速度と接続数を制限します。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/global-rate-limit: "key=&lt;key&gt; rate=&lt;rate&gt; conn=&lt;number_of_connections&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>key</code></td>
<td>ゾーンまたはサービスに基づいて着信要求のグローバル制限を設定するには、`key=zone` を使用します。 ヘッダーに基づいて着信要求のグローバル制限を設定するには、`X-USER-ID key=$http_x_user_id` を使用します。</td>
</tr>
<tr>
<td><code>rate</code></td>
<td><code>&lt;<em>rate</em>&gt;</code> を処理速度に置き換えます。 毎秒 (r/s) または毎分 (r/m) の速度として値を入力します。 例: <code>50r/m</code>。</td>
</tr>
<tr>
<td><code>number-of_connections</code></td>
<td><code>&lt;<em>conn</em>&gt;</code> を接続数に置き換えます。</td>
</tr>
</tbody></table>

</dd>
</dl>

<br />



### サービスの速度制限 (service-rate-limit)
{: #service-rate-limit}

特定のサービスに対して、要求処理速度と接続数を制限します。
{:shortdesc}

<dl>
<dt>説明</dt>
<dd>特定のサービスに対して、選択したバックエンドのすべてのパスに使用される単一の IP アドレスから渡される定義済みキーあたりの要求処理速度と接続数を制限します。
</dd>


<dt>サンプル Ingress リソース YAML</dt>
<dd>

<pre class="codeblock">
<code>apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/service-rate-limit: "serviceName=&lt;myservice&gt; key=&lt;key&gt; rate=&lt;rate&gt; conn=&lt;number_of_connections&gt;"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080</code></pre>

<table>
<caption>アノテーションの構成要素について</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="アイデア・アイコン"/>アノテーションの構成要素について</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td><code>&lt;<em>myservice</em>&gt;</code> を、処理速度を制限するサービスの名前に置き換えます。</li>
</tr>
<tr>
<td><code>key</code></td>
<td>ゾーンまたはサービスに基づいて着信要求のグローバル制限を設定するには、`key=zone` を使用します。 ヘッダーに基づいて着信要求のグローバル制限を設定するには、`X-USER-ID key=$http_x_user_id` を使用します。</td>
</tr>
<tr>
<td><code>rate</code></td>
<td><code>&lt;<em>rate</em>&gt;</code> を処理速度に置き換えます。 毎秒の速度を定義するには、r/s を使用して <code>10r/s</code> のようにします。 毎分の速度を定義するには、r/m を使用して <code>50r/m</code> のようにします。</td>
</tr>
<tr>
<td><code>number-of_connections</code></td>
<td><code>&lt;<em>conn</em>&gt;</code> を接続数に置き換えます。</td>
</tr>
</tbody></table>
</dd>
</dl>

<br />



