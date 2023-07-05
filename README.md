# Marketing Common Data Modelling Challenge

This repository contains the solution for the "Marketing Common Data Modelling Challenge." In this challenge, I reviewed ad performance reports from various platforms such as Bing, Facebook, TikTok, and Twitter. The objective was to combine and transform these reports into a required data structure and load them into Google BigQuery as a Data Warehouse (DWH). Finally, I explored the transformed data using Looker Studio and compared the generated visuals with a given dashboard, iteratively adjusting the data sources until achieving the desired final data.

## Challenge Overview

The challenge involved the following steps:

1. Reviewing ad performance reports from Bing, Facebook, TikTok, and Twitter.
2. Writing an SQL script in DBT Cloud to combine and transform the reports into the required data types and structure.
3. Utilizing Google BigQuery as the Data Warehouse to load the transformed data.
4. Exploring the final data using Looker Studio.
5. Comparing the generated visuals with a given dashboard.
6. Iteratively adjusting the data sources to align with the desired final data.

## Structure

The `seeds/` directory contains the source code for the project, organized into subdirectories
- `src_ads_bing_all_data.csv`
- `src_ads_creative_facebook_all_data.csv`
- `src_ads_tiktok_ads_all_data.csv`
- `src_promoted_tweets_twitter_all_data.csv`

`dbt/` includes the SQL scripts used for transforming the ad performance reports from each platform and combining them into a unified dataset 

`looker/` contains written fields and a dashboard with required visuals in Looker Studio.

## Instructions

To reproduce the solution or further explore the project, follow these steps:

1. Clone the [repository](https://github.com/abdr95/dbt_mcdm_challenge).
2. Connect to DBT Cloud and create a [project](https://cloud.getdbt.com/develop/180784/projects/265142).
3. Import the SQL scripts in dbt/models/ to your DBT project.

<pre>
     {{config(
    materialized='view')}}

   with
    bing as (
        select
            ad_id as ad_id,
            null as add_to_cart,
            adset_id as adset_id,
            campaign_id as campaign_id,
            channel as channel,
            clicks as clicks,
            null as comments,
            null as creative_id,
            date as date,
            null as engagements,
            imps as impressions,
            null as installs,
            null as likes,
            null as link_clicks,
            null as placement_id,
            null as post_click_conversions,
            null as post_view_conversions,
            null as posts,
            null as purchase,
            null as registrations,
            revenue as revenue,
            null as shares,
            spend as spend,
            conv as total_conversions,
            null as video_views
        from {{ ref("src_ads_bing_all_data") }}
    ),

    facebook as (
        select
            ad_id as ad_id,
            add_to_cart as add_to_cart,
            adset_id as adset_id,
            campaign_id as campaign_id,
            channel as channel,
            clicks as clicks,
            comments as comments,
            creative_id as creative_id,
            date as date,
            (views_2+shares_2+clicks_2) as engagements,
            impressions as impressions,
            mobile_app_install as installs,
            likes as likes,
            inline_link_clicks as link_clicks,
            null as placement_id,
            null as post_click_conversions,
            null as post_view_conversions,
            null as posts,
            purchase as purchase,
            complete_registration as registrations,
            null as revenue,
            shares as shares,
            spend as spend,
            purchase_2 as total_conversions,
            views as video_views
        from {{ ref("src_ads_creative_facebook_all_data") }}
    ),

    tiktok as (
        select 
            ad_id as ad_id,
            add_to_cart as add_to_cart,
            adgroup_id as adset_id,
            campaign_id as campaign_id,
            channel as channel,
            clicks as clicks,
            null as comments,
            null as creative_id,
            date as date,
            null as engagements,
            impressions as impressions,
            rt_installs as installs,
            null as likes, 
            null as link_clicks,
            null as placement_id,
            null as post_click_conversions,
            null as post_view_conversions,
            null as posts,
            purchase as purchase,
            registrations as registrations,
            null as revenue,
            null as shares,
            spend as spend,
            conversions as total_conversions,
            video_views as video_views
        from {{ref("src_ads_tiktok_ads_all_data")}}
    ),

    twitter as (
        select 
            null as ad_id,
            null as add_to_cart,
            null as adset_id,
            campaign_id as campaign_id,
            channel as channel,
            clicks as clicks,
            comments as comments,
            null as creative_id,
            date as date,
            engagements as engagements,
            impressions as impressions,
            null as installs,
            likes as likes,
            url_clicks as link_clicks,
            null as placement_id,
            null as post_click_conversions,
            null as post_view_conversions,
            null as posts,
            null as purchase,
            null as registrations,
            null as revenue,
            null as shares,
            spend as spend,
            null as total_conversions,
            video_total_views as video_views
        from {{ref("src_promoted_tweets_twitter_all_data")}}
    ),

    paid_ads__basic_performance as (
        select bing.* from bing
        UNION ALL
        select facebook.* from facebook
        UNION ALL
        select tiktok.* from tiktok
        UNION ALL
        select twitter.* from twitter)

   select 
    cast(ad_id as string) as ad_id,
    add_to_cart,
    cast(adset_id as string) as adset_id,
    cast(campaign_id as string) as campaign_id,
    channel,
    clicks,
    comments,
    cast(creative_id as string) as creative_id,
    date,
    engagements,
    impressions,
    installs,
    likes,
    link_clicks,
    cast(placement_id as string) as placement_id,
    post_click_conversions,
    post_view_conversions,
    posts,
    purchase,
    registrations,
    revenue,
    shares,
    spend,
    total_conversions,
    video_views
   from paid_ads__basic_performance
</pre>
   
5. Configure DBT Cloud to connect to your Google BigQuery account.
6. Run the DBT project to execute the transformations and load the transformed data into [Google BigQuery](https://console.cloud.google.com/bigquery?project=analytics-391807&ws=!1m5!1m4!4m3!1sanalytics-391807!2sdbt_abdr95!3smcdm_paid_ads_basic_performance).
7. Connect to Looker Studio and create a new project.
8. Customize the Looker visuals and dashboards as needed to match the required final data.
9. Compare the generated visuals with the provided dashboard and iteratively adjust the data sources until achieving the desired final data: [Link for Looker Studio](https://lookerstudio.google.com/s/gVvM17TZ-iI)

## Dependencies
The following tools and services were used in this project:

- DBT Cloud
- Google BigQuery
- Looker Studio
Make sure you have the necessary access and permissions to these tools before attempting to run or modify the project.
