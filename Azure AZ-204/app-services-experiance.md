# Azure App Services Experience

## Experience from hosting FriendFace using App Services

On 13/06/23 I managed to host my FriendFace app using Azure web apps, hosting the back end and front end in different web apps that connected to each other.

### Things I had to do to get it to work

- Change the MariaDB database to an in memory H2 database (followed [this](https://www.baeldung.com/spring-boot-h2-database) guide)
- Make sure the cross-origin was allowing my UI to access the API endpoints
- Change my UI fetch requests to access the API with the new domain name.

### Problems I had

- CORS was a nightmare (API) - ended up allowing all \*
- App was designed poorly
- HTTPS only by default (you need to manually let it accept HTTP)
- Need to enable logs, but they were mostly helpful
- Just restarting it suspiciously fixed loads of problems
- API and UI hosted **separately**, and you call one from another
- Make sure you **BUILD THE API AND UI** for non-static web apps
