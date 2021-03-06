## apacheインストール

```console
# yum -y install httpd mod_ssl
```

## apache概要

|Service Name|httpd|
|------------|-----|
|Protocol and port|80/TCP , 443/TCP|
|SELinux Module|apache 2.1.2|
|Deamon Program|/usr/sbin/httpd|
|Configuration files|/etc/httpd/conf/httpd.conf, /etc/httpd/conf.d/#.conf|
|Pid file|/var/run/httpd.pid|
|Lock file|/var/lock/subsys/httpd|
|Control script|/etc/init.d/httpd|
|Script parameters|start stop restart condrestart reload status fullstatas graceful help configtest|
|Startup|2 3 4 5/85 15|

## httpd.conf

### 動作環境の設定(Global Enviroment)

    ServerTokens OS           #SERVERレスポンスヘッダ
    ServerRoot "/etc/httpd"   #設定ファイルのパス
    PidFile run/httpd.pid       #プロセスIDファイルの指定
    TimeOut 60           #タイムアウト時間  rest(GET,POST,PUT,DELETE)
    KeepAlive Off        #1回のTCPセッションで、複数のHTTPリクエストを処理させる
    MaxKeepAliveRequests 100   #1回のTCPセッションで受け付ける事の出来るリクエスト数
    KeepAliveTimeout 15    #TCPセッションを切断せずに次のHTTPリクエストを待つ時間

```apache
<IfModule prefork.c>   #MPM prefockの設定
  StartServers       8    
  MinSpareServers    5    
  MaxSpareServers   20   
  ServerLimit      256  
  MaxClients       256  
  MaxRequestsPerChild  4000
</IfModule>

Listen 80  #待機するアドレス/ポートの設定
LoadModule  #モジュールの使用に関する設定
Include conf.d/#.conf  #他ファイルの読み込みに関する設定
#ExtendedStatus On   #ステータス情報の使用の設定
User apache   #プロセス所有者
Group apache
```

### Mainサーバに関する主要な設定

```apache
ServerAdmin root@localhost   #サーバ管理者の指定
#ServerName www.example.com:80 #サーバ名の指定
UseCanonicalName Off #サーバ名の指定 Off/On/Dns
DocumentRoot "/var/www/html" #ドキュメントルートの指定
ServerSignature On/Off/Email #サーバが生成するドキュメントフッターの設定
```

### ディレクトリアクセスに関する設定

```apache
UserDir disabled  #ユーザが公開するディレクトリの指定 disabled|enabled
#UserDir public_html
DirectoryIndex index.html index.html.var #インデックスファイルの指定  .varファイルを利用する事でブラウザの指定する言語に併せたページを表示する事が可能です
```

### ログに関する設定

```apache
HostnameLookups Off #ホスト名解決の設定 アクセスログに記録する際、クライアントのIPアドレスから,DNSに問い合わせて得られたホスト名で記録するかを指定。 On|Off|Double
#EnableMMAP off|on #メモリマッピングの設定
#EnableSendfile off|on #Sendfileの設定
```

### エラーログに関する設定

```apache
ErrorLog logs/error_log
LogLevel warn|emeg|alert|crit|error|warn|notice|info|debug
```

### アクセスログに関する設定

```apache
combined(複合型),common(基本型),referer(REFEREおよび、AGENTのみ)
LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
LogFormat "%h %l %u %t \"%r\" %>s %b" common
LogFormat "%{Referer}i -> %U" referer
LogFormat "%{User-agent}i" agent
#LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio
#CustomLog logs/access_log common
#CustomLog logs/referer_log referer
#CustomLog logs/agent_log agent
CustomLog logs/access_log combined
```

### エイリアスの設定

```apache
Alias /icons/ "/var/www/icons/"  #エイリアスの指定

<Directory "/var/www/icons">
    Options Indexes MultiViews FollowSymLinks
    AllowOverride None
    Order allow,deny
    Allow from all
</Directory>

<IfModule mod_dav_fs.c>
    # Location of the WebDAV lock database.
    DAVLockDB /var/lib/dav/lockdb
</IfModule>

ScriptAlias /cgi-bin/ "/var/www/cgi-bin/" #スクリプトエイリアスの指定
```

```apache
<Directory "/var/www/cgi-bin">
    AllowOverride None
    Options None
    Order allow,deny
    Allow from all
</Directory>

IndexOptions FancyIndexing VersionSort NameWidth=# HTMLTable Charset=UTF-8 #インデックス表示の書式設定
```

### アイコン情報の指定

```apache
AddIconByEncoding (CMP,/icons/compressed.gif) x-compress x-gzip
AddIconByType (TXT,/icons/text.gif) text/*
AddIconByType (IMG,/icons/image2.gif) image/*
AddIconByType (SND,/icons/sound2.gif) audio/*
AddIconByType (VID,/icons/movie.gif) video/*
```

### 挿入ファイルの指定

```apache
ReadmeName README.html
HeaderName HEADER.html
```

### 無視するファイルの指定

```apache
IndexIgnore .??# #~ ## HEADER# README# RCS CVS #,v #,t
MIME言語タイプとサフィックスの対応付け
AddLanguage ca .ca
AddLanguage cs .cz .cs
LanguagePriority en ca cs da de el eo es et fr he hr it ja ko ltz nl nn no pl pt pt-BR ru sv zh-CN zh-TW
ForceLanguagePriority Prefer Fallback
```

### 文字コードとサッフィクスの対応付け

```apache
AddDefaultCharset UTF-8
```

### MIMEタイプとサフィックスの対応付け

```apache
#AddType application/x-tar .tgz
```

### ハンドラとサフィックスの対応付け

```apache
#AddHandler cgi-script .cgi
```

### サーバ応答のフィルタの対応付け

```apache
AddOutputFilter INCLUDES .shtml
```

### エラードキュメントの設定

```apache
<IfModule mod_negotiation.c>
<IfModule mod_include.c>
    <Directory "/var/www/error">
        AllowOverride None
        Options IncludesNoExec
        AddOutputFilter Includes html
        AddHandler type-map var
        Order allow,deny
        Allow from all
        LanguagePriority en es de fr
        ForceLanguagePriority Prefer Fallback
    </Directory>

#    ErrorDocument 400 /error/HTTP_BAD_REQUEST.html.var
#    ErrorDocument 401 /error/HTTP_UNAUTHORIZED.html.var
#    ErrorDocument 403 /error/HTTP_FORBIDDEN.html.var

</IfModule>
</IfModule>
```

### アクセス制御に関する設定

```apache
<Directory /> #ディレクトリ単位のアクセス制御
    Options FollowSymLinks #制御オプション
    AllowOverride All
    Deny from All
</Directory>
<Directory "/var/www/html">
    Options Indexes FollowSymLinks
    AllowOverride All
    Order allow,deny
    Allow from all
</Directory>
```

#### Options

|Type|内容|
|----|---|
|None|全て無効にする|
|All|全て有効にする|
|ExecCGI|CGIプログラムの実行を許可する|
|FollowSymLinks|シンボリックリンクがあるとき、それを辿る事を許可する|
|Includes|SSIを許可する|
|IncludesNOEXEC|SSIを許可するが、#exec,#cmd,#includeによるプログラムの実行は禁止する|
|Indexes|ディレクトリインデックスの作成を許可する|
|MultiViews|Content negotiated MultiViewsを許可する|
|SymLinksOwnerMatch|シンボリックリンクとリンク先が同じ所有車である場合のみ、それを辿る事を許可する|

#### AllowOverride

|Type|内容|
|----|---|
|None|オーバライドを無効|
|All|すべて有効|
|AuthConfig|認証に関するディレクティブを有効にする。対象のディレクティブは、AuthDBMUserFile,AuthDBMGroupFile,AuthGroupFile,AuthName,AuthTypeなど|
|FileInfo|ドキュメントタイプを指定するディレクティブを有効にする。|
|Indexes|ディレクトリインデックスを制御するディレクティブを有効にする|
|Limit|アクセス制御を行うディレクティブを有効にする|
|Options|機能を制御するディレクティブを有効にする|
