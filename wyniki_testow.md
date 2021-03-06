---
title: "Raport z testów Apache Drill"
author: "Piotr Markwitan"
date: "8 maja 2018"
output: 
  html_document: 
    keep_md: yes
---


#Wykonane zapytania
##1. Zliczenie tweetów w bazie danych:
```
Drill: select count(1) from mongo.tweets2.tweets;
Mongo: db.tweets.count()
```


             1       2       3       4       5       6       7       8       9      10      11      12   średnia          sd
------  ------  ------  ------  ------  ------  ------  ------  ------  ------  ------  ------  ------  --------  ----------
Drill    4.290   0.385   0.145   0.121   0.131   0.122   0.161   0.118   0.112   0.150   0.120   0.113    0.1566   0.0817954
Mongo    0.009   0.008   0.008   0.007   0.008   0.008   0.007   0.008   0.008   0.008   0.008   0.008    0.0079   0.0003162

| wynik |
| ---: |
| 22845650|

##2. Liczba tweetów z podziałem na języki:
```
Drill: select lang, count(1) from mongo.tweets2.tweets group by lang order by 2 desc;
Mongo: db.tweets.aggregate([{$group: {_id:"$lang", liczba:{$sum:1 }}}, {$sort: {liczba: -1}}])
```

               1         2         3         4         5         6         7         8         9        10        11        12    średnia           sd
------  --------  --------  --------  --------  --------  --------  --------  --------  --------  --------  --------  --------  ---------  -----------
Drill    900.100   200.351   116.226   104.087   109.620   110.082   108.632   103.991   102.093   103.813   101.785   100.078   116.0680   29.9541132
Mongo     30.239    29.419    28.641    28.979    27.725    28.443    26.756    27.303    27.869    27.942    29.572    27.181    28.3074    0.8405083

| lang  |  EXPR$1   |
| --- | ---:|
| en    | 14895272  |
| es    | 3176756   |
| de    | 1134877   |
| pl    | 1122601   |
| und   | 760699    |
| fr    | 582216    |
| pt    | 471963    |
| it    | 211914    |
| in    | 127083    |
| tr    | 87524     |
| nl    | 49703     |
| sv    | 31284     |
| ht    | 25389     |
| et    | 24957     |
| fi    | 21792     |
| cy    | 17596     |
| da    | 15726     |
| ro    | 15245     |
| cs    | 12783     |
| no    | 12726     |
| uk    | 8344      |
| eu    | 7907      |
| el    | 5947      |
| sl    | 5702      |
| hu    | 4748      |
| lt    | 3416      |
| vi    | 3060      |
| is    | 2303      |
| lv    | 2020      |
| bg    | 1928      |
| sr    | 1000      |
| ne    | 444       |
| mr    | 379       |
| bn    | 176       |
| hy    | 82        |
| pa    | 54        |
| ka    | 34        |

##3. Średnia długość tweetu z podziałem na języki:
```
Drill: select lang, round(avg(text_length), 3) from mongo.tweets2.tweets group by lang order by 2 desc;
Mongo: db.tweets.aggregate([{$group: {_id:"$lang", srednia:{$avg:"$text_length"}}}, {$sort: {srednia: -1}}])
```

               1         2         3         4         5         6         7         8         9        10        11        12    średnia          sd
------  --------  --------  --------  --------  --------  --------  --------  --------  --------  --------  --------  --------  ---------  ----------
Drill    118.454   116.261   118.033   119.245   114.094   115.103   110.999   113.198   116.059   115.794   113.356   114.648   115.5000   1.7912296
Mongo     38.209    39.184    37.973    36.116    39.135    38.599    38.279    37.126    38.212    37.759    37.738    38.236    38.1266   0.5375089

| lang  |       EXPR$1        |
| --- | ---:|
| mr    | 239.398  |
| pa    | 238.241  |
| ne    | 225.216  |
| bn    | 202.813  |
| ka    | 192.706  |
| uk    | 175.93   |
| sr    | 168.729  |
| el    | 167.04   |
| hy    | 153.293  |
| bg    | 137.874  |
| vi    | 133.468  |
| es    | 127.392  |
| pl    | 125.7    |
| fr    | 124.286  |
| tr    | 122.683  |
| fi    | 122.682  |
| de    | 122.221  |
| en    | 121.13   |
| it    | 118.168  |
| ro    | 113.006  |
| cs    | 110.182  |
| sl    | 108.078  |
| pt    | 106.882  |
| nl    | 106.241  |
| sv    | 103.646  |
| in    | 99.889   |
| da    | 93.155   |
| no    | 90.697   |
| et    | 88.41    |
| eu    | 87.92    |
| und   | 85.568   |
| cy    | 84.777   |
| lv    | 84.645   |
| ht    | 84.521   |
| hu    | 76.362   |
| lt    | 69.515   |
| is    | 67.359   |

##4. Średnia liczba słów z podziałem na języki:
```
Drill: select lang, round(avg(words_count), 3) from mongo.tweets2.tweets group by lang order by 2 desc;
Mongo: db.tweets.aggregate([{$group: {_id:"$lang", srednia:{$avg:"$words_count"}}}, {$sort: {srednia: -1}}])
```

               1         2         3         4         5         6         7         8         9        10        11        12    średnia          sd
------  --------  --------  --------  --------  --------  --------  --------  --------  --------  --------  --------  --------  ---------  ----------
Drill    115.053   117.908   116.332   121.041   126.494   116.745   117.474   114.984   114.382   122.577   116.243   117.162   117.5519   2.4585773
Mongo     39.404    39.455    38.761    38.063    40.665    40.969    40.227    38.450    38.152    39.529    37.785    38.320    39.1026   0.8970112

| lang  | EXPR$1  |
| --- | ---: |
| pa    | 65.315  |
| mr    | 63.485  |
| ne    | 60.858  |
| bn    | 56.756  |
| ka    | 56.353  |
| uk    | 52.6    |
| sr    | 51.023  |
| el    | 45.763  |
| bg    | 39.407  |
| hy    | 37.512  |
| vi    | 27.151  |
| es    | 21.255  |
| fr    | 20.889  |
| en    | 19.705  |
| pl    | 19.575  |
| cs    | 19.417  |
| tr    | 19.139  |
| de    | 18.371  |
| it    | 18.181  |
| pt    | 17.982  |
| sv    | 17.832  |
| ro    | 17.319  |
| fi    | 17.312  |
| sl    | 16.451  |
| nl    | 16.325  |
| in    | 15.069  |
| da    | 14.783  |
| no    | 14.089  |
| und   | 13.145  |
| et    | 13.138  |
| lv    | 12.813  |
| eu    | 12.776  |
| ht    | 12.479  |
| hu    | 12.222  |
| cy    | 12.054  |
| is    | 10.304  |
| lt    | 9.889   |

##5. Kostka "ROLLUP"
Apache Drill nie wspiera zapytań ROLLUP i CUBE
```
Drill: 
create table pol_kostka as
select `year`, `month`, `day`, `hour`, liczba from (
select `year`, `month`, `day`, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `year`, `month`, `day`, `hour`
union
select `year`, `month`, `day`, -1, count(1) as liczba from mongo.tweets2.tweets group by `year`, `month`, `day`
union
select `year`, `month`, -1, -1, count(1) as liczba from mongo.tweets2.tweets group by `year`, `month`
union
select `year`, -1, -1, -1, count(1) as liczba from mongo.tweets2.tweets group by `year`
union
select -1, -1, -1, -1, count(1) as liczba from mongo.tweets2.tweets);
```
1,058 rows selected (1796.412 seconds)
+-----------+----------------------------+
| Fragment  | Number of records written  |
+-----------+----------------------------+
| 0_0       | 1058                       |
+-----------+----------------------------+
1 row selected (1773.775 seconds)


##6. Kostka "CUBE"
Apache Drill nie wspiera zapytań ROLLUP i CUBE
```
Drill:
select `year`, `month`, `day`, `hour`, liczba from (
select `year`, `month`, `day`, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `year`, `month`, `day`, `hour`
union select `year`, `month`, `day`, -1, count(1) as liczba from mongo.tweets2.tweets group by `year`, `month`, `day`
union select `year`, `month`, -1, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `year`, `month`, `hour`
union select `year`, `month`, -1, -1, count(1) as liczba from mongo.tweets2.tweets group by `year`, `month`
union select `year`, -1, `day`, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `year`, `day`, `hour`
union select `year`, -1, `day`, -1, count(1) as liczba from mongo.tweets2.tweets group by `year`, `day`
union select `year`, -1, -1, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `year`, `hour`
union select `year`, -1, -1, -1, count(1) as liczba from mongo.tweets2.tweets group by `year`
union select -1, `month`, `day`, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `month`, `day`, `hour`
union select -1, `month`, `day`, -1, count(1) as liczba from mongo.tweets2.tweets group by `month`, `day`
union select -1, `month`, -1, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `month`, `hour`
union select -1, `month`, -1, -1, count(1) as liczba from mongo.tweets2.tweets group by `month`
union select -1, -1, `day`, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `day`, `hour`
union select -1, -1, `day`, -1, count(1) as liczba from mongo.tweets2.tweets group by `day`
union select -1, -1, -1, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `hour`
union select -1, -1, -1, -1, count(1) as liczba from mongo.tweets2.tweets
)
order by case when `year` = -1 then 10000 else `year`end,
case when `month` = -1 then 10000 else `month`end,
case when `day` = -1 then 10000 else `day`end, 
case when `hour` = -1 then 10000 else `hour`end;
```

Powyższe zapytanie nie wykonało się w czasie 2 godzin i połączenie z serwerem zostało zamknięte.
Zmaterializowano kostkę:
```
use dfs.tmp;

create table kostka_1 as select `year`, `month`, `day`, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `year`, `month`, `day`, `hour`;
create table kostka_2 as select `year`, `month`, `day`, -1, count(1) as liczba from mongo.tweets2.tweets group by `year`, `month`, `day`;
create table kostka_3 as select `year`, `month`, -1, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `year`, `month`, `hour`;
create table kostka_4 as select `year`, `month`, -1, -1, count(1) as liczba from mongo.tweets2.tweets group by `year`, `month`;
create table kostka_5 as select `year`, -1, `day`, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `year`, `day`, `hour`;
create table kostka_6 as select `year`, -1, `day`, -1, count(1) as liczba from mongo.tweets2.tweets group by `year`, `day`;
create table kostka_7 as select `year`, -1, -1, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `year`, `hour`;
create table kostka_8 as select `year`, -1, -1, -1, count(1) as liczba from mongo.tweets2.tweets group by `year`;
create table kostka_9 as select -1, `month`, `day`, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `month`, `day`, `hour`;
create table kostka_10 as select -1, `month`, `day`, -1, count(1) as liczba from mongo.tweets2.tweets group by `month`, `day`;
create table kostka_11 as select -1, `month`, -1, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `month`, `hour`;
create table kostka_12 as select -1, `month`, -1, -1, count(1) as liczba from mongo.tweets2.tweets group by `month`;
create table kostka_13 as select -1, -1, `day`, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `day`, `hour`;
create table kostka_14 as select -1, -1, `day`, -1, count(1) as liczba from mongo.tweets2.tweets group by `day`;
create table kostka_15 as select -1, -1, -1, `hour`, count(1) as liczba from mongo.tweets2.tweets group by `hour`;
create table kostka_16 as select -1, -1, -1, -1, count(1) as liczba from mongo.tweets2.tweets;

create table kostka as 
select `year`, `month`, `day`, `hour`, liczba from kostka_1
union select `year`, `month`, `day`, `hour`, liczba from kostka_2
union select `year`, `month`, `day`, `hour`, liczba from kostka_3
union select `year`, `month`, `day`, `hour`, liczba from kostka_4
union select `year`, `month`, `day`, `hour`, liczba from kostka_5
union select `year`, `month`, `day`, `hour`, liczba from kostka_6
union select `year`, `month`, `day`, `hour`, liczba from kostka_7
union select `year`, `month`, `day`, `hour`, liczba from kostka_8
union select `year`, `month`, `day`, `hour`, liczba from kostka_9
union select `year`, `month`, `day`, `hour`, liczba from kostka_10
union select `year`, `month`, `day`, `hour`, liczba from kostka_11
union select `year`, `month`, `day`, `hour`, liczba from kostka_12
union select `year`, `month`, `day`, `hour`, liczba from kostka_13
union select `year`, `month`, `day`, `hour`, liczba from kostka_14
union select `year`, `month`, `day`, `hour`, liczba from kostka_15
union select `year`, `month`, `day`, `hour`, liczba from kostka_16;
```
