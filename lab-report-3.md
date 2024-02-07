# Lab Report 3
## Part 1

**Failure-inducing input**
```
public class LinkedListTests {
    private LinkedList list2;
    private static final int[] list2Values = {1, 2};


    @Before
    public void setUp() {
        list2 = new LinkedList();

        for (int i: list2Values) {
            list2.append(i);
        }
    }

    @Test
    public void testAppend2() {
        list2.append(3);
        assertEquals("1 2 3 ", list2.toString());
    }
}
```

**Input that doesn't induce a failure**
```
public class LinkedListTests {
    private LinkedList list1;
    private static final int[] list1Values = {1};


    @Before
    public void setUp() {
        list1 = new LinkedList();
        
        for (int i: list1Values) {
            list1.append(i);
        }
    }
    
    @Test
    public void testAppend1() {
        list1.append(2);
        assertEquals("1 2 ", list1.toString());
    }
}
```

**Symptoms**
![Image](/images/LinkedListSymptoms.png)

The input that doesn't induce a failure passes the test, but the input that does is unresolved.

**Bug Before and After**

Before:
```
    /**
     * Adds the value to the _end_ of the list
     * @param value
     */
    public void append(int value) {
        if(this.root == null) {
            this.root = new Node(value, null);
            return;
        }
        // If it's just one element, add if after that one
        Node n = this.root;
        if(n.next == null) {
            n.next = new Node(value, null);
            return;
        }
        // Otherwise, loop until the end and add at the end with a null
        while(n.next != null) {
            n = n.next;
            n.next = new Node(value, null);
        }
    }
```

After:
```
    /**
     * Adds the value to the _end_ of the list
     * @param value
     */
    public void append(int value) {
        if(this.root == null) {
            this.root = new Node(value, null);
            return;
        }
        // If it's just one element, add if after that one
        Node n = this.root;
        if(n.next == null) {
            n.next = new Node(value, null);
            return;
        }
        // Otherwise, loop until the end and add at the end with a null
        while(n.next != null) {
            n = n.next;
        }
        n.next = new Node(value, null);
    }
```
The issue was that, for linked lists with more than one element, the `append` function adds a new node after the current node in every iteration.
After the first iteration, it basically iterates forever on the nodes it makes itself. To solve this, I moved the line creating a new node outside
of the loop. This makes `n` iterate to the last node before adding any new nodes.

## Part 2

**grep -e**

The `-e` flag is used to specify a pattern for grep to search for. In particular, using the `-e` flag allows you to specify more than one pattern, 
which can be useful for more advanced searches. 

**grep -exclude**

The ``exclude` flag is used to specify a pattern to exclude from searches. 

