# MLFlow

## MySQL:

Login to database:

```sql
mysql -u root -p

mysql -u mlflow_user -p
```

Logout:

```sql
exit
```

Basic cmd:

```sql
SHOW DATABASES;
```

```sql
SHOW TABLES;
```

# Hard delete experiments

1. Select the tracking database:

```sql
-- the name of your database
USE mlflow_tracking_db;
```

2. (Optional) Select the table to see the data:

```sql
SELECT * FROM MLFLOW_TRACKING_DB.TABLES WHERE TABLE_SCHEMA = 'DATABASENAME';
SELECT * FROM alembic_version LIMIT 10;
SELECT * FROM datasets LIMIT 10;
SELECT * FROM experiment_tags LIMIT 10;
SELECT * FROM experiments LIMIT 10;
SELECT * FROM input_tags LIMIT 10;
SELECT * FROM inputs LIMIT 10;
SELECT * FROM latest_metrics LIMIT 10;
SELECT * FROM metrics LIMIT 10;
SELECT * FROM model_version_tags LIMIT 10;
SELECT * FROM model_versions LIMIT 10;
SELECT * FROM params LIMIT 10;
SELECT * FROM registered_model_aliases LIMIT 10;
SELECT * FROM registered_model_tags LIMIT 10;
SELECT * FROM registered_models LIMIT 10;
SELECT * FROM runs LIMIT 10;
SELECT * FROM tags LIMIT 10;
```

3. Get the `artifact_uri` of deleted run:

```sql
SELECT run_uuid, artifact_uri, lifecycle_stage  FROM runs WHERE lifecycle_stage = 'deleted';
```

4. Deleted run data in database:

```sql
DELETE FROM experiment_tags
WHERE experiment_id = ANY ( SELECT experiment_id FROM experiments WHERE lifecycle_stage = 'deleted' );
DELETE FROM latest_metrics
WHERE run_uuid = ANY ( SELECT run_uuid FROM runs WHERE experiment_id = ANY ( SELECT experiment_id FROM experiments WHERE lifecycle_stage = 'deleted' ) );
DELETE FROM metrics
WHERE run_uuid = ANY ( SELECT run_uuid FROM runs WHERE experiment_id = ANY ( SELECT experiment_id FROM experiments WHERE lifecycle_stage = 'deleted' ) );
DELETE FROM tags
WHERE run_uuid = ANY ( SELECT run_uuid FROM runs WHERE experiment_id = ANY ( SELECT experiment_id FROM experiments WHERE lifecycle_stage = 'deleted' ) );
DELETE FROM params
WHERE run_uuid = ANY ( SELECT run_uuid FROM runs WHERE experiment_id = ANY ( SELECT experiment_id FROM experiments WHERE lifecycle_stage = 'deleted' ) );
DELETE FROM runs
WHERE experiment_id = ANY ( SELECT experiment_id FROM experiments WHERE lifecycle_stage = 'deleted' );
DELETE FROM experiments
WHERE lifecycle_stage = 'deleted';
DELETE FROM latest_metrics
WHERE run_uuid = ANY ( SELECT run_uuid FROM runs WHERE lifecycle_stage = 'deleted' );
DELETE FROM metrics
WHERE run_uuid = ANY ( SELECT run_uuid FROM runs WHERE lifecycle_stage = 'deleted' );
DELETE FROM tags
WHERE run_uuid = ANY ( SELECT run_uuid FROM runs WHERE lifecycle_stage = 'deleted' );
DELETE FROM params
WHERE run_uuid = ANY ( SELECT run_uuid FROM runs WHERE lifecycle_stage = 'deleted' );
DELETE FROM runs
WHERE lifecycle_stage = 'deleted';
```

5. Remove objects from S3 bucket

There are several ways to delete objects from an Amazon S3 bucket using the AWS Command Line Interface (CLI). Here are some examples:

- To delete a single object from an S3 bucket, you can use the `rm` command. For example, to delete an object with the key `mykey` from the bucket `mybucket`, you can use the following command:

```other
aws s3 rm s3://mybucket/mykey
```

Copy

- To delete multiple objects from an S3 bucket, you can use the `rm` command with the `--recursive` option. For example, to delete all objects under the `myfolder` prefix in the `mybucket` bucket, you can use the following command:

```other
aws s3 rm s3://mybucket/myfolder --recursive
```

Copy

- To delete a specific version of an object from an S3 bucket, you can use the `delete-object` command. For example, to delete version `myversion` of the object with the key `mykey` from the bucket `mybucket`, you can use the following command:

```other
aws s3api delete-object --bucket mybucket --key mykey --version-id myversion
```

SELECT table_schema AS "Database", SUM(data_length + index_length) / 1024 / 1024 AS "Size (MB)" FROM information_schema.TABLES GROUP BY table_schema;

SELECT table_schema AS "Database", SUM(data_length + index_length) / 1024 / 1024 / 1024 AS "Size (GB)" FROM information_schema.TABLES GROUP BY table_schema;

