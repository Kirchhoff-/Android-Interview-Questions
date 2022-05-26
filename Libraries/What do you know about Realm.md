# Realm 
Realm includes an embedded, object-oriented database that lets you build real-time, offline-first applications. You can also use Realm's hosted application services as a secure backend that can sync data between devices, authenticate and manage users, and run serverless JavaScript functions.

## [What Problem Does MongoDB Realm Solve?](https://www.mongodb.com/docs/realm/get-started/introduction-mobile/#what-problem-does-mongodb-realm-solve-)
Mobile developers face a number of unique challenges. You must:
- Handle the unpredictable environment of mobile apps. Connections can be lost, devices can shut down at any time, and clients often update long after release;
- Maintain common data schemas and APIs between mobile clients, backend APIs, and databases;
- Stay aware of security vulnerabilities across all components in an ecosystem;
- Consistently serialize objects between networks, database storage, and application memory;
- Program in the languages and frameworks for one or more mobile operating systems.

All of these challenges present different obstacles. You can solve each in isolation with a wide variety of libraries and frameworks. Deciding the right solution for each problem with the right tradeoffs is a challenge mobile developers know all too well.

The combination of multiple environments creates even more challenges. For instance, you can use a Java library on your Android client to serialize objects, but that library likely wouldn't work on iOS. And this doesn't even take into account consistency across backend services.

## [Realm Database](https://www.mongodb.com/docs/realm/get-started/introduction-mobile/#realm-database)
Many of these challenges arise due to particularities of the mobile environment. These challenges include network reliability, local storage, and keeping UIs reactive. Realm Database solves many common mobile programming headaches:
- **Local storage**: Realm Database runs right on client devices. Access objects using the native query language for each platform. Storing, accessing, and updating your data is simple and lightweight;
- **Network reliability**: Realm Database is offline-first. You always read from and write to the local database, not over the network. When Realm Sync is enabled, Realm Database synchronizes data with MongoDB Realm over the network in a background thread. The sync protocol resolves conflicts consistently on each client and in the linked MongoDB Atlas cluster;
- **Reactive UI**: Live objects always reflect the latest data stored in Realm Database. You can subscribe to changes, letting you keep your UI consistently up to date.

## [Summary](https://www.mongodb.com/docs/realm/get-started/introduction-mobile/#summary)
- MongoDB Realm is a serverless application platform that takes care of the details of deployment and scaling for you;
- You can customize your Realm app with functions and triggers, custom permissions via rules, and authentication;
- Realm Database is an offline-first mobile object database in which you can directly access and store live objects without an ORM;
- Live Objects and Realm Sync offer significant benefits over traditional mobile development stacks:
  - Live Objects always reflect the latest state of data in the database, making it easier to keep your UI in sync with changes to your data;
  - Realm Sync synchronizes data between client-side realms and the server-side MongoDB Atlas cluster linked to your Realm app;
  - Realm Database synchronizes data in a background thread, abstracting away network usage so you don't have to worry about latency or dropped connections.

# Links
[MongoDB Realm Docs](https://www.mongodb.com/docs/realm)  

# Futher reading
[Java SDK Tutorial](https://www.mongodb.com/docs/realm/tutorial/java-sdk/#std-label-java-sdk-tutorial)

[realm-kotlin](https://github.com/realm/realm-kotlin)
