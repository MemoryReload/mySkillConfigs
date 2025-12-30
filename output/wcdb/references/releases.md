# Releases

Version history for this repository (35 releases).

## v2.1.15: WCDB 2.1.15
**Published:** 2025-11-11

* Fix build error on Xcode 26
* Support Mac Catalyst
* Fix mmap crash In windows
* Fix some known bugs

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.15)

---

## v2.1.14: WCDB 2.1.14
**Published:** 2025-09-02

* Support 16kb pagesize for Android
* Support using ksp in Kotlin version 2.1.0+
* Fix stack overflow crash
* Fix some known bugs

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.14)

---

## v2.1.13: WCDB 2.1.13
**Published:** 2025-07-03

* Fix compile error of Swift Package Manager
* Fix crash caused by in-transaction handle
* Convert ANSI error msg in windows to UTF8

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.13)

---

## v2.1.12: WCDB 2.1.12
**Published:** 2025-06-12

* Fix build error on Xcode 26 beta
* Fix empty string crash in java/kotlin winq
* Fix linking error on linux x86

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.12)

---

## v2.1.11: WCDB 2.1.11
**Published:** 2025-04-18

* Support to open database in readonly mode
* Support to create in-memory database in Java/Kotlin/C++/Swift
* Fix the exception problem of tableExist method in Java/Kotlin

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.11)

---

## v2.1.10: WCDB 2.1.10
**Published:** 2025-03-05

* Disable checkpoint fullsync in apple platform, it will significantly reduce disk IO
* Add main db file protection to windows
* Reduce memory cost of data compression
* Reduce handle usage of data backup
* Support to detect schema corruption
* Fix some known bugs and compile issues, especially the compile error of Xcode 16.3

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.10)

---

## v2.1.9: WCDB 2.1.9
**Published:** 2024-10-24

* Add lite mode (Set journal mode and synchronous flag to OFF)
* Fix memory leak and other known bugs

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.9)

---

## v2.1.8: WCDB 2.1.8
**Published:** 2024-09-18

* Increase migration speed
* Fix compile error of SwiftPM in Xcode 16

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.8)

---

## v2.1.7: WCDB 2.1.7
**Published:** 2024-08-18

* WCDB C++ supports OpenHarmony OS
* Support to recompress existing data with new configuration
* Support auto vacuum
* Support Swift6
* Fix some compile issues

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.7)

---

## v2.1.6: WCDB 2.1.6
**Published:** 2024-06-19

* Some bugfix for data repair, Java/Kotlin ORM and Swift query
* Fix some compile error and warning

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.6)

---

## v2.1.5: WCDB 2.1.5
**Published:** 2024-05-21

* Support legacy mmicu tokenizer in WCDB Java/Kotlin
* Support to rollback compression
* Improve performance of vacuum
* Add valueOr interface to Optional
* Some bugfix for compression and vacuum

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.5)

---

## v2.1.4: WCDB 2.1.4
**Published:** 2024-05-06

* Fix the compile error of WCDB Java/Koltin caused by minifyEnabled flag
* Add regular memory verification to Zstd dict

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.4)

---

## v2.1.3: WCDB 2.1.3
**Published:** 2024-04-29

* Add ProGuard config for WCDB Java/Kotlin
* Support to build WCDB C++ as a framework on Apple platform with CMake
* Support to insert an array of object pointers or object shared pointers in WCDB C++
* Fix the bug that error cannot be obtained through Database objects

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.3)

---

## v2.1.2: WCDB 2.1.2
**Published:** 2024-03-31

* Some bugfix for data compression
* Fix compile error of WCDB Objc on Carthage
* Fix compile error of WCDB C++ when built as a CMake submodule

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.2)

---

## v2.1.1: WCDB 2.1.1
**Published:** 2024-03-08

* Some bugfix for WCDB C++ and WCDB Swift
* Fix compile error on Mac Catalyst

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.1)

---

## v2.1.0: WCDB 2.1.0
**Published:** 2024-03-07

* Add WCDB Java/Kotlin
* Support data compression
* Support incremental data backup
* Support to configure a filter condition for data migration
* Support to use std::optional and std::shared_ptr in C++ ORM
* C++ ORM supports inheritance
* Add more monitoring capabilities
* WCDB Swift supports Swift Package Manager

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.1.0)

---

## v2.0.4: WCDB 2.0.4
**Published:** 2023-08-23

* Support to asynchronously interrupt database operations
* Optimize the handle management mechanism
* Some bugfix for WCDB C++ in Windows

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.0.4)

---

## v2.0.3: WCDB 2.0.3
**Published:** 2023-08-05

* Transaction supports nesting
* Some bug fix for data migration
* Fix compile error in new Xcode

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.0.3)

---

## v2.0.2.5: WCDB 2.0.2.5
**Published:** 2023-06-13

* Fix compile issue caused by header

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.0.2.5)

---

## v2.0.2: WCDB 2.0.2
**Published:** 2023-06-08

* WCDB C++ supports Windows
* Optimize C++ ORM performance
* Downgrade the minimum supported system version for iPhone

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.0.2)

---

## v2.0.1: WCDB 2.0.1
**Published:** 2023-04-25

* Support databases created by old versions of sqlcipher
* Fix crash of WCDB Swift in x86 emulator
* Support insertOrIgnore

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.0.1)

---

## v2.0.0: WCDB 2.0.0
**Published:** 2023-04-03

* Add WCDB C++
* New implementation of WCDB Swift
* New implementation of Winq
* New implementation of database repair
* Support data migration
* Support FTS5

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v2.0.0)

---

## v1.1.0: WCDB 1.1.0
**Published:** 2023-01-06

#### iOS/macOS
* Support Swift 5 for Xcode 14.
* Fix compile error for SQLCipher 4.1.0.
* Fix some bug.

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v1.1.0)

---

## v1.0.8.2: WCDB 1.0.8.2
**Published:** 2019-03-26

#### iOS/macOS
* Support Swift 4.2 for Xcode 10.2


[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v1.0.8.2)

---

## v1.0.8: WCDB 1.0.8
**Published:** 2018-11-12

## Android

* Support for [Room library](https://developer.android.com/topic/libraries/architecture/room) from Android Jetpack. See README and [Wiki](https://github.com/Tencent/wcdb/wiki/Android-WCDB-%E4%BD%BF%E7%94%A8-Room-ORM-%E4%B8%8E%E6%95%B0%E6%8D%AE%E7%BB%91%E5%AE%9A) for details.

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v1.0.8)

---

## v1.0.7.5: WCDB v1.0.7.5
**Published:** 2018-09-21

* Compatible with Swift 4.2
* Enable FTS5 macro for sqlcipher.

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v1.0.7.5)

---

## v1.0.7: WCDB v1.0.7
**Published:** 2018-08-28

#### iOS/macOS

* Fix nil string bug while inserting empty string.
* Reduce size of binary and provide a non-bitcode scheme by `--configuration WithoutBitcode` for Carthage.
* Avoid conflict between builtin SQLCipher and other SQLite based library(including system library).
* Code template is not installed automatically now. Developers can run `curl https://raw.githubusercontent.com/Tencent/wcdb/master/tools/templates/install.sh -s | sh` to install it manually.

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v1.0.7)

---

## v1.0.6.2: WCDB v1.0.6.2
**Published:** 2018-06-06

## iOS/macOS

It's a bug fixed version. Since Swift 4.1.x contains [bugs](https://github.com/Tencent/wcdb/issues/283) that make WCDB fails to compile, developers should use Xcode 10(with Swift 4.2).

##### Swift
* Compatible with Swift 4.2. The `ColumnEncodable` and `ColumnDecodable` is now changed. Check the code snippet, file template or wiki for the new usage.
* Use `Double` column type for `Date` 

FYI, a refactor is needed to fit the new conditional conformance design of Swift 4.2. We will finish it in next version.

## Android

* Use C++14 and libc++_static runtime on JNI routines.
* Fix "no implementation found" log on libwcdb.so initialization.
* Fix ProGuard rules when importing from AAR package.

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v1.0.6.2)

---

## v1.0.6: WCDB 1.0.6
**Published:** 2018-01-02

## iOS/macOS

It's the first release for WCDB Swift, which contains exactly the same features as the ObjC version, including:

* Object-Relational-Mapping based on Swift 4.0 `Codable` protocol
* WCDB Integrated Language Query
* Multithreading safety and concurrency
* Encryption based on SQLCipher
* Protection for SQL injection
* Full text search
* Corruption recovery
* ...

For further information, please check tutorial on wiki.

## Android

* Migrate to gradle plugin 3.0.0, target API 26, and minimal API 14.
* Support NDK r16, change default toolchain to clang.
* Various bug fixes.

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v1.0.6)

---

## v1.0.5: WCDB 1.0.5
**Published:** 2017-11-08

## iOS

* Builtin full-text search support for ORM.
  ```objc
  WCTProperty *tableProperty = WCTSampleFTSData.PropertyNamed(tableNameFTS).match("Eng*")];

  [databaseFTS getObjectsOfClass:WCTSampleFTSData.class fromTable:tableNameFTS where:tableProperty.match("Eng*")];
  ```
* Support read-only databases.
* Some minor bug fixes and code refactor.

## Android

* Optimize asynchronous checkpointer, greatly improve write performance with WAL and asynchronous checkpointing.
```java
SQLiteDatabase db = SQLiteDatabase.openOrCreateDatabaseInWalMode(...);
db.setAsyncCheckpointEnabled(true);
```
* Add benchmark for asynchronous checkpointer.
* Add connection pooling friendly interface `SQLiteDatabase.setSynchronousMode()` to set database synchronization mode.
* Enable `dbstat` virtual table while compiling.

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v1.0.5)

---

## v1.0.4: WCDB 1.0.4
**Published:** 2017-09-14

## Repair Kit

* Add `sqliterk_cancel` function to cancel ongoing output operations.
* Add corresponding Java interface to cancel operations on Android.

## iOS

* Builtin `WCTColumnCoding` supports all `id<NSCoding>` objects now.
* Compatible with iOS 11.
* `Fullfsync` is used by default for data integrity.
* Add `-initWithExistingTag:` for `WCTDatabase` to get existing database without path.

```objc
WCTDatabase* database = [WCTDatabase [alloc] initWithPath:path];
database.tag = 123;
WCTDatabase* withoutPath = [[WCTDatabase alloc] initWithExistingTag:123];
```

* Some minor bug fixes, performance improvement and code refactor.

## Android

* Add asynchronous checkpointing support and custom checkpointing callback. This can
improve performance in WAL mode.

```java
SQLiteDatabase db = SQLiteDatabase.openOrCreateDatabaseInWalMode(...);

// Use asynchronous checkpointing.
db.setAsyncCheckpointEnabled(true);

// OR use custom checkpointer.
SQLiteCheckpointListener callback = new SQLiteCheckpointListener() {
    //...
};
db.setCheckpointCallback(callback);
```

* Add `SQLiteTrace.onConnectionObtained(...)` interface to trace concurrency performance.
* Add cancelable version of `SQLiteDatabase.execSQL()`. See `CancellationSignal` for details.

```java
CancellationSignal signal = new CancellationSignal();
db.execSQL(longRunningSQL, args, signal);

// on another thread
signal.cancel();
```

* Enable `SQLITE_ENABLE_FTS3_PARENTHESIS` compilation option on SQLCipher, which enables `AND`, `OR` operators in FTS3/4.
* Use `CancellationSignal` for canceling `BackupKit`, `RecoverKit` and `RepairKit` operations. See repair sample for details.
* Add callback interface for `RepairKit` to show progress to the users. See `RepairKit.Callback` and `RepairKit.setCallback()`.
* Do not load `libwcdb.so` if it's already loaded on the first use. This makes WCDB compatible to Tinker framework.
* Various bug fixes.

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v1.0.4)

---

## v1.0.3: WCDB 1.0.3
**Published:** 2017-07-25

## Repair Kit

* Fix INTEGER PRIMARY KEY columns not properly recovered.

## iOS

* Add `WCTColumnCoding` support for all `WCTValue`. Developers can use `id<WCTColumnCoding>` objects for WINQ and all interfaces.
```objc
//WINQ
NSDate *now = [NSDate date];
[database getObjectsOfClass:Message.class fromTable:tableName where:Message.modifedTime==now];

//Interfaces
[database updateAllRowsInTable:tableName 
          onProperty:Message.modifiedTime 
            withValue:[NSDate date]];
```
* Add monitor for all executed SQL to check WINQ correctness.
```objc
//SQL Execution Monitor
[WCTStatistics SetGlobalSQLTrace:^(NSString *sql) {
  NSLog(@"SQL: %@", sql);
}];
```
* Update `WCTTableCoding` XCode file template for the best practice of isolating Objective C++ codes. See Wiki page for details.
* Some minor bug fixes.

## Android

* Add `CursorWindow.windowSize(int)` static method to set or get default size for cursor windows.
* `SQLiteDatabase.dump()` reports IDs for all threads that hold database connections, to aid dead-lock debugging.
* Fix crashing on devices fail to load ICU library.
* Fix `SQLiteTrace.onSQLExecuted(...)` reports negative execution time.

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v1.0.3)

---

## v1.0.2: WCDB 1.0.2
**Published:** 2017-07-06

## iOS

* Performance optimization and benchmark. See Wiki page for details.
* Change builtin `NSData` or `NSMutableData` column coding to raw data format. To be compatible with earlier versions, call 
```objc
[WCTCompatible sharedCompatible].builtinNSDataColumnCodingCompatibleEnabled = YES
```
* Add `attach`, `detach`, `vacuum`, `savepoiint`, `rollback`, `release`, `reindex`, `explain` statement and SQLCipher related pragma to WINQ.
* Remove auto increment for `insertOrReplace`.
* Rename `updateTable` to `updateRowsInTable`, and `statictics`(typo) to `statistics`.
* Some minor bug fixes.

## Android

* Performance optimization and benchmark. See Wiki page for details.
* Expose ProGuard rules to AAR package. Fix crash when minify is enabled in gradle.

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v1.0.2)

---

## v1.0.1: WCDB 1.0.1
**Published:** 2017-06-20

## iOS

* Add CocoaPods support.
* Add iOS 7 and macOS 10.9 support. Apps using WCDB can target iOS 7 now.
* Fix an issue that `[WCTDatabase canOpen]` never return YES.
* Fix an issue that the global tracer return some odd values.
* Add `@autoreleasepool` in `runTransaction` to avoid OOM.

## Android

* Add `x86_64` ABI support.
* Publish debug version of AAR and native symbols. To reference debug version of WCDB library, modify your `build.gradle`

```gradle
dependencies {
    // Append ":debug@aar" to reference debug library.
    compile 'com.tencent.wcdb:wcdb-android:1.0.1:debug@aar'
}
```

* **Device-locking** is available in cipher options. Databases created with device-locking enabled can be only accessed in the same device where the databases are created. Device-locking is currently **in alpha stage**. You can enable it with the following code:

```java
SQLiteCipherSpec cipher = new SQLiteCipherSpec()
        // add the following line to enable device-locking
        .setCipher(SQLiteCipherSpec.CIPHER_DEVLOCK);
SQLiteDatabase db = SQLiteDatabase.openOrCreateDatabase(path, key, cipher, ...);
```

* Various bug fixes.

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v1.0.1)

---

## v1.0.0: WCDB 1.0.0
**Published:** 2017-06-07

Initial release.

[View on GitHub](https://github.com/Tencent/wcdb/releases/tag/v1.0.0)

---

