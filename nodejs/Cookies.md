# Cookies

```js
const postLogin = (req, res, next) => {
  res.setHeader(
    "Set-Cookie",
    "Domain=example.com; loggedIn=true; SameSite=None;  Max-Age=1000; Secure; HttpOnly"
  );
  res.redirect("/");
};
```

## Options

1. Name=Value

2. Max-Age=(In seconds)

3. Secure

   - can only be sent if the domain is only using https

4. HttpOnly

   - If used, client side javascript cannot be used to modify the cookies

5. Path=/(exp=> /auth,/fr/docs)

   - Indicates that path must exist in the requested url of the browser to send the cookie header

6. SameSite=value

   - Strict => if a request originated from a diff domain or scheme(even with the same domain) i.e, not from where the same-site cookie was set, no cookies are sent
   - None => means that the browser sends the cookie with both cross-site and same-site requests. The secure attr must also be set when setting this value

7. Domain=value
   - Only the current domain can be set as the value, or a domain of a higher order. Setting the domain will make the cookie available to it, as well as all its sub-domains
