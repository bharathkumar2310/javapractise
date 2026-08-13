# Java File Handling Interview Notes (Part 1)
**Complete Interview Guide**

---

# Table of Contents

1. Introduction
2. Java IO Architecture
3. Byte Streams vs Character Streams
4. File Class
5. FileInputStream
6. FileOutputStream
7. BufferedInputStream
8. BufferedOutputStream
9. FileReader
10. FileWriter
11. BufferedReader
12. BufferedWriter
13. Scanner
14. PrintWriter
15. RandomAccessFile
16. Java NIO (Path & Files)
17. Try-With-Resources
18. Important Exceptions
19. Best Practices
20. Interview Questions

---

# 1. Introduction

Java provides two APIs for file handling.

## Old IO API

```
java.io
```

Introduced in JDK 1.0.

Examples

- File
- FileInputStream
- FileOutputStream
- FileReader
- FileWriter
- BufferedReader
- BufferedWriter
- ObjectInputStream
- ObjectOutputStream

---

## New IO API

```
java.nio.file
```

Introduced in Java 7.

Examples

- Path
- Paths
- Files

Modern Java applications mostly use **Path** and **Files**.

---

# 2. Java IO Architecture

```
                Object

                   |

        ---------------------

        |                   |

 InputStream          OutputStream

        |                   |

 FileInputStream   FileOutputStream

 BufferedInputStream BufferedOutputStream



               Reader

                  |

             FileReader

                  |

          BufferedReader



               Writer

                  |

             FileWriter

                  |

          BufferedWriter
```

---

# 3. Byte Streams vs Character Streams

## Byte Streams

Parent Classes

```
InputStream

OutputStream
```

Used for

- Images
- PDF
- Zip
- Audio
- Video
- Binary files

Everything is read as bytes.

---

## Character Streams

Parent Classes

```
Reader

Writer
```

Used for

- Text files
- CSV
- XML
- JSON
- Java source code

Works using Unicode characters.

---

## Interview Question

### When should we use InputStream?

Whenever data is binary.

Examples

- Image
- PDF
- Excel
- Zip

---

### When should we use Reader?

Whenever data is textual.

Examples

- txt
- csv
- json
- xml

---

# 4. File Class

Package

```java
java.io.File
```

Represents a file or directory path.

Important:

**File does NOT store file contents.**

It only represents metadata.

---

## Creating File Object

```java
File file = new File("sample.txt");
```

No file is created here.

Only Java object is created.

---

## exists()

Checks whether file exists.

```java
File file = new File("sample.txt");

System.out.println(file.exists());
```

Output

```
true

or

false
```

---

## createNewFile()

Creates file physically.

```java
File file = new File("sample.txt");

file.createNewFile();
```

Returns

```
true
```

if newly created.

Returns

```
false
```

if already exists.

Throws IOException.

---

## mkdir()

Creates one directory.

```java
File dir = new File("Notes");

dir.mkdir();
```

---

## mkdirs()

Creates nested directories.

```java
new File("A/B/C").mkdirs();
```

Creates

```
A

|

B

|

C
```

---

## delete()

Deletes file or empty directory.

```java
file.delete();
```

Returns boolean.

---

## renameTo()

```java
File oldFile = new File("a.txt");

File newFile = new File("b.txt");

oldFile.renameTo(newFile);
```

---

## length()

Returns file size.

```java
System.out.println(file.length());
```

Unit

```
bytes
```

---

## isFile()

```java
file.isFile();
```

Returns true if regular file.

---

## isDirectory()

```java
file.isDirectory();
```

Returns true if folder.

---

## canRead()

Checks permission.

---

## canWrite()

Checks permission.

---

## canExecute()

Checks executable permission.

---

## getName()

Returns

```
sample.txt
```

---

## getAbsolutePath()

Returns

```
C:\Users\ABC\Desktop\sample.txt
```

---

## getParent()

Returns parent folder.

---

## lastModified()

Returns timestamp.

---

## list()

Returns

```
String[]
```

Only names.

---

## listFiles()

Returns

```
File[]
```

Actual File objects.

Example

```java
File folder = new File("Notes");

for(File f : folder.listFiles())
{
    System.out.println(f.getName());
}
```

---

# Interview

Difference between

```
exists()

isFile()

isDirectory()
```

| Method | Meaning      |
|----------|--------------|
| exists() | Path exists  |
| isFile() | Regular file |
| isDirectory() | Directory    |

---

# 5. FileInputStream

Reads bytes from file.

Package

```java
java.io.FileInputStream
```

Used for

- Images
- PDFs
- Binary data

---

## Constructor

```java
FileInputStream fis =
new FileInputStream("sample.txt");
```

Throws

```
FileNotFoundException
```

---

## read()

Reads one byte.

```java
int data = fis.read();
```

Returns

```
ASCII value
```

Example

File

```
ABC
```

Output

```
65

66

67
```

---

## Reading Complete File

```java
FileInputStream fis =
new FileInputStream("abc.txt");

int ch;

while((ch=fis.read())!=-1)
{
    System.out.print((char)ch);
}

fis.close();
```

---

## Why int instead of byte?

Because

```
-1
```

indicates EOF.

byte cannot represent all values correctly for EOF.

---

## read(byte[])

Reads multiple bytes.

```java
byte[] arr = new byte[1024];

int count = fis.read(arr);
```

Faster.

---

## available()

Returns remaining bytes.

```java
fis.available();
```

---

# 6. FileOutputStream

Writes bytes.

Constructor

```java
FileOutputStream fos =
new FileOutputStream("abc.txt");
```

Creates file if not exists.

---

## write(int)

```java
fos.write('A');
```

Writes

```
A
```

---

## write(byte[])

```java
String str="Hello";

fos.write(str.getBytes());
```

---

## Append Mode

```java
FileOutputStream fos =
new FileOutputStream("abc.txt",true);
```

Without append

Entire file gets overwritten.

---

## close()

Always close stream.

```java
fos.close();
```

---

# 7. BufferedInputStream

Without buffering

```
Disk

↓

1 byte

↓

Java
```

Every read causes system call.

Very slow.

---

BufferedInputStream

```
Disk

↓↓↓↓↓

Buffer

↓↓↓↓↓

Program
```

Much faster.

---

Constructor

```java
BufferedInputStream bis =
new BufferedInputStream(fis);
```

---

Example

```java
BufferedInputStream bis =
new BufferedInputStream(
new FileInputStream("abc.txt"));

int ch;

while((ch=bis.read())!=-1)
{
    System.out.print((char)ch);
}

bis.close();
```

---

# 8. BufferedOutputStream

Buffers writes.

```java
BufferedOutputStream bos =
new BufferedOutputStream(
new FileOutputStream("abc.txt"));

bos.write("Hello".getBytes());

bos.flush();

bos.close();
```

---

## flush()

Writes buffer immediately.

Does NOT close stream.

---

## close()

Automatically performs

```
flush()

↓

release resource
```

---

Interview

Difference

```
flush()

close()
```

| flush() | close() |
|-----------|-----------|
| Writes buffered data | Writes + releases resource |

---

# 9. FileReader

Reads characters.

Package

```java
java.io.FileReader
```

Example

```java
FileReader fr =
new FileReader("abc.txt");

int ch;

while((ch=fr.read())!=-1)
{
    System.out.print((char)ch);
}

fr.close();
```

---

Use only for

```
Text files
```

---

# 10. FileWriter

Writes text.

Example

```java
FileWriter fw =
new FileWriter("abc.txt");

fw.write("Hello");

fw.close();
```

---

Append mode

```java
new FileWriter("abc.txt",true);
```

---

# Interview

Difference

```
FileWriter

FileOutputStream
```

| FileWriter | FileOutputStream |
|-------------|------------------|
| Characters | Bytes |
| Text | Binary |

---

# 11. BufferedReader

Most commonly used interview class.

Constructor

```java
BufferedReader br =
new BufferedReader(
new FileReader("abc.txt"));
```

---

## read()

Reads one character.

---

## readLine()

Most important method.

```java
String line;

while((line=br.readLine())!=null)
{
    System.out.println(line);
}
```

Returns

```
null
```

at EOF.

---

Why use BufferedReader?

- Fast
- Reads complete lines
- Less disk access

---

# 12. BufferedWriter

Example

```java
BufferedWriter bw =
new BufferedWriter(
new FileWriter("abc.txt"));

bw.write("Hello");

bw.newLine();

bw.write("World");

bw.close();
```

---

Methods

```
write()

newLine()

flush()

close()
```

---

# 13. Scanner

Can read from

- Keyboard
- File

Example

```java
Scanner sc =
new Scanner(new File("abc.txt"));

while(sc.hasNext())
{
    System.out.println(sc.next());
}
```

---

Methods

```
next()

nextLine()

nextInt()

nextDouble()

hasNext()
```

---

Scanner vs BufferedReader

| Scanner | BufferedReader |
|----------|----------------|
| Parsing support | Faster |
| Slower | Faster |
| Easy API | Simple API |

---

# 14. PrintWriter

Convenient text writer.

Example

```java
PrintWriter pw =
new PrintWriter("abc.txt");

pw.println("Hello");

pw.println(100);

pw.println(true);

pw.close();
```

Supports

```
println()

printf()

print()
```

---

# 15. RandomAccessFile

Allows reading and writing from any location.

Constructor

```java
RandomAccessFile raf =
new RandomAccessFile("abc.txt","rw");
```

Modes

```
r

rw
```

---

Move pointer

```java
raf.seek(20);
```

---

Read

```java
raf.read();
```

---

Write

```java
raf.writeBytes("Hello");
```

---

Interview Uses

- Database engines
- Video editing
- Log files
- File systems

---

# 16. Java NIO

Modern API.

Package

```java
java.nio.file
```

---

## Path

```java
Path path =
Paths.get("abc.txt");
```

---

## Files.exists()

```java
Files.exists(path);
```

---

## Files.createFile()

```java
Files.createFile(path);
```

---

## Files.createDirectory()

```java
Files.createDirectory(path);
```

---

## Files.createDirectories()

Creates nested folders.

---

## Files.readString()

```java
String text =
Files.readString(path);
```

---

## Files.readAllLines()

```java
List<String> lines =
Files.readAllLines(path);
```

---

## Files.writeString()

```java
Files.writeString(path,"Hello");
```

---

## Files.copy()

```java
Files.copy(source,target);
```

---

## Files.move()

```java
Files.move(source,target);
```

---

## Files.delete()

```java
Files.delete(path);
```

---

## Files.deleteIfExists()

```java
Files.deleteIfExists(path);
```

---

# Why NIO?

Advantages

- Cleaner API
- Faster
- Better error handling
- Supports symbolic links
- Better file copy performance

Modern Spring Boot projects generally prefer `Path` and `Files` over `File`.

---

# 17. Try-With-Resources

Introduced in Java 7.

Automatically closes resources.

Instead of

```java
FileInputStream fis = new FileInputStream("abc.txt");
try {
    // use fis
} finally {
    fis.close();
}
```

Use

```java
try (FileInputStream fis = new FileInputStream("abc.txt")) {

    int ch;
    while ((ch = fis.read()) != -1) {
        System.out.print((char) ch);
    }

}
```

The stream is closed automatically, even if an exception occurs.

Any class implementing `AutoCloseable` can be used with try-with-resources.

Examples:
- FileInputStream
- FileOutputStream
- BufferedReader
- Scanner
- PrintWriter

---

# 18. Important Exceptions

## FileNotFoundException

Occurs when the specified file does not exist while opening it for reading.

```java
new FileInputStream("missing.txt");
```

---

## IOException

General exception for input/output operations.

Examples:
- Disk full
- Permission denied
- Device failure

---

## EOF

End of file is **not** an exception in most stream APIs.

Instead:

```java
read() == -1
```

or

```java
readLine() == null
```

indicates EOF.

---

# 19. Best Practices

✅ Use **try-with-resources**.

✅ Prefer `Path` and `Files` over `File` for new code.

✅ Use buffered streams for better performance.

✅ Use `Reader`/`Writer` for text files.

✅ Use `InputStream`/`OutputStream` for binary files.

✅ Never assume the uploaded file name is safe.

✅ Close streams properly if not using try-with-resources.

✅ Avoid reading huge files entirely into memory.

---

# 20. Frequently Asked Interview Questions

## Basic

1. Difference between File and Path.
2. Why is NIO preferred?
3. FileInputStream vs FileReader.
4. OutputStream vs Writer.
5. BufferedReader vs Scanner.
6. Why are buffered streams faster?
7. Difference between `flush()` and `close()`.
8. What does `read()` return?
9. Why does `read()` return `int` instead of `byte`?
10. Difference between `list()` and `listFiles()`.

---

## Intermediate

11. How does buffering improve performance?
12. What is RandomAccessFile?
13. What is try-with-resources?
14. Why should we use AutoCloseable?
15. Difference between `mkdir()` and `mkdirs()`.
16. Difference between `delete()` and `deleteIfExists()`.
17. When would you use `Files.readAllLines()`?
18. When should you avoid `Files.readString()`?
19. Why is reading a huge file into memory dangerous?
20. How would you copy a large file efficiently?

---

## Advanced

21. How does Java buffering work internally?
22. Why are system calls expensive?
23. What happens if `flush()` is never called?
24. Does `close()` call `flush()`?
25. Why is `Path` immutable?
26. How does NIO improve scalability?
27. Difference between blocking IO and NIO (high level)?
28. Which classes are thread-safe?
29. How would you process a 10 GB log file?
30. What file-handling APIs are commonly used in Spring Boot?

---

# One-Page Cheat Sheet

| Class | Purpose |
|--------|---------|
| File | Represents a file or directory path |
| Path | Modern path representation |
| Files | Utility methods for file operations |
| FileInputStream | Read bytes |
| FileOutputStream | Write bytes |
| BufferedInputStream | Buffered byte reading |
| BufferedOutputStream | Buffered byte writing |
| FileReader | Read characters |
| FileWriter | Write characters |
| BufferedReader | Read text line by line |
| BufferedWriter | Buffered text writing |
| Scanner | Parse formatted input |
| PrintWriter | Convenient text output |
| RandomAccessFile | Random read/write access |

---

## Interview Summary

For product-based Java backend interviews, you should confidently explain:

- Old IO vs NIO
- Byte streams vs character streams
- Buffered streams and why they are faster
- `File` vs `Path`
- `flush()` vs `close()`
- `try-with-resources`
- Common exceptions (`IOException`, `FileNotFoundException`)
- Efficient handling of large files
- When to use each file-handling class

This is sufficient for the vast majority of Java file-handling interview questions.



# Java Serialization & Externalization Interview Notes (Part 2)
**Complete Interview Guide**

---

# Table of Contents

1. What is Serialization?
2. Why Serialization?
3. How Serialization Works Internally
4. Serializable Interface
5. ObjectOutputStream
6. ObjectInputStream
7. Deserialization
8. transient Keyword
9. static & final Fields
10. serialVersionUID
11. Serialization Inheritance Rules
12. NotSerializableException
13. writeObject() & readObject()
14. Externalizable
15. Serialization vs Externalization
16. Common Uses
17. Best Practices
18. Complete Interview Questions
19. Cheat Sheet

---

# 1. What is Serialization?

Serialization is the process of converting an object's state into a sequence of bytes so that it can be:

- Stored in a file
- Sent over a network
- Cached
- Transferred between JVMs

```
Java Object
     │
     ▼
Serialization
     │
     ▼
Byte Stream
     │
     ▼
File / Network / Database
```

---

# 2. Why Serialization?

Suppose we have

```java
class Employee {
    int id;
    String name;
}
```

When JVM stops,

```
Employee emp = new Employee();
```

is lost because objects exist only in memory.

Serialization allows us to save it permanently.

Example

```
Employee Object

↓

Serialize

↓

employee.ser

↓

Restart JVM

↓

Deserialize

↓

Employee Object
```

---

# Real World Uses

- HTTP Session Replication
- RMI
- Distributed Systems
- Caching
- Saving Game State
- Message Queues
- File Storage

---

# 3. How Serialization Works Internally

Suppose

```java
Employee emp =
new Employee(1,"John");
```

Memory

```
Employee

----------------

id = 1

name = John

----------------
```

Serialization

```
Employee Object

↓

JVM examines object

↓

Converts fields into bytes

↓

Writes bytes

↓

employee.ser
```

Only **object state** is written.

Methods are **never serialized**.

---

# 4. Serializable Interface

Package

```java
java.io.Serializable
```

Definition

```java
public interface Serializable {
}
```

Notice

No methods.

It is called a **Marker Interface**.

---

# Why Marker Interface?

It simply tells the JVM

```
"This class is allowed
to be serialized."
```

Without it

```
NotSerializableException
```

is thrown.

---

Example

```java
import java.io.Serializable;

class Employee implements Serializable {

    int id;

    String name;

}
```

---

# Interview

### Why doesn't Serializable have methods?

Because it is a marker interface.

The JVM only checks

```java
instanceof Serializable
```

before serialization.

---

# 5. ObjectOutputStream

Writes Java objects.

Package

```java
java.io.ObjectOutputStream
```

Example

```java
Employee emp =
new Employee();

ObjectOutputStream out =
new ObjectOutputStream(
new FileOutputStream("emp.ser"));

out.writeObject(emp);

out.close();
```

---

## writeObject()

Serializes object.

```java
out.writeObject(emp);
```

Writes

```
Entire object graph
```

provided referenced objects are also serializable.

---

# 6. ObjectInputStream

Reads serialized objects.

```java
ObjectInputStream in =
new ObjectInputStream(
new FileInputStream("emp.ser"));

Employee emp =
(Employee) in.readObject();

in.close();
```

---

## readObject()

Returns

```
Object
```

Needs casting.

---

# 7. Deserialization

Reverse process.

```
Bytes

↓

ObjectInputStream

↓

Java Object
```

---

Interview

### Does constructor execute?

No.

Object is recreated directly.

Constructor is skipped for serializable classes.

---

Example

```java
class Employee implements Serializable{

    Employee(){

        System.out.println("Constructor");

    }

}
```

Serialization

↓

Deserialization

Output

```
Constructor
```

only once.

---

# 8. transient Keyword

Suppose

```java
class Employee implements Serializable{

    String name;

    String password;

}
```

Password should never be saved.

Use

```java
transient String password;
```

---

Example

```java
class Employee implements Serializable{

    String username;

    transient String password;

}
```

After deserialization

```
username

↓

Restored

password

↓

null
```

---

Interview

### Why transient?

Sensitive information

Examples

- Password
- OTP
- Session Token
- JWT
- API Key
- Cache Data

---

# 9. static & final Fields

## Static

```java
static int companyId;
```

Not serialized.

Reason

Static belongs to class.

Not object.

---

## Final

```java
final int age=20;
```

Serialized.

Because it belongs to object state.

---

Summary

| Field | Serialized? |
|--------|-------------|
| normal | Yes |
| static | No |
| transient | No |
| final | Yes |

---

# 10. serialVersionUID

Every Serializable class has a version number.

```java
private static final long serialVersionUID = 1L;
```

---

Suppose Version 1

```java
class Employee{

String name;

}
```

Serialized.

Later

Version 2

```java
class Employee{

String name;

int age;

}
```

Now JVM detects

```
Class changed
```

May throw

```
InvalidClassException
```

---

Providing explicit UID

```java
private static final long serialVersionUID=1L;
```

helps maintain compatibility.

---

Interview

### Why define serialVersionUID manually?

Because autogenerated values change whenever the compiler detects structural changes, which may break deserialization of previously stored objects.

---

# 11. Serialization and Inheritance

## Case 1

Parent Serializable

Child Serializable

Works.

---

## Case 2

Parent Serializable

Child NOT Serializable

Still works because child inherits Serializable.

---

## Case 3

Parent NOT Serializable

Child Serializable

Works.

BUT

Parent constructor executes during deserialization.

Example

```java
class Person{

    Person(){

        System.out.println("Parent");

    }

}

class Employee extends Person
implements Serializable{

}
```

Deserialization

Output

```
Parent
```

---

Reason

Parent state is recreated using constructor.

---

# 12. NotSerializableException

Suppose

```java
class Address{

}

class Employee implements Serializable{

    Address address;

}
```

Address is not serializable.

Serialization

↓

Exception

```
NotSerializableException
```

---

Solutions

Option 1

Make Address Serializable.

```java
class Address implements Serializable{
}
```

Option 2

Make field transient.

```java
transient Address address;
```

---

# 13. Custom Serialization

Java allows overriding default serialization.

Methods

```java
private void writeObject(
ObjectOutputStream out)
```

```java
private void readObject(
ObjectInputStream in)
```

---

Example

```java
private void writeObject(
ObjectOutputStream out)
throws IOException{

    out.defaultWriteObject();

    System.out.println("Custom logic");

}
```

---

Interview

Used when

- Encrypt password
- Compress data
- Validate fields
- Audit logging

---

# 14. Externalizable

Package

```java
java.io.Externalizable
```

Definition

```java
public interface Externalizable
extends Serializable
```

Unlike Serializable

You must write everything manually.

---

Methods

```java
writeExternal()
```

```java
readExternal()
```

---

Example

```java
class Employee
implements Externalizable{

    public void writeExternal(
    ObjectOutput out)
    throws IOException{

        out.writeInt(id);

        out.writeUTF(name);

    }

    public void readExternal(
    ObjectInput in)
    throws IOException{

        id=in.readInt();

        name=in.readUTF();

    }

}
```

---

Advantages

- Faster
- Full control
- Ignore unnecessary fields

---

Disadvantages

- More code
- Easy to make mistakes

---

# Serializable vs Externalizable

| Serializable | Externalizable |
|--------------|----------------|
| Automatic | Manual |
| Reflection | Programmer controls everything |
| Easy | Complex |
| Common | Rare |
| Slower | Usually faster |
| Minimal code | More code |

---

# 15. Serialization Graph

Suppose

```java
Employee

↓

Address

↓

Country
```

If Employee is serialized,

every referenced object must also be Serializable.

Otherwise

```
NotSerializableException
```

---

# 16. Common Uses

## Session Replication

User logs in.

Session serialized.

Transferred to another server.

---

## Cache

Redis

Ehcache

Hazelcast

store serialized objects.

---

## Messaging

Kafka

RabbitMQ

ActiveMQ

Objects converted to bytes before sending (although JSON/Avro/Protobuf are more common than Java Serialization).

---

## Saving Objects

Example

```
Game Save

↓

Player Object

↓

Serialize

↓

player.save
```

---

# 17. Best Practices

✅ Always define

```java
serialVersionUID
```

---

✅ Never serialize passwords.

Use

```java
transient
```

---

✅ Prefer JSON/Protocol Buffers/Avro for communication between services.

---

✅ Use try-with-resources.

---

✅ Avoid Java Serialization for public APIs due to security concerns.

---

# 18. Frequently Asked Interview Questions

## Basic

1. What is serialization?
2. Why do we need serialization?
3. What is Serializable?
4. Why is Serializable a marker interface?
5. What is deserialization?
6. Difference between serialization and deserialization?
7. Which streams are used?

---

## Intermediate

8. What is transient?
9. Why isn't static serialized?
10. Is final serialized?
11. What is serialVersionUID?
12. What happens if UID changes?
13. What is InvalidClassException?
14. What is NotSerializableException?
15. Can constructor execute during deserialization?

---

## Advanced

16. What is object graph serialization?
17. How does JVM serialize objects internally?
18. Can singleton break because of serialization?
19. How can readResolve() preserve singleton?
20. What are writeObject() and readObject()?
21. What is Externalizable?
22. Serializable vs Externalizable?
23. Why is Java Serialization considered insecure?
24. Why do modern microservices prefer JSON/Avro/Protobuf?
25. How would you serialize a large object efficiently?

---

# 19. Cheat Sheet

| Concept | Description |
|----------|-------------|
| Serializable | Marker interface |
| ObjectOutputStream | Serialize object |
| ObjectInputStream | Deserialize object |
| transient | Skip field |
| static | Not serialized |
| final | Serialized |
| serialVersionUID | Version compatibility |
| Externalizable | Manual serialization |
| writeObject() | Custom serialization |
| readObject() | Custom deserialization |
| NotSerializableException | Object contains non-serializable field |
| InvalidClassException | UID mismatch |

---

# One-Page Interview Summary

### Serialization Flow

```
Object
   │
   ▼
ObjectOutputStream
   │
   ▼
Byte Stream
   │
   ▼
File / Network / Cache
   │
   ▼
ObjectInputStream
   │
   ▼
Object Restored
```

### Important Points to Remember

- `Serializable` is a **marker interface** (no methods).
- Serialization saves the **state** of an object, not its methods.
- Constructors of serializable classes are **not executed** during deserialization.
- `transient` fields are skipped.
- `static` fields are not serialized.
- `final` fields are serialized.
- Always declare `serialVersionUID`.
- If any referenced object is not serializable, `NotSerializableException` occurs.
- `Externalizable` provides complete manual control over serialization.
- For modern distributed systems, prefer JSON, Avro, or Protocol Buffers over Java Serialization.

---

## Interview Tip

For product-based Java backend interviews, you should be able to explain:
- How serialization works internally.
- Why `Serializable` is a marker interface.
- The purpose of `transient` and `serialVersionUID`.
- Object graph serialization.
- `NotSerializableException` and `InvalidClassException`.
- `Externalizable` vs `Serializable`.
- Why Java Serialization is less common in microservices today.



# Spring Boot File Handling Interview Notes (Part 3)
**Complete Interview Guide**

---

# Table of Contents

1. Introduction
2. Multipart Request
3. MultipartFile
4. File Upload Flow
5. Uploading Files
6. Uploading Multiple Files
7. Downloading Files
8. Returning Images
9. Storing Files
10. Database vs Local Disk vs Cloud
11. AWS S3 Integration
12. File Validation
13. Exception Handling
14. Large File Uploads
15. Streaming Files
16. Security Best Practices
17. Production Design
18. Frequently Asked Interview Questions
19. Cheat Sheet

---

# 1. Introduction

File handling is one of the most common Spring Boot interview topics.

Typical operations include:

- Upload files
- Download files
- Store metadata
- Validate files
- Stream large files
- Store in cloud (AWS S3, Azure Blob, GCS)

---

# Typical Architecture

```
Client
   │
   ▼
HTTP Multipart Request
   │
   ▼
Spring Boot Controller
   │
   ▼
MultipartFile
   │
   ▼
Validation
   │
   ▼
Storage Service
   │
   ├────────► Local Disk
   │
   ├────────► Database
   │
   └────────► Cloud Storage
                     │
                     ▼
                 Save Metadata
                     │
                     ▼
                  MySQL/Postgres
```

---

# 2. Multipart Request

Normal request

```
POST

Content-Type:

application/json
```

File upload request

```
POST

Content-Type:

multipart/form-data
```

Multipart request allows multiple parts.

Example

```
----------------

username

bharath

----------------

file

resume.pdf

----------------
```

---

# 3. MultipartFile

Spring converts uploaded files into

```java
MultipartFile
```

Package

```java
org.springframework.web.multipart.MultipartFile
```

---

# Important Methods

## getName()

Returns form field name.

---

## getOriginalFilename()

Returns

```
resume.pdf
```

---

## getContentType()

Returns

```
application/pdf

image/png

image/jpeg
```

---

## getSize()

Returns size

```
bytes
```

---

## isEmpty()

Checks upload.

---

## getBytes()

Returns byte[].

Avoid for huge files.

---

## getInputStream()

Returns InputStream.

Preferred for large files.

---

## transferTo()

Copies uploaded file.

```java
file.transferTo(destination);
```

Most commonly used.

---

# 4. File Upload Flow

```
Browser

↓

Multipart Request

↓

DispatcherServlet

↓

MultipartResolver

↓

MultipartFile

↓

Controller

↓

Validation

↓

Storage Service

↓

Disk / Cloud

↓

Database Metadata
```

---

# 5. Uploading File

Controller

```java
@RestController
@RequestMapping("/files")
public class FileController {

    @PostMapping("/upload")
    public String upload(
        @RequestParam MultipartFile file)
        throws IOException {

        file.transferTo(
            new File("uploads/" +
            file.getOriginalFilename()));

        return "Uploaded";

    }

}
```

---

HTML

```html
<form method="POST"
enctype="multipart/form-data">

<input type="file"
name="file">

<button>Upload</button>

</form>
```

---

# Service Layer

Better practice

```
Controller

↓

Service

↓

Storage
```

Example

```java
@Service
public class FileService{

    public void save(MultipartFile file)
    throws IOException{

        file.transferTo(...);

    }

}
```

---

# 6. Upload Multiple Files

```java
@PostMapping("/upload")

public String upload(

@RequestParam MultipartFile[] files)

throws IOException{

    for(MultipartFile file:files){

        file.transferTo(...);

    }

    return "Done";

}
```

---

Alternative

```java
List<MultipartFile>
```

---

# 7. Download File

Controller

```java
@GetMapping("/download")

public ResponseEntity<Resource>
download()

throws Exception{

Path path=
Paths.get("uploads/report.pdf");

Resource resource=
new UrlResource(path.toUri());

return ResponseEntity.ok()

.header(
HttpHeaders.CONTENT_DISPOSITION,
"attachment; filename=report.pdf")

.body(resource);

}
```

---

Flow

```
Disk

↓

Resource

↓

ResponseEntity

↓

Browser Download
```

---

# 8. Returning Images

```java
@GetMapping("/image")

public ResponseEntity<Resource>
image() throws Exception{

Path path=Paths.get("image.jpg");

Resource resource=
new UrlResource(path.toUri());

return ResponseEntity.ok()

.contentType(MediaType.IMAGE_JPEG)

.body(resource);

}
```

Browser renders image.

---

# 9. Where Should Files Be Stored?

Three choices.

---

## Option 1

Database

```
MySQL

Postgres
```

Store

```
BLOB
```

Advantages

- Transactional
- Backup together
- Easy consistency

Disadvantages

- Database grows quickly
- Slower
- Expensive

Best for

Small files

---

## Option 2

Local Disk

```
uploads/

resume.pdf
```

Advantages

- Fast
- Simple
- Cheap

Disadvantages

- Lost if server crashes
- Difficult in distributed systems

---

## Option 3

Cloud

Examples

```
AWS S3

Azure Blob

Google Cloud Storage
```

Advantages

- Highly available
- Replicated
- Durable
- Scalable
- Cheap

Best production choice.

---

# Database Design

Never store only file.

Store metadata.

Example

```
id

original_name

stored_name

size

content_type

path

uploaded_by

uploaded_at
```

---

# Why Generate New File Name?

Suppose

User1 uploads

```
resume.pdf
```

User2 uploads

```
resume.pdf
```

Old file gets overwritten.

Better

```
UUID

↓

3f8d7c.pdf
```

Save original filename in database.

---

# 10. AWS S3

Production architecture

```
Client

↓

Spring Boot

↓

S3 Bucket

↓

Database Metadata
```

---

Upload

```java
amazonS3.putObject(
bucketName,
key,
inputStream,
metadata);
```

---

Download

```java
amazonS3.getObject(...)
```

---

Delete

```java
amazonS3.deleteObject(...)
```

---

Interview

Why S3?

- Highly available
- Replication
- Virtually unlimited storage
- CDN integration
- Versioning

---

# 11. File Validation

Always validate.

---

## Size

application.properties

```properties
spring.servlet.multipart.max-file-size=10MB

spring.servlet.multipart.max-request-size=20MB
```

---

## Content Type

```java
file.getContentType();
```

Example

```
image/png

application/pdf
```

---

## Extension

```
pdf

png

jpg

jpeg
```

---

Never trust extension alone.

Always verify content type.

---

# 12. Exception Handling

Common exceptions

```
IOException

MaxUploadSizeExceededException

AccessDeniedException
```

---

Global Exception

```java
@RestControllerAdvice

public class GlobalException{

@ExceptionHandler(
MaxUploadSizeExceededException.class)

public String handle(){

return "Too Large";

}

}
```

---

# 13. Large File Upload

Never do

```java
byte[] bytes=file.getBytes();
```

for

```
5GB

10GB
```

Reason

Entire file loads into RAM.

---

Preferred

```java
InputStream input =
file.getInputStream();

Files.copy(
input,
destination);
```

Streaming.

Memory efficient.

---

# 14. Streaming Download

Instead of

```
byte[]
```

Use

```
InputStreamResource

Resource

StreamingResponseBody
```

---

Example

```java
@GetMapping("/download")

public ResponseEntity<InputStreamResource>
download()throws Exception{

InputStream in=
new FileInputStream(file);

return ResponseEntity.ok()

.body(
new InputStreamResource(in));

}
```

---

Why?

Memory efficient.

---

# 15. Security Best Practices

Never trust

```
Filename

Extension

Content-Type
```

Always validate.

---

Generate

```
UUID
```

Never use

```
../../windows/system32
```

This is called

```
Path Traversal Attack
```

---

Store uploads

Outside project folder.

---

Limit upload size.

---

Scan uploaded files for malware (production).

---

Restrict allowed MIME types.

---

Require authentication for uploads/downloads.

---

Avoid exposing absolute file paths.

---

# 16. Production Design

```
Client

↓

Spring Boot

↓

Validation

↓

Generate UUID

↓

Virus Scan

↓

Upload to S3

↓

Save Metadata

↓

Return Download URL
```

---

# Metadata Table

```
FileMetadata

----------------

id

original_name

stored_name

path

bucket

size

content_type

checksum

uploaded_by

created_at

----------------
```

---

Checksum

Useful for

- Duplicate detection
- Integrity verification

---

# 17. Frequently Asked Interview Questions

## Basic

1. What is MultipartFile?
2. Difference between multipart/form-data and application/json?
3. Which annotation receives uploaded file?
4. How do you upload multiple files?
5. How do you download a file?

---

## Intermediate

6. Why use transferTo()?
7. Why use InputStream instead of getBytes()?
8. Where should uploaded files be stored?
9. Database vs Local Disk?
10. Why save metadata separately?
11. How do you validate uploads?
12. How do you limit upload size?
13. How do you handle upload exceptions?

---

## Advanced

14. How would you upload files to AWS S3?
15. Why generate UUID filenames?
16. How do you prevent path traversal?
17. How would you stream a 10GB file?
18. How do you secure download endpoints?
19. How do you scale file uploads across multiple servers?
20. How would you design a document management service?

---

# 18. Real Interview Scenario

### Design a Resume Upload Service

Requirements

- Upload PDF resumes
- Maximum 5 MB
- Store files in S3
- Store metadata in MySQL
- Allow download
- Prevent duplicate filenames

Solution

```
Browser

↓

Multipart Request

↓

Controller

↓

Validate

↓

Generate UUID

↓

Upload to S3

↓

Save Metadata

↓

Return File ID

↓

Download using File ID
```

---

# 19. Cheat Sheet

| Class | Purpose |
|--------|---------|
| MultipartFile | Uploaded file |
| Resource | Download resource |
| UrlResource | File resource |
| InputStreamResource | Stream large files |
| ResponseEntity | HTTP response |
| Files | Java NIO operations |
| Path | Modern file path |

---

## Important MultipartFile Methods

| Method | Purpose |
|---------|---------|
| getOriginalFilename() | Client file name |
| getContentType() | MIME type |
| getSize() | File size |
| isEmpty() | Empty check |
| getBytes() | Entire file in memory |
| getInputStream() | Streaming |
| transferTo() | Save file |

---

## Production Best Practices

✅ Validate file size

✅ Validate MIME type

✅ Generate UUID filenames

✅ Save metadata in DB

✅ Store files in cloud (S3)

✅ Stream large files

✅ Never trust user filenames

✅ Protect download endpoints

✅ Use try-with-resources

✅ Scan uploads for malware

---

# One-Page Interview Summary

### Upload Flow

```
Client
   │
   ▼
Multipart Request
   │
   ▼
MultipartFile
   │
   ▼
Validate
   │
   ├── Size
   ├── MIME Type
   ├── Extension
   ▼
Generate UUID
   │
   ▼
Store File
   │
   ├── Local Disk
   ├── Database (small files)
   └── AWS S3 (recommended)
   │
   ▼
Save Metadata
   │
   ▼
Return Response
```

### Download Flow

```
Client
   │
   ▼
GET /files/{id}
   │
   ▼
Fetch Metadata
   │
   ▼
Locate File
   │
   ▼
Resource
   │
   ▼
ResponseEntity<Resource>
   │
   ▼
Browser Download
```

### Interview Tips

For Java Spring Boot backend interviews, you should confidently explain:

- How `MultipartFile` works.
- Difference between `getBytes()` and `getInputStream()`.
- How `transferTo()` saves uploaded files.
- Why cloud storage (S3) is preferred over local disk.
- How to validate uploads securely.
- How to stream large uploads/downloads.
- How to design a scalable file upload service with metadata stored separately.
- Common security issues such as path traversal, malicious uploads, and oversized files.

This level of knowledge is sufficient for most Spring Boot file-handling interview questions in product-based companies.






# Java File Handling, Serialization & Spring Boot File Handling
# Part 4 - Complete Interview Revision, Internal Working & FAQs

---

# Table of Contents

1. Complete Comparison Tables
2. Internal Working
3. File Upload Lifecycle in Spring Boot
4. Common Mistakes
5. Best Practices
6. Scenario-Based Interview Questions
7. System Design Questions
8. Coding Interview Questions
9. Tricky Interview Questions
10. One-Day Revision Notes
11. One-Page Cheat Sheet
12. Top 50 Interview Questions

---

# 1. Complete Comparison Tables

---

## File vs Path

| File | Path |
|------|------|
| Old IO | Java NIO |
| Mutable API | Immutable |
| Limited features | Rich API |
| Represents path | Represents path |
| Uses File methods | Uses Files utility class |
| Less preferred | Preferred |

---

## InputStream vs Reader

| InputStream | Reader |
|-------------|--------|
| Reads bytes | Reads characters |
| Binary files | Text files |
| Images | TXT |
| PDF | CSV |
| ZIP | JSON |
| Audio | XML |

---

## OutputStream vs Writer

| OutputStream | Writer |
|--------------|--------|
| Bytes | Characters |
| Binary output | Text output |
| Image writing | Text writing |

---

## BufferedReader vs Scanner

| BufferedReader | Scanner |
|----------------|----------|
| Faster | Slower |
| Reads line | Parses tokens |
| No parsing | nextInt(), nextDouble() |
| Better for large files | Better for console input |

---

## FileInputStream vs FileReader

| FileInputStream | FileReader |
|-----------------|------------|
| Bytes | Characters |
| Binary | Text |
| Image | TXT |
| PDF | CSV |

---

## FileOutputStream vs FileWriter

| FileOutputStream | FileWriter |
|-----------------|------------|
| Bytes | Characters |
| Binary | Text |

---

## flush() vs close()

| flush() | close() |
|----------|----------|
| Writes buffered data | Flush + closes stream |
| Can continue writing | Cannot use again |

---

## Serializable vs Externalizable

| Serializable | Externalizable |
|--------------|----------------|
| Automatic | Manual |
| Easy | More code |
| Reflection | Programmer controls everything |
| Most common | Rare |

---

## getBytes() vs getInputStream()

| getBytes() | getInputStream() |
|-------------|------------------|
| Entire file in memory | Streaming |
| Bad for huge files | Best for large files |
| Easy | Memory efficient |

---

## Local Storage vs Database vs Cloud

| Local Disk | Database | Cloud |
|------------|----------|-------|
| Fast | Transactional | Scalable |
| Cheap | Slower | Highly available |
| Single server | Small files | Production |

---

# 2. Internal Working

---

## How FileInputStream Works

```
Program

↓

read()

↓

JVM

↓

Operating System

↓

Disk

↓

Returns byte
```

Every read() performs a system call.

System calls are expensive.

---

## BufferedInputStream

```
Disk

↓↓↓↓↓↓↓↓↓↓

Buffer (8 KB)

↓

Program
```

Instead of reading one byte from disk every time,

Java reads thousands of bytes once.

Huge performance improvement.

---

## Why Buffered Streams Are Faster

Without Buffer

```
1000 bytes

↓

1000 disk reads
```

With Buffer

```
1000 bytes

↓

1 disk read

↓

Memory reads
```

Memory is much faster than disk.

---

## Serialization Internal Flow

```
Object

↓

JVM checks

instanceof Serializable

↓

Collects object state

↓

Converts to bytes

↓

ObjectOutputStream

↓

File
```

---

## Deserialization Flow

```
Bytes

↓

ObjectInputStream

↓

Allocate object

↓

Restore fields

↓

Return object
```

Notice:

Constructor is NOT executed for serializable classes.

---

# 3. Spring Boot Upload Lifecycle

```
Browser

↓

multipart/form-data

↓

DispatcherServlet

↓

MultipartResolver

↓

MultipartFile

↓

Controller

↓

Service

↓

Validation

↓

Storage

↓

Response
```

---

### What is MultipartResolver?

Spring component that parses

```
multipart/form-data
```

into

```
MultipartFile
```

Without it,

Spring cannot understand uploaded files.

Spring Boot auto-configures it.

---

# 4. Common Mistakes

---

## Mistake 1

Using

```java
getBytes()
```

for

```
5 GB file
```

Entire file loads into RAM.

Can cause

```
OutOfMemoryError
```

Better

```java
getInputStream()
```

---

## Mistake 2

Trusting filename.

Bad

```
resume.pdf
```

Good

```
UUID.pdf
```

---

## Mistake 3

Trusting extension

```
virus.exe

↓

rename

↓

virus.pdf
```

Always verify MIME type and, if security matters, inspect the file signature (magic bytes).

---

## Mistake 4

Saving uploads inside

```
src/main/resources
```

Bad.

Use

```
uploads/

or

Cloud Storage
```

---

## Mistake 5

Not closing streams.

Always use

```
try-with-resources
```

---

# 5. Best Practices

### Java IO

- Prefer `Path` and `Files`.
- Use buffered streams.
- Use try-with-resources.
- Use NIO for new projects.

---

### Serialization

- Always declare `serialVersionUID`.
- Use `transient` for sensitive fields.
- Avoid Java Serialization for public APIs.
- Prefer JSON/Avro/Protobuf between services.

---

### Spring Boot

- Validate file size.
- Validate MIME type.
- Generate UUID filenames.
- Store metadata separately.
- Stream large files.
- Use cloud storage in production.

---

# 6. Scenario-Based Interview Questions

---

## Scenario 1

User uploads

```
resume.pdf
```

Another user uploads

```
resume.pdf
```

Problem?

File overwritten.

Solution?

```
UUID

↓

a83c7d.pdf
```

---

## Scenario 2

User uploads

```
10 GB
```

How to upload?

Answer

Streaming

```
InputStream

↓

Files.copy()
```

Never

```
getBytes()
```

---

## Scenario 3

100 servers.

Where should uploads go?

Not local disk.

Use

```
AWS S3

Azure Blob

Google Cloud Storage
```

---

## Scenario 4

Need to store employee photo.

Should we store in database?

Small application

Yes.

Enterprise

Store image in S3.

Database stores path.

---

## Scenario 5

Need download URL.

Flow

```
DB

↓

Path

↓

S3

↓

Download
```

---

# 7. System Design Interview Questions

---

### Design Resume Upload System

Expected answer

```
Browser

↓

Multipart Request

↓

Validation

↓

UUID

↓

S3

↓

Metadata DB

↓

Return File ID
```

---

### Design Photo Service

Need

- Upload
- Download
- Delete
- Versioning
- CDN

---

### Design Document Management System

Need

- Authentication
- Authorization
- Virus Scan
- Metadata
- Search
- Version history
- Audit log

---

# 8. Coding Questions

### Question 1

Copy one file to another.

---

### Question 2

Count lines.

---

### Question 3

Find largest file.

---

### Question 4

Read file using BufferedReader.

---

### Question 5

Merge two files.

---

### Question 6

Serialize Employee object.

---

### Question 7

Deserialize Employee.

---

### Question 8

Upload multiple files.

---

### Question 9

Download image.

---

### Question 10

Validate PDF upload.

---

# 9. Tricky Interview Questions

---

### Why read() returns int?

Need

```
-1
```

to indicate EOF.

---

### Does close() call flush()?

Yes.

---

### Can we call flush() multiple times?

Yes.

---

### Can constructor execute during deserialization?

Serializable class

```
No
```

Non-serializable parent

```
Yes
```

---

### Is Serializable mandatory?

Only if using

```
ObjectOutputStream
```

---

### Can ArrayList be serialized?

Yes.

Most Java Collections implement Serializable.

---

### Is HashMap Serializable?

Yes.

---

### Can Thread be serialized?

No.

---

### Is String Serializable?

Yes.

---

### Can lambda be serialized?

Only if its target functional interface extends `Serializable`.

---

### Is transient inherited?

No.

It applies only to the field where it is declared.

---

### Can static fields be serialized?

No.

---

### Why use Path instead of File?

- Better API
- Cleaner
- More features
- NIO support

---

### Which is faster?

Buffered streams.

---

### Difference between transferTo() and Files.copy()?

`MultipartFile.transferTo()` is a convenience method for saving an uploaded file.

`Files.copy()` is a general Java NIO API that copies data between paths or streams.

---

# 10. One-Day Revision Notes

---

### Java IO

Know

- File
- Path
- Files
- BufferedReader
- BufferedWriter
- FileInputStream
- FileOutputStream
- RandomAccessFile

---

### Serialization

Know

- Serializable
- transient
- serialVersionUID
- ObjectOutputStream
- ObjectInputStream
- Externalizable

---

### Spring Boot

Know

- MultipartFile
- transferTo()
- getInputStream()
- Validation
- Download
- UUID
- S3

---

# 11. One-Page Cheat Sheet

```
File

↓

Path

↓

Files

↓

Buffered Streams

↓

Try-With-Resources

↓

Serializable

↓

ObjectOutputStream

↓

ObjectInputStream

↓

MultipartFile

↓

Validation

↓

UUID

↓

Cloud Storage

↓

Metadata DB
```

---

# 12. Top 50 Interview Questions

### Java IO

1. File vs Path?
2. InputStream vs Reader?
3. OutputStream vs Writer?
4. BufferedReader vs Scanner?
5. Why BufferedReader is faster?
6. What is File?
7. What is Path?
8. What is Files?
9. Why NIO?
10. What is RandomAccessFile?
11. Why read() returns int?
12. flush() vs close()?
13. try-with-resources?
14. AutoCloseable?
15. FileNotFoundException?
16. IOException?
17. mkdir() vs mkdirs()?
18. list() vs listFiles()?
19. Files.copy()?
20. Files.move()?

---

### Serialization

21. What is Serialization?
22. Why Serializable?
23. Marker interface?
24. transient?
25. serialVersionUID?
26. Constructor during deserialization?
27. Object graph?
28. NotSerializableException?
29. InvalidClassException?
30. writeObject()?
31. readObject()?
32. Externalizable?
33. Serializable vs Externalizable?
34. Static field?
35. Final field?

---

### Spring Boot

36. MultipartFile?
37. multipart/form-data?
38. transferTo()?
39. getBytes() vs getInputStream()?
40. Upload multiple files?
41. Download file?
42. File validation?
43. UUID filename?
44. Path traversal attack?
45. Large file upload?
46. Streaming download?
47. Local Disk vs Database vs S3?
48. Why S3?
49. How to design upload service?
50. Production best practices?

---

# Final Interview Summary

If you're preparing for **Java Backend Developer interviews**, you should be comfortable explaining:

### Java File Handling
- Old IO vs NIO
- File vs Path vs Files
- Byte streams vs Character streams
- Buffered streams
- Try-with-resources
- RandomAccessFile

### Serialization
- Serializable
- Marker interfaces
- ObjectOutputStream/ObjectInputStream
- `transient`
- `serialVersionUID`
- `Externalizable`
- Common exceptions

### Spring Boot File Handling
- `MultipartFile`
- File upload/download
- Validation
- Streaming large files
- Metadata design
- UUID filenames
- Local storage vs Database vs S3
- Security best practices

Mastering these topics is enough to confidently answer the vast majority of Java file handling and Spring Boot file upload/download interview questions asked in product-based companies.