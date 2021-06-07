# Go Language Specification

## Notation

- ### EBNF

  ```go
  Production  = production_name "=" [ Expression ] "." .
  Expression  = Alternative { "|" Alternative } .
  Alternative = Term { Term } .
  Term        = production_name | token [ "…" token ] | Group | Option | Repetition .
  Group       = "(" Expression ")" .
  Option      = "[" Expression "]" .
  Repetition  = "{" Expression "}" .
  ```

  ```go
  |   alternation
  ()  grouping
  []  option (0 or 1 times)
  {}  repetition (0 to n times)
  ```

  

## Source code representation

- ### Characters

  ```go
  newline        = /* the Unicode code point U+000A */ .
  unicode_char   = /* an arbitrary Unicode code point except newline */ .
  unicode_letter = /* a Unicode code point classified as "Letter" */ .
  unicode_digit  = /* a Unicode code point classified as "Number, decimal digit" */ .
  ```

- ### Letters and digits

  ```go
  letter        = unicode_letter | "_" .
  decimal_digit = "0" … "9" .
  binary_digit  = "0" | "1" .
  octal_digit   = "0" … "7" .
  hex_digit     = "0" … "9" | "A" … "F" | "a" … "f" .
  ```

## Lexical elements

- ### Comments

  ```go
  // Line comments
  /* General comments */
  ```

- ### Tokens

- ### Semicolons

- ## Identifiers

  ```go
  // identifiers = letter { letter | unicode_digit }
  a
  _x9
  ThisVariableIsExported
  αβ
  ```

- ### Keywords

  ```go
  break        default      func         interface    select
  case         defer        go           map          struct
  chan         else         goto         package      switch
  const        fallthrough  if           range        type
  continue     for          import       return       var
  ```

- ### Operators and punctuation

  ```go
  +    &     +=    &=     &&    ==    !=    (    )
  -    |     -=    |=     ||    <     <=    [    ]
  *    ^     *=    ^=     <-    >     >=    {    }
  /    <<    /=    <<=    ++    =     :=    ,    ;
  %    >>    %=    >>=    --    !     ...   .    :
       &^          &^=
  ```

- ### Integer literals

  ```go
  int_lit        = decimal_lit | binary_lit | octal_lit | hex_lit .
  decimal_lit    = "0" | ( "1" … "9" ) [ [ "_" ] decimal_digits ] .
  binary_lit     = "0" ( "b" | "B" ) [ "_" ] binary_digits .
  octal_lit      = "0" [ "o" | "O" ] [ "_" ] octal_digits .
  hex_lit        = "0" ( "x" | "X" ) [ "_" ] hex_digits .
  
  decimal_digits = decimal_digit { [ "_" ] decimal_digit } .
  binary_digits  = binary_digit { [ "_" ] binary_digit } .
  octal_digits   = octal_digit { [ "_" ] octal_digit } .
  hex_digits     = hex_digit { [ "_" ] hex_digit } .
  ```

  ```go
  42
  4_2
  0600
  0_600
  0o600
  0O600       // second character is capital letter 'O'
  0xBadFace
  0xBad_Face
  0x_67_7a_2f_cc_40_c6
  170141183460469231731687303715884105727
  170_141183_460469_231731_687303_715884_105727
  
  _42         // an identifier, not an integer literal
  42_         // invalid: _ must separate successive digits
  4__2        // invalid: only one _ at a time
  0_xBadFace  // invalid: _ must separate successive digits
  ```

- ### Floating-point literals

  ```go
  float_lit         = decimal_float_lit | hex_float_lit .
  
  decimal_float_lit = decimal_digits "." [ decimal_digits ] [ decimal_exponent ] |
                      decimal_digits decimal_exponent |
                      "." decimal_digits [ decimal_exponent ] .
  decimal_exponent  = ( "e" | "E" ) [ "+" | "-" ] decimal_digits .
  
  hex_float_lit     = "0" ( "x" | "X" ) hex_mantissa hex_exponent .
  hex_mantissa      = [ "_" ] hex_digits "." [ hex_digits ] |
                      [ "_" ] hex_digits |
                      "." hex_digits .
  hex_exponent      = ( "p" | "P" ) [ "+" | "-" ] decimal_digits .
  ```

  ```go
  0.
  72.40
  072.40       // == 72.40
  2.71828
  1.e+0
  6.67428e-11
  1E6
  .25
  .12345E+5
  1_5.         // == 15.0
  0.15e+0_2    // == 15.0
  
  0x1p-2       // == 0.25
  0x2.p10      // == 2048.0
  0x1.Fp+0     // == 1.9375
  0X.8p-0      // == 0.5
  0X_1FFFP-16  // == 0.1249847412109375
  0x15e-2      // == 0x15e - 2 (integer subtraction)
  
  0x.p1        // invalid: mantissa has no digits
  1p-2         // invalid: p exponent requires hexadecimal mantissa
  0x1.5e-2     // invalid: hexadecimal mantissa requires p exponent
  1_.5         // invalid: _ must separate successive digits
  1._5         // invalid: _ must separate successive digits
  1.5_e1       // invalid: _ must separate successive digits
  1.5e_1       // invalid: _ must separate successive digits
  1.5e1_       // invalid: _ must separate successive digits
  ```

- ### Imaginary literals

  ```go
  imaginary_lit = (decimal_digits | int_lit | float_lit) "i" .
  ```

  ```go
  0i
  0123i         // == 123i for backward-compatibility
  0o123i        // == 0o123 * 1i == 83i
  0xabci        // == 0xabc * 1i == 2748i
  0.i
  2.71828i
  1.e+0i
  6.67428e-11i
  1E6i
  .25i
  .12345E+5i
  0x1p-2i       // == 0x1p-2 * 1i == 0.25i
  ```

- ### Rune literals

  Several backslash excapes allow arbitrary values to be encoded as ASCII text:

  - `\x` followed by exactly two hexadecimal digits;
  - `\u` followed by exactly four hexadecimal digits;
  - `\U` followed by exactly eight hexadecimal digits;
  - a plain backslash `\` followed by exactly three octal digits.

  After a backslash, certain single-character escapes represent special values:

  ```go
  \a   U+0007 alert or bell
  \b   U+0008 backspace
  \f   U+000C form feed
  \n   U+000A line feed or newline
  \r   U+000D carriage return
  \t   U+0009 horizontal tab
  \v   U+000b vertical tab
  \\   U+005c backslash
  \'   U+0027 single quote  (valid escape only within rune literals)
  \"   U+0022 double quote  (valid escape only within string literals)
  ```

  ```go
  rune_lit         = "'" ( unicode_value | byte_value ) "'" .
  unicode_value    = unicode_char | little_u_value | big_u_value | escaped_char .
  byte_value       = octal_byte_value | hex_byte_value .
  octal_byte_value = `\` octal_digit octal_digit octal_digit .
  hex_byte_value   = `\` "x" hex_digit hex_digit .
  little_u_value   = `\` "u" hex_digit hex_digit hex_digit hex_digit .
  big_u_value      = `\` "U" hex_digit hex_digit hex_digit hex_digit
                             hex_digit hex_digit hex_digit hex_digit .
  escaped_char     = `\` ( "a" | "b" | "f" | "n" | "r" | "t" | "v" | `\` | "'" | `"` ) .
  ```

  ```go
  'a'
  'ä'
  '本'
  '\t'
  '\000'
  '\007'
  '\377'
  '\x07'
  '\xff'
  '\u12e4'
  '\U00101234'
  '\''         // rune literal containing single quote character
  'aa'         // illegal: too many characters
  '\xa'        // illegal: too few hexadecimal digits
  '\0'         // illegal: too few octal digits
  '\uDFFF'     // illegal: surrogate half
  '\U00110000' // illegal: invalid Unicode code point
  ```

- ### String literals

    ```go
    string_lit             = raw_string_lit | interpreted_string_lit .
    raw_string_lit         = "`" { unicode_char | newline } "`" .
    interpreted_string_lit = `"` { unicode_value | byte_value } `"` .
    ```

    ```go
    `abc`                // same as "abc"
    `\n
    \n`                  // same as "\\n\n\\n"
    "\n"
    "\""                 // same as `"`
    "Hello, world!\n"
    "日本語"
    "\u65e5本\U00008a9e"
    "\xff\u00FF"
    "\uD800"             // illegal: surrogate half
    "\U00110000"         // illegal: invalid Unicode code point

    // Following examples all represent the same string:
    "日本語"                                 // UTF-8 input text
    `日本語`                                 // UTF-8 input text as a raw literal
    "\u65e5\u672c\u8a9e"                    // the explicit Unicode code points
    "\U000065e5\U0000672c\U00008a9e"        // the explicit Unicode code points
    "\xe6\x97\xa5\xe6\x9c\xac\xe8\xaa\x9e"  // the explicit UTF-8 bytes
    ```

## Constants

- boolean constants
- rune constants
- integer constants
- floating-point constants
- complex constants
- string constants

rune, integer, floating-point, complex constants are collectively called numeric constants.

## Variables

Variables of interface type also have a distinct *dynamic* type, which is the concrete type of the value assigned to the variable at run time.

```go
var x interface{}  // x is nil and has static type interface{}
var v *T           // v has value nil, static type *T
x = 42             // x has value 42 and dynamic type int
x = v              // x has value (*T)(nil) and dynamic type *T
```

## Types

```go
Type      = TypeName | TypeLit | "(" Type ")" .
TypeName  = identifier | QualifiedIdent .
TypeLit   = ArrayType | StructType | PointerType | FunctionType | InterfaceType |
	    SliceType | MapType | ChannelType .
```

*Defined types*—boolean, numeric and string

*Undefined types(It bound to be a Composite types)*—array, struct, pointer, function, interface, slice, map and channel types—may be constructed using type literals.

```go
type (
	A1 = string
	A2 = A1
)

type (
	B1 string
	B2 B1
	B3 []B1
	B4 B3
)
// The underlying type of string, A1, A2, B1 and B2 is string.
// The underlying type of []B1, B3 and B4 is []B1
```

### Type declarations

- Alias declarations

  An alias declaration binds an identifier to the given type.

  ```go
  AliasDecl = identifier "=" Type .
  ```

  Within the scope of the identifier, it serves as an *alias* for the type.

  ```go
  type (
  	nodeList = []*Node  // nodeList and []*Node are identical types
  	Polar    = polar    // Polar and polar denote identical types
  )
  ```

- Type definitions

  A type definition creates a new, distinct type with the same underlying type and operations as the given type, and binds an identifier to it.

  ```go
  TypeDef = identifier Type .
  ```

  The new type is called a *defined type*. It is different from any other type, including the type it is created from.

  ```go
  type (
  	Point struct{ x, y float64 }  // Point and struct{ x, y float64 } are different types
  	polar Point                   // polar and Point denote different types
  )
  
  type TreeNode struct {
  	left, right *TreeNode
  	value *Comparable
  }
  
  type Block interface {
  	BlockSize() int
  	Encrypt(src, dst []byte)
  	Decrypt(src, dst []byte)
  }
  ```

  A *defined type* may have methods associated with it. It does not inherit any methods bound to the given type, but the method set of an interface type or of elements of a composite type remains unchanged.

  ```go
  // A Mutex is a data type with two methods, Lock and Unlock.
  type Mutex struct         { /* Mutex fields */ }
  func (m *Mutex) Lock()    { /* Lock implementation */ }
  func (m *Mutex) Unlock()  { /* Unlock implementation */ }
  
  // NewMutex has the same composition as Mutex but its method set is empty.
  type NewMutex Mutex
  
  // The method set of PtrMutex's underlying type *Mutex remains unchanged,
  // but the method set of PtrMutex is empty.
  type PtrMutex *Mutex
  
  // The method set of *PrintableMutex contains the methods
  // Lock and Unlock bound to its embedded field Mutex.
  type PrintableMutex struct {
  	Mutex
  }
  
  // The method set of *PrintableMutex contains the methods
  // Lock and Unlock bound to its embedded field Mutex.
  // but it can not to call the methods Lock and Unlock directly.
  type UnprintableMutex struct {
      *Mutex
  }
  
  // MyBlock is an interface type that has the same method set as Block.
  type MyBlock Block
  ```

  Type definitions may be used to define different boolean, numeric, or string types and associate methods with them.

  ```go
  type TimeZone int
  
  const (
  	EST TimeZone = -(5 + iota)
  	CST
  	MST
  	PST
  )
  
  func (tz TimeZone) String() string {
  	return fmt.Sprintf("GMT%+dh", tz)
  }
  ```

  ### Methods sets

  The method set of any other type `T` consists of all [methods](https://golang.org/ref/spec#Method_declarations) declared with receiver type `T`. The method set of the corresponding [pointer type](https://golang.org/ref/spec#Pointer_types) `*T` is the set of all methods declared with receiver `*T` or `T` (that is, it also contains the method set of `T`).

  The method set of a type determines the interfaces that the type implements and the methods that can be called using a receiver of that type.

  ### Boolean types

  A *boolean type* represents the set of Boolean truth values denoted by the predeclared constants `true` and `false`.

  The predeclared boolean type is `bool`, it is a defined type.

  ### Numeric types

  ```go
  uint8       the set of all unsigned  8-bit integers (0 to 255)
  uint16      the set of all unsigned 16-bit integers (0 to 65535)
  uint32      the set of all unsigned 32-bit integers (0 to 4294967295)
  uint64      the set of all unsigned 64-bit integers (0 to 18446744073709551615)
  
  int8        the set of all signed  8-bit integers (-128 to 127)
  int16       the set of all signed 16-bit integers (-32768 to 32767)
  int32       the set of all signed 32-bit integers (-2147483648 to 2147483647)
  int64       the set of all signed 64-bit integers (-9223372036854775808 to 9223372036854775807)
  
  float32     the set of all IEEE-754 32-bit floating-point numbers
  float64     the set of all IEEE-754 64-bit floating-point numbers
  
  complex64   the set of all complex numbers with float32 real and imaginary parts
  complex128  the set of all complex numbers with float64 real and imaginary parts
  
  byte        alias for uint8
  rune        alias for int32
  ```

  ### String types

  The length of the string is never negative. String are immutable: once created, it is impossible to change the contents of string. The predeclared string type is `string`; it is a defined type.

  A string's bytes can be a accessed by integer by integer indices 0 through `len(s)-1`. It is illegal to take the address of string's bytes.

  ### Array types

  The length of array is never negative. The emements can be addressed by integer indices 0 through `len(a)-1`.

  ```go
  ArrayType   = "[" ArrayLength "]" ElementType .
  ArrayLength = Expression .
  ElementType = Type .
  ```

  ```go
  [32]byte
  [2*N] struct { x, y int32 }
  [1000]*float64
  [3][5]int
  [2][2][2]float64  // same as [2]([2]([2]float64))
  ```

  ### Slice types

  A slice is a descriptor for a contiguous segment of an *underlying array* and provides access to a numbered sequence of elements from that array. The length of slice is never negative. The value of an uninitialized slice is `nil`.

  ```go
  SliceType = "[" "]" ElementType .
  ```

  unlike with arrays it may change during execution. The elements can be addressed by integer indices 0 through `len(s)-1`.

  A slice shares storage with its (underlying) array and with other slices of the same (underlying) array. by contrast, distinct arrays always represent distinct storage.

  The *capacity* is measure of slice extent; it is the sum of the length of the slice and the length of the array beyond the slice. It will be extended from past the end of the slice if the capacity of slice is overload. a slice of length up to that capacity can be created by *slicing* a new one from the original slice. Using the built-in function `cap(a)` to discover the capacity of a slice.

  A new, initialized slice value for a given element type `T` is made using the built-in function make, which takes a slice type and parameters specifying the length and optionally the capacity. A slice created with `make` always allocates a new , hidden array to which the returned slice value refers. That is, executing(connect with following)

  ```go
  make([]T, length, capacity)
  ```

  produces the same slice as allocating an array and  *slicing* it, so these two expressions are equivalent:

  ```go
  make([]int, 50, 100)
  new([100]int)[0:50]
  ```

  The inner slices must be initialized individually in case of slices of slices(or arrays of slices).

  ### Struct types

  The field names of struct may be specified explicitly (IdentifierList) or implicitly (EmbeddedField) and *non-blank* field names must be unique.

  ```go
  StructType    = "struct" "{" { FieldDecl ";" } "}" .
  FieldDecl     = (IdentifierList Type | EmbeddedField) [ Tag ] .
  EmbeddedField = [ "*" ] TypeName .
  Tag           = string_lit .
  ```

  ```go
  // An empty struct.
  struct {}
  
  // A struct with 6 fields.
  struct {
  	x, y int
  	u float32
  	_ float32  // padding
  	A *[]int
  	F func()
  }
  ```

  *embedded field*:

  ```go
  // A struct with four embedded fields of types T1, *T2, P.T3 and *P.T4
  struct {
  	T1        // field name is T1
  	*T2       // field name is T2
  	P.T3      // field name is T3
  	*P.T4     // field name is T4
  	x, y int  // field names are x and y
  }
  ```

  illegal declaration:

  ```go
  struct {
  	T     // conflicts with embedded field *T and *P.T
  	*T    // conflicts with embedded field T and *P.T
  	*P.T  // conflicts with embedded field T and *T
  }
  ```

  TODO: Promoted fields.

  The tags are made visible through a reflection interface and take part in type identity for structs but are otherwise ignored.

  ```go
  struct {
  	x, y float64 ""  // an empty tag string is like an absent tag
  	name string  "any string is permitted as a tag"
  	_    [4]byte "ceci n'est pas un champ de structure"
  }
  
  // A struct corresponding to a TimeStamp protocol buffer.
  // The tag strings define the protocol buffer field numbers;
  // they follow the convention outlined by the reflect package.
  struct {
  	microsec  uint64 `protobuf:"1"`
  	serverIP6 uint64 `protobuf:"2"`
  }
  ```

  ### Pointer types

  A pointer type denotes the set of all pointers to variables of a given type, called the *base type* of the pointer. The value of an uninitialized pointer is `nil`.

  ```go
  PointerType = "*" BaseType .
  BaseType    = Type .
  ```

  ```go
  *Point
  *[4]int
  ```

  ### Function types

  The value of an uninitialized variable of function type is `nil`.

  ```go
  FunctionType   = "func" Signature .
  Signature      = Parameters [ Result ] .
  Result         = Parameters | Type .
  Parameters     = "(" [ ParameterList [ "," ] ] ")" .
  ParameterList  = ParameterDecl { "," ParameterDecl } .
  ParameterDecl  = [ IdentifierList ] [ "..." ] Type .
  ```

  Within a list of parameters of results, the names (IdentifierList) must either all be present or all be absent.

  ```go
  func()
  func(x int) int
  func(a, _ int, z float32) bool
  func(a, b int, z float32) (bool)
  func(prefix string, values ...int)
  func(a, b int, z float64, opt ...interface{}) (success bool)
  func(int, int, float64) (float64, *[]int)
  func(n int) func(p *T)
  ```

  ### Interface types

  The value of an uninitialized variable of interface type is `nil`.

  ```go
  InterfaceType      = "interface" "{" { ( MethodSpec | InterfaceTypeName ) ";" } "}" .
  MethodSpec         = MethodName Signature .
  MethodName         = identifier .
  InterfaceTypeName  = TypeName .
  ```

  ```go
  // A simple File interface.
  interface {
  	Read([]byte) (int, error)
  	Write([]byte) (int, error)
  	Close() error
  }
  
  interface {
  	String() string
  	String() string  // illegal: String not unique
  	_(x int)         // illegal: method must have non-blank name
  }
  ```

  More than one type may implement an interface. For instance, if two types S1 and S2 have the method set

  ```go
  func (p T) Read(p []byte) (n int, err error)
  func (p T) Write(p []byte) (n int, err error)
  func (p T) Close() error
  ```

  (where T stands for either *S1* or *S2*) then the File interface is implemented by both *S1* and *S2*, regardless of what other methods *S1* and *S2* may have or share.

  All types implement the empty *interface*.

  ```go
  interface{}
  ```

  ```go
  type Reader interface {
  	Read(p []byte) (n int, err error)
  	Close() error
  }
  
  type Writer interface {
  	Write(p []byte) (n int, err error)
  	Close() error
  }
  
  // ReadWriter's methods are Read, Write, and Close.
  type ReadWriter interface {
  	Reader  // includes methods of Reader in ReadWriter's method set
  	Writer  // includes methods of Writer in ReadWriter's method set
  }
  
  type ReadCloser interface {
  	Reader   // includes methods of Reader in ReadCloser's method set
  	Close()  // illegal: signatures of Reader.Close and Close are different
  }
  ```

  ```go
  // illegal: Bad cannot embed itself
  type Bad interface {
  	Bad
  }
  
  // illegal: Bad1 cannot embed itself using Bad2
  type Bad1 interface {
  	Bad2
  }
  type Bad2 interface {
  	Bad1
  }
  ```

  ### Map types

  The value of an uninitialized map is `nil`.

  ```go
  MapType     = "map" "[" KeyType "]" ElementType .
  KeyType     = Type .
  ```

  The comparison operators `==` and `!=` must be fully defined for operands of the key type; thus the key type must not be a function, map, or slice. If the key type is an interface type, these comparison operators must be defined for the dynamic key values; failure will cause a run-time panic.

  ```go
  map[string]int
  map[*T]struct{ x, y float64 }
  map[string]interface{}
  ```

  A map may change during execution. Elements may be added during execution using assignments and retrieved with index expressions; they may be removed with the `delete` built-in function.

  ```go
  m := make(map[string]int)
  m[key] = 1
  delete(m, "key")
  ```

  The initial capacity dose not bound its size: maps grow to accommodate the number of items stored in them, with the exception of `nil` maps. A `nil` map is equivalent to an empty map except that no elements may be added.
  
  ### Channel types
  
  A channel provides a mechanism for concurrently executing functions to communicate by sending and receiving values of a specified element type. The value of an uninitialized channel is `nil`.
  
  ```go
  ChannelType = ( "chan" | "chan" "<-" | "<-" "chan" ) ElementType .
  ```
  
  The optional `<-` operator specifies the channel *direction*, *send* or *receive*, If no direction is given, the channel is *bidirectional*.
  
  ```go
  chan T          // can be used to send and receive values of type T
  chan<- float64  // can only be used to send float64s
  <-chan int      // can only be used to receive ints
  ```
  
  ```go
  make(chan int, 100) // capacity is an optional arguments.
  ```
  
  