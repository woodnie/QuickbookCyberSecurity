## Hash Total {#hash-total}

| **Illegal nested table :**                 Validation and Verification : Batch and Hash Totals             |
| --- |

Some [information systems](http://ictsmart.tripod.com/ict4/online/artisin.htm) process data that is entered into them in batches from documents. These systems typically use [batch processing](http://ictsmart.tripod.com/ict4/online/artprba.htm). Standard validation checks such as [range](http://ictsmart.tripod.com/ict4/online/artvvra.htm) and [format](http://ictsmart.tripod.com/ict4/online/artvvfo.htm) checks are used to identify typing errors during data entry, but when data is entered in batches there are two extra types of error that may occur :

*   A document may be accidentally missed out.
*   The data on a document is entered twice.

Batch and hash totals are two special validation checks that are used when data is entered in batches to identify if one of these types of error occurs.

**Batch Total**

Before the information on the documents is entered the user counts how many documents there are. This is the batch total.

As the data is entered the computer counts how many documents the data is typed from. After the data has been entered the total that was manually calculated is compared to the computer generated total. If the two differ then a document has been missed or some data has been entered twice.

**Hash Total**

Hash totals are more sophisticated than batch totals. They can be used to spot missed out or doubly entered documents and can also spot more complex errors such as one document being entered twice at the same time as another is missed out. A particular item on the batch of documents that must be entered is chosen. e.g. Hours Worked. The values of this item for each document that is to be input will be added up manually by the user. The total that is calculated is known as the hash total.

The data on the documents will then be entered. The computer will perform the same totalisation automatically on the chosen item. If the computer calculated total and the manually calculated total differ then a mistake has occurred during data entry.

If a batch or hash total identifies an error then the user will have to go back through all of the documents, checking which have been entered and which have not.

**Example**

These three documents recording the number of hours worked by the employees at a firm are to be entered in a batch (note that in a real system the number of documents would be larger than this) :

*   The batch total would be 3 as there are three documents.
*   If the field Hours Worked was chosen to calculate a hash total then the hash total would be 102.5 as 43+22.5+37=102.5
*   If the field Worker ID was chosen to calculate a hash total then the hash total would be 11847 as 0172+9023+2652=11847

The chosen totals would be calculated manually by the user before the data was entered. Probably only one total would be used. Now suppose that the data was entered but by mistake the user missed out the middle document.

*   The computer calculated batch total would be 2\.
*   If the field Hours Worked was chosen to calculate a hash total then the computer calculated hash total would be 80 as 43+37=80
*   If the field Worker ID was chosen to calculate a hash total then the computer calculated hash total would be 2824 as 0172+2652=2824

All of the different methods would spot the mistake as the user calculated and computer calculated totals differ. Suppose instead that the user missed out the middle document but entered the last document twice. The computer calculated totals would then be :

*   Computer batch total is 3\.
*   Computer hash total on Hours Worked is 117 as 43+37+37=117\.
*   Computer hash total on Worker ID is 5476 as 0172+2652+2652=5476\.

The batch total would not spot the mistake as the number of documents entered is correct (3). The hash totals would however spot it as they differ from the user calculated ones.

GCSE ICT Companion 04 - (C) P Meakin 2004