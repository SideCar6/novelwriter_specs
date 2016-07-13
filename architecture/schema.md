# NovelWriter Schema

This is the beginning of a place to start thinking on a low level about what
data will be stored where, and how it will be related. It will be organized by
service; obviously all services can/will have access to whatever data they need
from other services.

## Authentication

SQL storage:

```
table users
id                        INT
email                     VARCHAR
name                      VARCHAR
password                  VARCHAR
password_salt             VARCHAR
password_recovery_token   VARCHAR
```

## Writing/Editor

NoSQL storage (probably?):

```json
{
  "id": "Integer",
  "user_id": "Integer",
  "title": "String",
  "parent_id": "Integer",
  "content": "Text",
  "plot_id": "Integer"
}
```

## Social Engine

This is a big question. Could be graph database, could be SQL. Seems like it
probably shouldn't be NoSQL, because presumably we want to do a lot of
analytics on it. Assuming SQL:


Projects are the top-level entity under a user's account, and hold plots,
characters, etc.

```
projects

id
user_id
title
```

There will be a collection of plot points in a project, which define the plot
of the text.

```
plot_points

project_id
parent_id
summary
```

Characters appear in projects.

```
characters

user_id
name
```

Character projects joins characters and projects together. This is because a
given character may appear in multiple projects.

```
character_projects

character_id
project_id
```

Settings are locations for a plot.

```
settings

name
description
```

Project settings joins projects and settings together. This is because a given
setting may be appear in multiple projects/plots.

```
project_settings

project_id
setting_id
```

Social attributes describe characters, settings, and other pieces of fictional
worlds. Because various characters will have different attributes depending on
story context, by breaking them out into their own table we can keep things
snappy and normalized while allowing some schema flexibility.

```
social_attributes

type
type_id
key_name
value
```
