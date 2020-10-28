```yalm
version: '2'
services:

  wekandb:
    image: hasangnu/mongo
    container_name: wekan-db
    restart: always
    command: mongod --oplogSize 128
    networks:
      - wekan-tier
    expose:
      - 27017
    volumes:
      - ./wekan-db:/data/db
      - ./wekan-db-dump:/dump

  wekan:
    image: wekanteam/wekan
    container_name: wekan-app
    restart: always
    networks:
      - wekan-tier
    ports:
      - 80:8080
    environment:
      - MONGO_URL=mongodb://wekandb:27017/wekan
      - ROOT_URL=http://localhost
      - WITH_API=true
      - RICHER_CARD_COMMENT_EDITOR=false
      - CARD_OPENED_WEBHOOK_ENABLED=false
      - BIGEVENTS_PATTERN=NONE
      - BROWSER_POLICY_ENABLED=true
    depends_on:
      - wekandb

networks:
  wekan-tier:
    driver: bridge
```
```
docker-compose up -d
```
