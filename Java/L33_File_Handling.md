File Handling :

    FileHandling refers to the process of reading file and writing data to file

java.io and java.nio are packages used for file handling and I/O operations in Java

Why do we need to store in file ?

       1. Persistent Storage ---> Unlike variables files retain data even after program exists
       2. Logging and Debugging
       3. Config Management


2 types of ways file is stored

1. Text File (.txt): 
        
        Human readable form
        Stores characters using character encoding format like ASCII/UTF and then convert ASCII to binary this is how text data is stored
        
            .txt stores like this in memory
            Say we have 102 
            
            You store digits as characters:
            
            '1' → 49 → 00110001
            '0' → 48 → 00110000
            '2' → 50 → 00110010
            
            👉 Final:
            
            00110001 00110000 00110010  

If encoding is UTF-8 (modern standard):

    ✔ All languages work correctly

If encoding is ASCII (old system):
    
    ❌ Only basic English works
    ❌ Other languages become garbage

2. Binary file(.dat) :

        Binary files store raw bytes without any character encoding
        .pdf, .exe, .jpg, .mp3, .mp4

102 will be stored as raw binary

        102 = 00000000 00000000 00000000 01100110

It stores images by using pixels

| Part        | Purpose              |
| ----------- | -------------------- |
| Header      | Tells format & rules |
| Pixel data  | Actual image content |
| Compression | Makes file smaller   |

Each pixel contains RGB 

    for example  :
    
        full red (255 0 0)
        white (255 255 255)
        black(0 0 0)

so if you save hello in .txt file it will save in ascii  and convert ascii to binary 
so if you open this as pdf it will show gibbrish
           


**_Java IO Streams :**_
    
    Java IO Streams are used for reading and writing data between a Java program and external source
    Streams is a continuous flow of data elements that can be processed sequentially

2 types of IO Streams

1. Byte Stream
2. Character Stream


java.lang.Object
│
├──────────────────────── BYTE STREAMS ────────────────────────
│
├── java.io.InputStream (abstract)    Read Bytes
│     ├── FileInputStream
│     ├── ByteArrayInputStream
│     ├── PipedInputStream
│     ├── SequenceInputStream
│     ├── ObjectInputStream
│     └── FilterInputStream
│            ├── BufferedInputStream
│            ├── DataInputStream
│            └── PushbackInputStream
│
└── java.io.OutputStream (abstract) Write Bytes
├── FileOutputStream
├── ByteArrayOutputStream
├── PipedOutputStream
├── ObjectOutputStream
└── FilterOutputStream
├── BufferedOutputStream
└── DataOutputStream


──────────────────────────────────────────────────────────────

├──────────────────── CHARACTER STREAMS ──────────────────────
│
├── java.io.Reader (abstract)  Read Characters
│     ├── FileReader
│     ├── InputStreamReader
│     ├── BufferedReader
│     ├── StringReader
│     ├── CharArrayReader
│     └── PushbackReader
│
└── java.io.Writer (abstract) Write Characters
├── FileWriter
├── OutputStreamWriter
├── BufferedWriter
├── StringWriter
├── CharArrayWriter
└── PrintWriter



Java.io.File :

    Java.io.FIle class is used to represent file + directory path names in Java
    It provides methods to create, delete and get info about files and directories

    File Class is used to represent a file/ dir like checking if a file exist, creating or deleting file and getting all info
    about file but it cannot read or write data into the file

| Constructor                         | Description                                |
| ----------------------------------- | ------------------------------------------ |
| `File(String pathname)`             | Creates File object using full path string |
| `File(String parent, String child)` | Creates File using parent + child path     |
| `File(File parent, String child)`   | Creates File using parent File object      |
| `File(URI uri)`                     | Creates File from URI                      |


| Method                | Return Type | What it does                 |
| --------------------- | ----------- | ---------------------------- |
| `exists()`            | `boolean`   | Checks if file/folder exists |
| `createNewFile()`     | `boolean`   | Creates a new empty file     |
| `delete()`            | `boolean`   | Deletes file or empty folder |
| `mkdir()`             | `boolean`   | Creates a directory          |
| `mkdirs()`            | `boolean`   | Creates full directory path  |
| `isFile()`            | `boolean`   | Checks if it is a file       |
| `isDirectory()`       | `boolean`   | Checks if it is a folder     |
| `length()`            | `long`      | Returns file size in bytes   |
| `getName()`           | `String`    | Returns file name            |
| `getPath()`           | `String`    | Returns path as string       |
| `getAbsolutePath()`   | `String`    | Full path from root          |
| `list()`              | `String[]`  | Lists files in directory     |
| `renameTo(File dest)` | `boolean`   | Renames/moves file           |



🔥 2. java.nio.file.Path (Modern replacement of File)

| Method                                    | Return Type | Purpose                  |
| ----------------------------------------- | ----------- | ------------------------ |
| `Paths.get(String first, String... more)` | `Path`      | Creates Path from string |
| `Path.of(String first, String... more)`   | `Path`      | Modern way (Java 11+)    |


    👉 Represents path only (no file operations)
    👉 Immutable
    👉 Used with Files class for operations

| Method                   | Return Type | What it does                          |
| ------------------------ | ----------- | ------------------------------------- |
| `getFileName()`          | `Path`      | Returns file name                     |
| `getParent()`            | `Path`      | Returns parent directory              |
| `getRoot()`              | `Path`      | Root of file system                   |
| `toAbsolutePath()`       | `Path`      | Converts to full path                 |
| `normalize()`            | `Path`      | Removes redundant parts (`..`, `.`)   |
| `resolve(Path other)`    | `Path`      | Combines paths                        |
| `relativize(Path other)` | `Path`      | Gives relative path between two paths |
| `toString()`             | `String`    | String representation                 |


🔥 3. java.nio.file.Files (Utility class)

    👉 Performs actual file operations
    👉 Works with Path
    👉 Modern replacement of File I/O operations

doesnot have constructors it is a utility class


| Method                      | Return Type      | What it does             |
| --------------------------- | ---------------- | ------------------------ |
| `exists(Path)`              | `boolean`        | Checks file existence    |
| `createFile(Path)`          | `Path`           | Creates file             |
| `delete(Path)`              | `void`           | Deletes file             |
| `copy(Path, Path)`          | `Path`           | Copies file              |
| `move(Path, Path)`          | `Path`           | Moves file               |
| `readAllBytes(Path)`        | `byte[]`         | Reads full file as bytes |
| `readString(Path)`          | `String`         | Reads file as string     |
| `write(Path, byte[])`       | `Path`           | Writes bytes             |
| `writeString(Path, String)` | `Path`           | Writes text              |
| `size(Path)`                | `long`           | File size                |
| `newBufferedReader(Path)`   | `BufferedReader` | Buffered reading         |
| `newBufferedWriter(Path)`   | `BufferedWriter` | Buffered writing         |
| `list(Path)`                | `Stream<Path>`   | Lists directory contents |


❌ 1. File mixed two responsibilities

File was doing too much:

Representing a path (location)
Doing operations (delete, rename, list, etc.)

👉 This violates Single Responsibility Principle

❌ 3. Limited error handling

Most File methods return:

true / false

👉 No proper reason for failure

❌ 5. No support for modern I/O performance

Before NIO:

slow file operations
blocking I/O
no streams for directory listing


✅ 5. Non-blocking + scalable design

NIO = New I/O

supports streams
supports async I/O (in advanced APIs)
better performances


---------------------------------------------------------------------------------------------------------------------------------


InputStreams : 
  
    Used for reading binary data

| Method            | Return Type | Description          |
| ----------------- | ----------- | -------------------- |
| `read()`          | `int`       | Reads 1 byte         |
| `read(byte[])`    | `int`       | Reads multiple bytes |
| `available()`     | `int`       | Bytes available      |
| `close()`         | `void`      | Closes stream        |
| `skip(long)`      | `long`      | Skips bytes          |
| `mark(int)`       | `void`      | Marks position       |
| `reset()`         | `void`      | Returns to mark      |
| `markSupported()` | `boolean`   | Checks mark support  |


🟡 1. FileInputStream
📌 Purpose:

    Read data from file as bytes

| Constructor                    | Meaning     |
| ------------------------------ | ----------- |
| `FileInputStream(String name)` | file path   |
| `FileInputStream(File file)`   | File object |



🟡 2. ByteArrayInputStream

👉 Treat memory as input stream

| Constructor                        | Meaning          |
| ---------------------------------- | ---------------- |
| `ByteArrayInputStream(byte[] buf)` | read from memory |


🟡 3. BufferedInputStream

👉 Improves performance (reads in chunks)

| Constructor                           | Meaning              |
| ------------------------------------- | -------------------- |
| `BufferedInputStream(InputStream in)` | wraps another stream |



🟡 4. DataInputStream

👉 Read primitive types directly

| Constructor                       | Meaning     |
| --------------------------------- | ----------- |
| `DataInputStream(InputStream in)` | wrap stream |

| Method        | Return |
| ------------- | ------ |
| `readInt()`   | int    |
| `readFloat()` | float  |
| `readUTF()`   | String |


🟡 5. ObjectInputStream

    👉 Deserialization (objects from file)

| Constructor                         | Meaning |
| ----------------------------------- | ------- |
| `ObjectInputStream(InputStream in)` |         |


| Method         | Return |
| -------------- | ------ |
| `readObject()` | Object |


OutputStream : 

    Used for writing binary data

| Method                    | Return Type | Description          |
| ------------------------- | ----------- | -------------------- |
| `write(int)`              | `void`      | Writes 1 byte        |
| `write(byte[])`           | `void`      | Writes byte array    |
| `write(byte[], int, int)` | `void`      | Writes partial array |
| `flush()`                 | `void`      | Forces write to file |
| `close()`                 | `void`      | Closes stream        |


🟡 1. FileOutputStream

    👉 Write raw bytes to file

| Constructor                                     | Meaning       |
| ----------------------------------------------- | ------------- |
| `FileOutputStream(String name)`                 | write to file |
| `FileOutputStream(File file)`                   | File object   |
| `FileOutputStream(String name, boolean append)` | append mode   |


🟡 2. ByteArrayOutputStream

    👉 Write data into memory instead of file

| Constructor               | Meaning        |
| ------------------------- | -------------- |
| `ByteArrayOutputStream()` | default buffer |



🟡 3. BufferedOutputStream

    👉 Reduces disk writes → faster output

| Constructor                           | Meaning              |
| ------------------------------------- | -------------------- |
| `BufferedInputStream(InputStream in)` | wraps another stream |



🟡 8. DataOutputStream

    👉 Write structured binary data

| Constructor                          | Meaning |
| ------------------------------------ | ------- |
| `DataOutputStream(OutputStream out)` |         |

| Method              | Return |
| ------------------- | ------ |
| `writeInt(int)`     | void   |
| `writeFloat(float)` | void   |



🟡 10. ObjectOutputStream

    👉 Serialize objects into bytes

| Constructor                            | Meaning |
| -------------------------------------- | ------- |
| `ObjectOutputStream(OutputStream out)` |         |


| Method                | Return |
| --------------------- | ------ |
| `writeObject(Object)` | void   |


--------------------------------------------------------------------------------------------------------------------------------


🔹 1. Why Reader exists
    
    👉 InputStream reads bytes (binary data)
    👉 But text needs characters

So Java created:

    Reader → character-based input stream

✔ Handles:
    
    Unicode text (UTF-8)
    Multi-byte characters (Hindi, Chinese, etc.)
    Safer than byte streams for text

🔥 3. BASE CLASS METHODS (Reader)

| Method                   | Return Type | What it does              |
| ------------------------ | ----------- | ------------------------- |
| `read()`                 | `int`       | Reads one character       |
| `read(char[])`           | `int`       | Reads multiple characters |
| `read(char[], int, int)` | `int`       | Partial read              |
| `skip(long)`             | `long`      | Skips characters          |
| `ready()`                | `boolean`   | Checks if ready to read   |
| `mark(int)`              | `void`      | Marks position            |
| `reset()`                | `void`      | Goes back to mark         |
| `markSupported()`        | `boolean`   | Checks mark support       |
| `close()`                | `void`      | Closes stream             |



🔥 4. IMPORTANT READER CLASSES

🟡 FileReader

    👉 Reads text files character-by-character

| Constructor                   | Meaning     |
| ----------------------------- | ----------- |
| `FileReader(String fileName)` | file path   |
| `FileReader(File file)`       | File object |


🟡 BufferedReader

    👉 Fast reading + line-by-line reading

| Constructor                 | Meaning      |
| --------------------------- | ------------ |
| `BufferedReader(Reader in)` | wraps reader |


| Method       | Return |
| ------------ | ------ |
| `readLine()` | String |



🟡 InputStreamReader

    👉 Bridge between byte streams and character streams

| Constructor                         | Meaning              |
| ----------------------------------- | -------------------- |
| `InputStreamReader(InputStream in)` | converts byte → char |


🟡 StringReader

    👉 Treat string as input stream
| Constructor              | Meaning           |
| ------------------------ | ----------------- |
| `StringReader(String s)` | reads from string |



🟡 CharArrayReader

    👉 Allows “unreading” characters (push back)

| Constructor                   | Meaning              |
| ----------------------------- | -------------------- |
| `CharArrayReader(char[] buf)` | read from char array |




🔥 PART 2: java.io.Writer (FULL COMPLETE VIEW)

🔹 1. Why Writer exists
    
    👉 OutputStream writes bytes
    👉 But text needs characters

So Java created:

    Writer → character-based output stream

✔ Handles:
    
    text output
    Unicode safely
    formatting support


| Method                    | Return Type | What it does            |
| ------------------------- | ----------- | ----------------------- |
| `write(int)`              | void        | writes single character |
| `write(char[])`           | void        | writes array            |
| `write(String)`           | void        | writes string           |
| `write(String, int, int)` | void        | partial string          |
| `append(char)`            | Writer      | appends char            |
| `append(CharSequence)`    | Writer      | appends text            |
| `flush()`                 | void        | forces output           |
| `close()`                 | void        | closes stream           |



🔥 4. IMPORTANT WRITER CLASSES

🟡 FileWriter

    👉 Writes text to file
| Constructor                                   | Meaning     |
| --------------------------------------------- | ----------- |
| `FileWriter(String fileName)`                 | file path   |
| `FileWriter(File file)`                       | file object |
| `FileWriter(String fileName, boolean append)` | append mode |


🟡 BufferedWriter

    👉 Faster writing + line support
| Constructor                  | Meaning      |
| ---------------------------- | ------------ |
| `BufferedWriter(Writer out)` | wraps writer |

| Method      | Return |
| ----------- | ------ |
| `newLine()` | void   |


🟡 OutputStreamWriter


    👉 Bridge between Writer and OutputStream

| Constructor                            | Meaning              |
| -------------------------------------- | -------------------- |
| `OutputStreamWriter(OutputStream out)` | converts char → byte |


🟡 PrintWriter (VERY IMPORTANT)

    👉 formatted output (like System.out)

| Constructor                     | Meaning |
| ------------------------------- | ------- |
| `PrintWriter(String fileName)`  | file    |
| `PrintWriter(OutputStream out)` | stream  |

| Method      | Return      |
| ----------- | ----------- |
| `print()`   | void        |
| `println()` | void        |
| `printf()`  | PrintWriter |

🟡 StringWriter

| Constructor      | Meaning                 |
| ---------------- | ----------------------- |
| `StringWriter()` | writes to string buffer |


🟡 CharArrayWriter

| Constructor         | Meaning                     |
| ------------------- | --------------------------- |
| `CharArrayWriter()` | writes to memory char array |


| Problem in Stream          | Solution in Reader/Writer |
| -------------------------- | ------------------------- |
| byte-by-byte text handling | character-based handling  |
| encoding confusion         | Unicode support           |
| complex text processing    | easier APIs               |
| line handling missing      | BufferedReader.readLine() |
| formatted output missing   | PrintWriter               |
