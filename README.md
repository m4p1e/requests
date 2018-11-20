
[![license](http://img.shields.io/badge/license-MIT-red.svg?style=flat)](https://raw.githubusercontent.com/asmcos/requests/master/LICENSE)

# requests

Requests is an HTTP library  , it is easy to use. Similar to Python requests.  I want make it better with all my love by maple

# Installation

```
go get -u github.com/m4p1e/requests
```

# Start

``` go
package main

import "github.com/m4p1e/requests"

func main (){

        resp,err := requests.Get("http://m4p1e.com")
        if err != nil{
          return
        }
        println(resp.Text())
}
```

## Post

``` go
package main

import "github.com/m4p1e/requests"


func main (){

        data := requests.Datas{
          "name":"requests_post_test",
        }
        resp,_ := requests.Post("https://www.httpbin.org/post",data)
        println(resp.Text())
}

```

     Server return data...

``` json
{
  "args": {},
  "data": "",
  "files": {},
  "form": {
    "name": "requests_post_test"
  },
  "headers": {
    "Accept-Encoding": "gzip",
    "Connection": "close",
    "Content-Length": "23",
    "Content-Type": "application/x-www-form-urlencoded",
    "Host": "www.httpbin.org",
    "User-Agent": "Go-Requests 0.5"
  },
  "json": null,
  "origin": "114.242.34.110",
  "url": "https://www.httpbin.org/post"
}

```
# Chunking

Chunking is send a request with tranfer-encoding : chunked

``` go
package main

import (
  "github.com/m4p1e/requests"
  "fmt"
  "io"
  "time"
)

func main() {

    rd, wr := io.Pipe()
    
    go func(){

    b1 :=[]byte("test first!!!")

    wr.Write(b1)

    time.Sleep(5 *time.Second)

    b2 :=[]byte("test second!")

    wr.Write(b2)

    b3 :=make([]byte,0)

    wr.Write(b3)


    }()

    req,_:= requests.Chunking("http://127.0.0.1:9999",rd)

    fmt.Println(string(req.Content()))

}
```
# Post Json

``` go
data := map[string]string{
        "phone" : "11111111111",
        "v" : "9a6cd7899d0d2a28f797b15147722474",
      }

header := requests.Header{
      "Cookie" : "something",
      "Content-Type" : "application/json; charset=UTF-8"
    }

req,_:=requests.Post("https://example.com",data,header)
```
data can be map, struct etc. if tranfers more than one data need to be json ,datas will be in josn array

# Feature Support
  - Set headers
  - Set params
  - Multipart File Uploads
  - Sessions with Cookie Persistence
  - Proxy
  - Authentication
  - Json              done
  - Chunked Requests  done
  - Debug             done
  - SetTimeout

# EnableCookie

it will using the Cookiejar in go


``` go
requests.EnableKeepCookie()

```

# EnableDebug

it will show pre request and response

``` go
requests.EnableDebug()
```

# Set header


### example 1

``` go
req := requests.Requests()

resp,err := req.Get("http://m4p1e.com",requests.Header{"Referer":"http://www.jeapedu.com"})
if (err == nil){
  println(resp.Text())
}
```

### example 2

``` go
req := requests.Requests()
req.Header.Set("accept-encoding", "gzip, deflate, br")
resp,_ := req.Get("http://m4p1e.com",requests.Header{"Referer":"http://www.jeapedu.com"})
println(resp.Text())

```

### example 3

``` go
h := requests.Header{
  "Referer":         "http://www.jeapedu.com",
  "Accept-Language": "zh-CN,zh;q=0.9,en;q=0.8",
}
resp,_ := req.Get("http://m4p1e.com",h)

h2 := requests.Header{
  ...
  ...
}
h3,h4 ....
// two or more headers ...
resp,_ = req.Get("http://m4p1e.com",h,h2,h3,h4)
```


# Set params

``` go
p := requests.Params{
  "title": "The blog",
  "name":  "file",
  "id":    "12345",
}
resp,_ := req.Get("http://www.cpython.org", p)

```


# Auth

Test with the `correct` user information.

``` go
req := requests.Requests()
resp,_ := req.Get("https://api.github.com/user",requests.Auth{"asmcos","password...."})
println(resp.Text())
```

github return

```
{"login":"asmcos","id":xxxxx,"node_id":"Mxxxxxxxxx==".....
```

# JSON

``` go
req := requests.Requests()
req.Header.Set("Content-Type","application/json")
resp,_ = req.Get("https://httpbin.org/json")

var json map[string]interface{}
resp.Json(&json)

for k,v := range json{
  fmt.Println(k,v)
}
```


# SetTimeout

```
req := Requests()
req.Debug = 1

// 20 Second
req.SetTimeout(20)
req.Get("http://golang.org")
```

# Get Cookies

``` go
resp,_ = req.Get("https://www.httpbin.org")
coo := resp.Cookies()
// coo is [] *http.Cookies
println("********cookies*******")
for _, c:= range coo{
  fmt.Println(c.Name,c.Value)
}
```
