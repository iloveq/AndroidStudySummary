List:
   
```
    public interface List<E> extends Collection<E> {
        boolean contains(Object o);
    }
```
   
ArrayList:
   
```
    public boolean contains(Object o) {
        return indexOf(o) >= 0;
    }
    
    public int indexOf(Object o) {
        if (o == null) {
            for (int i = 0; i < size; i++)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = 0; i < size; i++)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }

```
