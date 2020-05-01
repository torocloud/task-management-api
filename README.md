# Task Management API
A simple task management api with CRUD operations

### Prerequisites

  - Apache Maven 3+
  - [Martini Desktop](https://www.torocloud.com/martini/download)

### Building the Martini Package

```
$ mvn clean package
```
This will create a ZIP file named `task-management-api.zip` containing all the files (services, models, etc.) needed under the `target` folder. This ZIP file is what we call a [Martini Package](https://docs.torocloud.com/martini/latest/developing/package/) which then you can import in Martini Desktop to get started. You can learn more how to import a Martini Package by visiting our [documentation](https://docs.torocloud.com/martini/latest/developing/package/importing/)

### Getting the Martini Package from TORO Marketplace

You can also get this package via TORO Marketplace. See our documentation on [How to import a Martini Package from Marketplace](https://docs.torocloud.com/martini/latest/developing/package/importing/#from-the-marketplace).

### Usage
This package exposes operations for a simple task management REST API that allows you to do CRUD operations. You can find the [Gloop REST API](https://docs.torocloud.com/martini/latest/developing/gloop/api/rest/) file at `api/TaskManagement` under the `code` folder after importing the package to your Martini Desktop application.

### Setup

This Martini Package automatically sets up the required resources for you. During the package startup, Martini will execute the configured _Startup Service_ that initializes the database to be used for storing the record that will be creating for using this notes api.

This package uses HSQL as the default datastore but you can change this to MySQL, Postgres and etc by updating the [package properties](https://docs.torocloud.com/martini/latest/developing/package/properties/) located at [conf](https://docs.torocloud.com/martini/latest/developing/package/directory/) folder. Below is what the contents of the properties file look like

```
# The database connection name to be used
database.name=task_management

# The database type to be used
database.type=hsql

# HSQL database configuration to be used
hsql.driver=org.hsqldb.jdbc.JDBCDriver
hsql.connection.url=jdbc:hsqldb:file:${toroesb.home}data/hsql/task_management.db;hsqldb.tx=MVCC;sql.syntax_mys=true
hsql.username=sa
hsql.password=

# MySQL database configuration to be used
mysql.driver=com.mysql.cj.jdbc.Driver
mysql.connection.url=jdbc:mysql://localhost:3306/task_management?allowMultiQueries=true&sessionVariables=sql_mode='ANSI'
mysql.username=root
mysql.password=
```
* `database.type` sets the database provider the Martini package will use. If you will use the default hsql config, you don't need to change anything in the hsql configuration. **Note**: If you will use a different hsql database, make sure that you add `sql.syntax_mys=true` in the connection properties. This ensures that the SQL query from the SQL Services in this package will be compatible with hsql.
### Operations

The base url is `<host>/api/taskmanagement/1.0` where `host` is the location where the Martini is deployed. By default, it's `localhost:8080`.

#### Task Management

`GET /task`

Returns all list of task

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/taskmanagement/1.0/task \
  -H 'accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully get all task",
    "warnings": [],
    "task": [
        {
            "id": "2e674860-6c86-4f71-9a49-39edcb24889a",
            "title": "Nice",
            "description": "Nice Description",
            "assignee": "UNASSIGNED",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-06T13:26:58+0800",
            "updated_date": "2020-04-06T13:26:58+0800"
        },
        {
            "id": "4a0c285e-3376-4489-8d77-b56ca3d4a37e",
            "title": "New",
            "description": "New",
            "assignee": "UNASSIGNED",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-16T22:00:02+0800",
            "updated_date": "2020-04-16T22:00:02+0800"
        }
    ]
}
```

`POST /task`

Creates a new task. **Note**: The `assignee` and `reporter` will be the reference to the account/user of the users `Account/User` table in the database.

**Sample Request**

**curl**
```
curl -X POST \
  http://localhost:8080/api/taskmanagement/1.0/task/ \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
    "title": "New Task",
    "description": "Task Description",
    "assignee": "0e8d5143-606e-4fe3-866f-60883a1d5707",
    "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707"
}'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully created task",
    "warnings": [],
    "task": {
        "id": "03f5f333-4f8d-4ea9-85a5-b667acecc600",
        "title": "New Task",
        "description": "Task Description",
        "assignee": "0e8d5143-606e-4fe3-866f-60883a1d5707",
        "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
        "status": "OPEN",
        "created_date": "2020-04-23T08:47:15+0800",
        "updated_date": "2020-04-23T08:47:15+0800"
    }
}
```

`PUT /task`

Updates a task

**Sample Request**

**curl**
```
curl -X PUT \
  http://localhost:8080/api/taskmanagement/1.0/task/ \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
    "id": "03f5f333-4f8d-4ea9-85a5-b667acecc600",
    "description": "Update Description"
}'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully update task",
    "warnings": []
}
```

`GET /task/<taskId>`

Returns a single task that matches the given `taskId`

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/taskmanagement/1.0/task/03f5f333-4f8d-4ea9-85a5-b667acecc600
```

**Sample Response**

If the request is sucessful, it will return an HTTP response `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully get task",
    "warnings": [],
    "task": {
        "id": "03f5f333-4f8d-4ea9-85a5-b667acecc600",
        "title": "New Task",
        "description": "Update Description",
        "assignee": "UNASSIGNED",
        "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
        "status": "OPEN",
        "created_date": "2020-04-23T08:47:15+0800",
        "updated_date": "2020-04-23T08:49:41+0800"
    }
}
```

`DELETE /task/<taskId>`

Deletes an task that matches the given `taskId`

**Sample Request**

**curl**
```
curl -X DELETE \
  http://localhost:8080/api/taskmanagement/1.0/task/03f5f333-4f8d-4ea9-85a5-b667acecc600
```

**Sample Response**

If the request is sucessful, it will return an HTTP response `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully deleted task",
    "warnings": []
}
```

`GET /task/assignee/<assigneeGuid>`

Returns all list of task of the assignee that matches the given `assigneeGuid`

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/taskmanagement/1.0/task/assignee/778e248b-e2c0-4998-8635-99e4c91d1750 \
  -H 'accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully get all task",
    "warnings": [],
    "task": [
        {
            "id": "fd0dca83-2f30-42fa-b732-55ca655699d4",
            "title": "Task Number Three",
            "description": "This a task number three description",
            "assignee": "778e248b-e2c0-4998-8635-99e4c91d1750",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-03T11:33:42+0800",
            "updated_date": "2020-04-03T11:33:42+0800"
        }
    ]
}
```

`GET /task/reporter/<reporterGuid>`

Returns all list of task of the reporter that matches the given `reporterGuid`

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/taskmanagement/1.0/task/reporter/0e8d5143-606e-4fe3-866f-60883a1d5707 \
  -H 'accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully get all task",
    "warnings": [],
    "task": [
        {
            "id": "03f5f333-4f8d-4ea9-85a5-b667acecc600",
            "title": "New Task",
            "description": "Update Description",
            "assignee": "UNASSIGNED",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-23T08:47:15+0800",
            "updated_date": "2020-04-23T08:49:41+0800"
        },
        {
            "id": "2e674860-6c86-4f71-9a49-39edcb24889a",
            "title": "Nice",
            "description": "Nice Description",
            "assignee": "UNASSIGNED",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-06T13:26:58+0800",
            "updated_date": "2020-04-06T13:26:58+0800"
        }
    ]
}
```

`GET /task/status`

Returns all list of task that matches the `status` query

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/taskmanagement/1.0/task/status?status=OPEN \
  -H 'accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully get all task",
    "warnings": [],
    "task": [
        {
            "id": "03f5f333-4f8d-4ea9-85a5-b667acecc600",
            "title": "New Task",
            "description": "Update Description",
            "assignee": "UNASSIGNED",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-23T08:47:15+0800",
            "updated_date": "2020-04-23T08:49:41+0800"
        },
        {
            "id": "2e674860-6c86-4f71-9a49-39edcb24889a",
            "title": "Nice",
            "description": "Nice Description",
            "assignee": "UNASSIGNED",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-06T13:26:58+0800",
            "updated_date": "2020-04-06T13:26:58+0800"
        }
    ]
}
```

`GET /task/assignee/<assigneeGuid>/status`

Returns all list of task that matches the `status` query and the given `assigneeGuid`

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/taskmanagement/1.0/task/assignee/778e248b-e2c0-4998-8635-99e4c91d1750/status?status=OPEN \
  -H 'accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully get all task",
    "warnings": [],
    "task": [
        {
            "id": "fd0dca83-2f30-42fa-b732-55ca655699d4",
            "title": "Task Number Three",
            "description": "This a task number three description",
            "assignee": "778e248b-e2c0-4998-8635-99e4c91d1750",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-03T11:33:42+0800",
            "updated_date": "2020-04-03T11:33:42+0800"
        }
    ]
}
```

`GET /task/reporter/<reporterGuid>/status`

Returns all list of task that matches the `status` query and the given `reporterGuid`

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/taskmanagement/1.0/task/reporter/0e8d5143-606e-4fe3-866f-60883a1d5707/status?status=OPEN \
  -H 'accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully get all task",
    "warnings": [],
    "task": [
        {
            "id": "4a0c285e-3376-4489-8d77-b56ca3d4a37e",
            "title": "New",
            "description": "New",
            "assignee": "UNASSIGNED",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-16T22:00:02+0800",
            "updated_date": "2020-04-16T22:00:02+0800"
        },
        {
            "id": "fd0dca83-2f30-42fa-b732-55ca655699d4",
            "title": "Task Number Three",
            "description": "This a task number three description",
            "assignee": "778e248b-e2c0-4998-8635-99e4c91d1750",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-03T11:33:42+0800",
            "updated_date": "2020-04-03T11:33:42+0800"
        }
    ]
}
```

`GET /task/range`

Returns all list of task that matches the `start_date` and `end_date` query

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/taskmanagement/1.0/task/range?start_date=2020-01-01&end_date=now \
  -H 'accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully get all task",
    "warnings": [],
    "task": [
        {
            "id": "2e674860-6c86-4f71-9a49-39edcb24889a",
            "title": "Nice",
            "description": "Nice Description",
            "assignee": "UNASSIGNED",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-06T13:26:58+0800",
            "updated_date": "2020-04-06T13:26:58+0800"
        },
        {
            "id": "4a0c285e-3376-4489-8d77-b56ca3d4a37e",
            "title": "New",
            "description": "New",
            "assignee": "UNASSIGNED",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-16T22:00:02+0800",
            "updated_date": "2020-04-16T22:00:02+0800"
        }
    ]
}
```

`GET /task/assignee/<assigneeGuid>/range`

Returns all list of task that matches the `start_date` and `end_date` query and given `assigneeGuid`

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/taskmanagement/1.0/task/assignee/778e248b-e2c0-4998-8635-99e4c91d1750/range?start_date=2020-01-01&end_date=now \
  -H 'accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully get all task",
    "warnings": [],
    "task": [
        {
            "id": "fd0dca83-2f30-42fa-b732-55ca655699d4",
            "title": "Task Number Three",
            "description": "This a task number three description",
            "assignee": "778e248b-e2c0-4998-8635-99e4c91d1750",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-03T11:33:42+0800",
            "updated_date": "2020-04-03T11:33:42+0800"
        }
    ]
}
```

`GET /task/reporter/<reporterGuid>/range`

Returns all list of task that matches the `start_date` and `end_date` query and given `reporterGuid`

**Sample Request**

**curl**
```
curl -X GET \
  http://localhost:8080/api/taskmanagement/1.0/task/reporter/0e8d5143-606e-4fe3-866f-60883a1d5707/range?start_date=2020-01-01&end_date=now \
  -H 'accept: application/json'
```

**Sample Response**

If the request is successful, it will return an HTTP status code `200` with the response payload below:
```
{
    "result": "OK",
    "message": "Successfully get all task",
    "warnings": [],
    "task": [
        {
            "id": "2e674860-6c86-4f71-9a49-39edcb24889a",
            "title": "Nice",
            "description": "Nice Description",
            "assignee": "UNASSIGNED",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-06T13:26:58+0800",
            "updated_date": "2020-04-06T13:26:58+0800"
        },
        {
            "id": "4a0c285e-3376-4489-8d77-b56ca3d4a37e",
            "title": "New",
            "description": "New",
            "assignee": "UNASSIGNED",
            "reporter": "0e8d5143-606e-4fe3-866f-60883a1d5707",
            "status": "OPEN",
            "created_date": "2020-04-16T22:00:02+0800",
            "updated_date": "2020-04-16T22:00:02+0800"
        }
    ]
}
```