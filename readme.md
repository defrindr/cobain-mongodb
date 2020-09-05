# Mongo DB practice

## Requirements
* installed mongo

## Clone Repository
```
git clone https://github.com/defrindr/pintar.mongo
```

## Database for practice

to import database ```db_exams``` and ```example``` , you must :

1. go to repo dir
2. run this command
```
mongorestore --archive="example" --nsFrom="example.*" --nsTo="example.*"
mongorestore --archive="db_exams" --nsFrom="db_exams.*" --nsTo="db_exams.*"
```


