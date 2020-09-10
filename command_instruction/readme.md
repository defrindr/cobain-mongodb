# PINTAR.Mongo

## Table of Content

- [PINTAR.Mongo](#pintarmongo)
    - [Table of Content](#table-of-content)
    - [Run Mongo shell](#run-mongo-shell)
        - [Mongod](#mongod)
            - [Start as daemon](#start-as-daemon)
    - [Dump dan Restore Database](#dump-dan-restore-database)
    - [Basic Command](#basic-command)
        - [Help Command](#help-command)
    - [Collection](#collection)
    - [Find document](#find-document)
        - [Show specific field](#show-specific-field)
        - [Hide field](#hide-field)
    - [Insert document](#insert-document)
        - [1 document](#1-document)
        - [Insert many document](#insert-many-document)
    - [Update document](#update-document)
    - [Where Clause](#where-clause)
        - [LIKE query](#like-query)
        - [IN query](#in-query)
        - [Nested column](#nested-column)
    - [Order Data](#order-data)
        - [Sort by Ascending](#sort-by-ascending)
        - [Sort by Descending](#sort-by-descending)
    - [Distinct](#distinct)
    - [JOIN query](#join-query)
    - [Concat String](#concat-string)
    - [Parse Array](#parse-array)
    - [Push Array ( Grouping )](#push-array--grouping-)
    - [Advanced query](#advanced-query)


## Run Mongo shell

0. Buka Terminal

1. pergi ke directory database, atau buat directory baru. <br>
   example: 'cd ~/Documents/Learn/mongodb/database'

2. Jalankan perintah ```mongod```

3. Buka Terminal Baru

4. Jalankan perintah ```mongo```

> Catatan :  
> Jika mengalami <i>error</i> ```sock``` pada tahap 2, hapus file ```sock``` yang berada di ```/tmp```


### Mongod

```
cd [path]
mongod
```

Atau

```
mongod --dbpath [path] --port [port]
```

Contoh : 
```
mongod --dbpath ~/Documents/Learn/mongodb/database --port 27017
```

#### Start as daemon

```
mongod --fork  --logpath [path_to_mongod_log]
```

## Dump dan Restore Database

DUMP :
```
mongodump --archive="[archive_name]" --db="[db_name]"
```

RESTORE :
```
mongorestore --archive="[archive_name]" --nsFrom="[old_db_name].*" --nsTo="[new_db_name].*" 
```

> Catatan :
> Hanya bekerja pada mongo 4.2+




## Basic Command

Menampilkan semua database :
```
show dbs;
```

Menggunakan database :
```
use [database_name];
```

Menampilkan daftar Koleksi :
```
show collections
```

Menghapus Database :
```
use [database_name]

db.dropDatabase()
```

Membersihkan Layar :
```
cls
```


### Help Command

Instruksi dasar :
```
help
```

Instruksi berkaitan dengan database :
```
db.help()
```

Instruksi berkaitan dengan collection :
```
db.[collection_name].help()
```

Instruksi berkaitan dengan pencarian data :
```
db.[collection_name].find().help()
```


## Collection

Menambahkan Koleksi :
```
db.createCollection([collection_name])
```

Mengubah nama koleksi :
```
db.[collection_name].renameCollection([new_collection_name])
```

Menghapus Koleksi :
```
db.[collection_name].drop()
```

## Find document

Menampilkan Semua dokumen :
```
db.[collection_name].find()
```

Menampilkan 1 dokumen :
```
db.[collection_name].findOne() 
```

Mempercantik hasil <i>output</i> :
```
db.[collection_name].find().pretty();
```

### Show specific field
```
db.[collection_name].find({}, {
    field_1: 1,
    field_2: 1,
    field_3: 1
}).pretty() 
```
Versi SQL :
```sql
select _id, field_1, field_2, field_3 from [collection_name];
```

### Hide field
```
db.[collection_name].find({}, {
    _id: 0,
    field_1: 1,
    field_2: 1,
    field_3: 0
}).pretty()
```
Versi SQL :
```sql
select field_1, field_2 from students;
```

## Insert document

### 1 document
```
db.[collection_name].insert(
	{
		nama: 'Defri Indra Mahardika',
		usia: 17
	}
)
```

### Insert many document
```
db.[collection_name].insertMany([
	{
		date: 1,
		month: "January",
		description: "New Year"
	},
	{
		date: 1,
		month: "Juny",
		description: "Birth of Pancasila"
	},
	{
		date: 25,
		month: "December",
		description: "Natal"
	},
])
```

Versi SQL :
```sql
insert into `[collection_name]`(date, month, description) values
( 1, "January", "New Year"),
( 1, "Juny", "Birth of Pancasila"),
( 25, "December", "Natal");
```

## Update document

Mengubah dokumen spesifik :
```
db.[collection_name].update({
    field_name: field_value
}, {
	$set: {
		new_field_1: new_value_1,
		new_field_2: new_value_2
	}
})
```

Mengubah semua dokumen :
```
db.[collection_name].update({}, {
	$set: {
		new_field_1: new_value_1,
		new_field_2: new_value_2
	}
},
{
	upsert: false,
	multi: true
})
```

## Where Clause

### LIKE query

```
db.[collection_name].find({ username: /.*\-chan/ })
```
Versi SQL :
```sql
select * from [collection_name] where username like '%-kun';
```

<hr>

```
db.[collection_name].find({ username: /Mrs\..*/ })
```

Versi SQL :
```sql
select * from [collection_name] where username like 'Mrs.%';
```

<hr>

```
db.[collection_name].find({ username: /.*Jean.*/ });
```

Versi SQL :
```sql
select * from [collection_name] where username like '%Jean%';
```

### IN query
```
db.[collection_name].find({
	jenis_kelamin: {
		$in : ["P","L"]
	}
}).pretty()
```

Versi SQL :
```sql
select * from [collection_name] where jenis_kelamin in ("P", "L");
```

### Nested column

```
db.[collection_name].find({ 
    'column_1.column_2': "Ponorogo" 
}).pretty()
```

Versi SQL :
```sql
select * from [collection_name] where `column_1.column_2` = 'Ponorogo';
```

## Order Data

### Sort by Ascending
```
db.[collection_name].find().sort({
    fullname: 1
})
```

Versi SQL :
```sql
select * from [collection_name] order by fullname ASC;
```

### Sort by Descending
```
db.[collection_name].find().sort({
    fullname: -1
})
```
Versi SQL :
```sql
select * from [collection_name] order by fullname DESC;

```

## Distinct

```
db.[collection_name].distinct('[field_name]')
```
Versi SQL :

```sql
select distinct([field_name]) from [collection_name]
```

## JOIN query

> Catatan :
> Restore ```db_exams``` untuk menjalankan contoh dibawah.


```
db.exams.aggregate([{
	$lookup: {
		from: 'siswa',
		localField: 'siswa',
		foreignField: '_id',
		as: 'siswa'
	}
},
{
	$lookup: {
		from: 'mapels',
		localField: 'mapel',
		foreignField: '_id',
		as: 'mapel'
	}
},{
	$replaceRoot: { 
		newRoot: { 
			$mergeObjects: [
				{$arrayElemAt: [ "$siswa",0 ]}, // add `siswa` schema to root schema
				{$arrayElemAt: [ "$mapel",0 ]}, // add `mapels` schema to root schema
				"$$ROOT"
			]
		}
	}
},
{ $project: {_id: 0,siswa: 0, mapel:0} } // drop schema siswa & mapels , and field _id
]).pretty()
```




atau dengan menggunakan <i>pipeline</i> <br>
dan mengembalikan <i>specific field</i>

```
db.exams.aggregate([{
	$lookup: {
		from: 'siswa',
		let: {siswa_id: "$siswa"}, // declare variable
		pipeline: [
			{ $match: {
				$expr: {
					$eq:["$_id", "$$siswa_id"] // equal
				}
			} },
			{ $project : { _id: 0, nama: 1 }} // show / hide fields
		],
		as: 'siswa'
	}
}, {
	$lookup: {
		from: 'mapels',
		let: {mapel_id: "$mapel"}, // declare variable
		pipeline: [
			{ $match: {
				$expr: {
					$eq:["$_id", "$$mapel_id"] // equal
				}
			} },
			{ $project : { _id: 0, nama_mapel: 1 }} // show / hide fields
		],
		as: 'mapel'
	}
},{
	$unwind: "$siswa" // un array
},{
	$unwind: "$mapel" // un array
},{
	$replaceRoot: { 
		newRoot: { 
			$mergeObjects: [
				{nama: "$siswa.nama"}, // add `nama` to root schema
				{nama_mapel: "$mapel.nama_mapel"}, // add `nama_mapel` to root schema
				"$$ROOT"
			]
		}
	}
},{
	$project: {
		_id: 0,
		siswa: 0, // hide siswa
		mapel: 0, // hide mapel
	}
}]).pretty()
```

## Concat String
```
db.[collection_name].aggregate({
	$replaceWith: {
		[new_field_name] : {
			$concat: ["$[field_name_1]",  ".","$[field_name_2]"]
		}
	}
});
```

## Parse Array

Prepare Data :
```
db.createCollection('listBuah')

db.listBuah.insert({
	buah: ["apel", "jambu", "melon", "semangka"]
})
```

Query :
```
db.listBuah.aggregate({
	$unwind: "$buah"
}).pretty()
```

## Push Array ( Grouping )

> Catatan :
> Restore ```db_exams``` untuk menjalankan contoh dibawah.

```
db.exams.aggregate([{
	$group: {
		_id: {
			nama_siswa: "$siswa"
		},
		list_mapel: {
			$push : {
				mapel: "$mapel",
				nilai: {
					$avg: "$nilai"
				}
			}
		}
	}
},{
	$sort: {
		_id: 1
	}
}]).pretty()

```

## Advanced query

> Catatan :
> Restore ```db_exams``` untuk menjalankan contoh dibawah.

```
db.exams.aggregate([{
	$lookup: {
		from: 'siswa',
		let: {siswa_id: "$siswa"},
		pipeline: [
			{ $match: { $expr: { $eq:["$_id", "$$siswa_id"] } } },
			{ $project : { _id: 0, nama: 1 }}
		],
		as: 'siswa'
	}
}, {
	$lookup: {
		from: 'mapels',
		let: {mapel_id: "$mapel"}, // declare variable
		pipeline: [
			{ $match: { $expr: { $eq:["$_id", "$$mapel_id"] } } },
			{ $project : { _id: 0, nama_mapel: 1 }} // show / hide fields
		],
		as: 'mapel'
	}
},
{ $unwind: "$siswa"}, // un array
{ $unwind: "$mapel" }, // un array
{ $replaceRoot: { newRoot: { $mergeObjects: [ {nama: "$siswa.nama"}, {nama_mapel: "$mapel.nama_mapel"}, "$$ROOT" ] } } },
{ $project: { _id: 0, siswa: 0, mapel: 0} },
{
	$group: {
		_id: {
			$concat: ["$nama", " at ","$nama_mapel"]
		},
		"rata rata nilai": {
			$avg: "$nilai"
		},
		"nilai tertinggi": {
			$max: "$nilai"
		},
		"nilai terendah": {
			$min: "$nilai"
		},
		"total nilai": {
			$sum: "$nilai"
		},
	}
}
]).pretty()


```























