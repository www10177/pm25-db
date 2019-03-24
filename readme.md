# Reminder(0324)

- Date大致parse好了(year,month,day)，不過會有些特例無法parse 還沒處理

- 目前crawl的like list 清單請見partial.sql

- 空污相關粉專的資料在parsed_fb_air.db(僅此db有reaction list)

- ~~posts(table) 裡的grrr,sigh,wow...(column)和reactions(table)裡的reaction(column)內的文字不一致(前者為義大利文版，後者英文)~~

- 已調整，Posts table裡的column 調整如下，另外reacitons table裡raction column除了一般能按的6種心情，還有Tankful和pride兩個期間限定(?)的心情，若有需要處理請留意

  - Like

  - Grrr-> Angry

  - Love

  - Ahah -> Haha

  - Wow

  - Sigh ->Sad

    

  - Tankful(感恩節限定?)(/story.php?story_fbid=10155959085691988&id=170435726987)

  - pride (彩虹)(only /story.php?story_fbid=10156102147686988&id=170435726987)

- 資料有NaN，記得處理(photo的部分都NaN)





---



# fb.db

Time :　2008 ~ 2019/3/1

有6個尚未crawl

 - [x] 侯友宜(網址有更動，尚未crawl)
 - [ ] 呂秋遠(個人profile，並非粉專)
 - [ ] 莊秉潔(個人profile，並非粉專)
 - [ ] 傅瑜(個人profile，並非粉專)
 - [x] 韓國瑜後援會(私人社團，等待加入中)
 - [ ] 韓國瑜粉絲後援團 必勝！撐起一片藍天 (私人社團，等待加入中)

## Columns

`Crawl_from` : 從哪個粉專爬下來的

`source` : post來源(包含發布者和分享自哪個人)  

## Todo 

- [x] `Date` 欄位格式不一(2018-11-11; Aug,4,,2018 也許還有其他格式，不確定)
- [x] `reaction`,` like`...混雜文字 ex: 1.1k; 蔡英文 Tsai Ing-wen and 6.2K others
- [ ] 社團爬下來的資料似乎不齊全



## References

https://github.com/rugantio/fbcrawl



----

----



# cofacts.db

## Tables 

- articles

- replies

- article_replies

## Fields

### `articles`

The instant messages LINE bot users submitted into the database.

| Field            | Data type | Description |
| ----------------------- | -------- | ---- |
| `id`                      | String     |  |
| `references`              | Enum string     | Where the message is from. Currently the only possible value is `LINE`. |
| `userIdsha`                  | String     | Author of the article.|
| `appId`                   | String     |  |
| `tags`                    | Text     | Preserved for [category labels](https://github.com/cofacts/rumors-api/issues/32), currently empty. |
| `normalArticleReplyCount` | Integer     | The number of replies are associated to this article, excluding the deleted reply associations. |
| `text`                    | Text     | The instant message text |
| `hyperlinks`              | Text     | Preserved. Now empty. |
| `createdAt`               | ISO time string     | When the article is submitted to the database. |
| `updatedAt`               | ISO time string     | Preserved, currently identical to `createdAt` |
| `lastRequestedAt`         | ISO time string     | The submission time of the last `reply_request` is sent on the article, before the article is replied.  |

### `article_replies`

Articles and replies are in has-and-belongs-to-many relationship. That is, an article can have multiple replies, and a reply can be connected to multiple similar articles.

`article_replies` is the "join table" between `articles` and `replies`, bringing `articleId` and `replyId` together, along with other useful properties related to this connection between an article and a reply.

One pair of `articleId`, `replyId` will map to exactly one `article_reply`.


| Field            | Data type | Description |
| --------------------- | -------- | - |
| `articleId`             | String     | Relates to `id` field of `articles` |
| `replyId`               | String     | Relates to `id` field of `replies` |
| `userId`                | String     | The user connecting the reply with the article |
| `negativeFeedbackCount` | Integer     | Number of `article_reply_feedbacks` that has score `-1` |
| `positiveFeedbackCount` | Integer     | Number of `article_reply_feedbacks` that has score `1` |
| `replyType`             | Enum string     | Duplicated from `replies`'s type. |
| `appId`                 | String     | |
| `status`                | Enum string     | `NORMAL`: The reply and article are connected. `DELETED`: The reply does not connect to the article anymore. |
| `createdAt`             | ISO time string     | The time when the reply is connected to the article |
| `updatedAt`             | ISO time string     | The latest date when the reply's status is updated |


### `replies`

Editor's reply to the article.

| Field            | Data type | Description |
| --------- | -------- | - |
| `id`        | String     | |
| `type`      | Enum string     | Type of the reply chosen by the editor. `RUMOR`: The article contains rumor. `NON_RUMOR`: The article contains fact. `OPINIONATED`: The article contains personal opinions. `NOT_ARTICLE`: The article should not be processed by Cofacts. |
| `reference` | Text     | For `RUMOR` and `NON_RUMOR` replies: The reference to support the chosen `type` and `text`. For `OPINIONATED` replies: References containing different perspectives from the `article`. For `NOT_ARTICLE`: empty string. |
| `userId`    | String     | The editor that authored this reply. |
| `appId`     | String     | |
| `text`      | Text     | Reply text writtern by the editor |
| `createdAt` | ISO Time string     | When the reply is written |

  

## References

https://github.com/cofacts/opendata/













