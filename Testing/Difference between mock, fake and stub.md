# Mock vs Fake vs Stub
In automated unit testing, it may be necessary to use objects or procedures that look and behave like their release-intended counterparts, but are actually simplified versions that reduce the complexity and facilitate testing. Gerard Meszaros uses the term Test Double as the generic term for any kind of pretend object used in place of a real object for testing purposes. The most commonly used test doubles are fake, mock and stub.

For an example, we will consider various options of test doubles for the following interface:
```
interface Database {
    fun setUserName(id: Int, name: String)
    fun getUserNameById(id: Int): String?
}
```
In production implementation of `Database` interface will use on disk database. 

## Fake 
Fakes are objects that have working implementations, but not same as production one. Usually they take some shortcut and have simplified version of production code. In memory databse is good example of Fake. This fake implementation will not engage database, but will use a simple collection to store data. This allows us to do integration test of services without starting up a database and performing time consuming requests.

```
class InMemoryDatabase: Database {

    val map: HashMap<Int, String> = hashMapOf()

    override fun setUserName(id: Int, name: String) {
        map.put(id, name)
    }

    override fun getUserNameById(id: Int): String? {
        return map.get(id)
    }
}
```

## Stub
Stubs provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test.  Instead of the real object, we introduced a stub and defined what data should be returned. 

```
class StubDatabase: Database {
    
    override fun setUserName(id: Int, name: String) {
        //Nothing happen here
    }

    override fun getUserNameById(id: Int): String? {
        if (id == -1) {
            return null
        } else {
            return "John"
        }
    }
}
```

## Mock
Mock is very similar to Stub but the extra state is changed during program execution to check if something happened. In test assertion we can verify on Mocks that all expected actions were performed. The most popular library for work with mock - Mockito.

```
class MockDatabaseTest {

    val databaseMock = mock(Database::class.java)

    @Test
    fun exampleOfStub() {
        `when`(databaseMock.getUserNameById(1)).thenReturn("John")

        assertThat(databaseMock.getUserNameById(1)).isEqualTo("John")
        `verify`(databaseMock).getUserNambById(1)
    }
}
```


## Links
https://blog.pragmatists.com/test-doubles-fakes-mocks-and-stubs-1a7491dfa3da  
https://martinfowler.com/articles/mocksArentStubs.html  
https://stackoverflow.com/questions/346372/whats-the-difference-between-faking-mocking-and-stubbing  
https://www.telerik.com/blogs/fakes-stubs-and-mocks  
http://xunitpatterns.com/Test%20Double.html  
https://en.wikipedia.org/wiki/Test_double  
