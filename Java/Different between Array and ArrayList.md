* **Resizable**:   *Array* is static in size that is fixed length data structure, one cannot change the length after creating the Array object. *ArrayList* is dynamic in size. Each *ArrayList* object has instance variable capacity which indicates the size of the *ArrayList*. As elements are added to an *ArrayList* its capacity grows automatically.

* **Performance**: Performance of *Array* and *ArrayList* depends on the operation you are performing:
1. *resize()* opertation : Automatic resize of *ArrayList* will slow down the performance as it will use temporary array to copy elements from the old array to new array. *ArrayList* is internally backed by Array during resizing  as it calls the native implemented method `System.arrayCopy(src,srcPos,dest,destPos,length)`.
2. *add() or get()* operation : adding an element or retrieving an element from the array or arraylist object has almost same  performance , as for *ArrayList* object these operations  run in constant time.

* **Primitives**:  *ArrayList* cannot contains primitive data types (like int, float, double) it can only contains Object while *Array* can contain both primitive data types as well as objects. One get a misconception that we can store primitives(int,float,double) in *ArrayList* , but it is not true

* **Type-Safety**:  In Java, one can ensure Type Safety through Generics. while *Array* is a homogeneous data structure, thus it will contain objects of specific class or primitives of specific data type. In array if one tries to store the different data type other than the specified while creating the array object, `ArrayStoreException` is thrown.

* **Multi-dimensional**:  *Array* can be multi-dimensional, while *ArrayList* is always single dimensional.

* **Adding elements**: We can insert elements into the *ArrayList* object using the add() method while  in array we insert elements using the assignment operator.
