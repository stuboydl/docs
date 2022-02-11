# COPY

<PageHeader />

## Description

The jBASE COPY command allows the user to copy specific or selected records from a specified file to the terminal, printer or another file. The DICT keyword can be used to specify that the record or records should be copy from or to a dictionary file. The command may be as:

```
COPY {DICT} filename{,section} {recordlist | *} {(Options}
```
where:

- **filename** is the name of a valid file; must be a supported jBASE file type.
- **section** is the name of a data section
- **recordlist** is the list of record identifiers to be copied. If the **recordlist** is omitted then the active SELECT list, if present, is used. An asterisk (**\***) denotes all records in the file.  
- Options:

| Option | Explanation |
| --- | --- |
| A | Force ASCII mode. Newline becomes attribute mark and vice versa |
| B | Force binary mode. No newline conversion |
| D | Delete record after copied |
| F | Output new page between ids, for T or P option |
| I | Suppress or display records ids as records are copied. Emulation dependent |
| N | Suppress pagination, for T or P option |
| O | Overwrite record in target if it already exists |
| P | Direct record contents to the spooler |
| T | Direct record contents to the terminal |
| S | Suppress line numbers, for T or P option |
| X | Hexadecimal display, for T or P option |

### Alternate syntax

```
COPY FROM {DICT} SourceFilename TO {DICT} TargetFilename {OrigID,{NewID}} {...} {Options}
```

where:

- **SourceFilename** is the file from which the records are copied from
- **TargetFilename** is the file where the records are copied to
- **OrigID** is the record ID to be copied
- **NewID** is the optional destination record ID

... denotes multiple **OrigID,NewID** sets where each set is separated by a space

- Options are as:

| Option | Explanation |
| --- | --- |
| ALL | All records in SourceFilename are copied |
| OVERWRITING | Overwrite TargetFilename records with SourceFilename records |
| DELETING | Delete SourceFilename record after it is copied |
| CRT | Copy records to the screen |
| LPTR | Send output to the spooler |
| NEW.PAGE | Paginates the display |
| NUM.SUPP | Suppresses line numbers, valid only with CRT or LPTR options |
| UPDATING | Copies the record only if the the record already exists in TargetFilename |
| ID.SUP | Suppress output of record IDs, valid only with CRT or LPTR options |
| SQUAWK | Verbose output for the DELETING, OVERWRITING and UPDATING options |
| HEX | Display records in hexadecimal display, valid only with CRT or LPTR options |

## Note

> On Windows, confusion can occur when running in the *sh* or *msh* [emulation mode](./../../jshell/README.md) of the jSHELL. Rather than the expected record copy, COPY will invoke the Windows file copy command when used in either of these modes. The JCOPY command can be used to ensure that the record copy is invoked regardless of jSHELL emulation mode.

The Alternate Syntax is useful when a single command line is required to do the copy.

When copying from a hashed file to a directory type file, most illegal characters in the item IDs will automatically be converted. For example, on Linux, if an ID was ABC/DEF, the slash is illegal in a filename and will be converted on the fly to ABC]2fDEF. Unfortunately, this is not the case for empty string (null) item IDs. jBase COPY will simply halt in the middle of a copy with a meaningless message that requires user intervention. Warning - attempting to script this will provide hours of frustration and hair pulling.

### Examples

```
COPY File1 Record1 (T
```

Copies Record1 from File1 to the terminal.

```
COPY File1 RecordId1 (OD
TO :(DICT File2 RecordId2
```

Copies RecordId1 from File1 to dictionary file File2]D overwriting RecordId2. Once copied the original Record1 is deleted from File1.

```
COPY FROM File1 TO DICT File2 RecordId1,RecordId2 OVERWRITING DELETING
```

Same as previous example, but uses the alternate syntax.

```
JCOPY EMPLOYEE 4090
TO : (WORK
```

Copies record 4090 from the EMPLOYEE file to record 4090 in the file WORK.

```
COPY FROM ORDERS TO ORDER.HISTORY ALL
```

Copies all records from the ORDERS file to the ORDER.HISTORY file

Back to [Files](./../README.md)

  
<PageFooter />
