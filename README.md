1. Explain what is checksum and the importance of checksum and how hadoop performs checksum
A checksum is an error-detection method the transmitter computes a numerical value according to the number of set or unset bits in a message and sends it along with each message frame. At the receiver end, the same checksum formula is applied to the message frame to retrieve the numerical value. If the received checksum value matches the sent value, the transmission is considered to be successful and error-free. A checksum may also be known as a hash sum. 
Difference in checksum shows that the entire message has not been transmitted or some errors are introduced in the received message. 

The procedure for checksum from messages is called a checksum function and is performed using a checksum algorithm. Efficient checksum algorithms produce different results with large probabilities if messages are corrupted. Parity bits and check digits are special checksum cases suitable for small blocks of data. Few error-correcting codes based on checksums are even capable of recovering the original data. 
The most commonly-used checksum tools include:
1.	cksum - Unix commands generating 32-bit cyclic redundancy check (CRC) and byte count for an input file
2.	md5sum - Unix command generating Message-Digest Algorithm 5 (MD5) sum
3.	jdigest - Java GUI tool generating MD5 and Secure Hash Algorithm (SHA) sums
4.	Jacksum - Java application programming interface that incorporates numerous checksum implementations and permits any number of extensions
5.	jcksum - Java libraries used to calculate checksum using different algorithms

2. Explain the anatomy of file write to HDFS
DistributedFileSystem makes an RPC call to the namenode to create a new file in the filesystem namespace, with no blocks associated with it. The namenode performs some checks if checks pass, the namenode makes a record of the new file. The DistributedFileSystem returns an FSDataOutputStream for the client to start writing data, just as in the read case. FSDataOutputStream wraps a DFSOutputStream, which handles communication with the datanodes and namenode.

3. Explain how HDFS handles failures during file write
When copying data into HDFS, it is important to consider cluster balance. HDFS works best when the file blocks are evenly spread across the cluster, so you want to ensure that distcp doesn’t disrupt this. For example, if you specified -m 1, a single map would do the copy, apart from being slow and not using the cluster resources efficiently which means that the first replica of each block would reside on the node running the map until the disk filled up. The second and third replicas would be spread across the cluster, but this one node would be unbalanced. By having more maps than nodes in the cluster, this problem is avoided. It is not always possible to prevent a cluster from becoming unbalanced.
