
## Dokku For [BookStack](https://github.com/ssddanbrown/BookStack)

### Quickstart
1. Create your container:  
  `dokku apps:create <app-name>`
2. Install Plugin [Dokku-Mysql](https://github.com/dokku/dokku-mysql)
3. Create and link mysql to your app:  
  `dokku mysql:create <mysql-name>`  
  `dokku mysql:link <mysql-name> <app-name>`
4. Set env `APP_KEY` and `APP_URL` to app:  
  `dokku config:set <app-name> APP_KEY=SomeRandomStringWith32Characters APP_URL=https://containerdomain.example`
4. Push to dokku:  
  `git push dokku master`
5. Access terminal and run command to increase max upload size (nginx default is 1mb). If you use a different size, edit `php.ini` too:
  ```
  APP_NAME=<app-name>
  SIZE=100m
  sudo mkdir /home/dokku/${APP_NAME}/nginx.conf.d/
  echo -e 'client_max_body_size '${SIZE}';\nclient_body_timeout 300s;' | sudo tee -a /home/dokku/${APP_NAME}/nginx.conf.d/upload.conf
  sudo chown -R dokku:dokku /home/dokku/${APP_NAME}/nginx.conf.d
  sudo service nginx reload
  ```
7. gg!

### Mount Persistent Storage
1. Creat two direcories `.../storage` and `.../uploads` on the dokku host machine
2. Mount the two volumes:  
  `dokku storage:mount <app-name> <storage-dir>:/var/www/bookstack/public/storage`  
  `dokku storage:mount <app-name> <uploads-dir>:/var/www/bookstack/public/uploads`
3. Restart the application:  
  `dokku ps:restart <app-name>`

### Backup Database
`dokku mysql:clone <mysql-name> <mysql-backup>`

### Updading BookStack
1. Backup Database:  
  `dokku mysql:clone <mysql-name> <mysql-backup>`
2. Change version number in Dockerfile:  
  `BOOKSTACK_VERSION=<bookstack-version-number> \`
3. Commit and Push changes to dokku

### Using S3 to storage images and files:
Set to app follow envs:
```
STORAGE_TYPE="s3"
STORAGE_S3_KEY="**KEY**"
STORAGE_S3_SECRET="**SECRET**"
STORAGE_S3_REGION="**REGION**"
STORAGE_S3_BUCKET="**BUCKET**"
```
Set if you using S3 custom domains/cdns:  
`STORAGE_URL="**URL**"`

Thanks for [docker-bookstack](https://github.com/solidnerd/docker-bookstack) <3
