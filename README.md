# RoBlob
RoBlob lets you store and retrieve data onto the Roblox DataStore for your favorite games, wheither it's multi-save information to use for storing multiple copies of saved data, moderation data for your favorite Roblox games, or just saving binary forms to keep exploiters and hackers from being able to read your data directly (as it would require further compilation of hacking software).

## How to Install
Simply put the following in your source code:
```
local RoBlob = require(6902125609)
```

## Using RoBlob
This library introduces two types: a Reader object, and a Writer object. Methods in these types can be called in one of the following forms:
```
RoBlob.Readers.MethodName(Blob)
```
Or
```
Blob:MethodName()
```

Please note that `MethodName` does not exist inside the module.

## Constructors
```lua
RoBlob.NewReader(DataStore: GlobalDataStore, Key: string)
```
This function creates a Reader object. By calling this function, RoBlob attempts to read a string obtained by a data store entry. If the request limit has been reached or the operation fails, `nil` is returned.

```lua
RoBlob.NewWriter(DataStore: GlobalDataStore, Key: string)
```
This function creates a Writer object. The DataStore and Key is contained within the object, but to complete it, you must call `Flush` afterwards.

## The `Reader` object

A reader object contains data that was read from the Roblox script itself. This object does not have the `Flush` method, but it stores an index indicating the current position.

### Methods
```lua
RoBlob.Readers.ReadByte(Blob)
```
Attempts to read a single byte (an unsigned integer ranging from `0` to `255`). If the byte was out of range or at the end of the file, this method returns `nil`.

```lua
RoBlob.Readers.ReadUnsigned(Blob)
```
Attempts to read a 4-byte unsigned integer from the blob and returns a value from `0` to `4294967295`.

```lua
RoBlob.Headers.ReadInteger(Blob)
```
Reads an unsigned integer from the blob, and offsets it so it is naturally signed. This is the same as `RoBlob.Readers.ReadUnsigned(Blob) - 2147483648`.

```lua
RoBlob.Readers.ReadDecimal(Blob)
```
Attempts to read an 8-byte decimal number. The first bytes indicate a signed whole number, and the next 4 bytes indicate the decimal part as an unsigned integer.

```lua
RoBlob.Readers.ReadString(Blob)
```
Attempts to read a string, preceded by its length represented as an unsigned 4-byte integer. A 32-byte string would have a length of 36 bytes in a blob.

## The `Writer` object

A writer contains not only the output string, but also the data store and key to use when `Flush` is called.

### Methods
```lua
RoBlob.Writers.WriteByte(Blob, Byte: number)
```
Writes a single unsigned byte into the buffer. The input argument `Byte` is wrapped and truncated to fit exactly `1` byte.

```lua
RoBlob.Writers.WriteUnsigned(Blob, Unsigned: number)
```
Writes a 4-byte unsigned integer into the buffer. The input argument `Unsigned` is wrapped and truncated to fit exactly `4` bytes.
This method returns `true` if successful, and `false` otherwise.

```lua
RoBlob.Writers.WriteInteger(Blob, Signed: number)
```
Writes a 4-byte signed integer into the buffer. The input argument `Signed` is wrapped and truncated to fit exactly `4` bytes, and set an offset before it is written. This method returns `true` if successful, and `false` otherwise.

```lua
RoBlob.Writers.WriteDecimal(Blob, Decimal: number)
```
Writes an 8-byte signed decimal into the buffer. The input argument `Decimal` is wrapped for a `4`-byte whole number sequence, and the decimal part is simply expanded and truncated. This method returns `true` if successful, and `false` otherwise.

```lua
RoBlob.Writers.WriteString(Blob, String: string)
```
Writes the length of the string `String` as an unsigned integer, followed by a `Byte` sequence of individual characters. The string is left unfiltered, so if you are going to display it to others, you will need to filter it beforehand.

