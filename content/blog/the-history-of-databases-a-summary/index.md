---
title: "The history of databases: a quick summary"
date: "2019-11-25T12:04:10.000Z"
---

## Databases

The appearence of the term "database" coincided with the availability of [direct-access storage](https://en.wikipedia.org/wiki/Direct-access_storage_device) (mid-60's). The term represented a contrast with the tap-based systems of the past, allowing shared interactive use rather than batch processing.

## Network databases

General purpose database systems emerged during that time.

[CODASYL](https://en.wikipedia.org/wiki/CODASYL): primary key, navigation relationships, or scanning.

[IBM IMS (Information Mamagement System)](https://en.wikipedia.org/wiki/IBM_Information_Management_System), running on the [System/360](https://en.wikipedia.org/wiki/IBM_System/360), initially developed for [the Apollo program](https://en.wikipedia.org/wiki/Apollo_program). Strictly hierarchical.

Both IMS and CODASYL classify as **network databases**.

## 1970's relational databases

[Edgar Codd](https://en.wikipedia.org/wiki/Edgar_F._Codd), at the time working for IBM on storage, became frustrated by the limitations imposed by network databases.

Initial idea: tables of fixed-length records. Different table for different types of identity.

The relational part comes from the capability of entities can refer other entitities.

A relational database can express both hierarchical or navigational models, as well as its native tabular model.

To query it, the author of the relational algebra proposed a set-oriented languaged, which would later spawn SQL.

Based on Codd's paper, [INGRES](https://en.wikipedia.org/wiki/Ingres_(database)) was created by two people at Berkeley ([Eugene Wong](https://en.wikipedia.org/wiki/Eugene_Wong) and [Micheal Stonebraker](https://en.wikipedia.org/wiki/Michael_Stonebraker)).

## Late 1970's: SQL DBMSs

IBM started working on an implementation of Codd's paper named [System R](https://en.wikipedia.org/wiki/IBM_System_R), first single-table and then later, in the late 70's, on a multi-table implementation.

Later, multi-user versions were developed and were tested by customers in 1978 and 1979, by which time a standardized language named SQL had been added.

The success of these experiments lead IBM to create a true production of System R known as SQL/DS and later, Database 2 ([DB2](https://en.wikipedia.org/wiki/IBM_Db2_Family)).

By around the same time, [Larry Ellison](https://en.wikipedia.org/wiki/Larry_Ellison) developed the [Oracle database](https://en.wikipedia.org/wiki/Oracle_Database), based off IBM's papers on System R, and released Oracle version 2 in 1979.

Stonebraker took his learnings from INGRES and started a project named Postgres (now known as [PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL)), since then used in many mission-critical applications.

## 1980's on the desktop

Spreadsheet software (like [Lotus 123](https://en.wikipedia.org/wiki/Lotus_1-2-3)) and database software (like [dBASE](https://en.wikipedia.org/wiki/DBase)) appeared for desktop computers. dBASE became a huge success during the 80's and 90's.

## Document databases

Do not require fixed schemas, avoid join operations and are designed to scale horizontally.

## Now

In recent years, there has been a strong for massively distributed databases with high partition tolerance. But the CAP theorem states that, in the presence of a network partition, you can either provide consistency or availability.

For that, many new databases are using what is called eventual consistency, guaranteeding availability during network partitions.