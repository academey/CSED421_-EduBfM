# EduBfM Report

Name: Park Hyunjoon

Student id: 20150968

# Problem Analysis

Implement data structures and operations for buffer managers that keep pages or trains on disk on main memory.

# Design For Problem Solving

## High Level
bufferPool manages the buffer elements currently in use using bufferTable, a data structure for storing information about page/train.

In the process of continuously inquiring, adding, and deleting elements in the data structure, it is necessary to implement an operation to replace and delete elements. In this case, the replacement logic sets the replacement victim through the second chance replacement algorithm.

## Low Level

We are going to use Dirtybit to determine which trainId should be flushed to disk now. If the reference bit of current checking entry (indicated by BI_NEXTVICTIM(type), macro for bufInfo[type].nextVictim) is set, then simply clear the bit for the second chance and proceed to the next entry, otherwise the current buffer indicated by BI_NEXTVICTIM(type) is selected to be returned.
FILL WITH YOUR LOW LEVEL (CODE LEVEL) DESIGN

# Mapping Between Implementation And the Design

- Insert
HashValue was obtained through BFM_HASH, and a value using BI_HASHTABLEENTRY was added to the value of BI_NEXTHASHENTRY. Then, the index entered in BI_HASHTABLEENTRY (type, hashValue) is added.

- Delete 
Likewise, hashValue is obtained through BFM_HASH, and an index starting with BI_NEXTHASHENTRY is obtained. Then, compare the corresponding indexes one space forward until the same key value is obtained. And if prev is NUL, give it to the full value, otherwise, put the removed value as a pointer to the value, resulting in the same result as removed.

- LookUp
Likewise, hashValue is obtained through BFM_HASH, and an index starting with BI_NEXTHASHENTRY is obtained. Then, compare the corresponding indexes one space forward until the same key value is obtained. If i is the same as NIL, it spits out an error of none. Returns the index value at the end of the iteration statement.

- DeleteAll
For both types, insert the NIL while turning the table.

- AllocTrain
Find NEXT_VICTIM and return an error if there is no buffer for the type. Then, check the referral bits of all BITS through the while statement, and then determine victim. And if the dirty bit of the victim is set, flush it to the disk.

- DiscardAll
Go around each type to initialize PageNo and Bits, call deleteAll, and initialize HashTable.

- FlushAll
This is to flush a specific trainId. However, at this time, if there is no table or an error occurs during Wirtrain, the corresponding error is output. Then initialize the DIRTY Bit.

- FreeTrain
This is to free a specific trainId. However, at this time, if it is not in the table or the fixed number is less than 0, a specific string is output and set back to 0.

- ReadTrain
Reads the train from the disk and loads it into a given buffer using functions that are implemented.

- GetTrain
This is to call a specific trainId. If it is not in the table, or if an error occurs during AllocTrain, spit out the error, otherwise set the Fixed bit and the Referer bit.