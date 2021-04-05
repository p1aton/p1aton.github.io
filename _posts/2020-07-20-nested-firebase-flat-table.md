---
layout: post
title:  "GBQ & Firebase convert nested to flat"
crawlertitle: "This is our first post"
summary: "Таблицы с вложенной структурой"
date:   2021-02-01 23:09:47 +0700
categories: posts
tags: ['Веб аналитика']
#author: Felipe
---


Запрос для преобразования данных с вложенной структурой в плоский датасет.



{% highlight sql %}

#standardSQL
SELECT event_timestamp, user_pseudo_id, geo.country, geo.region, geo.city, event_name, event_previous_timestamp, user_first_touch_timestamp, max(param.value.string_value) as firebase_screen_class, max(param.value.int_value) as ga_session_number, device.category, device.mobile_brand_name, device.mobile_model_name, device.mobile_os_hardware_model, device.operating_system, device.operating_system_version, device.advertising_id, device.language, device.is_limited_ad_tracking, device.time_zone_offset_seconds, app_info.id, app_info.version, app_info.install_store, app_info.firebase_app_id, app_info.install_source, traffic_source.name, traffic_source.medium, traffic_source.source, platform, stream_id
FROM `firebase-public-project.analytics_153293282.events_*`,
 UNNEST (event_params) AS param
WHERE param.key = "firebase_screen_class"
OR param.key = "ga_session_number"
GROUP BY event_timestamp, user_pseudo_id, geo.country, geo.region, geo.city, event_name, event_previous_timestamp, device.mobile_model_name, device.advertising_id, user_first_touch_timestamp, device.category, device.mobile_brand_name, device.mobile_model_name, device.mobile_os_hardware_model, device.operating_system, device.operating_system_version, device.advertising_id, device.language, device.is_limited_ad_tracking, device.time_zone_offset_seconds, app_info.id, app_info.version, app_info.install_store, app_info.firebase_app_id, app_info.install_source, traffic_source.name, traffic_source.medium, traffic_source.source, platform, stream_id
ORDER BY event_timestamp
LIMIT 1000;


{% endhighlight %}



