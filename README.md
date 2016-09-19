# Ludicrously fast json library for scala

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/7d84fbd2dcee449885b39c5b1a77c443)](https://www.codacy.com/app/jan-cajthaml/json?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=jancajthaml-scala/json&amp;utm_campaign=Badge_Grade) [![Dependency Status](https://www.versioneye.com/user/projects/57dc1a3f500a3100425c97b3/badge.svg?style=flat-square)](https://www.versioneye.com/user/projects/57dc1a3f500a3100425c97b3) [![Build Status](https://travis-ci.org/jancajthaml-scala/json.svg?branch=master)](https://travis-ci.org/jancajthaml-scala/json)

* casual json serialization ~80ys (~0.08ms)
* casual json deserialization ~60ys (~0.06ms)

## What is does

maps json string to Map and vice versa

## How it works

It decomposes json parsing/loading problems into mapping flat json to key-value pairs and
flat map into "k:v," string. Blocks of {} and instance of Map[String,Any] means recursion.

`some features and some data types are TBD` 

## Implementation tricks:

### jsonloads

(k, v) = chunk separated by `,` splitted by `:`

| leading char of v | resolved as                            | type            | TBD?                                  |
| ----------------- |:--------------------------------------:|:---------------:|:-------------------------------------:|
| t                 | true                                   | Boolean         |                                       |
| f                 | false                                  | Boolean         |                                       |
| 0                 | 0                                      | Integer         |                                       |
| 1                 | _ > Int.MAX ? _.parseLong : _.parseInt | Integer or Long |                                       |
| 2                 | _ > Int.MAX ? _.parseLong : _.parseInt | Integer or Long |                                       |
| 3                 | _ > Int.MAX ? _.parseLong : _.parseInt | Integer or Long |                                       |
| 4                 | _ > Int.MAX ? _.parseLong : _.parseInt | Integer or Long |                                       |
| 5                 | _ > Int.MAX ? _.parseLong : _.parseInt | Integer or Long |                                       |
| 6                 | _ > Int.MAX ? _.parseLong : _.parseInt | Integer or Long |                                       |
| 7                 | _ > Int.MAX ? _.parseLong : _.parseInt | Integer or Long |                                       |
| 8                 | _ > Int.MAX ? _.parseLong : _.parseInt | Integer or Long |                                       |
| 9                 | _ > Int.MAX ? _.parseLong : _.parseInt | Integer or Long |                                       |
| "                 | " + _.toString + "                     | String          |                                       |
| n                 | null                                   | null            |                                       |
| -                 | ---                                    | ---             | YES (negative number)                 |
| u                 | ---                                    | ---             | YES (undefined or unicode)            |
| e                 | ---                                    | ---             | YES (decimal number sci notation)     |
| {                 | ---                                    | ---             | YES (recursion to nested block enter) |
| }                 | ---                                    | ---             | YES (recursion to nested block leave) |

### jsondumps

| v match             | resolved into              |
| ------------------- |:--------------------------:|
| v:String            | String(" + x.toString + ") |
| null                | String(null)               |
| v: Map[String, Any] | recursion                  |
| v:Any               | String(x.toString)         |
