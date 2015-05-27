#YAO's An ORM
Yao is an ORM for Golang, inspired in [Laravel eloquent ORM](http://laravel.com/docs/5.0/eloquent)
Currently only compatible with postgresql.

## Generate your models
Get the yao command line tool with:  
`
go get github.com/alfonsodev/yao
`  

Then you can run `yao gen` to generate automatically your models (`yao gen -h` , too se all the options)  

`
yao gen -d dbname -H host
`
It will generate: 
  - `./models` folder
  - iside it a go package for each postgres schema (namespace) in your database.
  - a .go file for each table
  - a helper file query.go for .Where .And .Or .. functions.  
Remember default schema name is public, so at least you should have  `./models/public`  

## Usage
For these examples let's supouse your have a database `foodb` with a schema named `usermanager` with a `users` table.
You could run `yao gen -d foodb -H localhost` and it would create a `/models/usermanager/users.go` file ready to use.
Have a look to [models test](https://github.com/alfonsodev/yao/blob/master/models_test.go) to see real examples.  
```
import (
  "database/sql"
  UM "github.com/alfonsodev/yao/models/usermanager"
)
func main() {
  // use a regular database/sql connection
  db, err = sql.Open("postgres", "dbname=yaotest sslmode=disable")
  // pass it to NewNameOfTable and it will create a struct for you
  user := UM.NewUsers(db)
  // Use scan for ints or string 
  user.Username.Scan("Albert")
  user.Joinedon.Scan(695510502)
  // Insert
  user.Save()
  // Get all rows
  users, error := UM.AllUsers()
  // Get with Where clause
  students, error := UM.UsersWhere("Email", "LIKE", "%.edu").Get()
  // Chainable And,Ors conditions
  students, error := UM.UsersWhere("Email", "LIKE", "%.edu").And("Location", "=", "Zurich").Get()
  students, error := UM.UsersWhere("Email", "LIKE", "%.edu")
                            .Or("Email", "LIKE", "%.co.uk")
                            .And("Joinedon", "=", 695510502).Get()
}

```
