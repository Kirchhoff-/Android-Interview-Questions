# GET vs POST

**GET** - It requests the data from a specified resource

Example:
```
https://api.themoviedb.org/3/movie/1/account_states
```

Feature: 
- Remains in the browser history
- Can be bookmarked
- Can be cached
- Have length restrictions
- Should never be used when dealing with sensitive data
- Should only be used for retrieving the data

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

| GET| POST |
|---|---|
| Passes request parameter in URL  | Passes request parameter in request body  |
| Parameters remain in browser history because they are part of the URL  | Parameters are not saved in browser history  |
| Can be bookmarked  | Cannot be bookmarked  |
| URL length is restricted. A safe URL length limit is often 2048  | No restrictions for data length  |
| Only ASCII characters allowed  | No restrictions. Binary data is also allowed  |
| Mostly used for view purpose (e.g. SQL SELECT)  | Mainly use for update purpose (e.g. SQL INSERT or UPDATE)  |

## Links
https://www.w3schools.com/tags/ref_httpmethods.asp  
https://www.diffen.com/difference/GET-vs-POST-HTTP-Requests  
https://www.javatpoint.com/get-vs-post  
https://developers.themoviedb.org/3/movies  
