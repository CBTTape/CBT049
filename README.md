# CBT049
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. 
Due to amazing work by Alison Zhang and Jake Choi repos are no longer deleted.

```
//***FILE 049 is an old program from Bruce Leland called STATS.     *   FILE 049
//*           This program reports statistics on the internal       *   FILE 049
//*           structure of partitioned datasets and other dataset   *   FILE 049
//*           types.  Its original place was from File 213 of the   *   FILE 049
//*           old CBT Tapes, before Arnie made his wholesale        *   FILE 049
//*           deletions.  (This program came from CBT Tape Version  *   FILE 049
//*           249.)                                                 *   FILE 049
//*                                                                 *   FILE 049
//*           This program is still extremely relevant to us,       *   FILE 049
//*           after lo these many years....                         *   FILE 049
//*                                                                 *   FILE 049
//*           The program is extremely easy to run, and gives       *   FILE 049
//*           you a lot of info.                                    *   FILE 049
//*                                                                 *   FILE 049
//*           email address:   bleland@serena.com                   *   FILE 049
//*                                                                 *   FILE 049
//*       Description:  This program formats information on         *   FILE 049
//*           several types of disk data sets.  It reads            *   FILE 049
//*           through the entire data set and outputs disk          *   FILE 049
//*           track usage, record sizes, counts and other           *   FILE 049
//*           statistics.                                           *   FILE 049
//*                                                                 *   FILE 049
//*           In addition, several data set validity checks are     *   FILE 049
//*           performed during input processing to insure that      *   FILE 049
//*           the data set will be usable by the system for         *   FILE 049
//*           non-EXCP processing.                                  *   FILE 049
//*                                                                 *   FILE 049
//*           If any errors are encountered, the Return Code is     *   FILE 049
//*           set to 4095 (or the program abends); otherwise,       *   FILE 049
//*           the Return Code is set to the minimum of 4094 and     *   FILE 049
//*           the number of tracks which should compress out        *   FILE 049
//*           for partitioned data sets.                            *   FILE 049
//*                                                                 *   FILE 049
//*       Definitions (for Partitioned Data Sets):                  *   FILE 049
//*                                                                 *   FILE 049
//*           a.  Real Member - a non-alias member name which       *   FILE 049
//*               is present in the directory.                      *   FILE 049
//*           b.  Gas Member - a member of a partitioned data       *   FILE 049
//*               set which has been replaced or deleted from       *   FILE 049
//*               the data set.  A gas member does not have an      *   FILE 049
//*               entry in the directory pointing to it; disk       *   FILE 049
//*               storage occupied by gas members is made           *   FILE 049
//*               usable for other members by an IEBCOPY            *   FILE 049
//*               compress operation.                               *   FILE 049
//*                                                                 *   FILE 049
//*               Note: Gas members can be resurrected by the       *   FILE 049
//*               TSO PDS command if it is given the beginning      *   FILE 049
//*               TTR address and a member name.                    *   FILE 049
//*                                                                 *   FILE 049
//*       Program PARM (only the first parm character is            *   FILE 049
//*                     significant; at most one of the             *   FILE 049
//*                     following may be specified):                *   FILE 049
//*                                                                 *   FILE 049
//*           a.  Labelonly  - Label information is to be           *   FILE 049
//*                            formatted but no data set reads      *   FILE 049
//*                            are to be performed (except the      *   FILE 049
//*                            read for any ISAM Format 2           *   FILE 049
//*                            DSCB).                               *   FILE 049
//*           b.  Nogas      - No gas member report is to be        *   FILE 049
//*                            provided for partitioned data        *   FILE 049
//*                            sets.                                *   FILE 049
//*           c.  Errorsonly - Only error messages are to be        *   FILE 049
//*                            output.                              *   FILE 049
//*           d.  Allextents - All extents of the data set are      *   FILE 049
//*                            to be read regardless of the         *   FILE 049
//*                            DS1LSTAR setting.                    *   FILE 049
//*       Operation:                                                *   FILE 049
//*                                                                 *   FILE 049
//*           a.  The program performs a RDJFCB to get the          *   FILE 049
//*               DSName and volume name; an OBTAIN to get the      *   FILE 049
//*               Format 1 DSCB; a DEVTYPE to get the device        *   FILE 049
//*               characteristics; and an OPEN to initialize        *   FILE 049
//*               the data set's Data Extent Block (DEB)            *   FILE 049
//*               information.                                      *   FILE 049
//*           b.  The program formats and outputs DEB and DSCB      *   FILE 049
//*               information.                                      *   FILE 049
//*           c.  The program then reads through the data set       *   FILE 049
//*               and outputs disk track usage, record sizes,       *   FILE 049
//*               counts and other statistics.                      *   FILE 049
//*           d.  Additional processing:                            *   FILE 049
//*               1. For Physical Sequential, Direct or VSAM        *   FILE 049
//*                  data sets, no additional processing is         *   FILE 049
//*                  performed.                                     *   FILE 049
//*               2. For ISAM data sets, the program reads          *   FILE 049
//*                  through the entire data set (there may be      *   FILE 049
//*                  several files of data) and reports on each     *   FILE 049
//*                  file.  also, the program inputs the ISAM       *   FILE 049
//*                  label (Format Two DSCB) record and             *   FILE 049
//*                  provides a data set profile which includes     *   FILE 049
//*                  data set reorganization data and data set      *   FILE 049
//*                  characteristics.                               *   FILE 049
//*               3. For Partitioned Data Sets, if the data set     *   FILE 049
//*                  name and a member name is allocated to the     *   FILE 049
//*                  input data set, the member is processed        *   FILE 049
//*                  like a sequential data set.                    *   FILE 049
//*               4. For other Partitioned Data Sets, the           *   FILE 049
//*                  program compares directory TTR's against       *   FILE 049
//*                  actual disk addresses to provide a report      *   FILE 049
//*                  by gas member:                                 *   FILE 049
//*                  a.  For load libraries, the linkage-edit       *   FILE 049
//*                      date and the names of the first few        *   FILE 049
//*                      CSECTs are provided.                       *   FILE 049
//*                  b.  For other libraries, the first 79          *   FILE 049
//*                      characters of each gas member is           *   FILE 049
//*                      output.                                    *   FILE 049
//*                  Statistics are maintained on the size of       *   FILE 049
//*                  gas and real members and the number of         *   FILE 049
//*                  alias members.  STATS checks for aliases       *   FILE 049
//*                  which have no real entries and apparent        *   FILE 049
//*                  aliases (two real members with the same        *   FILE 049
//*                  TTR).                                          *   FILE 049
//*                                                                 *   FILE 049
```
