# spring-boot-oauth2-password-flow

It is a Spring Boot application, that contains all necessary configurations to be able to try oauth2 authorization (password flow).
It uses JWT token key for the authorization.

There is a **hsql** embedded database in the application, and it contains two default users (they are uploaded by **DefaultDataGenerator.java**) 

**admin / admin**
  - role: ROLE_ADMIN
  - privilege: PRIVILEGE_ADMIN_READ

**user / user**
  - role: ROLE_USER
  - privilege: PRIVILEGE_USER_READ


## Try it

1. mvnw spring-boot:run
2. **get access_token** for

admin

```
curl -X POST -vu client:secret http://localhost:8080/oauth/token -H "Accept: application/json" -d "password=admin&username=admin&grant_type=password&scope=read%20write&client_secret=secret&client_id=client"
```

user

```
curl -X POST -vu client:secret http://localhost:8080/oauth/token -H "Accept: application/json" -d "password=user&username=user&grant_type=password&scope=read%20write&client_secret=secret&client_id=client"
```

It will return something like that:
```
{
"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0OTE0NjYxMTYsInVzZXJfbmFtZSI6InVzZXIiLCJhdXRob3JpdGllcyI6WyJQUklWSUxFR0VfVVNFUl9SRUFEIl0sImp0aSI6IjQ4MDVhZGQ3LWMzNTgtNDkzMC05ODkwLTEzNjNkNjJiZmQ0ZiIsImNsaWVudF9pZCI6ImNsaWVudCIsInNjb3BlIjpbInJlYWQiLCJ3cml0ZSJdfQ.7nMeIVuskhkmHXxX6CC6RZf9A_aXxsaoTXev6av4h64",
"token_type":"bearer","refresh_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX25hbWUiOiJ1c2VyIiwic2NvcGUiOlsicmVhZCIsIndyaXRlIl0sImF0aSI6IjQ4MDVhZGQ3LWMzNTgtNDkzMC05ODkwLTEzNjNkNjJiZmQ0ZiIsImV4cCI6MTQ5NDAxNDkxNiwiYXV0aG9yaXRpZXMiOlsiUFJJVklMRUdFX1VTRVJfUkVBRCJdLCJqdGkiOiI2MmU0MTU3Yy1hOWNiLTRlYjMtODg1Ni0wMmJhOWI1ZjQ3OWQiLCJjbGllbnRfaWQiOiJjbGllbnQifQ.1fexTQcFC80VkqbDo5zJfCzq0vbPPvJVPp8Nr3CwH68",
"expires_in":43199,
"scope":"read write",
"jti":"4805add7-c358-4930-9890-1363d62bfd4f"}
```
From this, you need "access_token", you can check it via **jwt.io**.

3. add **Authorization** header, with Bearer <token>

```
curl -H "Authorization: bearer <token>" http://localhost:8080/user
```

OR

```
curl -H "Authorization: bearer <token>" http://localhost:8080/admin
```

Of course the http://localhost:8080/admin endpoint is accessible only for admin, and the http://localhost:8080/user is for user.

## Technology Stack

* Java 8
* Spring boot 1.5.2