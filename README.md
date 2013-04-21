grimrose-heroku-sample
======================

http://grails.org/Quick+Start のサンプルアプリケーションを https://www.heroku.com/ にデプロイするサンプルです。

* **herokuで予めアカウントを登録しておく必要があります**

herokuの準備をする
---------------------

https://devcenter.heroku.com/articles/quickstart を参考にStep 3: Login まで実行する。

プロジェクトを作成する
--------------------------

```bash
grails create-app $project
```

ドメインクラスを作成する
---------------------------

```bash
cd $project
grails create-domain-class org.example.Book
```

コントローラークラスを作成する
--------------------------------

```bash
grails create-controller org.example.Book 
```

ローカル環境で確認する
----------------------------

```bash
grails run-app
```

heroku postgresの設定
-------------------------

[DataSource.groovy](https://github.com/grimrose/grimrose-heroku-sample/blob/master/grails-app/conf/DataSource.groovy)
```bash
# grails-app/conf/DataSource.groovy

// environment specific settings
environments {

...

production {
    dataSource {
        dbCreate = "update"
        driverClassName = "org.postgresql.Driver"
        dialect = org.hibernate.dialect.PostgreSQLDialect
    
        uri = new URI(System.env.DATABASE_URL?:"postgres://test:test@localhost/test")

        url = "jdbc:postgresql://"+uri.host+uri.path
        username = uri.userInfo.split(":")[0]
        password = uri.userInfo.split(":")[1]
    }
}

...

```

postgresql jdbcドライバの追加
--------------------------------

[BuildConfig.groovy](https://github.com/grimrose/grimrose-heroku-sample/blob/master/grails-app/conf/BuildConfig.groovy)
```bash
# grails-app/conf/BuildConfig.groovy

grails.project.dependency.resolution = {

...

dependencies {
        runtime 'postgresql:postgresql:8.4-702.jdbc3'
    }

...

```

gitの設定
------------------

* レポジトリの初期化

```bash
git init
```

* gitignoreの追加

```bash
grails integrate-with --git
```

* commit

```bash
git add .
git commit -m 'first commit'
```

herokuにデプロイするための準備
---------------------------------

```bash
heroku create
```

Procfileの追加
--------------------

[Procfile](https://github.com/grimrose/grimrose-heroku-sample/blob/master/Procfile)
```bash
web: java $JAVA_OPTS -jar server/jetty-runner.jar --port $PORT target/*.war
```

OpenJDK7の設定
---------------------

[system.properties](https://github.com/grimrose/grimrose-heroku-sample/blob/master/system.properties)
```bash
java.runtime.version=1.7
```

設定ファイルをコミット
-----------------------------

```bash
git add Procfile system.properties
git commit -m 'add heroku deploy files'
```

herokuへのデプロイ
--------------------

```bash
git push heroku master
```

アプリケーションへアクセス
------------------------------

```bash
heroku open
```


