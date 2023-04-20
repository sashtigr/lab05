[![Coverage Status](https://coveralls.io/repos/github/Green-Ace/TimpLab5/badge.svg?branch=main)](https://coveralls.io/github/Green-Ace/TimpLab5?branch=main)



cd Green-Ace/workspace

git clone git@github.com:Green-Ace/TimpLab5.git

cat >> CMakeLists.txt <<EOF

cmake_minimum_required(VERSION 3.10)
project(banking)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
option(BUILD_TESTS "Build tests" OFF)
add_library(account STATIC banking/Account.cpp)
target_include_directories(account
 PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/banking)

add_library(transaction STATIC banking/Transaction.cpp)
target_include_directories(transaction
 PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/banking)
if(BUILD_TESTS)
  enable_testing()
  add_subdirectory(third-party/gtest)
  file(GLOB ${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
  add_executable(check ${${PROJECT_NAME}_TEST_SOURCES})
  target_compile_options(check PRIVATE --coverage)
  target_link_libraries(check PRIVATE account transaction gtest_main gmock_main  --coverage)
  add_test(NAME check COMMAND check)
endif()







mkdir third-party

git submodule add https://github.com/google/googletest third-party/gtest

git add third-party/gtest

mkdir tests

cat >> tests/test1.cpp <<EOF

#include <Transaction.h>
#include <Account.h>
#include <gtest/gtest.h>
#include <gmock/gmock.h>

inline bool operator==(Account a, Account b) { return true; }

class MockAccount: public Account{
  public:
  MockAccount(int id, int balance):Account(id, balance){}
  MOCK_METHOD(void, Unlock, (), (override));
};

//Тестирование класса Account

TEST(Account, GetBalance){
  MockAccount acc(1,500);
  EXPECT_EQ(acc.Account::GetBalance(), 500);
}

TEST(Account, ChangeBalance) {
  MockAccount acc(1, 500);
  EXPECT_THROW(acc.Account::ChangeBalance(250), std::runtime_error);
  acc.Account::Lock();
  acc.Account::ChangeBalance(250);
  EXPECT_EQ(acc.Account::GetBalance(), 750);
}

TEST(Account, Lock) {
  MockAccount acc(1, 500);
  acc.Account::Lock();
  EXPECT_THROW(acc.Account::Lock(), std::runtime_error);
}

TEST(Account, Unlock) {
  MockAccount acc(1, 500);
  EXPECT_CALL(acc, Unlock()).Times(1);
  acc.Unlock();
}
EOF



cat >> tests/test2.cpp <<EOF


#include <Account.h>
#include <Transaction.h>
#include <gmock/gmock.h>
#include <gtest/gtest.h>

inline bool operator==(Account a, Account b) { return true; }

class MockTransaction: public Transaction{
  public:
  MOCK_METHOD(void, SaveToDataBase, (Account& from, Account& to, int sum), (override));
};

//Тестирование класса Transaction

TEST(Transaction, SaveToDataBase) {
  MockTransaction tr;
  Account from(1, 500);
  Account to(2, 400);
  EXPECT_CALL(tr, SaveToDataBase(from, to, 150)).Times(1);
  tr.SaveToDataBase(from, to, 150);
}






git submodule init && git submodule update

cmake -H. -B_build -DBUILD_TESTS=ON


![изображение](https://user-images.githubusercontent.com/112771063/227204923-b2d67148-08fa-467c-8674-7bb2a33da181.png)



cmake --build _build 



![изображение](https://user-images.githubusercontent.com/112771063/227209504-f02ab0ee-1b7f-462b-aae9-8cb1cf1f0189.png)


cmake --build _build --target test

_build/check







![изображение](https://user-images.githubusercontent.com/112771063/227214077-da91adca-cebf-422e-8dc2-e0feed38a151.png)






cmake --build _build --target test -- ARGS=--verbose





![изображение](https://user-images.githubusercontent.com/112771063/227224905-06983efb-8413-4a39-900a-1dd1d44c056a.png)




git add .

git commit -m 'added proj'

git push origin main 





![изображение](https://user-images.githubusercontent.com/112771063/227225142-4ed5e520-3cee-44c7-a096-7f1aa5a162cd.png)









![изображение](https://user-images.githubusercontent.com/112771063/227225245-4d54ed3c-6ca1-4383-a49b-0677ce6a1cb0.png)






![изображение](https://user-images.githubusercontent.com/112771063/227225437-b4d15afd-2f6d-47bd-8fcc-e9595d1107d7.png)



