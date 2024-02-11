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

The input that doesn't induce a failure passes the test, but the input that does is unresolved, suggesting an infinite loop.

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

Here, it highlights all instances of "rna" in red. In this case, using the `--color` tag made me quickly realize that `grep` had not only retrieved the instances of rna being used in the biological sense, but all instances in all words.

Found with `man grep`

**`grep -C`**

The `-C` flag lets you specify an amount of lines around the matched pattern to provide as context.

```
~/Projects/CSE15L/docsearch main
default ❯ grep -rC 10 'withered'  technical
technical/911report/chapter-3.txt-                Over time the procedures came to be referred to as "the wall." The term "the wall"
technical/911report/chapter-3.txt-                is misleading, however, because several factors led to a series of barriers to
technical/911report/chapter-3.txt-                information sharing that developed.
technical/911report/chapter-3.txt-            
technical/911report/chapter-3.txt-            The Office of Intelligence Policy and Review became the sole gatekeeper for passing
technical/911report/chapter-3.txt-                information to the Criminal Division. Though Attorney General Reno's procedures did
technical/911report/chapter-3.txt-                not include such a provision, the Office assumed the role anyway, arguing that its
technical/911report/chapter-3.txt-                position reflected the concerns of Judge Royce Lamberth, then chief judge of the
technical/911report/chapter-3.txt-                Foreign Intelligence Surveillance Court. The Office threatened that if it could not
technical/911report/chapter-3.txt-                regulate the flow of information to criminal prosecutors, it would no longer present
technical/911report/chapter-3.txt:                the FBI's warrant requests to the FISA Court. The information flow withered.
technical/911report/chapter-3.txt-            
technical/911report/chapter-3.txt-            The 1995 procedures dealt only with sharing between agents and criminal prosecutors,
technical/911report/chapter-3.txt-                not between two kinds of FBI agents, those working on intelligence matters and those
technical/911report/chapter-3.txt-                working on criminal matters. But pressure from the Office of Intelligence Policy
technical/911report/chapter-3.txt-                Review, FBI leadership, and the FISA Court built barriers between agents-even agents
technical/911report/chapter-3.txt-                serving on the same squads. FBI Deputy Director Bryant reinforced the Office's
technical/911report/chapter-3.txt-                caution by informing agents that too much information sharing could be a career
technical/911report/chapter-3.txt-                stopper. Agents in the field began to believe-incorrectly-that no FISA information
technical/911report/chapter-3.txt-                could be shared with agents working on criminal investigations.
technical/911report/chapter-3.txt-            
```

Here, I specified 10 so it gave me 10 lines of context around the pattern "withered". This could be helpful if you are looking at a specfic keyword to see what your file(s) say about it,  so you want some additional context.

```
~/Projects/CSE15L/docsearch main
default ❯ grep -rC 5 'North American Free Trade Agreement'  technical
technical/government/Gen_Account_Office/gg96118.txt-Customs anticipated that trade issues would assume greater
technical/government/Gen_Account_Office/gg96118.txt-prominence in the coming years as developing countries continue to
technical/government/Gen_Account_Office/gg96118.txt-industrialize, corporations continue to expand internationally, and
technical/government/Gen_Account_Office/gg96118.txt-trade barriers continue to fall. Further, the proliferation of
technical/government/Gen_Account_Office/gg96118.txt-international trade agreements, such as the U.S.-Canada Free Trade
technical/government/Gen_Account_Office/gg96118.txt:Agreement of 1989, the North American Free Trade Agreement, and the
technical/government/Gen_Account_Office/gg96118.txt-General Agreement on Tariffs and Trade, should lead to further
technical/government/Gen_Account_Office/gg96118.txt-increases in trade and travel volume.
technical/government/Gen_Account_Office/gg96118.txt-Internally, Customs anticipated that as public pressures to
technical/government/Gen_Account_Office/gg96118.txt-reduce the federal deficit continued, no real growth would occur in
technical/government/Gen_Account_Office/gg96118.txt-the agency's funding. Customs also anticipated attrition among its
```

I tagged this `grep` command with `-C 5`, so it gave me five lines of context around instances of "North American Free Trade Agremeent". Again, it is helpful to see what your files are generally saying about the keyword.

**`grep -w`**

The `-w` flag ensures that the pattern only matches whole words. 
```
~/Projects/CSE15L/docsearch main
default ❯ grep -rw 'RNA' technical 
technical/plos/journal.pbio.0020419.txt:        Once the structure of DNA was known, the physicist George Gamow formed the RNA Tie Club,
technical/plos/journal.pbio.0020419.txt:        Over the twenty years since the RNA Tie Club, molecular biology had matured. Francis
technical/plos/journal.pbio.0030024.txt:        messenger RNA sequences, allowing scientists to test individual brain tissue samples for
technical/plos/journal.pbio.0020145.txt:        2003) and a good review article of small inhibitory RNA (Dillin 2003).
technical/plos/pmed.0020103.txt:          Total RNA was prepared by using TRIZOL (Invitrogen) and RQ1 RNase-free DNase (Promega,
technical/plos/pmed.0020103.txt:          template for in vitro transcription to produce riboprobes with a digoxigenin-RNA labeling
technical/plos/pmed.0020116.txt:        syndrome. There are four main serotypes of the dengue RNA virus; dengue hemorrhagic fever
technical/plos/pmed.0020274.txt:        In all, 49 samples (5.2%) were positive for HCoV-NL63 RNA. Viral RNA was more prevalent
technical/plos/pmed.0020274.txt:        clinical symptoms associated with HCoV-NL63 infection. Samples in which only HCoV-NL63 RNA
technical/plos/pmed.0020274.txt:        RNA.
technical/plos/journal.pbio.0030021.txt:        of germline sex determination. RNA interference and gene knockout approaches have shown
technical/plos/journal.pbio.0020224.txt:        1997; Voinnet et al. 1998) used grafting to demonstrate the spreading of RNA silencing in
technical/plos/journal.pbio.0020224.txt:        RNA silencing (termed posttranscriptional gene silencing in plants, quelling in fungi,
technical/plos/journal.pbio.0020224.txt:        and RNA interference in animals) refers to the phenomenon whereby specific gene transcript
technical/plos/journal.pbio.0020224.txt:        levels are reduced in the presence of a related RNA. From studies of RNA silencing in
technical/plos/journal.pbio.0020224.txt:        that the signal is a nucleic acid, most likely an RNA, but the identity of the signal
technical/plos/pmed.0010028.txt:          RNA was extracted from clones and tetramer-positive cells using TRIzol (Invitrogen,
technical/plos/pmed.0020075.txt:        allele alone. In the H1975 cell line, it was possible to obtain adequate quantities of RNA
technical/plos/journal.pbio.0020121.txt:        prions don't contain nucleic acids—only protein. Without DNA or RNA to issue biochemical
technical/plos/pmed.0010064.txt:          Interruption of therapy was individually timed to occur after two HIV RNA measurements of
technical/plos/pmed.0010064.txt:          magnitude as defined by mean HIV-1 plasma RNA area under the curve (AUC
technical/plos/pmed.0010064.txt:          HIV RNA ) was measured as a secondary outcome at weeks 12 and 20 of
technical/plos/pmed.0010064.txt:          HIV RNA up to 12 and 20 wk. In all cases, a two-sided alpha level of
technical/plos/pmed.0010064.txt:          RNA remaining less than 5,000 copies/ml for the two groups (
technical/plos/pmed.0010064.txt:          HIV RNA analysis at week 12 (single TI, median = 124,621
technical/plos/pmed.0010064.txt:          HIV RNA ; repeated TI, median = 100,400 [47,221–365,731] AUC
technical/plos/pmed.0010064.txt:          HIV RNA ; 
technical/plos/pmed.0010064.txt:          HIV RNA ; repeated TI, median = 153,097 [67,427–515,421] AUC
technical/plos/pmed.0010064.txt:          HIV RNA ; 
technical/plos/pmed.0010064.txt:        observed durable resuppression of plasma viral RNA level in many patients who had
technical/plos/pmed.0020161.txt:            Total RNA was extracted by using the RNeasy kit and DNase I treatment (Qiagen,
technical/plos/pmed.0020161.txt:            Valencia, California, United States). Total RNA (2 μg each) was reverse transcribed
technical/plos/pmed.0020161.txt:            Total RNA (5 μg) from primary MSCs, from hESMPC-H9.1, hESMPC-H1.2, and three samples
```
By adding the `-w` flag, I made sure that "RNA" only matched whole words. Thus, I was able to bring up instances of only RNA and not something like rRNA or tRNA, which could help if you need the specificty.

```
~/Projects/CSE15L/docsearch main
default ❯ grep -rw 'infra' technical
technical/government/About_LSC/commission_report.txt:Program. See discussion infra Part III(B)(2). Congress further
technical/government/About_LSC/commission_report.txt:discussion infra Part III(B)(2).
technical/government/About_LSC/commission_report.txt:United States, see discussion infra Part III(D)(2), and
technical/government/About_LSC/commission_report.txt:status. See discussion infra Part II (C)(1). Thus, prior to FY
technical/biomed/1471-2334-3-9.txt:        vide infra ). The ability of these
technical/biomed/1471-213X-1-1.txt:        supra-orbital, infra-orbital, and post-oral vibrissae and
technical/biomed/1471-2261-2-11.txt:          midline abdominal incision the infra-renal abdominal
technical/biomed/1471-2261-2-11.txt:          Blood flow rate (mL/min) in the infra-renal aorta was
technical/biomed/1471-2261-2-11.txt:          The infra-renal aorta of 5 mm long, starting 2 mm
```
Similarly, this usage ensured that only instances where "infra" was used as a word was brought up. That way, I don't have to look at words like "infrastructure" or "infrared" and look at only "infra".

**`grep -n`**

The `-n` flag makes it so that output lines are labeled with their respective line numbers in the file.

```
~/Projects/CSE15L/docsearch main
default ❯ grep -rn 'finch' technical
technical/biomed/1471-2105-3-2.txt:7:        Galapagos finches led to an appreciation of the structural
technical/biomed/1471-2105-3-2.txt:10:        the finches' structural features was the foundation for his
```

It added an additional number after the file names which represents the line number of the given lines. Now, if I want to locate that specific line in the file, I can just open the file and move to the line number that `grep` returns.

```
~/Projects/CSE15L/docsearch main
default ❯ grep -rn 'George W. Bush' technical
technical/government/Gen_Account_Office/Testimony_Jul17-2002_d02957t.txt:931:Security, President George W. Bush, June 2002.
technical/plos/journal.pbio.0020054.txt:39:        trees with trunks of nine inches (22.8 cm) in diameter or less. Under George W. Bush,
technical/911report/chapter-3.txt:1239:                to be Presidential Decision Directives; for President George W. Bush, National
technical/911report/chapter-1.txt:6:    Tuesday, September 11, 2001, dawned temperate and nearly cloudless in the eastern United States. Millions of men and women readied themselves for work. Some made their way to the Twin Towers, the signature structures of the World Trade Center complex in New York City. Others went to Arlington, Virginia, to the Pentagon. Across the Potomac River, the United States Congress was back in session. At the other end of Pennsylvania Avenue, people began to line up for a White House tour. In Sarasota, Florida, President George W. Bush went for an early morning run.
technical/911report/chapter-6.txt:1109:                Gore or his Republican opponent, Texas Governor George W. Bush, would become
technical/911report/chapter-8.txt:309:                President George W. Bush on August 6, 2001.37 Redacted material is indicated by
```
Again, it tells me what line numbers the lines are on. This is especially helpful when its really deep in the text, like some examples here. It would take forever to find something on line 1239 manually, but since it tells you, you can find it really quickly.

