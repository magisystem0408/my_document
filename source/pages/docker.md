# Dockerあたり

## dockerコマンド

### ビルド
```docker
docker image build -t sample:latest .
```

### コンテナの停止
```docker
docker container stop sample
```

### コンテナの削除
```docker
docker container rm sample
```

### コンテナの確認
```docker
docker container ls -a

docker ps

docker ps -a
```

### コンテナのログ確認
```docker
docker container logs sample
```

### コンテナの一括削除
```docker
docker system prune -a 
```

## docker-composeコマンド

### ビルド
```docker
docker-compose build
```

### コンテナの作成と起動
```docker
docker-compose up -d

-dでバックグランドで起動する
```

### コンテナの停止・削除
```docker
docker-compose down
```

### コンテナの一覧を表示
```docker
docker-compose ps
```

### ログ表示
```docker
docker-compose logs
```

### 起動中のコンテナにコマンド実行
```docker
docker-compose exec <サービス> <コマンド>
```