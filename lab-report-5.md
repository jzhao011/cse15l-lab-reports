# Lab Report 5
## Part 1

**Student:**
Hi, I am having some trouble trying to set up my grading script. Every time I run it on one of the example repositories, I get something like this:

```
~/Projects/CSE 15L/list-examples-grader bugged
default ❯ bash grade.sh https://github.com/ucsd-cse15l-f22/list-methods-corrected 
Cloning into 'student-submission'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
Finished cloning
Found Files
JUnit version 4.13.2
..
Time: 0.006

OK (2 tests)

/ Succeeded
```

As you can see in the last line, I can't make the script output the number of succeeded tests out of the total number of tests. Here is my code for `grade.sh`:

```
CPATH='.:../lib/hamcrest-core-1.3.jar:../lib/junit-4.13.2.jar'

rm -rf student-submission
rm -rf grading-area

mkdir grading-area

git clone $1 student-submission
echo 'Finished cloning'

if [[ -f student-submission/ListExamples.java ]]
then
    echo "Found Files"
    cp student-submission/ListExamples.java grading-area/ListExamples.java
    cp TestListExamples.java grading-area/TestListExamples.java
else
    echo "ListExamples.java not found"
    exit 1
fi

cd grading-area
javac -cp $CPATH *.java

if [[ $? -ne 0 ]]
then
    echo "Program failed to compile"
    exit 1
fi

touch junit-output.txt
java -cp $CPATH org.junit.runner.JUnitCore TestListExamples < junit-output.txt

if [[ $(grep FAILURES junit-output.txt) == "" ]]
then
    TESTSRUN=$(grep -Eo "([0-9]+ tests)" junit-output.txt | grep -Eo "[0-9]+")
    echo $TESTSRUN/$TESTSRUN Succeeded
else
    TESTSRUN=$(grep -Eo "Tests run: [0-9]+" junit-output.txt | grep -Eo "[0-9]+")
    FAILURES=$(grep -Eo "Failures: [0-9]+" junit-output.txt | grep -Eo "[0-9]+")

    echo $(($TESTSRUN-$FAILURES))/$TESTSRUN Succeeded
fi
```

I think the error has to do with the bottom part of `grade.sh` when I am extracting the information on the tests run, but I am not sure where. How should I proceed from here?

**TA:**

Hi, have you tried running your `grep` commands on the terminal by themselves to see what they output? This could help you narrow down where your symptoms are originating from.

**Student:**

I ran it and it outputted nothing, which is consistent with the output of the tests in the terminal.

```
~/Projects/CSE 15L/list-examples-grader bugged/grading-area
default ❯ grep -Eo "Tests run: [0-9]+" junit-output.txt | grep -Eo "[0-9]+"

~/Projects/CSE 15L/list-examples-grader bugged/grading-area
default ❯ 
```
I'm still not sure where the bug is originating from.

**TA:**

Maybe the regular expression you wrote for grep doesn't match the actual contents of the file? Could you try comparing the two?

**Student:**

It turns out, my `junit-output.txt` file is empty! I looked at the code and realized that there was a typo on the line where I was redirecting the output from running the tests to `junit-output.txt`, which kept the file empty.

It was `java -cp $CPATH org.junit.runner.JUnitCore TestListExamples < junit-output.txt` when it should have been `java -cp $CPATH org.junit.runner.JUnitCore TestListExamples > junit-output.txt`. 

```
~/Projects/CSE 15L/list-examples-grader bugged
default ❯ bash grade.sh https://github.com/ucsd-cse15l-f22/list-methods-corrected 
Cloning into 'student-submission'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
Finished cloning
Found Files
2/2 Succeeded
```

As you can see, it works now. Thanks for your help!

**TA:**

No problem!


**All Information About the Setup**

Directory Structure:

```
GradeServer.java
Server.java
TestListExamples.java
grade.sh
grading-area/
  ListExamples.class
  ListExamples.java
  StringChecker.class
  TestListExamples$ShortStringFilter.class
  TestListExamples.class
  TestListExamples.java
  junit-output.txt
lib/
  hamcrest-core-1.3.jar
  junit-4.13.2.jar
student-submission/
  ListExamples.java
```

Contents of each file before fixing the bug:

`grade.sh`

```
CPATH='.:../lib/hamcrest-core-1.3.jar:../lib/junit-4.13.2.jar'

rm -rf student-submission
rm -rf grading-area

mkdir grading-area

git clone $1 student-submission
echo 'Finished cloning'

if [[ -f student-submission/ListExamples.java ]]
then
    echo "Found Files"
    cp student-submission/ListExamples.java grading-area/ListExamples.java
    cp TestListExamples.java grading-area/TestListExamples.java
else
    echo "ListExamples.java not found"
    exit 1
fi

cd grading-area
javac -cp $CPATH *.java

if [[ $? -ne 0 ]]
then
    echo "Program failed to compile"
    exit 1
fi

touch junit-output.txt
java -cp $CPATH org.junit.runner.JUnitCore TestListExamples < junit-output.txt

if [[ $(grep FAILURES junit-output.txt) == "" ]]
then
    TESTSRUN=$(grep -Eo "([0-9]+ tests)" junit-output.txt | grep -Eo "[0-9]+")
    echo $TESTSRUN/$TESTSRUN Succeeded
else
    TESTSRUN=$(grep -Eo "Tests run: [0-9]+" junit-output.txt | grep -Eo "[0-9]+")
    FAILURES=$(grep -Eo "Failures: [0-9]+" junit-output.txt | grep -Eo "[0-9]+")

    echo $(($TESTSRUN-$FAILURES))/$TESTSRUN Succeeded
fi
```

`TestListExamples.java`

```
import static org.junit.Assert.*;
import org.junit.*;

import java.util.*;

public class TestListExamples {
    public class ShortStringFilter implements StringChecker{
        @Override
        public boolean checkString(String s) {
            return s.length() < 3;
        }
    }

    @Test(timeout = 500)
    public void testFilter() {
        String[] input1Values = {"a", "ab", "abc", "bc", "abcdc", "d"};
        List<String> input1 = new ArrayList<>();

        for (String s: input1Values) {
            input1.add(s);
        }

        assertArrayEquals(new String[]{"a", "ab", "bc", "d"}, 
                ListExamples.filter(input1, new ShortStringFilter()).toArray());

    }

    @Test(timeout = 500)
    public void testMerge() {
        String[] input1Values = {"a", "e", "g", "l", "u"};
        String[] input2Values = {"a", "c", "f", "m", "o", "x", "y", "z"};

        List<String> input1 = new ArrayList<>();
        List<String> input2 = new ArrayList<>();

        for (String s: input1Values) {
            input1.add(s);
        }
        for (String s: input2Values) {
            input2.add(s);
        }

        assertArrayEquals(new String[]{"a", "a", "c", "e", "f", "g", "l", "m", "o", "u", "x", "y", "z"}, 
                ListExamples.merge(input1, input2).toArray());
    }

}
```

Command ran to trigger bug:

`bash grade.sh https://github.com/ucsd-cse15l-f22/list-methods-corrected`

Bug Fix:

Change the `<` to `>` in `java -cp $CPATH org.junit.runner.JUnitCore TestListExamples < junit-output.txt` (line 31 in `grade.sh`).

## Part 2

Some of the cool things I learned from the second half of the lab this quarter were bash scripting and `jdb`. I had heard of bash scripting before, but it was always a foreign concept to me and I never understood what I could potentially use it. Learning about how stringing together terminal commands like `grep` and utilizing error output with `$?` was really interesting and seems like a very powerful tool I can use in the future. 

`jdb` was interesting because I had always used the debugger in the IDE. I knew that debugging was a separate program that was ran on because in my limited C++ experience I knew you had to install something called `gdb` to debug it. Seeing how `jdb` works from the terminal made me much better understand how the debugging program works under the gui.
