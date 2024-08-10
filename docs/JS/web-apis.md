- Histrory API
- Storage API
- Forms API
- Worker API
- Fetch API
- Geolocation API





`API`: 

you can use the GeoLocation API to get the coordinates of the users with two lines of code. You don’t need to worry about how it works in the backend

`web API`: an application programming interface (API) for web

>  power system at your home. When you plug the cable into the socket, you get electricity. You don’t need to worry about how electricity comes into the socket







- Browser API (Client-Side JavaScript API)
- Server API
- Third Party APIs



## browser APIS

- **Storage API** − It allows you to store the data in the browser's local storage.
- **DOM API** − It allows you to access DOM elements and manipulate them.
- **History API** − It allows you to get the browser’s history.
- **Fetch API** − It allows you to fetch data from web servers.
- **Forms API** − It allows you to validate the form data.





- localStorage

```js
localStorage.setItem(key, value); // To set key-value pair
localStorage.getItem(key); 
localStorage.removeItem(key)
```

- sessionStorage



## Cookie Vs localStorage Vs sessionStorage

Here, we have given the difference between the cookie, localStorage, and sessionStorage objects.

| Feature       | Cookie                                            | Local storage                          | Session storage                                    |
| :------------ | :------------------------------------------------ | :------------------------------------- | :------------------------------------------------- |
| Storage Limit | 4 KB per cookie                                   | 5 MB                                   | 5 MB                                               |
| Expiry        | It has an expiry date.                            | It never expires.                      | It gets deleted when you close the browser window. |
| Accessibility | It can be accessed on both the client and server. | It can be accessed by the client only. | It can be accessed by the client only.             |
| Security      | It can be vulnerable.                             | It is fully secured.                   | It is fully secured.                               |



| Property/Method     | Description                                                  |
| :------------------ | :----------------------------------------------------------- |
| key(n)              | To get the name of the nth key from the local or session storage. |
| length              | To get the count of key-value pairs in the local or session storage. |
| getItem(key)        | To get a value related to the key passed as an argument.     |
| setItem(key, value) | To set or update key-value pair in the local or session storage. |
| removeItem(key)     | To remove key-value pairs from the storage using its key.    |
| clear()             | To remove all key-value pairs from the local or session storage. |





> a set of JavaScript bindings to native iOS, Android and Web APIs. Using these bindings, we're able to control the use interface and access device hardware, like the camera.



## form API

| Property        | Description                                                  |
| :-------------- | :----------------------------------------------------------- |
| customError     | It contains a true boolean value when you set the custom validity message. |
| patternMismatch | When the parent element's value doesn't match the pattern, it sets true. |
| rangeOverflow   | It returns a boolean value based on whether the input value is greater than the max attribute's value. |
| rangeUnderflow  | It returns a boolean value based on whether the input value is less than the min attribute's value. |
| stepMismatch    | It returns a boolean value based on whether the step is mismatching in the numeric input. |
| tooLong         | If the length of the input element's value is greater than the maxLength attribute's value, it returns true. Otherwise, it returns false. |
| typeMismatch    | When the type of entered value doesn't match the 'type' attribute's value, it returns true. |
| valueMissing    | It returns a boolean value based on whether the input element is empty. |
| valid           | It returns true when the input element is valid.             |
