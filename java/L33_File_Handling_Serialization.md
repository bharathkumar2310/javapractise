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


```java
public static void main(String[] args) throws IOException {

        File file = new File("C:\\Users\\bharathkumar.y\\learn\\cricket.txt.txt");
        File renamedFile = new File("C:\\Users\\bharathkumar.y\\learn\\cricket.txt.txt");

        File file1 = new File("C:\\Users\\bharathkumar.y\\learn\\exists\\desnot_exist.txt");
        File file2 = new File("C:\\Users\\bharathkumar.y\\learn\\exists\\desnot_exist_1.txt");

        if(file.exists()) {
            System.out.println("file" + file.getName() + " exists");
            System.out.println("FileName : " + file.getName());
            System.out.println("Path : " + file.getPath());
            System.out.println("Absolute Path " + file.getAbsolutePath());
            System.out.println("File's Parent : " + file.getParent());
            long fileLength = file.length();
            System.out.println("File length : " + fileLength);
            System.out.println("Is this a file : " + file.isFile());
            System.out.println("can Read :" + file.canRead());
            System.out.println("Can Write :" + file.canWrite());

            long time = file.lastModified();
            System.out.println("File lastModified time " + time);
            boolean fileRenamed = file.renameTo(renamedFile);
            System.out.println("isFileRenamed " + fileRenamed);
            boolean reRenamed = renamedFile.renameTo(file);
            System.out.println("isFileRerenamed " + reRenamed);
        } else {
            System.out.println("file" + file.getName() + " does not exists");
        }
        
        File dir = new File("C:\\Users\\bharathkumar.y\\learn\\exists");
        boolean created = dir.mkdir();
        System.out.println("Exists directory created" + created);

        if(!file1.exists()) {
            boolean isCreated = file1.createNewFile();
            System.out.println("New file created : " + isCreated);
        }
        if(!file2.exists()) {
            boolean isCreated = file2.createNewFile();
            System.out.println("New file created : " + isCreated);
        }
        
        if(dir.isDirectory()) {
            String[] files = dir.list();
            for(String fileEle : files) {
                System.out.println("fileEle : " + fileEle);
            }
            for(File filesEle : dir.listFiles()) {
                System.out.println("Absolute Path " + filesEle.getAbsolutePath());
            }
        }
        boolean isfile1Deleted = file1.delete();
        boolean isfile2Deleted = file2.delete();
        System.out.println("isfile1Deleted : " + isfile1Deleted);
        System.out.println("isfile2Deleted : " + isfile2Deleted);
        
   }


```

Path returns what we have passed in FIle constructor , absollute path returns the actual full path

getCanonicalPath()


      File file = new File("temp\\..\\cricket.txt");

      System.out.println(file.getPath());
      System.out.println(file.getAbsolutePath());
      System.out.println(file.getCanonicalPath());

Suppose current directory is:

      C:\Users\bharathkumar.y\learn

Output:
      
      temp\..\cricket.txt
      C:\Users\bharathkumar.y\learn\temp\..\cricket.txt
      C:\Users\bharathkumar.y\learn\cricket.txt


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

FIle Streams : (Part Of Java.io)

      A File Stream is a stream that connects your Java program to a file so that data can flow between them.

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



| Method                                    | Description                                                                  |
| ----------------------------------------- | ---------------------------------------------------------------------------- |
| `int read()`                              | Reads a single character and returns its Unicode value. Returns `-1` at EOF. |
| `int read(char[] cbuf)`                   | Reads characters into an array. Returns number of characters read.           |
| `int read(char[] cbuf, int off, int len)` | Reads up to `len` characters into array starting at index `off`.             |
| `long skip(long n)`                       | Skips `n` characters.                                                        |
| `boolean ready()`                         | Checks whether the stream is ready to be read.                               |
| `void close()`                            | Closes the reader and releases resources.                                    |
| `void mark(int readAheadLimit)`           | Marks current position (only if supported).                                  |
| `void reset()`                            | Returns to marked position.                                                  |
| `boolean markSupported()`                 | Checks if marking is supported.                                              |


      

🔥 4. IMPORTANT READER CLASSES

🟡 FileReader

    👉 Reads text files character-by-character

| Constructor                   | Meaning     |
| ----------------------------- | ----------- |
| `FileReader(String fileName)` | file path   |
| `FileReader(File file)`       | File object |


      public static void main(String[] args) throws IOException {

        File file = new File("C:\\Users\\bharathkumar.y\\learn\\cricket.txt.txt");
        FileReader reader = new FileReader(file);

        int ch;
        while((ch = reader.read()) != -1) { // returns uncicode value of the char and return -1 and EOF
            System.out.print((char) ch);
        }
        System.out.println("////-------------------------------------------------------------------------------------------------////");
        reader.close();
        FileReader reader1 = new FileReader(file);
        char[] chArr = new char[500];
        int noOfCharRead = reader1.read(chArr); // reads to the length of the char array
        System.out.println("No Of Characters read :" + noOfCharRead);
        for(char c : chArr) {
            System.out.print(c);
        }
        System.out.println();
        System.out.println("////-------------------------------------------------------------------------------------------------////");

        long noOfChartoSkip = reader1.skip(500);

        int noOfCharRead1 = reader1.read(chArr, 5, 100);  // reads 100 character and saves it from index 5
        System.out.println("No Of Characters read 1 :" + noOfCharRead1);
        for(char c : chArr) {
            System.out.print(c);
        }
        System.out.println();
        System.out.println("////-------------------------------------------------------------------------------------------------////");
        reader1.close();
      }



When you create FileReader reader1 = new FileReader(file);

      Java asks OS: “Open this file”
      OS opens the file on disk
      OS gives Java a file handle (ID)
      Pointer is set to START of file


      🔹 1. Locates the file
      
      OS checks:
      
      Does test.txt exist?
      Where is it stored on disk?
      HDD / SSD blocks
      
      If not found → error
      
      🔹 2. Creates an “open file entry”
      
      OS creates a record in memory like:
      
      Open File Table:
      
      FD = 3
      File = test.txt
      Position = 0
      Mode = READ
      
      This is NOT the file itself — just metadata.
      
      🔹 3. Allocates a file descriptor (ID)
      
      OS gives your program something like:
      File Descriptor = 3
      This is how Java refers to the file from now on.
      
      🔹 4. Sets file pointer to start
      Position = 0 (start of file)
      So reading begins from the beginning.
      
      🔹 5. Prepares buffers (memory space)
      
      OS prepares temporary memory areas:
      
      for fast reading from disk
      avoids slow disk access every time


🔹 First: What is the “OS buffer”?

      When a file is read, OS does NOT send data directly from disk to Java.
      Instead, it uses a temporary memory area called:
      
      👉 Kernel Buffer (OS buffer cache)

🔥 So where is this buffer created?
✔ Answer:

      The OS creates buffers in main memory (RAM), inside the kernel space

🧠 Memory is divided into 2 parts
      
      +---------------------------+
      |        USER SPACE         |  ← Java, Python programs
      |   (JVM, FileReader etc.)  |
      +---------------------------+
      |        KERNEL SPACE       |  ← OS (Linux/Windows kernel)
      |   File buffers live here  |
      +---------------------------+

From OS buuffer it goes to JVM buffer 

      OS buffer (kernel memory)
      ↓ copy system call
      JVM buffer (user memory)


🔥 STEP 2: You call read()

      int ch = reader.read();
      Now the real action begins.

🔥 STEP 3: JVM checks internal memory buffer

      Java first asks:
      
      “Do I already have data in memory buffer?”
      
      Case A: buffer has data
      return next character immediately

      Case B: buffer is empty (first time)
      go to OS and ask for data
      🔥 STEP 4: JVM asks OS
      
      JVM says:
      
      “Give me next part of file”
      
      OS responds:
      
      👉 It does NOT send 1 character
      👉 It sends a chunk of data (example 4KB or more)
      
      So OS does:
      
      Disk → read block → send to memory
      
      Now memory has:
      
      H E L L O
      🔥 STEP 5: JVM stores it in buffer
      
      Now Java keeps it in memory:
      
      Buffer in RAM:
      [H][E][L][L][O]
      🔥 STEP 6: JVM returns ONE character
      
      Even though buffer has many characters:
      
      ch = 'H'
      
      So you get only the first character.
      
      🔥 STEP 7: Next read()
      reader.read();
      
      Now JVM says:
      
      “I already have buffer, give next char”
      
      Returns:
      
      E
      
      Then:
      
      L
      L
      O
      🔥 STEP 8: End of file
      
      After last character:
      
      reader.read() → -1
      
      Meaning:
      
      “Nothing left in file”

WHen EOF JVM only sends -1

      
      FileReader
      ↓
      InputStreamReader
      ↓
      FileInputStream
      ↓
      OS file descriptor
      ↓
      Disk

      FileReader = InputStreamReader + FileInputStream (pre-wrapped)

| Class             | Role                                                 |
| ----------------- | ---------------------------------------------------- |
| InputStreamReader | converts bytes → chars                               |
| FileInputStream   | reads bytes from file                                |
| FileReader        | convenience wrapper for file-based character reading |

Without FileReader we need to do like this
      
      new BufferedReader(
      new InputStreamReader(
      new FileInputStream("test.txt")
      )
      );


LIMITATIONS : 

      Disk
      ↓
      OS Page Cache
      ↓
      JVM byte buffer (raw bytes)
      ↓
      InputStreamReader/FileReader
      ↓ (decode bytes → chars)
      Java characters

for each read it needs to use InputStreamReader to convert bytes to char


with buffered reader

      raw bytes
      ↓ decode many at once
      char[] buffer (8192 chars)
      ↓
      read()
      read()
      read()
      read()


BufferedReader also supports readLine

---------------------------------------------------------------------------------------------------------------------------------------


🟡 BufferedReader

    👉 Fast reading + line-by-line reading

| Constructor                 | Meaning      |
| --------------------------- | ------------ |
| `BufferedReader(Reader in)` | wraps reader |


| Method       | Return |
| ------------ | ------ |
| `readLine()` | String |





        File file = new File("C:\\Users\\bharathkumar.y\\learn\\cricket.txt.txt");
        BufferedReader reader = new BufferedReader(new FileReader(file));
        int ch;
        while((ch = reader.read()) != -1) { // returns uncicode value of the char and return -1 and EOF
            System.out.print((char) ch);
        }
        System.out.println("////-------------------------------------------------------------------------------------------------////");
        reader.close();


      File file = new File("C:\\Users\\bharathkumar.y\\learn\\cricket.txt.txt");
      BufferedReader reader = new BufferedReader(new FileReader(file));

      String line;

      while((line = reader.readLine()) != null) {
          System.out.println(line);
      }
      reader.close();




----------------------------------------------------------------------------------------------------------------------------------------
🟡 InputStreamReader

    👉 Bridge between byte streams and character streams

| Constructor                         | Meaning              |
| ----------------------------------- | -------------------- |
| `InputStreamReader(InputStream in)` | converts byte → char |


InputStreamReader sits on top of an InputStream (such as FileInputStream) and converts bytes into Java characters.

      FileInputStream fis = new FileInputStream("data.txt");
      InputStreamReader isr = new InputStreamReader(fis);
      
      Architecture:
      
            File
            ↓
            FileInputStream
            ↓
            InputStreamReader
            ↓
            char
      
      Example:
      
         int ch = isr.read();
         System.out.println((char) ch);
      
      If the file contains:A
      The byte is:65
      InputStreamReader decodes it and returns:'A'

      new BufferedReader(
      new InputStreamReader(
      new FileInputStream("test.txt")
      )
      );



-------------------------------------------------------------------------------------------------------------------------
🟡 StringReader

    👉 Treat string as input stream
| Constructor              | Meaning           |
| ------------------------ | ----------------- |
| `StringReader(String s)` | reads from string |




------------------------------------------------------------------------------------------------------------------------

🟡 CharArrayReader

    👉 Allows “unreading” characters (push back)

| Constructor                   | Meaning              |
| ----------------------------- | -------------------- |
| `CharArrayReader(char[] buf)` | read from char array |


-------------------------------------------------------------------------------------------------------------------------------------------------

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



File file = new File("C:\\Users\\bharathkumar.y\\learn\\cricket_new.txt.txt");
BufferedReader reader = new BufferedReader(new FileReader(file));

        File file1 = new File("C:\\Users\\bharathkumar.y\\learn\\cricket.txt.txt");
        BufferedWriter writer = new BufferedWriter(new FileWriter(file1, true));

        String line;

        while((line = reader.readLine()) != null) {
            writer.write(line);
            writer.newLine();
        }
        reader.close();
        writer.close();

        BufferedReader reader1 = new BufferedReader(new FileReader(file1));

        while((line = reader1.readLine()) != null) {
            System.out.println(line);
        }
        reader1.close();



----------------------------------------------------------------------------------------------------------------

🟡 BufferedWriter

    👉 Faster writing + line support
| Constructor                  | Meaning      |
| ---------------------------- | ------------ |
| `BufferedWriter(Writer out)` | wraps writer |

| Method      | Return |
| ----------- | ------ |
| `newLine()` | void   |


-------------------------------------------------------------------------------------------------------------------------

🟡 OutputStreamWriter


    👉 Bridge between Writer and OutputStream

| Constructor                            | Meaning              |
| -------------------------------------- | -------------------- |
| `OutputStreamWriter(OutputStream out)` | converts char → byte |




--------------------------------------------------------------------------------------------------------------------------
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



---------------------------------------------------------------------------------------------------------------------------------------


Serialization :

      Process by which objects are converted to bytes
      For a class to be serialized it needs to implement Serializable
      If not and when you try try to save to a file it will throw exception(Not Serializable Exception)
      Java cannot automatically serialize every class because many objects represent resources (database connections, threads, sockets, streams, locks, etc.) that cannot be meaningfully saved and recreated. Serializable is an explicit opt-in that tells Java the class's state is suitable for serialization.
     By default classes like String, Integer implements Serializable
     It is just a marker interface
     Primitives no need Serializable as they can be converted directly to bytes



      File file1 = new File("C:\\Users\\bharathkumar.y\\learn\\cricket.txt.txt");
      ObjectOutputStream os = new ObjectOutputStream( new FileOutputStream(file1));
      CricketPlayer shoaibAkhtar = new CricketPlayer(
      "021",
      "Shoaib Akhtar",
      "Pakistan",
      "Fast Bowler",
      "1997-2011",
      394,
      544
      );
      ObjectMapper mapper = new ObjectMapper();
      String json = mapper.writeValueAsString(shoaibAkhtar);

      System.out.println(json);
      os.writeObject(shoaibAkhtar);
      os.close();



2. transient

         Used when you don't want a field to be serialized.

         class Employee implements Serializable {
         
             private String name;
         
             transient String password;
         }

Object:

      Employee e = new Employee();
      e.name = "John";
      e.password = "secret123";

After serialization/deserialization:

      name = John
      password = null

because password was skipped.

Why use transient?

Sensitive data:

      transient String password;
      transient String creditCard;
      
      Temporary/calculated fields:
      
      transient Connection connection;
      
      Database connections cannot be serialized.


------------------------------------------------------------------------------------------------------------------------


writeObject()

      Customizes serialization.
      Normally JVM serializes fields automatically.

You can override that behavior:
      
      private void writeObject(ObjectOutputStream out)
      throws IOException {
      
          out.defaultWriteObject();
      
          System.out.println("Custom serialization");
      }

JVM automatically calls this method during serialization.

Example:
      
      class Employee implements Serializable {
      
          String name;
      
          private void writeObject(ObjectOutputStream out)
                  throws IOException {
      
              System.out.println("Writing object");
      
              out.defaultWriteObject();
          }
      }

When:

      oos.writeObject(emp);

runs, JVM invokes your writeObject().

5. readObject()

Customizes deserialization.
      
      private void readObject(ObjectInputStream in)
      throws IOException, ClassNotFoundException {
      
          in.defaultReadObject();
      
          System.out.println("Reading object");
      }

JVM automatically calls it when deserializing.

Example
      
      class Employee implements Serializable {
      
          String name;
      
          private void readObject(ObjectInputStream in)
                  throws IOException, ClassNotFoundException {
      
              in.defaultReadObject();
      
              System.out.println("Object restored");
          }
      }

When:

ois.readObject();

runs, JVM invokes your readObject().


---------------------------------------------------------------------------------------------------------------------------

serialVersionUID is one of the most commonly asked serialization interview questions.

What is it?

      It's a version number for a serializable class.

      public class CricketPlayer implements Serializable {
      
          private static final long serialVersionUID = 1L;
      
          private String playerId;
          private String name;
      }
Why is it needed?

Imagine:

      Version 1
      public class CricketPlayer implements Serializable {
      
          private static final long serialVersionUID = 1L;
      
          private String name;
      }

You serialize an object:

      oos.writeObject(player);

      File contains object data.

Later you modify the class
      
      public class CricketPlayer implements Serializable {
      
          private static final long serialVersionUID = 1L;
      
          private String name;
          private int runs;
      }

Now you try:

      ois.readObject();

      Since the serialVersionUID is still 1L, JVM treats the classes as compatible.
      The new field gets its default value:
      
      runs = 0
      What if you don't define it?
      
      Java generates one automatically.

Example:

      public class CricketPlayer implements Serializable {
      private String name;
      }

JVM computes a UID based on:

      class name
      fields
      methods
      modifiers

If you later change the class:

      private int runs;

the generated UID changes.

Then:

ois.readObject();

may throw:

      java.io.InvalidClassException

because the stored UID and current UID don't match.

Example

Serialized object:

      serialVersionUID = 1L
      
      Current class:
      
      serialVersionUID = 2L
      
      Result:
      
      InvalidClassException:
      local class incompatible
      
      JVM refuses to deserialize.
      
      Best practice
      
      Always define it explicitly:
      
      private static final long serialVersionUID = 1L;

----------------------------------------------------------------------------------------------------------------------------


PrintWriter

      PrintWriter is used to write formatted text easily.

      PrintWriter is Different
      
      PrintWriter is not tied to files.
      
      It can write to any Writer:
      
      new PrintWriter(new FileWriter("a.txt"));
      new PrintWriter(new StringWriter());
      new PrintWriter(new BufferedWriter(...));
      
      Flow:
      
      PrintWriter
      ↓
      Some Writer
      
      It delegates to whatever Writer you provide.



Instead of:

      BufferedWriter bw =
      new BufferedWriter(new FileWriter("file.txt"));

      bw.write("Runs: ");
      bw.write(String.valueOf(544));

you can do:

      PrintWriter pw =
      new PrintWriter("file.txt");

      pw.println("Runs: " + 544);
Useful methods
      
      pw.print("Hello");
      pw.println("World");
      pw.printf("Runs: %d", 544);

Output:
      
      HelloWorld
      Runs: 544

Example


         BufferedWriter bw =
         new BufferedWriter(
         new FileWriter("data.txt"));
         
         bw.write("Name: ");
         bw.write(name);
         bw.newLine();
         
         bw.write("Age: ");
         bw.write(String.valueOf(age));



      
      PrintWriter pw = new PrintWriter("cricket.txt");
      pw.println("Player: Shoaib Akhtar");
      pw.println("Country: Pakistan");
      
      pw.close();
      
      File:
      
      Player: Shoaib Akhtar
      Country: Pakistan


2. try-with-resources

Before Java 7:

      BufferedReader br = null;

      try {
      br = new BufferedReader(
      new FileReader("file.txt"));
      
      } finally {
      if (br != null) {
      br.close();
      }
      }

Lots of boilerplate.

After Java 7:

      try (BufferedReader br =
      new BufferedReader(
      new FileReader("file.txt"))) {
      
          System.out.println(br.readLine());
      }

Java automatically calls:

      br.close();
      even if an exception occurs.

Why use it?

      Prevents resource leaks.

Bad:

      FileInputStream fis =
      new FileInputStream("file.txt");
      
      // forgot close()
      
      Good:
      
      try (FileInputStream fis =
      new FileInputStream("file.txt")) {
      
      }
      
      Automatically closes.

Interview answer

      try-with-resources automatically closes resources that implement AutoCloseable, reducing boilerplate code and preventing resource leaks.

3. flush() vs close()

         This is asked very often.
         
         flush()
         
         Forces buffered data to be written immediately.
         
         BufferedWriter bw =
         new BufferedWriter(
         new FileWriter("file.txt"));
         
         bw.write("Hello");
         
         bw.flush();
         
         After flush():
         
         Hello
         
         is guaranteed to be written.
         
         But the writer is still usable.
         
         bw.write(" World");
         
         works.
         
         close()
         bw.close();
         
         does two things:
         
         1. flush()
            2. release resource
         
         After close:
         
         bw.write("abc");
         
         throws:
         
         IOException: Stream closed
         Example
         bw.write("Hello");
         
         bw.flush();
         
         bw.write("World");
         
         bw.close();
         
         Result:
         
         HelloWorld


-----------------------------------------------------------------------------------------------------------------------------


Scanner sc = new Scanner(System.in)

String s = sc.nextLine(); the thread waits till Os buffer have data
After we press keyboard the Os will send that to OS Buffer

System.in ---> in System class we have Input stream which reads bytes from the Os buffer

System.in fetches raw data ---> scanner  uses InputStreamReader and converts it to character



----------------------------------------------------------------------------------------------------------------------------

    System.out ---> In system class we have PrintStream 
    println() ---> method of print reader

      What the OS sees
      
      The OS receives bytes:
      72 101 108 108 111
      and sends them to the terminal.
      What the terminal does
      
      The terminal interprets those bytes using an encoding and displays:


      System.out is connected by the JVM to the operating system's standard output (stdout) when the Java program starts.
      
      Before Java Starts
      
      Suppose you run:
      
      java MyProgram
      
      The operating system creates a process for the JVM.
      
      Every process gets three standard streams:
      
      stdin   (Standard Input)
      stdout  (Standard Output)
      stderr  (Standard Error)
      
      Think:
      
      Keyboard  ---> stdin
      Console   <--- stdout
      Console   <--- stderr
      JVM Startup
      
      When the JVM starts, it creates:
      
      System.in
      System.out
      System.err
      
      Internally something like:
      
      System.setIn(...)
      System.setOut(...)
      System.setErr(...)
      
      is executed during JVM initialization.
      
      So:
      
      System.out
      
      becomes a PrintStream whose underlying OutputStream is connected to the OS's stdout.


Object
│
├── InputStream
│   ├── FileInputStream
│   ├── ByteArrayInputStream
│   ├── BufferedInputStream
│   ├── DataInputStream
│   ├── ObjectInputStream
│   ├── SequenceInputStream
│   └── PipedInputStream
│
└── OutputStream
├── FileOutputStream
├── ByteArrayOutputStream
├── BufferedOutputStream
├── DataOutputStream
├── ObjectOutputStream
├── PrintStream
└── PipedOutputStream



Object
│
├── Reader
│   ├── InputStreamReader
│   │    └── FileReader
│   ├── BufferedReader
│   ├── CharArrayReader
│   ├── StringReader
│   └── PipedReader
│
└── Writer
├── OutputStreamWriter
│    └── FileWriter
├── BufferedWriter
├── CharArrayWriter
├── StringWriter
├── PrintWriter
└── PipedWriter