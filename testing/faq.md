    
    
    # InjectMocks vs Mocks
    @Mock create a mock. @InjectMocks create an instance of the class and injects the mocks that are created with the @Mock (or @Spy) annotations into this instance.


    > **Note:** that you must use RunWith(MockitoJunitRunner.class) or Mockito.initMock(this) to initialize these mocks and inject them.

    ```java
    @RunWith(MockitoJUnitRunner.class)
    public class SomeManagerTest {

        @InjectMocks
        private SomeManager someManager;

        @Mock
        private SomeDependency someDependency; // this will be injected into someManager

        //tests...

    }
    ``` 