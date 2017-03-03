---
layout: post
title: "Postgres on Mac Os X"
date: 2017-03-02
categories: postgres macosx
tags: postgres-admin
---
Install the Mac App for Postgres here: <http://postgresapp.com/> (UNTIL STEP 3 INCLUDED!!!)

```sql
createuser daniele -d -r -s -P
createdb myDbForFun -U daniele
dropdb myDbForFun
dropuser daniele 
```

In case you need `geometry` like:

```sql
CREATE TABLE myregions (
  name      VARCHAR(8)              NOT NULL,
  geometry  GEOMETRY (MULTIPOLYGON) NOT NULL,
  lod       INTEGER                 NOT NULL,
  id        SERIAL                  NOT NULL  PRIMARY KEY
);
```
use the follwoing command first:

```sql
CREATE EXTENSION postgis;
```

Check first if the table has the right owner with `\dt` and if not:

```sql
ALTER TABLE spatial_ref_sys OWNER TO daniele;
```
