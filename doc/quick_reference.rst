.. currentmodule:: bitstring

.. _quick_reference:

###############
Quick Reference
###############

This section gives a summary of the bitstring module's classes, functions and attributes.

There are four classes that are bit containers, so that each element is a single bit.
They differ based on whether they can be modified after creation and on whether they have the concept of a current bit position.

.. |nbsp| unicode:: 0xa0
   :trim:

.. list-table::
   :widths: 20 15 15 50
   :header-rows: 1

   * - Class
     - Mutable?
     - Streaming methods?
     -
   * - :ref:`bits_quick_reference`
     - |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| ✘
     - |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| ✘
     - An efficient, immutable container of bits.
   * - :ref:`bitarray_quick_reference`
     - |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| ✔
     - |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| ✘
     - Like ``Bits`` but it can be changed after creation.
   * - :ref:`constbitstream_quick_reference`
     - |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| ✘
     - |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| ✔
     - Immutable like ``Bits`` but with a bit position and reading methods.
   * - :ref:`bitstream_quick_reference`
     - |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| ✔
     - |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| ✔
     - Mutable like ``BitArray`` but with a bit position and reading methods.


The final class is a flexible container whose elements are fixed-length bitstrings.

.. list-table::
   :widths: 20 15 15 50

   * - :ref:`array_quick_reference`
     - |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| ✔
     - |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| |nbsp| ✘
     - An efficient list-like container where each item has a fixed-length binary format.

----


.. _bits_quick_reference:

Bits
----

:class:`Bits` is the most basic class and is just a container of bits. It is immutable, so once created its value cannot change.


``Bits(__auto, length: Optional[int], offset: Optional[int], **kwargs)``

The first parameter (usually referred to as `auto`) can be many different types, including parsable strings, a file handle, a bytes or bytearray object, an integer or an iterable.

A single initialiser from `kwargs` can be used instead of `auto`, including  ``bin``, ``hex``, ``oct``, ``bool``, ``uint``, ``int``, ``float``, ``bytes`` and ``filename``.

Examples::

   Bits('0xef')
   Bits(float=-50.5, length=32)
   Bits('uint10=99')
   Bits(uint=99, length=10)

Methods
^^^^^^^

* :meth:`~Bits.all` -- Check if all specified bits are set to 1 or 0.
* :meth:`~Bits.any` -- Check if any of specified bits are set to 1 or 0.
* :meth:`~Bits.copy` -- Return a copy of the bitstring.
* :meth:`~Bits.count` -- Count the number of bits set to 1 or 0.
* :meth:`~Bits.cut` -- Create generator of constant sized chunks.
* :meth:`~Bits.endswith` -- Return whether the bitstring ends with a sub-bitstring.
* :meth:`~Bits.find` -- Find a sub-bitstring in the current bitstring.
* :meth:`~Bits.findall` -- Find all occurrences of a sub-bitstring in the current bitstring.
* :meth:`~Bits.join` -- Join bitstrings together using current bitstring.
* :meth:`~Bits.pp` -- Pretty print the bitstring.
* :meth:`~Bits.rfind` -- Seek backwards to find a sub-bitstring.
* :meth:`~Bits.split` -- Create generator of chunks split by a delimiter.
* :meth:`~Bits.startswith` -- Return whether the bitstring starts with a sub-bitstring.
* :meth:`~Bits.tobitarray` -- Return bitstring as a ``bitarray`` object from the `bitarray <https://pypi.org/project/bitarray>`_ package.
* :meth:`~Bits.tobytes` -- Return bitstring as bytes, padding if needed.
* :meth:`~Bits.tofile` -- Write bitstring to file, padding if needed.
* :meth:`~Bits.unpack` -- Interpret bits using format string.


Special methods
^^^^^^^^^^^^^^^

Also available are operators that will return a new bitstring (or check for equality):

* :meth:`== <Bits.__eq__>` / :meth:`\!= <Bits.__ne__>` -- Equality tests.
* :meth:`[] <Bits.__getitem__>` -- Get an element or slice.
* :meth:`+ <Bits.__add__>` -- Concatenate with another bitstring.
* :meth:`* <Bits.__mul__>` -- Concatenate multiple copies of the current bitstring.
* :meth:`~ <Bits.__invert__>` -- Invert every bit of the bitstring.
* :meth:`\<\< <Bits.__lshift__>` -- Shift bits to the left.
* :meth:`>> <Bits.__rshift__>` -- Shift bits to the right.
* :meth:`& <Bits.__and__>` -- Bit-wise AND between two bitstrings.
* :meth:`| <Bits.__or__>` -- Bit-wise OR between two bitstrings.
* :meth:`^ <Bits.__xor__>` -- Bit-wise XOR between two bitstrings.

Properties
^^^^^^^^^^

These read-only properties of the ``Bits`` object are interpretations of the binary data and are calculated as required.
Many require the bitstring to be specific lengths.

* :attr:`~Bits.bin` / ``b`` -- The bitstring as a binary string.
* :attr:`~Bits.bool` -- For single bit bitstrings, interpret as True or False.
* :attr:`~Bits.bytes` -- The bitstring as a bytes object.
* :attr:`~Bits.float` / ``floatbe`` / ``f`` -- Interpret as a big-endian floating point number.
* :attr:`~Bits.floatle` -- Interpret as a little-endian floating point number.
* :attr:`~Bits.floatne` -- Interpret as a native-endian floating point number.
* :attr:`~Bits.e4m3float` -- Interpret as an 8 bit float with 1:4:3 format.
* :attr:`~Bits.e5m2float` -- Interpret as an 8 bit float with 1:5:2 format.
* :attr:`~Bits.bfloat` / ``bfloatbe`` -- Interpret as a big-endian bfloat floating point number.
* :attr:`~Bits.bfloatle` -- Interpret as a little-endian bfloat floating point number.
* :attr:`~Bits.bfloatne` -- Interpret as a native-endian bfloat floating point number.
* :attr:`~Bits.hex` / ``h`` -- The bitstring as a hexadecimal string.
* :attr:`~Bits.int` / ``i`` -- Interpret as a two's complement signed integer.
* :attr:`~Bits.intbe` -- Interpret as a big-endian signed integer.
* :attr:`~Bits.intle` -- Interpret as a little-endian signed integer.
* :attr:`~Bits.intne` -- Interpret as a native-endian signed integer.
* :attr:`~Bits.len` -- Length of the bitstring in bits.
* :attr:`~Bits.oct` / ``o`` -- The bitstring as an octal string.
* :attr:`~Bits.se` -- Interpret as a signed exponential-Golomb code.
* :attr:`~Bits.ue` -- Interpret as an unsigned exponential-Golomb code.
* :attr:`~Bits.sie` -- Interpret as a signed interleaved exponential-Golomb code.
* :attr:`~Bits.uie` -- Interpret as an unsigned interleaved exponential-Golomb code.
* :attr:`~Bits.uint` / ``u`` -- Interpret as a two's complement unsigned integer.
* :attr:`~Bits.uintbe` -- Interpret as a big-endian unsigned integer.
* :attr:`~Bits.uintle` -- Interpret as a little-endian unsigned integer.
* :attr:`~Bits.uintne` -- Interpret as a native-endian unsigned integer.

----

.. _bitarray_quick_reference:


BitArray
--------

``Bits`` ⟶ ``BitArray``

:class:`BitArray` adds mutating methods to ``Bits``. The constructor is the same as for ``Bits``.

Additional methods
^^^^^^^^^^^^^^^^^^

All of the methods listed above for the ``Bits`` class are available, plus:

* :meth:`~BitArray.append` -- Append a bitstring.
* :meth:`~BitArray.byteswap` -- Change byte endianness in-place.
* :meth:`~BitArray.clear` -- Remove all bits from the bitstring.
* :meth:`~BitArray.insert` -- Insert a bitstring.
* :meth:`~BitArray.invert` -- Flip bit(s) between one and zero.
* :meth:`~BitArray.overwrite` -- Overwrite a section with a new bitstring.
* :meth:`~BitArray.prepend` -- Prepend a bitstring.
* :meth:`~BitArray.replace` -- Replace occurrences of one bitstring with another.
* :meth:`~BitArray.reverse` -- Reverse bits in-place.
* :meth:`~BitArray.rol` -- Rotate bits to the left.
* :meth:`~BitArray.ror` -- Rotate bits to the right.
* :meth:`~BitArray.set` -- Set bit(s) to 1 or 0.

Additional special methods
^^^^^^^^^^^^^^^^^^^^^^^^^^

The special methods available for the ``Bits`` class are all available, plus some which will modify the bitstring:

* :meth:`[] <BitArray.__setitem__>` -- Set an element or slice.
* :meth:`del <BitArray.__delitem__>` -- Delete an element or slice.
* :meth:`+= <BitArray.__iadd__>` -- Append bitstring to the current bitstring.
* :meth:`*= <BitArray.__imul__>` -- Concatenate multiple copies of the current bitstring.
* :meth:`\<\<= <BitArray.__ilshift__>` -- Shift bits in-place to the left.
* :meth:`>>= <BitArray.__irshift__>` -- Shift bits in-place to the right.
* :meth:`&= <BitArray.__iand__>` -- In-place bit-wise AND between two bitstrings.
* :meth:`|= <BitArray.__ior__>` -- In-place bit-wise OR between two bitstrings.
* :meth:`^= <BitArray.__ixor__>` -- In-place bit-wise XOR between two bitstrings.


``BitArray`` objects have the same properties as ``Bits``, except that they are all (with the exception of ``len``) writable as well as readable.

----

.. _constbitstream_quick_reference:


ConstBitStream
--------------

``Bits`` ⟶ ``ConstBitStream``

:class:`ConstBitStream` adds a bit position and methods to read and navigate in an immutable bitstream.
If you wish to use streaming methods on a large file without changing it then this is often the best class to use.

The constructor is the same as for ``Bits`` / ``BitArray`` but with an optional current bit position.

``ConstBitStream(auto, length: Optional[int], offset: Optional[int], pos: int = 0, **kwargs)``

All of the methods, special methods and properties listed above for the ``Bits`` class are available, plus:

Additional methods
^^^^^^^^^^^^^^^^^^

* :meth:`~ConstBitStream.bytealign` -- Align to next byte boundary.
* :meth:`~ConstBitStream.peek` -- Peek at and interpret next bits as a single item.
* :meth:`~ConstBitStream.peeklist` -- Peek at and interpret next bits as a list of items.
* :meth:`~ConstBitStream.read` -- Read and interpret next bits as a single item.
* :meth:`~ConstBitStream.readlist` -- Read and interpret next bits as a list of items.
* :meth:`~ConstBitStream.readto` -- Read up to and including next occurrence of a bitstring.

Additional properties
^^^^^^^^^^^^^^^^^^^^^

* :attr:`~ConstBitStream.bytepos` -- The current byte position in the bitstring.
* :attr:`~ConstBitStream.pos` -- The current bit position in the bitstring.

----

.. _bitstream_quick_reference:


BitStream
---------

``Bits`` ⟶ ``BitArray / ConstBitStream`` ⟶ ``BitStream``


:class:`BitStream` contains all of the 'stream' elements of ``ConstBitStream`` and adds all of the mutating methods of ``BitArray``.
The constructor is the same as for ``ConstBitStream``.
It has all the methods, special methods and properties of the ``Bits``, ``BitArray`` and ``ConstBitArray`` classes.

It is the most general of the four classes, but it is usually best to choose the simplest class for your use case.

----

.. _array_quick_reference:

Array
-----

A bitstring :class:`Array` is a contiguously allocated sequence of bitstrings of the same type.
It is similar to the ``array`` type in the `array <https://docs.python.org/3/library/array.html>`_ module, except that it is far more flexible.


``Array(dtype: str, initializer, trailing_bits)``

The `dtype` can any single fixed-length token as described in :ref:`format_tokens` and :ref:`compact_format`.

The `inititalizer` will typically be an iterable such as a list, but can also be many other things including an open binary file, a bytes or bytearray object, another ``bitstring.Array`` or an ``array.array``.

The `trailing_bits` typically isn't used in construction, and specifies bits left over after interpreting the stored binary data according to the data type `dtype`.

Both the dtype and the underlying bit data (stored as a :class:`BitArray`) can be freely modified after creation, and element-wise operations can be used on the ``Array``. Modifying the data or format after creation may cause the :attr:`~Array.trailing_bits` to not be empty.

Initialization examples::

    Array('>H', [1, 10, 20])
    Array('float16', a_file_object)
    Array('int4', stored_bytes)


Methods
^^^^^^^

* :meth:`~Array.append` -- Append a single item to the end of the Array.
* :meth:`~Array.astype` -- Cast the Array to a new dtype.
* :meth:`~Array.byteswap` -- Change byte endianness of all items.
* :meth:`~Array.count` -- Count the number of occurences of a value.
* :meth:`~Array.equals` -- Compare with another Array for exact equality.
* :meth:`~Array.extend` -- Append multiple items to the end of the Array from an iterable.
* :meth:`~Array.fromfile` -- Append items read from a file object.
* :meth:`~Array.insert` -- Insert an item at a given position.
* :meth:`~Array.pop` -- Return and remove an item.
* :meth:`~Array.pp` -- Pretty print the Array.
* :meth:`~Array.reverse` -- Reverse the order of all items.
* :meth:`~Array.tobytes` -- Return Array data as bytes object, padding with zero bits at the end if needed.
* :meth:`~Array.tofile` -- Write Array data to a file, padding with zero bits at the end if needed.
* :meth:`~Array.tolist` -- Return Array items as a list.

Special methods
^^^^^^^^^^^^^^^

These non-mutating special methods are available. Where appropriate they return a new ``Array``.

* :meth:`[] <Array.__getitem__>` -- Get an element or slice.
* :meth:`+ <Array.__add__>` -- Add value to each element.
* :meth:`- <Array.__sub__>` -- Subtract value from each element.
* :meth:`* <Array.__mul__>` -- Multiply each element by a value.
* :meth:`/ <Array.__truediv__>` -- Divide each element by a value.
* :meth:`// <Array.__floordiv__>` -- Floor divide each element by a value.
* :meth:`% <Array.__mod__>` -- Take modulus of each element with a value.
* :meth:`\<\< <Array.__lshift__>` -- Shift value of each element to the left.
* :meth:`>> <Array.__rshift__>` -- Shift value of each element to the right.
* :meth:`& <Array.__and__>` -- Bit-wise AND of each element.
* :meth:`| <Array.__or__>` -- Bit-wise OR of each element.
* :meth:`^ <Array.__xor__>` -- Bit-wise XOR of each element.
* :meth:`- <Array.__neg__>` -- Unary minus of each element.
* :meth:`abs() <Array.__abs__>` -- Absolute value of each element.


For example::

    >>> b = Array('i6', [30, -10, 1, 0])
    >>> b >> 2
    Array('i6', [7, -3, 0, 0])
    >>> b + 1
    Array('i6', [31, -9, 2, 1])
    >>> b + b
    Array('i6', [30, -10, 1, 0, 30, -10, 1, 0])

Comparison operators will output an ``Array`` with a ``dtype`` of ``'bool'``.

* :meth:`== <Array.__eq__>` / :meth:`\!= <Array.__ne__>` -- Equality tests.
* :meth:`\< <Array.__lt__>` -- Less than comparison.
* :meth:`\<= <Array.__le__>` -- Less than or equal comparison.
* :meth:`> <Array.__gt__>` -- Greater than comparison.
* :meth:`>= <Array.__ge__>` -- Greater than or equal comparison.


Mutating versions of many of the methods are also available.

* :meth:`[] <Array.__setitem__>` -- Set an element or slice.
* :meth:`del <Array.__delitem__>` -- Delete an element or slice.
* :meth:`+= <Array.__add__>` -- Add value to each element in-place.
* :meth:`-= <Array.__sub__>` -- Subtract value from each element in-place.
* :meth:`*= <Array.__mul__>` -- Multiply each element by a value in-place.
* :meth:`/= <Array.__truediv__>` -- Divide each element by a value in-place.
* :meth:`//= <Array.__floordiv__>` -- Floor divide each element by a value in-place.
* :meth:`%= <Array.__mod__>` -- Take modulus of each element with a value in-place.
* :meth:`\<\<= <Array.__lshift__>` -- Shift bits of each element to the left in-place.
* :meth:`>>= <Array.__rshift__>` -- Shift bits of each element to the right in-place.
* :meth:`&= <Array.__and__>` -- In-place bit-wise AND of each element.
* :meth:`|= <Array.__or__>` -- In-place bit-wise OR of each element.
* :meth:`^= <Array.__xor__>` -- In-place bit-wise XOR of each element.

Example::

    >>> a = Array('float16', [1.5, 2.5, 7, 1000])
    >>> a[::2] *= 3.0  # Multiply every other float16 value in-place
    >>> a
    Array('float16', [4.5, 2.5, 21.0, 1000.0])


The bit-wise logical operations (``&``, ``|``, ``^``) are performed on each element with a ``Bits`` object, which must have the same length as the ``Array`` elements.
The other element-wise operations are performed on the interpreted data, not on the bit-data.
For example this means that the shift operations won't work on floating point formats.


Properties
^^^^^^^^^^

* :attr:`~Array.data` -- The complete binary data in a ``BitArray`` object. Can be freely modified.
* :attr:`~Array.dtype` -- The data type or typecode. Can be freely modified.
* :attr:`~Array.itemsize` -- The length *in bits* of a single item. Read only.
* :attr:`~Array.trailing_bits` -- If the data length is not a multiple of the `dtype` length, this ``BitArray`` gives the leftovers at the end of the data.

----

General Information
-------------------

.. _format_tokens:

Format tokens
^^^^^^^^^^^^^

Format strings are used when constructing bitstrings, as well as reading, packing and unpacking them, as well as giving the format for :class:`Array` objects.
They can also be auto promoted to bitstring when appropriate - see :ref:`auto_init`.


=================== ===============================================================================
``'int:n'``         ``n`` bits as a signed integer.
``'uint:n'``        ``n`` bits as an unsigned integer.
``'intbe:n'``	    ``n`` bits as a byte-wise big-endian signed integer.
``'uintbe:n'``      ``n`` bits as a byte-wise big-endian unsigned integer.
``'intle:n'``       ``n`` bits as a byte-wise little-endian signed integer.
``'uintle:n'``      ``n`` bits as a byte-wise little-endian unsigned integer.
``'intne:n'``       ``n`` bits as a byte-wise native-endian signed integer.
``'uintne:n'``      ``n`` bits as a byte-wise native-endian unsigned integer.
``'float:n'``       ``n`` bits as a big-endian floating point number (same as ``floatbe``).
``'floatbe:n'``     ``n`` bits as a big-endian floating point number (same as ``float``).
``'floatle:n'``     ``n`` bits as a little-endian floating point number.
``'floatne:n'``     ``n`` bits as a native-endian floating point number.
``'bfloat[:16]'``   16 bits as a big-endian bfloat floating point number (same as ``bfloatbe``).
``'bfloatbe[:16]'`` 16 bits as a big-endian bfloat floating point number (same as ``bfloat``).
``'bfloatle[:16]'`` 16 bits as a little-endian floating point number.
``'bfloatne[:16]'`` 16 bits as a native-endian floating point number.
``'e4m3float'``    8 bits as a 1:4:3 format floating point number.
``'e5m2float'``    8 bits as a 1:5:2 format floating point number.
``'hex:n'``         ``n`` bits as a hexadecimal string.
``'oct:n'``         ``n`` bits as an octal string.
``'bin:n'``         ``n`` bits as a binary string.
``'bits:n'``        ``n`` bits as a new bitstring.
``'bytes:n'``       ``n`` bytes as a ``bytes`` object.
``'ue'``            next bits as an unsigned exponential-Golomb code.
``'se'``            next bits as a signed exponential-Golomb code.
``'uie'``           next bits as an interleaved unsigned exponential-Golomb code.
``'sie'``           next bits as an interleaved signed exponential-Golomb code.
``'bool[:1]'``      next bit as a boolean (True or False).
``'pad:n'``         next ``n`` bits will be ignored (padding). Only applicable when reading, not creating.
=================== ===============================================================================

The ``':'`` before the length is optional, and is mostly omitted in the documentation, except where it improves readability.

The ``hex``, ``bin``, ``oct``, ``int``, ``uint`` and ``float`` properties can all be shortened to just their initial letter.

Bitstring literals
^^^^^^^^^^^^^^^^^^

To make a literal quantity (one that directly represents a sequence of bits) you can use any of the format tokens above followed by an ``'='`` and a value to initialise with.
For example::

    s = BitArray('float32=10.125, int7=-9')
    s.append('hex:abc')

You can also create binary, octal and hexadecimal literals by starting a string with ``'0b'``, ``'0o'`` and ``'0ox'`` respectively::

    t = BitArray('0b101')
    t += '0x001f'


.. _compact_format:

Compact format strings
^^^^^^^^^^^^^^^^^^^^^^

Another option is to use a format specifier similar to those used in the ``struct`` and ``array`` modules. These consist of a character to give the endianness, followed by more single characters to give the format.

The endianness character must start the format string:

=======   =============
``'>'``   Big-endian
``'<'``   Little-endian
``'='``   Native-endian
=======   =============

.. note::
    * For native-endian ``'@'`` and ``'='`` can both be used and are equivalent. The ``'@'`` character was required for native-endianness prior to version 4.1 of bitstring.

    * For 'network' endianness use ``'>'`` as network and big-endian are equivalent.

This is followed by at least one of these format characters:

=======   ===============================
``'b'``   8 bit signed integer
``'B'``   8 bit unsigned integer
``'h'``   16 bit signed integer
``'H'``   16 bit unsigned integer
``'l'``   32 bit signed integer
``'L'``   32 bit unsigned integer
``'q'``   64 bit signed integer
``'Q'``   64 bit unsigned integer
``'e'``   16 bit floating point number
``'f'``   32 bit floating point number
``'d'``   64 bit floating point number
=======   ===============================

The exact type is determined by combining the endianness character with the format character, but rather than give an exhaustive list a single example should explain:

========  ======================================   ===========
``'>h'``  Big-endian 16 bit signed integer         ``intbe16``
``'<h'``  Little-endian 16 bit signed integer      ``intle16``
``'=h'``  Native-endian 16 bit signed integer      ``intne16``
========  ======================================   ===========

As you can see all three are signed integers in 16 bits, the only difference is the endianness. The native-endian ``'=h'`` will equal the big-endian ``'>h'`` on big-endian systems, and equal the little-endian ``'<h'`` on little-endian systems. For the single byte codes ``'b'`` and ``'B'`` the endianness doesn't make any difference, but you still need to specify one so that the format string can be parsed correctly.

------

Module level
------------

Functions
^^^^^^^^^
* :func:`~bitstring.pack` -- Create a new ``BitStream`` according to a format string and values.

Exceptions
^^^^^^^^^^
* :class:`~bitstring.Error` -- Base class for module exceptions.
* :class:`~bitstring.ReadError` -- Reading or peeking past the end of a bitstring.
* :class:`~bitstring.InterpretError` -- Inappropriate interpretation of binary data.
* :class:`~bitstring.ByteAlignError` -- Whole-byte position or length needed.
* :class:`~bitstring.CreationError` -- Inappropriate argument during bitstring creation.

Module variables
^^^^^^^^^^^^^^^^
* :data:`~bitstring.bytealigned` -- Determines whether a number of methods default to working only on byte boundaries.
* :data:`~bitstring.lsb0` -- If True, index bits with the least significant bit (the final bit) as bit zero.

