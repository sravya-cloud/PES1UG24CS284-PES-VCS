## 📸 Screenshots

### Screenshot 1A
![1A](images/1A.png)

### Screenshot 1B
![1B](images/1B.jpeg)

### Screenshot 2A
![2A](images/2A.jpeg)

### Screenshot 2B
![2B](images/2B.jpeg)

### Screenshot 3A
![3A](images/3A.jpeg)

### Screenshot 3B
![3B](images/3B.jpeg)

### Screenshot 4A
![4A](images/4A.jpeg)

### Screenshot 4B
![4B](images/4B.jpeg)

### Screenshot 4C
![4C](images/4C.jpeg)
Answer:

Content-addressable storage (CAS) is a method of storing data where the location or identifier of the data is derived from the content itself, typically using a cryptographic hash function such as SHA-256.

In PES-VCS, every object (blob, tree, commit) is stored based on the hash of its content rather than its filename or path.

 Key Reasons:
1. Data Deduplication

If two files have identical content, they produce the same hash.

Only one copy is stored.
Saves storage space.
Avoids redundancy.

Example:
If file1.txt and file2.txt have same content → only one blob object is created. 
2. Data Integrity & Corruption Detection

Since the hash depends on content:

Even a small change → completely different hash.
When reading an object, its hash is recomputed and verified.

 If mismatch → data corruption detected.

3. Immutability

Objects are never modified after creation.

Any change creates a new object with a new hash
Old data remains intact

 This ensures:

Reliable history
Safe version tracking
4. Efficient Version Control

Instead of copying full files repeatedly:

Only changed content is stored
Unchanged content is reused
 Makes operations like commit fast and storage-efficient.

5. Unique Identification

Each object is uniquely identified by its hash.

No need for filenames
No collision in practice (due to SHA-256)
 Conclusion:

Content-addressable storage provides:

efficiency
integrity
immutability
deduplication

This is the core idea behind Git and PES-VCS, enabling reliable and efficient version control.

 Q6: Why are atomic writes (temp file + rename + fsync) used?

Answer:

Atomic writes ensure that file updates are performed safely and consistently, even in the presence of crashes, power failures, or interruptions.

In PES-VCS, atomic writing is implemented using:

temporary file
fsync()
rename()
 Problem Without Atomic Writes

If we directly write to a file:

Program crashes midway → file becomes corrupted
Partial writes → inconsistent data
Repository may break completely
 Solution: Atomic Write Process
1. Write to Temporary File

Instead of writing directly:

Write data to a temp file (.tmp)
Original file remains unchanged

 Prevents corruption of existing data

2. Flush to Disk (fsync)
fsync(fd);
Forces OS to write buffered data to disk
Ensures data is physically stored

 Prevents loss due to system crash or power failure

3. Atomic Rename
rename(temp_file, actual_file);
Rename operation is atomic (all-or-nothing)
Either:
old file exists OR
new file exists
Never a partial file
 Benefits:
 Consistency

Repository is always in a valid state

 Crash Safety

Even if system crashes:

No partial writes
No corrupted files
 Reliability

Ensures durability of data

🔹 Example in PES-VCS

Used in:

object storage
index file writing
HEAD updates

 Ensures repository never becomes unusable

🔹 Conclusion:

Atomic writes guarantee:

safe updates
data integrity
crash resistance

They are essential for building a robust version control system like PES-VCS.
