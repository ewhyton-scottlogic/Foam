# Creating a Java/Spring API with Azure MongoDB for CosmosDB

I was tasked to create a simple API in spring boot which links up with a MongoDB for CosmosDB and verify it works by hitting it from Postman. I followed [this](https://www.knowledgefactory.net/2022/07/azure-cosmos-db-mongodb-api-spring-boot-build-rest-crud-apis.html) guide.

Things that went well:

- Once it was working looking on the UI at the data in the DB was very easy.
- Making DB from portal was very easy.
- Didn't need a schema which was very handy, I'm guessing it could create it from the model.

Pain points:

- Bug with spring 3.0.0 where it couldn't find the database name from the `application.properties` file so had to alter the connection string to specify the database.
- Debugging this was a nightmare, couldn't create the beans, and it was very hard to find why.
