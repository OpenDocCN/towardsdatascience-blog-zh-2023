# 使用 Google Cloud Recommendation AI 构建推荐系统

> 原文：[https://towardsdatascience.com/build-a-recommender-system-using-google-cloud-c0929f0c3080?source=collection_archive---------0-----------------------#2023-06-21](https://towardsdatascience.com/build-a-recommender-system-using-google-cloud-c0929f0c3080?source=collection_archive---------0-----------------------#2023-06-21)

## 使用 Google Cloud Recommendation AI 实现高度先进的推荐系统

[](https://muffaddal-qutbuddin.medium.com/?source=post_page-----c0929f0c3080--------------------------------)[![Muffaddal Qutbuddin](../Images/2c97b8f972adb48a513a0f5a361ac947.png)](https://muffaddal-qutbuddin.medium.com/?source=post_page-----c0929f0c3080--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c0929f0c3080--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c0929f0c3080--------------------------------) [Muffaddal Qutbuddin](https://muffaddal-qutbuddin.medium.com/?source=post_page-----c0929f0c3080--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F13dafa14dc1c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-recommender-system-using-google-cloud-c0929f0c3080&user=Muffaddal+Qutbuddin&userId=13dafa14dc1c&source=post_page-13dafa14dc1c----c0929f0c3080---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c0929f0c3080--------------------------------) ·14 分钟阅读·2023年6月21日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc0929f0c3080&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-recommender-system-using-google-cloud-c0929f0c3080&source=-----c0929f0c3080---------------------bookmark_footer-----------)![](../Images/ef26bc024ae7ebec5beef70760064e63.png)

[来源 pixabay](https://pixabay.com/illustrations/ai-generated-robot-technology-8015425/)

想象一下谷歌自己的机器学习工程师在为你的网站和应用程序实施推荐系统。借助 Google Cloud Recommendation AI，你可以利用正在用于驱动 YouTube、Google 广告和其他 Google 产品的推荐系统，为我们提供个性化服务。

在本文中，我将指导您如何实施 [Google Cloud Recommendation AI](https://cloud.google.com/recommendations)，以便为您的客户提供个性化体验。

# Google Cloud Recommendation AI 的好处？

Noon、IKEA、KINGUIN 以及许多其他公司使用 Google Cloud Recommendation AI 提供跨渠道的高性能推荐。

![](../Images/762b18e441829676eb250a8ff2429ad9.png)

Muffaddal 关于 Google Cloud Recommendation AI 对某些企业的初步影响

在工作量和资源方面，构建和部署推荐系统可能是一个复杂且耗时的任务。它通常涉及数据工程师、数据科学家和机器学习工程师的协作，以创建和运营一个全面的推荐系统。然而，使用 Google Cloud Recommendation AI 的情况却大不相同。

通过利用 Google Cloud Recommendation AI，实施过程变得显著简化且高效。这项完全托管的服务使您能够快速提供个性化体验，节省宝贵的时间和资源。实施 Google Cloud Recommendation AI 可以在以下几个方面带来明显的改进：

1.  **转化率：** 通过向用户提供量身定制的产品或内容推荐，您可以提升他们的购物或浏览体验，从而增加转化的可能性。

1.  **客户满意度：** 个性化推荐满足了个别用户的具体偏好和需求，带来了更高的客户满意度和参与度。

1.  **收入生成：** 通过提高转化率和客户满意度，实施 Google Cloud Recommendation AI 可以对您的收入流产生积极影响。

1.  **时间节省：** 作为一项完全托管的服务，Google Cloud Recommendation AI 减少了广泛开发和维护工作的需求，使您能够专注于业务的其他关键方面。

1.  **可扩展性：** Google Cloud Recommendation AI 旨在处理大量数据，并且能够随着业务增长无缝扩展，确保一致的性能和准确性。

通过利用 Google Cloud Recommendation AI 的能力，您可以释放显著改善客户体验、转化率、收入和运营效率的潜力。

[*想要实施推荐 AI？让我们聊聊*](https://www.linkedin.com/in/muffaddal-qutbuddin/)。

# Google Cloud Recommendation AI 可以在个性化方面实施的领域

Google Cloud Recommendation AI 提供了一系列机器学习模型和配置，可以在各种业务环境中有效地利用，以提升用户体验并推动更高的投资回报。以下是 Google Cloud Recommendation AI 可以应用的一些关键领域：

1.  个性化推荐：在主页上实施“为您推荐”或“推荐给您”部分，根据用户的个人偏好和行为提供量身定制的建议。

1.  商品推荐：在产品详情页展示“您可能喜欢的其他商品”部分，推荐用户高度可能感兴趣或购买的商品。

1.  购物车推荐：利用“常一起购买”部分，建议用户购买与所选商品常一起购买的其他产品，从而提升用户的购物体验。

1.  相似商品推荐：在产品页面展示“相似商品”部分，展示与当前浏览商品具有相似属性的商品，尤其在浏览的商品缺货时非常有用。

1.  重复购买推荐：在不同页面（如主页、产品详情、购物车）实施“再次购买”功能，建议用户基于之前的互动记录，推荐可能会再次购买的商品。

1.  销售推荐：利用“促销”部分展示打折商品，鼓励用户探索并进行购买。

此外，Google Cloud 推荐 AI 还支持客观优化，使企业能够根据特定目标改进其模型。例如，内容管理平台如 Medium 可能会优化产品浏览量，而电子商务网站则专注于转化优化。

你还可以结合多个目标来最大化个性化的好处。例如，可以使用优化参与度的模型来促进新用户的探索，而针对返回用户的转化和收入优化则可以发挥作用。

不要忘记电子邮件个性化。Google Cloud 推荐 AI 驱动的模型也可以与电子邮件结合使用，以提升业务目标。例如，我们可以在用户购买商品后，通过电子邮件进行追加销售或交叉销售。通过“为您推荐”模型进行精准定位，增加用户互动。

结合以上提到的所有内容，我们可以在用户在我们平台上的整个生命周期内完全改变他们的体验，从而增加 LTV 和业务收入。

# 如何实现 Google Cloud 推荐 AI？

推荐 AI 需要两个方面来驱动机器学习模型。一是用户活动，二是用户进行活动的产品详情。

活动或操作作为事件发送，商品详情作为目录存储在推荐 AI 中。一旦我们获得了所需数量的事件和目录详情，我们可以训练我们的机器学习模型，为用户提供基于用户过去行为和属性的个性化列表。

让我们逐一详细了解这些内容。

![](../Images/f1f561461cbd711ce53536681ab9c749.png)

使用 Google Cloud 推荐 AI 的数据管道，由 Muffaddal 提供

## 1- 导入产品详情

目录可以有许多字段和属性，但 `id`、`name`、`title` 和 `categories` 是必需字段，必须提供。

[点击这里了解有关目录所有可用字段的更多详情](https://cloud.google.com/retail/docs/reference/rest/v2/projects.locations.catalogs.branches.products)。

假设我们的产品详细信息已经存在于 BigQuery 中，我们将利用 BigQuery 和 Recommendation AI 的集成来导入目录数据。

![](../Images/6ae5cc2476f47292852a38f49868d14b.png)

Google Cloud Recommendation AI 的产品目录导入过程，由 Muffaddal 提供

Google Cloud Recommendation AI 需要一个特定的 BigQuery 表模式。因此，我们必须创建一个符合要求格式的表，并将目录数据插入其中。

假设我们的目录表在 BigQuery 中具有以下字段

1.  name: 产品名称

1.  id: 产品的 id

1.  category: 产品的分配类别

1.  description: 产品的描述

1.  url: 网站上产品的 URL

1.  image_link: 产品的公开访问图片链接

1.  city: 产品可用的城市

根据上述产品目录详细信息，我们需要为 BigQuery 表创建模式。[从这里获取模式](https://cloud.google.com/retail/docs/catalog#schema)，并根据我们的数据进行更新。

我们可以为目录设置许多字段。数据越多越好。为了演示的目的，我将使用最常见的字段。请注意，可为空的字段是可选的。

`type` 字段是我们决定产品是变体还是主产品的地方。本文中我将使用 `PRIMARY`。 [点击这里了解更多详情](https://cloud.google.com/retail/docs/reference/rest/v2/projects.locations.catalogs#ProductLevelConfig.FIELDS.ingestion_product_type)。

一旦我们的表准备好，我们可以使用以下查询将目录数据从主表插入到此表中。

```py
insert into `recommendersystem.product_data` 

(
  name,id,type,primaryProductId,collectionMemberIds,gtin,categories,title,brands,description,languageCode,attributes,  tags,    
  priceInfo,rating,expireTime,ttl,availableTime,availability,availableQuantity,fulfillmentInfo, uri, images,audience,colorInfo,sizes,materials,patterns,conditions,retrievableFields,publishTime,promotions
)

SELECT 
   name,
   cast(id as string) as id,
   "PRIMARY" as type, 
   cast(id as string) as primaryProductId,
    null as collectionMemberIds,
    null as gtin,
   array [categories] as categories,
   name as title,
   array[title] as brands,
   ifnull(description,name) as description,
    null as languageCode,
   [
      struct(
      'product_location' as key,  STRUCT(array[ifnull(city,"empty")] as text,  cast(null as ARRAY<FLOAT64>) as numbers, true as searchable, true as indexable ) as value
      )]
   as attributes,
   ARRAY_CONCAT(
      [ifnull(location,"empty")],
      [ifnull(categories,"empty")]      
    ) as tags,
    null as  priceInfo,
    null as  rating,
    null as  expireTime,
    null as  ttl,
    null as  availableTime,
    null as  availability,
    null as  availableQuantity,
    null as  fulfillmentInfo,
   url,
   array[struct(image_url) as uri, null as height, null as width)] as images,
   null as audience,
   null as colorInfo,
   null as sizes,
   null as materials,
   null as patterns,
   null as conditions,
   null as retrievableFields,
   null as publishTime,
   null as promotions
FROM `product.product_details`
```

数据在我们的新表中可用后，我们准备将其导入 Recommendation AI。

在 [Google Cloud 的零售 AI 数据标签](https://console.cloud.google.com/ai/retail/catalogs/default_catalog/data/catalog?cloudshell=false&project=test-prod)中，点击左上角的导入以导入数据。

![](../Images/055a7b65294fc3318814aaa1a59f161b.png)

向 Google Cloud Recommendation AI 导入目录详细信息，由 Muffaddal 提供

将出现一个面板，如下图所示。选择导入类型中的“产品目录”和数据源中的“BigQuery”。提供 BigQuery 表路径，选择一个分支并点击导入。

![](../Images/d706837814880b92b738ca27d5fa9644.png)

Google Cloud Recommendation AI 的目录导入面板，由 Muffaddal 提供

等待几分钟以查看 Retail AI 数据表中的目录详细信息。

## 2- 导入历史事件

接下来，我们需要将用户的历史数据导入 Recommendation AI。此步骤是可选的，但这样做有助于构建更好的机器学习模型。

类似于目录，我们需要在BigQuery表中拥有符合要求格式的事件。

以下是Google Cloud Recommendation AI接受的事件

![](../Images/47ff0ac09b832560b90c6d9f0111834f.png)

可以发送到Google Cloud Recommendation AI的事件，由Muffaddal提供

在所有这些事件中，`home-page-view`、`detail-page-viewed`、`add-to-cart`和`purchase-complete`对于全面激活AI模型是必要的。

每个事件都有一个特定的表模式，用于数据导入。[你可以在这里详细查看](https://cloud.google.com/retail/docs/user-events#bigquery_6)。

1- `home-page-view`、`detail-page-viewed`和`add-to-cart`的表模式如下

```py
[
    {
        "name": "eventType",
        "type": "STRING",
        "mode": "REQUIRED"
    },
    {
        "name": "visitorId",
        "type": "STRING",
        "mode": "REQUIRED"
    },
    {
        "name": "eventTime",
        "type": "STRING",
        "mode": "REQUIRED"
    },
 {
   "name": "productDetails",
   "type": "RECORD",
   "mode": "REPEATED",
   "fields": [
     {
       "name": "product",
       "type": "RECORD",
       "mode": "REQUIRED",
       "fields": [
         {
           "name": "id",
           "type": "STRING",
           "mode": "REQUIRED"
         }
       ]
     },
     {
       "name": "quantity",
       "type": "INTEGER",
       "mode": "REQUIRED"
     }
   ]
 },
    {
        "name": "attributes",
        "type": "RECORD",
        "mode": "NULLABLE",
        "fields": [
            {
                "name": "deviceType",
                "type": "RECORD",
                "mode": "NULLABLE",
                "fields": [
                    {
                        "name": "text",
                        "type": "STRING",
                        "mode": "REPEATED"
                    }
                ]
            }
        ]
    }
]
```

2- `purchase-complete`事件的模式如下

```py
[
 {
   "name": "eventType",
   "type": "STRING",
   "mode": "REQUIRED"
 },
 {
   "name": "visitorId",
   "type": "STRING",
   "mode": "REQUIRED"
 },
 {
   "name": "eventTime",
   "type": "STRING",
   "mode": "REQUIRED"
 },
 {
   "name": "productDetails",
   "type": "RECORD",
   "mode": "REPEATED",
   "fields": [
     {
       "name": "product",
       "type": "RECORD",
       "mode": "REQUIRED",
       "fields": [
         {
           "name": "id",
           "type": "STRING",
           "mode": "REQUIRED"
         }
       ]
     },
     {
       "name": "quantity",
       "type": "INTEGER",
       "mode": "REQUIRED"
     }
   ]
 },
 {
   "name": "purchaseTransaction",
   "type": "RECORD",
   "mode": "REQUIRED",
   "fields": [
     {
       "name": "revenue",
       "type": "FLOAT",
       "mode": "REQUIRED"
     },
     {
       "name": "currencyCode",
       "type": "STRING",
       "mode": "REQUIRED"
     }
   ]
 },
 {
    "name": "attributes",
    "type": "RECORD",
    "mode": "NULLABLE",
    "fields": [
        {
            "name": "deviceType",
            "type": "RECORD",
            "mode": "NULLABLE",
            "fields": [
                {
                    "name": "text",
                    "type": "STRING",
                    "mode": "REPEATED"
                }
            ]
        },
        {
            "name": "cityName",
            "type": "RECORD",
            "mode": "NULLABLE",
            "fields": [
                {
                    "name": "text",
                    "type": "STRING",
                    "mode": "REPEATED"
                }
            ]
        }
    ]
}

]
```

一旦你创建了表，可以使用以下查询将数据插入到新表中。

1- `home-page-viewed`的SQL插入查询如下

```py
insert into `recommendersystem.user_event_home_page_view` (eventType, visitorId,eventTime,attributes)
  SELECT 
  'home-page-view' as eventType,
  visitorId,
  eventTime,
  struct(
      struct([deviceType] as text)as deviceType,
      struct([city] as text)as cityName 
  )
    as attributes,
  from (
    select *, 
 deviceType from  `recommendersystem.user_event_history` 
```

2- `detail-page-viewed`、`add-to-cart`的SQL插入查询为

```py
insert into `recommendersystem.user_event_add_to_cart` (eventType, visitorId,eventTime,productDetails,attributes)
SELECT 
'add-to-cart' as eventType,
visitorId,
eventTime,
[
    struct( struct(product_id  as id) as product , 1 as quantity) 
] as productDetails,
 struct(
     struct([deviceType] as text)as deviceType,
     struct([city] as text)as cityName 

 )
   as attributes,
from (
  select *, 
 deviceType from  `recommendersystem.user_event_history` 
-- limit 100
)
```

3- `purchase-complete`事件的SQL插入查询为

```py
insert into `recommendersystem.user_event_purchase_complete` (eventType, visitorId,eventTime,productDetails,purchaseTransaction,attributes)
SELECT 
'purchase-complete' as eventType,
cast( visitorId as string) as visitorId,
eventTime,
[
    struct( struct(product_id  as id) as product , 1 as quantity) 
] as productDetails,
struct(safe_cast(revenue as float64) as revenue, 'USD' as currencyCode) as purchaseTransaction,
 struct(
     struct([deviceType] as text)as deviceType,
     struct([city] as text)as cityName 

 )
   as attributes
from (
  select *, 
deviceType from  `recommendersystem.user_purchase_event` 
)
```

注：访客ID和用户ID可以相同，也可以不同。这取决于用户是否需要在使用你的产品之前登录。

注：Google Cloud [推荐AI也支持Google Analytics 4原始数据](https://cloud.google.com/retail/docs/import-user-events#bq-ga4)。如果你有这些数据，则无需进行转换，可以直接导入。

要导入历史数据，请像以前一样转到零售AI的数据标签页，然后点击顶部的导入。

在导入面板中，选择导入类型为用户事件，选择以下选项，然后点击导入。

![](../Images/7d4fbcd860b347c0b5df29d4c9310e28.png)

将历史事件导入Google Cloud Recommendation AI，由Muffaddal提供

我们已经成功将用户事件导入到Google Cloud Recommendation AI中。

## 3- 发送实时事件

接下来，我们需要向用户发送实时事件，以便AI模型可以重新训练并在用户与平台互动的过程中提升推荐效果。

有三种方法将用户事件发送到Google Cloud Recommendation AI。使用 [javascript pixel](https://cloud.google.com/retail/docs/record-events#pixel)、[使用API](https://cloud.google.com/retail/docs/record-events#write) 和 [GTM](https://cloud.google.com/retail/docs/record-events#gtm)。

本文将使用API，因为它可以覆盖所有情况，无论网站或应用的性质如何。

这是发送`home-page-viewed`事件的curl调用

```py
curl -X POST \
     -H "Authorization: Bearer ya29.a0AasRrdaM8jq4J0FBtJpsdhu87ZJcPlr4-8NqkDdGmLYtQ7P-riTx5man4k2maqWGNIsL1007a4BClSsfVbgHyjycaKn_5bviofo5XCbvCeO5-kkepnb_RUgy6prxRX7X8pi2PFHxX-kbmSmQgeEoILQQnK_aYMtTagSFzkUXC12Q2A5VmlfXR5cvSW-a80XxGOikpEf1jHuwusQx2EftBITnhPaYvg6Xi08qzvAEnmKgYicqCqY5o9d9ixav1jm6bd0r7A" \
     -H "Content-Type: application/json; charset=utf-8" \
     -H "X-Goog-User-Project: test-prod "
     --data "{
         'eventType': 'home-page-view',
         'visitorId': '12',
         'eventTime': '2021-09-28T03:33:33.000001Z',
         'attributionToken': 'ABC',
         'attributes': {
            'city_name': {
              'text': ['karachi']
            },
            'device_type': {
              'text': ['iOS']
            },
         },
         'userInfo': {
           'userId': '123',
         }
}"\
"https://retail.googleapis.com/v2/projects/test-prod/locations/global/catalogs/default_catalog/userEvents:write"
```

`detail-page-view`的Curl调用

```py
curl -X POST \
     -H "Authorization: Bearer ya29.a0ARrdaMsd84J0FBtZdJp2jhu87ZJcPlr4-8NqkDdGmLYtQ7P-riTx5man4k2maqWGNIsL1007a4BClSsfVbgHyjycaKn_5bviofo5XCbvCeO5-kkepnb_RUgy6prxRX7X8pi2PFHxX-kbmSmQgeEoILQQnK_aYMtTagSFzkUXC12Q2A5VmlfXR5cvSW-a80XxGOikpEf1jHuwusQx2EftBITnhPaYvg6Xi08qzvAEnmKgYicqCqY5o9d9ixav1jm6bd0r7A" \
     -H "Content-Type: application/json; charset=utf-8" \
     -H "X-Goog-User-Project: test-prod "
     --data "{
         'eventType': 'detail-page-view',
         'visitorId': '123',
         'eventTime': '2021-09-28T03:33:33.000001Z',
         'attributionToken': 'ABC',
         'attributes': {
            'city_name': {
              'text': ['karachi']
            },
            'device_type': {
              'text': ['iOS']
            },
         },
         'productDetails': [{
           'product': {
             'id': '2806'
           }
          }],
         'userInfo': {
           'userId': '123',
         }
}"\
"https://retail.googleapis.com/v2/projects/test-prod/locations/global/catalogs/default_catalog/userEvents:write"
```

`add-to-cart`的Curl调用

```py
curl -X POST \
     -H "Authorization: Bearer ya29.sdrdaM8jq4J0FBtZdJp2jsdshu87ZJcPlr4-8NqkDdGmLYtQ7P-riTx5man4k2maqWGNIsL1007a4BClSsfVbgHyjycaKn_5bviofo5XCbvCeO5-kkepnb_RUgy6prxRX7X8pi2PFHxX-kbmSmQgeEoILQQnK_aYMtTagSFzkUXC12Q2A5VmlfXR5cvSW-a80XxGOikpEf1jHuwusQx2EftBITnhPaYvg6Xi08qzvAEnmKgYicqCqY5o9d9ixav1jm6bd0r7A" \
     -H "Content-Type: application/json; charset=utf-8" \
     -H "X-Goog-User-Project: test-prod"
     --data "{
         'eventType': 'add-to-cart',
         'visitorId': '123',
         'eventTime': '2021-09-28T03:33:33.000001Z',
         'attributionToken': 'ABC',
         'attributes': {
            'city_name': {
              'text': ['karachi']
            },
            'device_type': {
              'text': ['iOS']
            },
         },
         'productDetails': [{
           'product': {
             'id': '2806'
           },
     'quantity':1
          }],
         'userInfo': {
           'userId': '123',
         }
}"\
"https://retail.googleapis.com/v2/projects/test-prod/locations/global/catalogs/default_catalog/userEvents:write"
```

`purchase-complete`的Curl调用

```py
curl -X POST \
     -H "Authorization: Bearer ya29.a0ARrsddaM8jJ0FBtZdJsdjhu87ZJcPlr4-8NqkDdGmLYtQ7P-riTx5man4k2maqWGNIsL1007a4BClSsfVbgHyjycaKn_5bviofo5XCbvCeO5-kkepnb_RUgy6prxRX7X8pi2PFHxX-kbmSmQgeEoILQQnK_aYMtTagSFzkUXC12Q2A5VmlfXR5cvSW-a80XxGOikpEf1jHuwusQx2EftBITnhPaYvg6Xi08qzvAEnmKgYicqCqY5o9d9ixav1jm6bd0r7A" \
     -H "Content-Type: application/json; charset=utf-8" \
     -H "X-Goog-User-Project: test-prod "
     --data "{
         'eventType': 'purchase-complete',
         'visitorId': '123',
         'eventTime': '2021-09-28T03:33:33.000001Z',
         'attributionToken': 'ABC',
         'attributes': {
            'city_name': {
              'text': ['karachi']
            },
            'device_type': {
              'text': ['iOS']
            },
         },
         'productDetails': [{
           'product': {
             'id': '2806'
           },
     'quantity':'1'
          }],
          'purchaseTransaction':{
              "id": 'transacion-id-here',
              "revenue": 'orderPrice-here',
              "currencyCode": 'USD',
               "quantity":'1'
            }
         'userInfo': {
           'userId': '123',
         }
}"\
"https://retail.googleapis.com/v2/projects/test-prod/locations/global/catalogs/default_catalog/userEvents:write"
```

你需要使用Google Cloud生成授权令牌以发送事件。

你可以在Retail AI的事件标签页中查看实时事件。

![](../Images/094cc3a159a1943f6c15461b0b489081.png)

Retail AI用户事件，由Muffaddal提供

注意：如果你在导入目录之前或期间记录了用户事件，请 [重新加入任何事件](https://cloud.google.com/retail/docs/manage-user-events#rejoin-event) 这些事件是在目录导入完成之前记录的。

最后，我们只差一步就能创建第一个推荐模型。

## 构建推荐模型

Google Cloud Recommendation AI 支持以下机器学习模型。

![](../Images/448690cbe365527801083f3b00e1b9c3.png)

Google Cloud Recommendation AI 支持的 AI 模型，由 Muffaddal 提供

对于这篇文章，我们将使用 `Recommended For You` 模型。你可以 [点击这里查看有关可用模型的更多细节](https://cloud.google.com/retail/docs/models)。

进入模型选项卡，点击“创建模型”开始配置你的 ML 模型。

![](../Images/ced464572c69732b1838de5d9c775d40.png)

Google Cloud Recommendation AI 中的模型创建，由 Muffaddal 提供

接下来，在模型类型中选择 `Recommended For You` 模型。

![](../Images/598f6fc420bb22d96a1737fca183546a.png)

Google Cloud Recommendation AI 中的 AI 模型选择，由 Muffaddal 提供

我们的目标是提高购买量，因此我们希望我们的 ML 模型优化转换率。选择 `conversion rate (CVR)` 作为模型目标。

![](../Images/0bfdb3e48588937f6476748a0a3d591f.png)

Google Cloud Recommendation AI 中的模型目标，由 Muffaddal 提供

设置调优频率为 `every three month`，并按属性值过滤为 `auto`

![](../Images/0d82870cd623370b78128512ed895b2b.png)

Google Cloud Recommendation AI 模型的调优和选项卡设置，由 Muffaddal 提供

然后点击“创建”按钮。这将开始模型训练。等待一两天，AI 模型将准备就绪。时间取决于训练所需的数据量。

## 服务

一旦 AI 模型创建完成，就该配置服务，以便我们可以调用 AI 模型并获取个性化列表。

进入服务配置，点击顶部的“创建服务”按钮。选择下图所示的推荐设置。

![](../Images/05ac59af229bbc0f8c78378d0abc32aa.png)

Google Cloud Recommendation AI 中的模型服务配置，由 Muffaddal 提供

命名你的服务并点击“继续”。

选择我们创建的 `recommended for you` 模型。这将把我们的模型附加到这个服务配置中。

偏好选项卡是你决定模型行为的地方。我们可以使用自动设置。

![](../Images/ff1e4fec696af54506eaca7c2bcf8413.png)

Google Cloud Recommendation AI 的偏好设置，由 Muffaddal 提供

点击“创建”按钮。注意配置的 ID，因为它将用于调用模型 API。

## 获取推荐

以下是从我们创建的模型获取推荐的 curl 调用。

```py
curl -X POST \
    -H "Authorization: Bearer ya29.aARrdaM9Bm57OTsdsIQAzGT15GwYzZpVfssffknWPNJ8gpKRk6IHSFmGqs1nBpAlaRRg2fQvtJgtUDGsuIc-h-j0RMLkAPy7FjxQ4tQbYZl62ba-4q4oRx-oY2KwYDA-pEQW77SACo2a8hS1zEUZHyyHCO3V-PycSBetJeldjib5VYo969D1PFVF33WSSRLPIP9uBcTW9ABoYthSOioTePlaICbwV1p8dlXesnCH8PdPNuKPxJJI3rzrnIghKXUKSQb4E-mc" \
    -H "Content-Type: application/json; charset=utf-8" \
    -H "X-Goog-User-Project: test-prod"\

    --data  '{

     "pageSize":100,

          "userEvent": {
              "eventType": "home-page-view",
              "visitorId": "123",
              "userInfo": {
                  "userId": "123"
              },
              "experimentIds": "123"
            }
          }' \
https://retail.googleapis.com/v2/projects/test-prod/locations/global/catalogs/default_catalog/placements/<your serving id here>:predict
```

传递准确的用户 ID、服务 ID、项目 ID 和授权令牌，你将获得所提供用户 ID 的个性化列表。

与你的开发人员分享以上代码，他应该能够在你的网站和应用程序上提供个性化的部分。

## 实验

我强烈建议你在首次启动推荐系统时进行 A/B 测试。这将帮助你了解提供个性化体验带来的实际价值。

# 最终思考

这篇文章应该足够帮助你轻松构建一个完全可扩展的推荐系统。但请记住，使用 Google Cloud Recommendation AI 还有更多细节需要了解。

例如，在设置多个模型时，归因令牌至关重要，保持目录更新也很重要，以便向用户提供最新的产品，同时还需考虑 Google Cloud 的定价问题。还有许多其他方面。

[如果你需要我在 Google Cloud Recommendation AI 方面的帮助，随时联系我。](https://www.linkedin.com/in/muffaddal-qutbuddin/)

# 结论

如果你正在寻找一种强大的方法来构建个性化推荐系统，Google Cloud Recommendation AI 是你的解决方案。

Google Cloud Recommendation AI 不仅允许你使用 Google 自有的机器学习模型，还能让你跳过整个推荐系统架构设计的过程。它能迅速为你提供推荐的价值。

及时实现 Google Cloud Recommendation AI，以提高用户参与度、留存率和业务收入。

## 类似阅读

[](/similar-products-you-may-like-237953b3d320?source=post_page-----c0929f0c3080--------------------------------) [## 你可能喜欢的类似产品

### 推荐系统用于在产品详细页面上向用户推荐类似的项目。

towardsdatascience.com](/similar-products-you-may-like-237953b3d320?source=post_page-----c0929f0c3080--------------------------------) [](/rfm-analysis-using-bigquery-ml-bfaa51b83086?source=post_page-----c0929f0c3080--------------------------------) [## 使用 BigQuery ML 进行 RFM 分析

### 使用 RFM 分析在 BigQuery ML 中进行用户细分，并在 Data Studio 中进行可视化。

towardsdatascience.com](/rfm-analysis-using-bigquery-ml-bfaa51b83086?source=post_page-----c0929f0c3080--------------------------------)
