# PUT vs POST

**PUT** - It is used to send data to a server to create/update a resource.

Example:
```
http://www.google.com/users/234

PUT /new.html HTTP/1.1
Host: example.com
Content-type: text/html
Content-length: 20

<p>New File</p>
```

Feature: 
- Cannot be bookmarked
- Should only be used for update the data
- Never cached
- Do not retain in the browser history

**POST** - It submits the processed data to a specified resource
Example: 
```
https://api.themoviedb.org/3/movie/1/rating

RequestBody
{
  "value": 8.5
}
```
Feature: 
- Cannot be bookmarked
- Have no restrictions on length of data
- Never cached
- Do not retain in the browser history

| PUT| POST |
|---|---|
| Is idempotent | Is not idempotent  |
| If you send the same request multiple times, the result will remain the same  | If you send the same POST request more than one time, you will receive different results  |
| Mostly used for UPDATE query | Mainly use for create query |

## Links
https://www.keycdn.com/support/put-vs-post  
https://restfulapi.net/rest-put-vs-post/  
https://www.guru99.com/put-vs-post.html  
https://developers.themoviedb.org/3/movies  
