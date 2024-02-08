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

**`grep --color=auto`**

Highlights the matched text with the given `GREP_COLOR environment` variable.
```
~/Projects/CSE15L/docsearch main
default ❯ grep -r --color=auto 'base pair' technical
technical/plos/journal.pbio.0020223.txt:        Watson-Crick base pairing, the proximity of the synthetic reactive groups elevates their
technical/plos/journal.pbio.0020190.txt:        sequence, which is a specific series of eight base pairs in the DNA of the bacterial
technical/plos/journal.pbio.0020190.txt:        chromosomes, on the order of one or two thousand base pairs of DNA (or less—their length is
technical/biomed/1471-2156-2-3.txt:          three exons. The first exon contains 279 base pairs (bp)
technical/biomed/1471-2121-3-10.txt:        both encode a 10-base pair sequence that is identical to
technical/biomed/1471-2121-3-10.txt:        the P3 sequence with an insertion of 3 base pairs at
technical/biomed/gb-2001-2-4-research0010.txt:          necessarily true because of Watson-Crick base pairing.
technical/biomed/gb-2003-4-4-r24.txt:        important considering that within a few hundred base pairs,
technical/biomed/gb-2001-2-4-research0011.txt:          sequenced to completion, yielding a 1,036 base pair (bp)
technical/biomed/1471-2229-2-3.txt:        stringency with a 300 base pair homologous 
technical/biomed/1471-2229-2-3.txt:        conducted at high stringency with a 1700 base pair
```
Colored image:
![Image](/images/grepcolor.png)

Here it highlights "base pair" in red. When you are using `grep` to look for a specific instance where the pattern is used, this could help you see where the pattern is quickly.

```
~/Projects/CSE15L/docsearch main
default ❯ grep -r --color=auto 'rna' technical
technical/government/About_LSC/LegalServCorp_v_VelazquezSyllabus.txt:alternative source of vital information on the client's
technical/government/About_LSC/Progress_report.txt:Commitment: Internal capacity and expertise to support
technical/government/About_LSC/Progress_report.txt:The Creation of Efforts to Link with the International Justice
technical/government/About_LSC/Progress_report.txt:international legal aid community. In June 2001 and again in
technical/government/About_LSC/Progress_report.txt:legal issues, government and alternative funding sources, access,
technical/government/About_LSC/Strategic_report.txt:articles in the MIE Journal and NLADA Update. She promoted
technical/government/About_LSC/Strategic_report.txt:Conference of State Court Administrators, the International Legal
technical/government/About_LSC/Strategic_report.txt:internal review sessions, and special sessions with OPP staff to
technical/government/About_LSC/Strategic_report.txt:electronically. The Internal Revenue Service is a partner in this
technical/government/About_LSC/Strategic_report.txt:people, and other services, including mediation and alternative
technical/government/About_LSC/Strategic_report.txt:LSC expanded its on-site program reviews to include internal
technical/government/About_LSC/Strategic_report.txt:international invitations to address legal aid groups. Highpoints
technical/government/About_LSC/Strategic_report.txt:o International Legal Aid Group (Tokyo)
```

Colored image:
![Image](/images/grepcolor2.png)

Here, it highlights all instances of "rna" in red. In this case, using the `--color` tag made me quickly realize that `grep` had not only retrieved the instances of RNA being used in the biological sense, but all instances in all words.

Found with `man grep`

**`grep -e`**

The `-e` flag is used to specify a pattern for grep to search for. In particular, using the `-e` flag allows you to specify more than one pattern, 
which can be useful for more advanced searches. 

**`grep -exclude`**

The ``exclude` flag is used to specify a pattern to exclude from searches. 

**`grep -n`**
Output lines are labeled with their respective line numbers in the file.


