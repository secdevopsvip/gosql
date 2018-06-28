# gosql
Keep simple

<a href="https://travis-ci.org/ilibs/gosql"><img src="https://travis-ci.org/ilibs/gosql.svg" alt="Build Status"></a>

## Usage

Connection database and use sqlx native function,See the https://github.com/jmoiron/sqlx

```go
import "github.com/ilibs/gosql"

func main(){
	configs["default"] = &Config{
		Enable:  true,
		Driver:  "mysql",
		Dsn:     "root:123456@tcp(127.0.0.1:3306)/test?charset=utf8&parseTime=True&loc=Asia%2FShanghai",
		ShowSql: true,
	}

    //connection database
	gosql.Connect(configs)

	gosql.DB().QueryRowx("select * from users where id = 1")
}

```

## CRUD interface

```go
type Users struct {
	Id        int       `db:"id"`
	Name      string    `db:"name"`
	Email     string    `db:"email"`
	Status    int       `db:"status"`
	CreatedAt time.Time `db:"created_at"`
	UpdatedAt time.Time `db:"updated_at"`
}

func (u *Users) DbName() string {
	return "default"
}

func (u *Users) TableName() string {
	return "users"
}

func (u *Users) PK() string {
	return "id"
}

//Get
user := &Users{}
Model(user).Where("id=1").Get()

//All
user := make([]*Users,0)
Model(&user).All()

//Create and Timestamp Tracking
Model(&User{Name:"test",Email:"test@gmail.com"}).Create()

//Update
Model(&User{Name:"test2",Email:"test@gmail.com"}).Where("id=1").Update()
//If you need to update the null value, you can do so
Model(&User{Status:0}).Where("id=1").Update("status")

//Delete
Model(&User{}).Where("id=1").Delete()

```

## Timestamp Tracking
If your fields contain the following field names, they will be updated automatically

```
AUTO_CREATE_TIME_FIELDS = []string{
    "create_time",
    "create_at",
    "created_at",
    "update_time",
    "update_at",
    "updated_at",
}
AUTO_UPDATE_TIME_FIELDS = []string{
    "update_time",
    "update_at",
    "updated_at",
}
```

## Thanks

sqlx https://github.com/jmoiron/sqlx