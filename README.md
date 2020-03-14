# Django-on-Docker

#### Django 用の Docker イメージを Dockerfile から作成

```shell
$ docker-compose build
```
以下のようなことをやっています。

1. Python のイメージをダウンロード
2. ダウンロードしたイメージに `pip` を用いて `requirements.txt` に記述のある `django` パッケージをインストール

#### Django プロジェクトを作成

```shell
docker-compose run web django-admin startproject django_project_name .

```

最後の `.` を忘れないように注意してください。

このコマンドで、 Django プロジェクトに関するファイルが作成されます。
1. django_project_name ディレクトリ
2. manage.py ファイル

また、docker/postgres/ ディレクトリには PostgreSQL のデータが配置されます。

#### 起動

```shell
$ docker-compose up
```

http://localhost:8000/ にアクセスして、以下の画面が表示されれば完了です。
![image.png](https://qiita-image-store.s3.amazonaws.com/0/190905/d6910059-b01a-ccd0-f5b0-8aa8e9774277.png)

#### DB 設定

`django_project_name` ディレクトリ内の `settings.py` の DATABASES の記述を PostgreSQL 用に書き直します。

```python:settings.py
...
# Database
# https://docs.djangoproject.com/en/2.1/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
    }
}
...
```

```
bash

$ docker-compose exec web python manage.py migrate
```

migrate に成功すれば完了です。


# 以降の操作

```shell
# docker コンテナ停止
$ docker-compose down

# pip インストール, requirements.txt 更新
$ docker-compose run web pip install {パッケージ名}
$ docker-compose run web pip freeze > requirements.txt 

# manage.py コマンド
$ docker-compose run web python manage.py createsuperuser
$ docker-compose run web python manage.py makemigration
```


