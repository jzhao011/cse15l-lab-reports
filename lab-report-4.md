# Lab Report 4

## Log in to ieng6

![Image](/images/logintoieng6.png)

Keys Pressed: `<up> <enter> <enter>`. The command `ssh jez011@ieng6-201.ucsd.edu` is one up in search history, so I pressed `<up>` once to access it. The way my terminal is set up, I have to press `<enter>` once to confirm selection of this command and `<enter>` again to execute it, so I press it twice.

## Clone forked repository

![Image](/images/clonefork.png)

Keys Pressed: c s 1 `<Tab> <Enter>`. After typing "cs1", I press `<Tab>` to autocomplete it to the command `cs15lwi24`, then I execute it.

Keys Pressed: g i t `<Space>` c l o `<Tab> <Cmd> + V <Enter>`. After typing "git clo", I press `<Tab>` for autcomplete. Then, I press `Ctrl + V` to paste the link to the repository and execute the command, `git clone git@github.com:jzhao011/lab7.git`.

Keys Pressed: c d `<Space>` l `<Tab> <Enter>`. I type "cd l", and press `<Tab>` to autocomplete to `cd lab7/`. I then execute the command.

## Run tests, demonstrating they fail

![Image](/images/runtestsdemonstratefail.png)

Keys Pressed: `<up> <up> <up> <up> <up> <up> <enter>`, `<up> <up> <up> <up> <up> <up> <enter>`. The `javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java` command is six up in search history, so I pressed `<up>` six times to select it. After executing it, the `java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListExamples.java` command became six up in search history as well, so I selected and executed it the same way.

## Edit code to fix

![Image](/images/editcode.png)

Keys Pressed: v i m `<Space>` L `<Tab>` . `<Tab> <Enter>`. I typed "vim L" and pressed `<Tab>` to autocomplete. Since there are four files in the directory that start with "L", `ListExamples.class`, `ListExamples.java`, `ListExamplesTests.class`, and `ListExamplesTests.java`, it autocompletes to the longest shared substring that starts from the beginning and we get `vim ListExamples`. To distinguish the `ListExamples.java` file, I add a period and press `<Tab>` again. This autocompletes the command to `vim ListExamples.java`. Curiously, it never autocompletes to the `.class` file, even if you try autocompleting `vim ListExamples.c`. I am not completely sure why that is, but one possible reason could be because `vim` knows that a `.class` file is not something the user would need to edit as plain text. As such, even though the `.class` file also fits `ListExamples.`, it autocompletes to the `.java` file instead.

Keys Pressed: : 4 4 `<Enter>` e r 2 `<Esc>` : x `<Enter>`. Typing the colon brings vim to Command Mode, and typing '44' and hitting <Enter> brings the cursor to the 44th line.

## Run tests, demonstrating they succeed

![Image](/images/runtestsdemonsratesuccess.png)

`<up> <up> <up> <enter>`, `<up> <up> <up> <enter>

## commit and push changes

![Image](/images/commitandpush.png)

g i t `<Space>` a d d `<Space> <tab> <enter>`

g i t `<Space>` c o m `<tab>` - m ' F i x `<Space>` L i s t E x a m p l e s . j a v a '

g i t `<Space>` p u s h `<Enter>`

