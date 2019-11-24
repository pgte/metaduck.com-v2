---
title: 'Local-first data for web apps: looking around'
date: 2019-07-27T15:04:10.000Z
---

* Archaeology
  * Batch processing systems - Punching cards, don't keep state between executions, mainly used for arithmetic operations or digital manipulation.
  * Time-sharing - multi-user systems. For the first time, multiple users running multiple processes that can access the same resources.
  * PC: the decay of time-sharing
  * But there's still the database server (OLAP), serving businesses through LANs or WANs
  * Central point of failure
  * Requires an online connection
  * The cloud brings back time-sharing
  * Put services through enormous work loads

## Databases

The appearence of the term "database" coincided with the availability of direct-access storage (mid-60's). The term represented a contrast with the tap-based systems of the past, allowing shared interactive use ratrher than batch processing.

## Network databases

General purpose database systems emerged during that time.

CODASYL: primary key, navigation relationships, or scanning.

IBM IMS (Information Mamagement System), running on the System/360, initially developed for the Apollo program. Strictly hierarchical.

Both IMS and CODASYL classify as **network databases**.

## 1970's relational databases

Edgar Codd, at the time working for IBM on storage, became frustrated by the limitations imposed by network databases.

Initial idea: tables of fixed-length records. Different table for different types of identity.

The relational part comes from the capability of entities can refer other entitities.

A relational database can express both hierarchical or navigational models, as well as its native tabular model.

To query it, the author of the relational algebra proposed a set-oriented languaged, which would later spawn SQL.

Based on Codd's paper, INGRES was created by two people at Berkeley (Eugene Wong and Micheal Stonebraker).

## Late 1970's: SQL DBMSs

IBM started working on an implementation of Codd's paper named System R, first single-table and then later, in the late 70's, on a multi-table implementation.

Later, multi-user versions were developed and were tested by customers in 1978 and 1979, by which time a standardized language named SQL had been added.

The success of these experiments lead IBM to create a true production of System R known as SQL/DS and later, Database 2 (DB2).

By around the same time, Larry Ellison developed the Oracle database, based off IBM's papers on System R, and released Oracle version 2 in 1979.

Stonebraker took his learnings from INGRES and started a project named Postgres (now known as PostgreSQL, since then used in many mission-critical applications.

## 1980's on the desktop

Spreadsheet software (like Lotus 123) and database software (like dBASE) appeared for desktop computers. dBASE became a huge success during the 80's and 90's.

## Document databases

Do not required fixed schemas, avoid join operations and are designed to scale horizontally.

## Now

In recent years, there has been a strong for massively distributed databases with high partition tolerance. But the CAP theorem states that, in the presence of a network partition, you can either provide consistency or availability.

For that, many new databases are using what is called eventual consistency, guaranteeding availability during network partitions.



# Archeology

In the beginning of electronic digital information systems, computers were used to make computations

In the beginning of information systems there were no networks. There was only a single user using one computer, and the data was read, changed and saved in a storage peripheral. Then came the time-sharing devices where multiple users could each have a separate session that acted like if each owned one computer, while in reality they were sharing resources. In these systems, multiple users shared the same storage media. If two users wanted to read and manipulate the same data, they had to synchronize between them. So the things that first resembled databases started appearing. They allowed different users access the same data, and somehow guaranteed that the data would not get corrupted and stay coherent under concurrent manipulation by different users.

Then remote terminals were invented. By using a very simple and low-bandwidth network, users could open text-based terminal sessions into the central computer, where the application actually ran. When wide area access was required, these terminals would somehow connect to the central computer (text-based terminals require not much bandwidth) and run a terminal session there. At this time, the data being exchanged was only the keyboard inputs in one direction and textual data in the other). The data itself was still safely kept inside the hard drive of a computer at some data cetnter.

Then local networks started to be a reality, feasilble to install in an office, building or even connect the whole campus. Now, instead of terminals, we could have smarter computers where the application could be run. To access shared data, though, that application had to connect to a service that would serve the data. These services were the primordial databases, where, through the local network, different users running different applications could, direct or indirectly, inquire and mutate a data set.

These databases started becoming a very important part of any IT infra-structure. They would have to provide secure, safe and robust access to data, guaranteeing that it would not be lost and that users could safely manipulate it concurrently.

This model was then extrapolated into the wide area networks and, later, the whole internet. One single database server would serve all the requests coming from all the users of a given data set. But the sheer scale experienced by some internet applications would mean that this model could not be sustained for long.

To make the workload on a given data set sustainable, that work would have to start being spread through different computers. Multiple CPUs operting on the same data via a high-speed network-attached storage started appearing. These CPUs would then have to coordinate between themselves to guarantee that the data was not currupted by concurrent manipulation. But then the bottleneck became the peripheral itself.



---------


At this time there was a huge difference between the capacity of the central server and the user computer. The user computer had much less computational power and storage than the server, so the data had only one place to get stored.

This was convinent for several reasons: it was simpler to reason about persistence, concurrency and security. Having only one source of truth, where all the operations were funneled through, you didn't have to think much about concurrency. If two users were editing the same

## Derivative: the concurrency model



## Derivative: the security model


# Local-first data

For the last few years of my careeer I've been exploring different software architecture decisions that can support web applications that can work offline. In the last few years some advances have been made in allowing web applications to work well even in the absense of a network connection, or at least in the presence of a degraded network connection. For instance, service workers can allow a web app to function more like a native app, with a process of installation where the app is downloaded and then cached locally. This can be made not only for downloading app code, but also for any web asset or any other content served through an HTTP request that can be stored locally.

This is great news for web applications. Instead of having to hit the network to download code, images or HTML on each page request, it only has to perform one request per asset, cache them, and then in the future, every time the application starts or the application needs one of these assets, these requests can be served locally, directly from the cache.

So, if an application needs an image, a JS file, a CSS file or any other file, but it doesn't have connectivity to the origin server, the service worker can instead serve that asset transparently. But what about application data? Typically in a web app, data is read and manipulated by making an HTTP request to a service. When the application finds itself with degraded or non-existent connectivity to this service, they simply cannot read or write any of that data. What can a service worker do about this? Well, if some data has been fetched in the past using HTTP, and if it got cached locally, and the cache is still valid, this can still work. But if that hasn't happened yet, the application is out of luck.

What about operations, like changing or creating a piece of data? In this case, there's not much a service worker can do. Sure, it could queue the requests for delivering later when connectivity is up, but there's no way that the service worker can guess what would be the response from the service. The request may be invalid, and perhaps only the service could know that.

But what could an app do in this case when it's offline, except fail? It turns out that you have some options.

## Local-first data

In local-first data, all data is stored locally to the application. If an application needs to fetch data, it gets that data from this local storage. Since there's no network involved, it shol be very fast to get that data from storage into the application memory.

Operations that modify the data, also do it locally, affecting only local storage. Again, and since there is no network involved, these mutations should also be very quick to perform. This mutation can later then be propagated to a remote service or peer, but only if and when there is network connectivity to it.

## How does replication happen?

a) State replication

b) Operation replication

c) A combination of both

This is the most effective solution. When devices are starting out or are too far behind, it's useful to use state replication to replicate the state of the entire database at a given point. Once this replication is done, it can proceed from that point using solely operation replication. This uses the best of both worlds, yielding faster convergence times for both the cases where the device is fresh or far behind, and for replicating operations as they happen with a minimum latency and bandwidth.

## This means eventual consistency, right?

But it's not a requirement to have one or more remote servers or devices that this data can be replicated to and from. But if you want that data to live somewhere else other than the device, or if you want to trigger some operation on a remote service, you need to get that data there.

To achieve that you'll have some mechanism of replication of that data. Local mutations in data then have to be replicated into the remote replica. Also, changes in the remote replica have to be replicated to the local replica.

If you think about it, there's going to be points in time where at least one the replicas of this one database is in a different state than all the others.

This means that the same piece of data can now be changed in at least two parts at about the same time. What happens when there are conflicts? Hopefully, the application or the service can eventually detect that and somehow resolve that conflict. This could be done by a) deterministically selecting one of the versions of the data,  b) asking the user to resolve the conflict or c) somehow automatically merging both changes.

> Multi-writer conflict resolution is a complex problem that can be discussed at length, and I plan to keep discussing this further on in this in future articles.

## Persistence, replication and durability



## What about data reactivity and replication?


## What about authentication?

## Concerns

### Device security

Access to data in mobile devices. Data exfiltration. Multiple uncontrolled devices. An IT administrator / CSO 's worst nightmare.


## Exploring solutions

### IPFS

### Ambients by Haja Networks

### CouchDB and PouchDB

Source: http://docs.couchdb.org/en/stable/replication/intro.html

Replication: from source to destination

Transient and persistent replication

Master-master replication: two replication, one in each direction

Filtering replication through selector objects (specific selector syntax) or filter functions (defined in a Javascript function that gets the document returns `true` or `false`, indicating whether that document should or not be replicated).


PouchDB: migrating data to clients!

#### Conflict model

In the presence of a conflict, CouchDB picks one "winner", but still keeps the looser around. This choice is deterministic, so that the same version is picked between replicas and replicas are kept consistent after a conflict has occurred.

CouchDB keeps a revision tree with all the revisions of a document that are known. The way to resolve a conflict is to delete the merged leaf nodes along the other branches.

#### Working with conflicting documents

In CouchDB a document not only contains a unique identifier for that document in that database (the `_id` field), but also contains a unique revision in the `_rev` field. This field changes every time the document is updated, and by default CouchDB keeps all the revisions for all the documents around (unless you prune the database). This may come in handy if you need to resolve conflicts.

When you get a document from the database, by default you get no information about conflicts. You only see the winner document.  But if can request the conflicts, and the document will contain an additional `_conflicts` field that contains the revision IDs of all the documents that conflicted with this one. You can then fetch each document revision individually.


#### Merging

Merging is an application-specific function.

If you need diffing with previous versions, it may help to find the common ancestor version. You can do this by asking CouchDB for the revisions information of a particular document.