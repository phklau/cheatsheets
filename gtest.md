# Gtest

https://google.github.io/googletest/primer.html
```cpp
#include gtest.h 
// GIVEN:
// WHEN:
// THEN:
TEST(Suitename, Testname) {


}
```

## Assert vs. Expect
- assert returns total failure → test will be aborted
- expect return failure, but test will be further executed

https://google.github.io/googletest/reference/assertions.html
```cpp
EXPECT_EQ(test, expected) << "costum failed message e.g. " << test << "was not" << expected;
EXPECT_TRUE/FALSE
EXPECT_STREQ(const char*, const char*)
EXPECT_FLOAT_EQ // 4 digits of LSB tolerance
EXPECT_DOUBLE_EQ // 4 digits of LSB tolerance
EXPECT_NEAR(test, expected, abs_error)
```
## Fixtures
 → Data and functions that have to be reused in the tests of a testsuite
 - for each test a NEW object of the fixture class is constructed
 - for each test the SetUp() and TearDown() will be called

```cpp
class ObjectTest : public ::testing::Test // Naming Convention: <TestedData/Funcion>Test
{
    proteced:
        void SetUp() override {
            // initalize Members
            // called for each test
            testInt = 0;
        }
        int testInt;
        fixtureFoo(int a) { return a++; }
}

// TEST_F(;FixtureClass>, <TestName>)
TEST_F(ObjectTest, IsIntZero)
{
    EXPECT_EQ(testInt, 0);
}

TEST_F(ObjectTest, IntNotZero)
{
    EXPECT_FALSE(fixtureFoo(testInt) == 0);
}
```

### Shared over the whole test suite
- will only be constructed once when running the test suite
```cpp
class ObjectTest : public ::testing:Test 
{
    protected:
    // Naming from Testcase to Testsuit is changed between the gtest Versions !!!
        static void SetUpTestCase() {
            someConstant = 100;
        }
        static int someConstant;
}
// define the variable so it can be used
int ObjectTest::someConstant;
```

## Parameterized Tests